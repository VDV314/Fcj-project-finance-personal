---
title : "Các bước chuẩn bị"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### IAM permissions
Gắn IAM permission policy sau vào tài khoản aws user của bạn để triển khai và dọn dẹp tài nguyên trong workshop này.
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

#### Khởi tạo DynamoDB
Tạo 2 bảng DynamoDB theo như hình:
![SpendingTrackingTable](/images/5-Workshop/5.2-Prerequisite/SpendingTable.png)
![BudgetTrackingTable](/images/5-Workshop/5.2-Prerequisite/BudgetTable.png)
Kết Qủa Sau Khi Tạo Xong:
![ListTable](/images/5-Workshop/5.2-Prerequisite/listTable.png)
