---
title: "Workshop"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


# Workshop: Xây dựng Ứng dụng Finance Tracker với AWS Serverless

## Tổng quan

Trong workshop này, bạn sẽ học cách xây dựng một ứng dụng web serverless hoàn chỉnh để quản lý tài chính cá nhân sử dụng các dịch vụ AWS. Ứng dụng **Finance Tracker** cho phép người dùng theo dõi chi tiêu, quản lý ngân sách hàng tháng, và xem các thống kê tài chính - tất cả đều được triển khai trên kiến trúc serverless của AWS.

## Kiến trúc Workshop

![Architecture Diagram](/images/2-Proposal/architectural.png)

Workshop sử dụng kiến trúc serverless với các thành phần chính:

- **Frontend**: Ứng dụng được host trên AWS Amplify
- **API Layer**: Amazon API Gateway để expose REST API endpoints
- **Business Logic**: AWS Lambda function xử lý nghiệp vụ
- **Data Storage**: Amazon DynamoDB lưu trữ dữ liệu chi tiêu và ngân sách
- **Security**: AWS IAM quản lý quyền truy cập giữa các dịch vụ

## Mục tiêu Workshop

Sau khi hoàn thành workshop này, bạn sẽ:

✅ Hiểu cách thiết kế và triển khai ứng dụng serverless trên AWS  
✅ Tạo và cấu hình DynamoDB tables để lưu trữ dữ liệu NoSQL  
✅ Viết và triển khai AWS Lambda function bằng Python  
✅ Tạo REST API với Amazon API Gateway  
✅ Tích hợp API Gateway với Lambda function  
✅ Triển khai ứng dụng React lên AWS Amplify với CI/CD  
✅ Cấu hình IAM roles và policies cho bảo mật  
✅ Xử lý CORS cho ứng dụng web  


#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Truy cập đến S3 từ VPC](5.3-API-Gateway/)
4. [Truy cập đến S3 từ TTDL On-premises](5.4-Lambda/)
5. [VPC Endpoint Policies (làm thêm)](5.5-Amplify/)
