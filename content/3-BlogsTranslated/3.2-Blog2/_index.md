---
title: "Blog 2"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---


# How Stellantis streamlines floating license management with serverless orchestration on AWS

*by Göksel SARIKAYA and Milosz Stawarski on 12 JUN 2025*

This post is written by Goeksel Sarikaya, Senior Delivery Consultant at AWS, and Milosz Stawarski, Senior Software Architect at Stellantis.

Software licensing is a critical aspect of many organizations' operations, with various models available to suit different needs. Two common types are named user licenses, which are assigned to specific individuals, and floating licenses, which can be shared among a pool of users. Some independent software vendors (ISVs) offer both options, whereas others might have limitations, particularly in cloud environments.

In this post, we explore a unique scenario where an ISV, unable to provide a floating license option for cloud usage, worked with Stellantis to develop an alternative solution. This approach, implemented with the ISV's permission, treats named user licenses as if they were floating, automatically assigning and removing them based on the state of user workbench instances.

This solution is not intended to circumvent licensing terms or reduce costs at the expense of ISVs. Rather, it's a collaborative approach to address specific customer needs when traditional floating licenses aren't available. We will demonstrate how the solution uses serverless AWS services like Amazon EventBridge, AWS Lambda, Amazon DynamoDB, and AWS Systems Manager, keeping in mind that any similar implementation should only be pursued with explicit permission from the software vendor.

---

## Overview of Stellantis

Stellantis N.V., born from the merger of FCA and PSA Group, leads the change towards software defined vehicles (SDV). As part of this transformation, AWS and Stellantis created the Virtual Engineering Workbench (VEW), a modular framework to develop, integrate, and test vehicle software in the cloud, ultimately connecting their vehicles to the cloud.

The VEW provides predefined environments tailored to specific use cases. These environments come fully equipped with the tools, integrated development environments (IDEs), and licensing necessary for developers to jumpstart their projects.

For more details on VEW, refer to Stellantis' SDV transformation with the Virtual Engineering Workbench on AWS.

---

## Overview of solution

As the number of developers and projects grew, Stellantis faced a challenge in managing the limited number of named user licenses for their software tools. The manual process of assigning and revoking licenses became increasingly time-consuming and inefficient, potentially hindering the agility and productivity of their development teams.

Stellantis and AWS tackled this challenge head-on by collaborating on an innovative, dynamic license management solution using AWS serverless services. This solution transforms the traditional named user license model into a more flexible floating license system, automatically assigning and revoking licenses based on the state of user workbench instances. The licenses and solution discussed in this post pertain solely to the use of standalone software tools such as those used in automotive domains. These do not involve sharing of user data or content when licenses are reused.

Before we dive into the detailed workflow of the solution, let's examine the high-level architecture. The following diagram illustrates how various AWS services work together to create this efficient license management system.

![blog2 Architecture](/images/3-Blog/Architecture-blog2.png)

## Architecture 
This architecture uses key AWS services such as EventBridge, Lambda, DynamoDB, and Systems Manager to create a scalable, serverless solution that significantly reduces administrative overhead and optimizes license utilization.

In the following sections, we explore each component of this architecture in detail, explaining how they interact to provide a seamless license management experience for Stellantis' VEW.

---

## In workbench accounts (user accounts)

The design is serverless and based on an event-driven approach. The workflow in the user accounts is as follows:

1. Workbench instances are Amazon Elastic Compute Cloud (Amazon EC2). Their start and stop automatically sends AWS events.

2. An EventBridge rule invokes a Lambda function when such an event occurs. This function checks the tags on the EC2 instance to distinguish workbenches from other EC2 instances. Two tags are important for identifying workbench instances: `vew:workbench:ownerId` and `vew:workbench:type`.

3. The Lambda function creates a custom event with the following data: `user-id`, `workbench-type`, `workbench-state`, and `instance-id`, and sends this event to the default event bus.

4. An EventBridge rule forwards the custom event to a custom event bus in the license server account.

---

## In license server account

The following steps take place in the license server account:

1. An EventBridge rule invokes a Lambda function.

2. This function interacts with a DynamoDB table that stores a mapping of licensed products to users. The function does the following:
   - Deduces the licensed products present in the workbench from the workbench type.
   - For each licensed product, it verifies if the combination of product and user is already present in the DynamoDB table.
   - If the workbench is starting:
     - If the combination is already present, it increases the count of workbenches in the table for this item by 1.
     - If the combination is not present, it creates a new item in the table (product, user-id, workbench-count, timestamp).
   - If the workbench is stopping, it decreases the count of workbenches in the table for this item by 1. If the count becomes 0, the item is deleted.

3. Any update to the DynamoDB table triggers another Lambda function.

4. If the change in the table is a creation of a new entry or deletion of an entry, this function writes the current timestamp to a Systems Manager parameter in both cases. This is so that if no changes are detected in the database, we don't unnecessarily run the xLC (License Client for related product) caller function.

5. Another Lambda function is invoked every minute. It compares the timestamp written in the Systems Manager parameter indicating a DynamoDB item creation or deletion with the last time the function called the xLC CLI to assign users to a license.

6. If the DynamoDB timestamp is earlier, the function stops. If the DynamoDB timestamp is later, the function queries the table for obtaining the user-id for each product.

7. To maintain a comprehensive record of license assignment operations, you can enable data plane events for DynamoDB in AWS CloudTrail.

8. For each licensed product, the function uses Run Command, a capability of Systems Manager, to invoke the xLC CLI API on the license server to assign named users to a license for a product. The function provides the list of users assigned to the product to the API. This updates the named user list on the license server—the list is completely overwritten, which includes adding new user IDs and removing ones that are no longer needed.

---

## Benefits and key features

The solution offers the following benefits:

- **Automated license assignment and removal** – Users are automatically assigned licenses when their workbench instances start, and licenses are returned to the pool when instances stop, providing efficient license utilization.

- **Scalable and serverless architecture** – The solution is built on serverless AWS services, allowing it to scale seamlessly as the number of users and workbench instances grows, without the need for provisioning or managing servers.

- **Centralized license management** – The license server account acts as a central hub for managing licenses across multiple workbench accounts, simplifying administration and providing a unified view of license usage.

- **Reduced administrative overhead** – By automating the license assignment and removal process, the solution can significantly reduce the administrative burden associated with manual license management.

- **Optimized license utilization** – Licenses are assigned only when needed and returned to the pool when no longer required, maximizing license availability and minimizing idle licenses.

- **Monitoring and metrics** – The solution provides monitoring capabilities and license usage metrics, enabling better visibility and informed decision-making regarding license procurement and allocation.

---

## Conclusion

By implementing this serverless solution, it is possible to transform a manual named user license management systems to an automated floating license system for software tools. The event-driven architecture and serverless components provide efficient and scalable license assignment and removal based on the workbench instance state.

This solution has streamlined the license management process, reducing administrative overhead and optimizing license utilization. It is now possible to provision software tools more efficiently, improving productivity and resource allocation across the organization. Additionally, the centralized license management and monitoring capabilities provide better visibility and control over license usage, enabling informed decision-making and cost optimization.

Overall, this AWS based floating license solution has empowered organizations to use software tools more effectively, while minimizing the operational burden associated with license management. For more serverless learning resources, visit Serverless Land.

---
