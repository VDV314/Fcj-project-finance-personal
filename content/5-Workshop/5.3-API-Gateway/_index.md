---
title : "Create API Gateway"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 5.3. </b> "
---


In this section, you will create a REST API using Amazon API Gateway to expose your Lambda function as HTTP endpoints. The API Gateway will act as the "front door" for your application, allowing the React frontend to communicate with the backend Lambda function.

## What is Amazon API Gateway?

Amazon API Gateway is a fully managed service that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale. It acts as a gateway between your clients (web/mobile applications) and your backend services.

### Key Benefits:
- **Easy API Creation**: Create RESTful APIs with a few clicks
- **Scalability**: Automatically scales to handle any amount of traffic
- **Security**: Built-in authorization and authentication
- **Monitoring**: Integrates with CloudWatch for monitoring and logging
- **CORS Support**: Easy configuration for cross-origin requests

### API Structure

The API Gateway will have two main resources:
- `/updateBudget` - POST method to update budget information
- `/updateExpense` - POST method to add/update expenses

## Step 1: Create REST API

1. Navigate to **API Gateway** in the AWS Console
2. Click **Create API**
3. Choose **REST API** (not REST API Private)
4. Click **Build**
5. Configure the API:
   - **Choose the protocol**: REST
   - **Create new API**: New API
   - **API name**: `expenseManagerAPI`
   - **Description**: API for Finance Tracker application (optional)
   - **Endpoint Type**: Regional
6. Click **Create API**

![Create REST API](/images/5-Workshop/5.3-API-Gateway/CreateRESTAPI.png)



## Step 2: Create Resources

Now you'll create two resources (endpoints) for your API.

### 2.1 Create /updateBudget Resource

1. In the API Gateway console, select your API
2. Click **Resources** in the left sidebar
3. Select the root resource `/`
4. Click **Create Resource**
5. Configure the resource:
   - **Resource Path**: `/`
   - **Resource Name**: `updateBudget`
   - **CORS (Cross Origin Resource Sharing)**: Check this box
6. Click **Create resource**

![Budget Resource](/images/5-Workshop/5.3-API-Gateway/CreateResourcesupdateBudget.png)

### 2.2 Create /updateExpense Resource

1. Select the root resource `/` again
2. Click **Create Resource**
3. Configure the resource:
   - **Resource Path**: `/`
   - **Resource Name**: `updateExpense`
   - **CORS (Cross Origin Resource Sharing)**: Check this box
4. Click **Create resource**

![Expense Resource](/images/5-Workshop/5.3-API-Gateway/CreateResourcesupdateExpenses.png)



## Step 3: Create POST Methods

Now you'll add POST methods to both resources to handle incoming requests.

### 3.1 Create POST Method for /updateBudget

1. Select the `/updateBudget` resource
2. Click **Create method**
3. In the **Method details**:
   - **Method type**: POST
   - **Integration type**: AWS Service
   - **AWS Region**: us-east-1 (or your region)
   - **AWS Service**: DynamoDB
   - **HTTP method**: POST
   - **Action name**: PutItem
   - **Execution role**: `arn:aws:iam::{account-id}:role/APIGateway-to-DynamoDB`
   - **Credential cache**: Do not add caller credentials to cache key
   - **Content handling**: Passthrough
   - **Integration timeout**: 29000 milliseconds
4. Click **Create method**

![PostMethods](/images/5-Workshop/5.3-API-Gateway/Createmethod.png)

### 3.2 Create POST Method for /updateExpense

1. Select the `/updateExpense` resource
2. Click **Create method**
3. Configure with the same settings as above:
   - **Method type**: POST
   - **Integration type**: AWS Service
   - **AWS Service**: DynamoDB
   - **Action name**: PutItem
4. Click **Create method**

![PostMethods1](/images/5-Workshop/5.3-API-Gateway/Createmethod1.png)



## Step 4: Configure Method Integration

For each method, you need to configure how API Gateway transforms requests before sending to DynamoDB.

### 4.1 Configure Integration Request

1. Select the POST method under `/updateBudget`
2. Click **Integration request** tab
3. Click **Edit**

![ConfigMethod](/images/5-Workshop/5.3-API-Gateway/configMethod.png)
![ConfigHeaderMapping](/images/5-Workshop/5.3-API-Gateway/ConfigHeaderMapping.png)
### 4.2 Configure Mapping Templates for /updateBudget

1. Expand **Mapping templates**
2. Click **Add mapping template**
3. Enter **Content-Type**: `application/json`
4. Click the checkmark
5. In the **Template body**, enter:

```json
{
    "TableName": "budgetTrackingTable",
    "Item": {
        "id": {"S": "$input.params('budget')"},
        "actual-budget": {"S": "$input.params('budget')"}
    }
}
```

6. Click **Save**



**Method execution flow for /updateBudget:**

![Config Request to Budget](/images/5-Workshop/5.3-API-Gateway/configrequesttoBudget.png)

### 4.3 Configure Mapping Templates for /updateExpense

1. Select the POST method under `/updateExpense`
2. Click **Integration request** tab
3. Expand **Mapping templates**
4. Add a new mapping template with **Content-Type**: `application/json`
5. In the **Template body**, enter:

```json
{
    "TableName": "SpendingTrackingTable",
    "Item": {
        "id": {"S": "$input.params('id')"},
        "amount": {"N": "$input.params('amount')"},
        "category": {"S": "$input.params('category')"},
        "date": {"S": "$input.params('date')"},
        "description": {"S": "$input.params('description')"}
    }
}
```

6. Click **Save**

**Method execution flow for /updateExpense:**

![Config Request to Update Expense](/images/5-Workshop/5.3-API-Gateway/configrequesttoupdateExpense.png)



## Step 5: Enable CORS

CORS (Cross-Origin Resource Sharing) must be enabled to allow your web frontend to call the API from a different domain.

### 5.1 Enable CORS for /updateBudget

1. Select the `/updateBudget` resource
2. Click **Enable CORS**
3. Review the CORS configuration:
   - **Access-Control-Allow-Headers**: `Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token`
   - **Access-Control-Allow-Methods**: `DELETE,GET,HEAD,OPTIONS,PATCH,POST,PUT`
   - **Access-Control-Allow-Origin**: `*`
4. Click **Save**

### 5.2 Enable CORS for /updateExpense

Repeat the same steps for the `/updateExpense` resource.

![Enable CORS](/images/5-Workshop/5.3-API-Gateway/enableCORS.png)



## Step 6: Deploy API

Now that your API is configured, you need to deploy it to make it accessible.

1. Click **Deploy API** button
2. In the **Deploy API** dialog:
   - **Stage**: Select `*New stage*`
   - **Stage name**: `Dev`
   - **Deployment description**: Initial deployment (optional)
3. Click **Deploy**

![Deploy API](/images/5-Workshop/5.3-API-Gateway/DeloyApi.png)