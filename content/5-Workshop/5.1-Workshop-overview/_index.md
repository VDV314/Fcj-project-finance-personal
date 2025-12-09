---
title: "Introduction"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Finance Tracker Workshop - Introduction

## Overview

Welcome to the **Finance Tracker Workshop**! In this hands-on workshop, you will build a complete serverless web application for tracking personal finances using AWS services. This application demonstrates modern cloud-native development practices.

## What is Finance Tracker?

Finance Tracker is a web-based application that helps users manage their personal finances by:

- 💰 **Tracking Expenses**: Record and categorize daily spending
- 📊 **Budget Management**: Set monthly budget limits and monitor spending against targets
- 📈 **Financial Insights**: View spending patterns and budget utilization
- 🔒 **Secure Storage**: Store financial data securely in the cloud

## Why Serverless Architecture?

This workshop uses a **serverless architecture** which offers several advantages:

### Key Benefits:

**No Server Management**
- AWS manages all infrastructure
- No need to provision, scale, or maintain servers
- Focus on application logic instead of infrastructure

**Automatic Scaling**
- Scales automatically based on demand
- Handles traffic spikes without manual intervention
- Ensures consistent performance

**Cost-Effective**
- Pay only for what you use
- No costs for idle resources
- Perfect for variable workloads

**High Availability**
- Built-in redundancy and fault tolerance
- AWS handles infrastructure reliability
- Multi-AZ deployment by default

**Faster Development**
- Reduced operational overhead
- Faster time to market
- Focus on business value

## Technologies You Will Use

### AWS Services:

| Service | Purpose | Why We Use It |
|---------|---------|---------------|
| **Amazon DynamoDB** | NoSQL Database | Fast, flexible data storage for expenses and budgets |
| **AWS Lambda** | Serverless Compute | Execute business logic without managing servers |
| **Amazon API Gateway** | API Management | Create and manage RESTful APIs |
| **AWS Amplify** | Frontend Hosting | Deploy and host React application with CI/CD |
| **AWS IAM** | Security & Access Control | Manage permissions between services |

### Frontend Technologies:

- **React.js** - Modern JavaScript library for building user interfaces
- **JavaScript (ES6+)** - Programming language for frontend logic
- **HTML5 & CSS3** - Structure and styling

### Backend Technologies:

- **Python 3.14** - Programming language for Lambda functions
- **Boto3** - AWS SDK for Python
- **REST API** - Communication protocol between frontend and backend

### Data Flow:

1. **User Interaction**: User enters expense or budget data in the frontend
2. **API Request**: Frontend sends HTTPS request to API Gateway
3. **Lambda Execution**: API Gateway triggers Lambda function
4. **Data Processing**: Lambda validates and processes the data
5. **Database Operation**: Lambda reads/writes data to DynamoDB
6. **Response**: Results are returned to frontend and displayed to user