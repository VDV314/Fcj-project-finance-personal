---
title: "Translated Blogs"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

###  [Blog 1 - Introducing Cedar Analysis: Open Source Tools for Verifying Authorization Policies](3.1-Blog1/)
This blog introduces Cedar Analysis, a new open source toolkit that helps developers verify the behavior of their Cedar authorization policies. You will learn about Cedar - an authorization system for implementing fine-grained access controls, the challenges of managing authorization at scale, and how Cedar Analysis uses automated reasoning techniques to comprehensively analyze policies. The article guides you through the Cedar Symbolic Compiler, the Cedar Analysis CLI capabilities for comparing policy sets and detecting conflicts, and demonstrates practical examples of policy refactoring with formal verification to ensure policies behave as intended across all scenarios.

###  [Blog 2 - How Stellantis streamlines floating license management with serverless orchestration on AWS](3.2-Blog2/)
This blog introduces how Stellantis transformed their manual named user license management system into an automated floating license solution using AWS serverless services. You will learn about the challenges of managing limited software licenses across growing development teams, how the event-driven architecture uses Amazon EventBridge, AWS Lambda, Amazon DynamoDB, and AWS Systems Manager to automatically assign and revoke licenses based on workbench instance states. The article demonstrates the complete workflow from user accounts to license server account, explains the benefits of centralized license management, and shows how this serverless solution reduces administrative overhead while optimizing license utilization for the Virtual Engineering Workbench (VEW).

###  [Blog 3 - Using Strands Agents with Claude 4 Interleaved Thinking](3.3-Blog3/)
This blog introduces how to use Strands Agents SDK with Claude 4's interleaved thinking beta feature to build AI agents that solve complex tasks with tools. You will learn about the model-driven approach where developers equip agents with tools and prompts instead of defining rigid workflows, how the event loop manages model invocations and tool executions, and the powerful interleaved thinking capability that allows Claude to reflect and adjust plans dynamically after tool calls. The article demonstrates practical examples like calculating ISS distances, explains the differences between interleaved thinking and traditional ReAct methods, and shows how to enable this feature on Amazon Bedrock to build more sophisticated, efficient AI agents.
