---
title: "Workshop"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


# Workshop: Building Finance Tracker Application with AWS Serverless

## Overview

In this workshop, you will learn how to build a complete serverless web application for personal finance management using AWS services. The **Finance Tracker** application allows users to track expenses, manage monthly budgets, and view financial statistics - all deployed on AWS serverless architecture.

## Workshop Architecture

![Architecture Diagram](/images/2-Proposal/architectural.png)

The workshop uses a serverless architecture with the following main components:

- **Frontend**: React application hosted on AWS Amplify
- **API Layer**: Amazon API Gateway to expose REST API endpoints
- **Business Logic**: AWS Lambda function for business processing
- **Data Storage**: Amazon DynamoDB to store expense and budget data
- **Security**: AWS IAM to manage access permissions between services

## Workshop Objectives

After completing this workshop, you will:

✅ Understand how to design and deploy serverless applications on AWS  
✅ Create and configure DynamoDB tables for NoSQL data storage  
✅ Write and deploy AWS Lambda functions using Python  
✅ Create REST APIs with Amazon API Gateway  
✅ Integrate API Gateway with Lambda functions  
✅ Deploy React applications to AWS Amplify with CI/CD  
✅ Configure IAM roles and policies for security  
✅ Handle CORS for web applications  
✅ Test and debug serverless applications  


#### Content

1. [Workshop overview](5.1-Workshop-overview)
2. [Prerequiste](5.2-Prerequiste/)
3. [Access S3 from VPC](5.3-API-Gateway/)
4. [Access S3 from On-premises](5.4-Lambda/)
5. [VPC Endpoint Policies (Bonus)](5.5-Amplify/)
