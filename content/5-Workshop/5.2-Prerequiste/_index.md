---
title : "Prerequiste"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

#### IAM permissions
Add the following IAM permission policy to your user account to deploy and cleanup this workshop.
```
APIGateway-to-DynamoDB Policy/Permissions
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Action": [
                "dynamodb:putItem",
                "dynamodb:Query",
                "dynamodb:DeleteItem"
            ],
            "Resource": "*"
        }
    ]
}

```

#### Initialize DynamoDB
Create 2 DynamoDB tables as shown below:
SpendingTrackingTable
![SpendingTrackingTable](/images/5-Workshop/5.2-Prerequisite/SpendingTable.png)
BudgetTrackingTable
![BudgetTrackingTable](/images/5-Workshop/5.2-Prerequisite/BudgetTable.png)
Result After Creation:
![ListTable](/images/5-Workshop/5.2-Prerequisite/listTable.png)