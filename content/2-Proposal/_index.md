---
title: "Proposal"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Personal Finance Management Application
## AWS Serverless Personal Finance Management Solution

### 1. Executive Summary
The Finance Management Application is designed for individuals who need better control over their personal finances. The system supports unlimited transactions with detailed categorization, budget planning, and financial analysis. The platform leverages modern web technologies to provide real-time monitoring, detailed reports, and a user-friendly interface, with secure access managed through an authentication system. The application is scalable from individual users to supporting families with multiple members tracking shared expenses.

### 2. Problem Statement
*Current Problem*  
Many individuals struggle with manual expense tracking using spreadsheets or notebooks, which becomes difficult to manage over time. There is no centralized system for real-time financial overview or spending pattern analysis.

*Solution*  
The platform uses AWS Serverless architecture to provide a scalable financial management system. Users can access via PC or mobile view through AWS Amplify, enter transactions, categorize expenses/income, set budgets, and view real-time analytical reports.

*Benefits and Return on Investment (ROI)*  
The solution provides individuals and families with comprehensive financial management capabilities through a modern web platform. It reduces manual expense tracking time, improves budget discipline, and increases savings potential through spending awareness.

### 3. Solution Architecture
The platform uses AWS Serverless architecture to manage personal financial data with flexible scalability. Users access through a responsive web interface (PC and Mobile) hosted on AWS Amplify, interact with API Gateway to handle financial operations, and data is securely stored in DynamoDB. The architecture is designed based on serverless principles. The architecture is detailed below:

![Finance Tracker Architecture](/images/2-Proposal/architectural.png)

*AWS Services Used*
- *AWS Amplify*: Host web interface with automatic CI/CD from GitHub, supporting responsive PC view and Mobile view.
- *AWS Lambda*: Process business logic (add/edit/delete transactions, create reports).
- *Amazon API Gateway*: RESTful API gateway handling all client requests.
- *Amazon DynamoDB*: Store transaction details and user budget management.
- *Amazon CloudWatch*: Monitor logs, metrics, performance of Lambda, API Gateway, DynamoDB; automatic alerts on errors.
- *IAM (Identity and Access Management)*: IAM Roles grant permissions for Lambda to access DynamoDB, CloudWatch and IAM Permissions control detailed access rights for each AWS service.

*Component Design*
- *User Interface*: AWS Amplify hosts the application.
- *Authentication & Security*: IAM Roles and IAM Permissions manage detailed access rights for each AWS service.
- *API Gateway*: Amazon API Gateway handles all REST API requests from client.
- *Business Logic Processing*: AWS Lambda functions handle serverless processing for transaction CRUD and report generation.
- *Data Storage*: Amazon DynamoDB with 2 tables - Expenses Details (store transactions) and Budget (manage budgets).
- *System Monitoring*: Amazon CloudWatch collects logs, metrics from Lambda/API Gateway/DynamoDB.

### 4. Technical Implementation
*Implementation Phases*  
The project consists of 4 phases:
1. *Research and Draw Architecture*: Analyze requirements for income/expense management, categorization, reporting.
2. *Cost Calculation and Feasibility Check*: Use AWS Pricing Calculator to estimate and adjust.
3. *Adjust Architecture to Optimize Cost/Solution*: Optimize Lambda functions, Optimize DynamoDB query patterns.
4. *Development, Testing, Deployment*: Program interface using Reactjs, AWS services with CDK/SDK, then test and put into operation.

*Technical Requirements*
- *Front End*: HTML5, CSS3, Javascript.
- *Back End*: AWS Lambda Service, Use AWS CDK/SDK for programming, API Gateway REST API, Amazon DynamoDB Database.

### 5. Roadmap & Deployment Milestones
- *Internship (Month 1–3)*:
    - Month 1: Learn how to use AWS services.
    - Month 2: Design and adjust architecture.
    - Month 3: Deploy, test, put into use.

### 6. Budget Estimate

*Infrastructure Costs*
- AWS Lambda: $0.00/month (1,000 requests, 128 MB memory, 200 ms execution)
- Amazon API Gateway: $0.01/month (2,000 REST API requests)
- Amazon DynamoDB: $0.13/month (2 tables, 500 read/write requests, < 1 GB storage)
- AWS Amplify: $0.35/month (Static hosting, 5 build minutes/month, 2 GB served)
- Amazon S3: $0.01/month (< 1 GB storage, 1,000 requests)
- Amazon CloudWatch: $0.50/month (Lambda logs, 1 GB logs ingestion)
- Data Transfer: $0.09/month (1 GB outbound)

*Total*: $1.17 USD/month, $14.04 USD/year

### 7. Risk Assessment
*Risk Matrix*
- API Gateway unavailable: High impact, low probability
- Loss of expense data: High impact, low probability.
- Budget overrun: Medium impact, low probability.

*Mitigation Strategies*
- API Gateway: Temporary storage with LocalStorage and sync when reconnected.
- Budget overrun: Set up cost alerts.
- Loss of expense data: Enable DynamoDB Point-in-Time Recovery + periodic Export.

### 8. Expected Outcomes
*Technical Improvements*: Real-time data entry, Instant analysis with automatically updated charts instead of manual work.
*Long-term Value*: Apply to expense management to avoid overspending monthly income, can be reused for future projects.