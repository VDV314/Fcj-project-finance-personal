---
title: "Bản đề xuất"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---




# Ứng Dụng Quản Lý Tài Chính Cá Nhân
## Giải pháp AWS Serverless Quản Lý Tài Chính Cá Nhân  

### 1. Tóm tắt điều hành  
Ứng dụng Quản Lý Tài Chính được thiết kế cho các cá nhân cần kiểm soát tốt hơn tài chính cá nhân của họ. Hệ thống hỗ trợ số lượng giao dịch không giới hạn với phân loại chi tiết, lập kế hoạch ngân sách, và phân tích tài chính. Nền tảng tận dụng các công nghệ web hiện đại để cung cấp giám sát theo thời gian thực, báo cáo chi tiết, và giao diện thân thiện với người dùng, với truy cập an toàn được quản lý thông qua hệ thống xác thực. Ứng dụng có khả năng mở rộng từ người dùng cá nhân đến hỗ trợ gia đình với nhiều thành viên theo dõi chi tiêu chung.

### 2. Tuyên bố vấn đề  
*Vấn đề hiện tại*  
Nhiều cá nhân gặp khó khăn với việc theo dõi chi tiêu thủ công bằng bảng tính hoặc sổ ghi chép, điều này trở nên khó quản lý theo thời gian. Không có hệ thống tập trung cho tổng quan tài chính theo thời gian thực hoặc phân tích mô hình chi tiêu

*Giải pháp*  
Nền tảng sử dụng kiến trúc AWS Serverless  để cung cấp hệ thống quản lý tài chính và có khả năng mở rộng. Người dùng có thể truy cập qua PC hoặc mobile view thông qua AWS Amplify, nhập giao dịch, phân loại chi tiêu/thu nhập, đặt ngân sách, và xem báo cáo phân tích theo thời gian thực. 

*Lợi ích và hoàn vốn đầu tư (ROI)*  
Giải pháp cung cấp cho cá nhân và gia đình khả năng quản lý tài chính toàn diện thông qua nền tảng web hiện đại. Nó giảm thời gian theo dõi chi tiêu thủ công, cải thiện kỷ luật ngân sách , và tăng khả năng tiết kiệm thông qua nhận thức chi tiêu.  

### 3. Kiến trúc giải pháp  
Nền tảng sử dụng kiến trúc AWS Serverless để quản lý dữ liệu tài chính cá nhân với khả năng mở rộng linh hoạt. Người dùng truy cập qua giao diện web responsive (PC và Mobile) được host trên AWS Amplify, tương tác với API Gateway để xử lý các thao tác tài chính, và dữ liệu được lưu trữ an toàn trong DynamoDB. Kiến trúc được thiết kế theo nguyên tắc serverless. Kiến trúc được mô tả chi tiết bên dưới:



![Finance Tracker Architecture](/images/2-Proposal/architectural.png)

*Dịch vụ AWS sử dụng*  
- *AWS Amplify*: Host giao diện web với CI/CD tự động từ GitHub, hỗ trợ PC view và Mobile view responsive.
- *AWS Lambda*: Xử lý logic nghiệp vụ (thêm/sửa/xóa giao dịch,tạo báo cáo).  
- *Amazon API Gateway*: Cổng API RESTful xử lý tất cả requests từ client.  
- *Amazon DynamoDB*: Lưu trữ chi tiết giao dịch và Quản lý ngân sách người dùng.  
- *Amazon CloudWatch*:Giám sát logs, metrics, performance của Lambda, API Gateway, DynamoDB; cảnh báo tự động khi có lỗi.  
- *IAM (Identity and Access Management)*: IAM Roles Phân quyền cho Lambda truy cập DynamoDB, CloudWatch và IAM Permissions Kiểm soát chi tiết quyền truy cập từng dịch vụ AWS.

*Thiết kế thành phần*  
- *Giao diện người dùng*: AWS Amplify host ứng dụng.  
- *Xác thực & bảo mật*: IAM Roles và IAM Permissions quản lý quyền truy cập chi tiết cho từng dịch vụ AWS.  
- *Cổng API*: Amazon API Gateway xử lý tất cả REST API requests từ client.  
- *Xử lý logic nghiệp vụ*: AWS Lambda functions xử lý serverless cho CRUD giao dịch và tạo báo cáo.  
- *Lưu trữ dữ liệu*: Amazon DynamoDB gồm 2 tables - Expenses Details (lưu giao dịch) và Budget (quản lý ngân sách).  
- *Giám sát hệ thống*: Amazon CloudWatch thu thập logs, metrics từ Lambda/API Gateway/DynamoDB.  

### 4. Triển khai kỹ thuật  
*Các giai đoạn triển khai*  
Dự án gồm 2 phần — thiết lập trạm thời tiết biên và xây dựng nền tảng thời tiết — mỗi phần trải qua 4 giai đoạn:  
1. *Nghiên cứu và vẽ kiến trúc*: Phân tích yêu cầu quản lý thu chi, phân loại theo danh mục, báo cáo.  
2. *Tính toán chi phí và kiểm tra tính khả thi*: Sử dụng AWS Pricing Calculator để ước tính và điều chỉnh.  
3. *Điều chỉnh kiến trúc để tối ưu chi phí/giải pháp*: Tối ưu Lambda function,Tối ưu DynamoDB query patterns.  
4. *Phát triển, kiểm thử, triển khai*: Lập trình giao diện bằng Reactjs,AWS services với CDK/SDK,sau đó kiểm thử và đưa vào vận hành.  

*Yêu cầu kỹ thuật*  
- *Front End*: HTML5, CSS3,Javascript.  
- *Back End*: Service AWS Lambda,Sử dụng AWS CDK/SDK để lập trình,API Gateway REST API,Database Amazon DynamoDB.  

### 5. Lộ trình & Mốc triển khai  
- *Thực tập (Tháng 1–3)*:  
    - Tháng 1: Học cách sử dụng các dịch vụ AWS.  
    - Tháng 2: Thiết kế và điều chỉnh kiến trúc.  
    - Tháng 3: Triển khai, kiểm thử, đưa vào sử dụng.  

### 6. Ước tính ngân sách  

*Chi phí hạ tầng*  
- AWS Lambda: $0.00/tháng (1.000 requests, 128 MB memory, 200 ms execution)
- Amazon API Gateway: $0.01/tháng (2.000 REST API requests)
- Amazon DynamoDB: $0.13/tháng (2 tables, 500 read/write requests, < 1 GB storage)
- AWS Amplify: $0.35/tháng (Static hosting, 5 build minutes/tháng, 2 GB served)
- Amazon S3: $0.01/tháng (< 1 GB storage, 1.000 requests)
- Amazon CloudWatch: $0.50/tháng (Lambda logs, 1 GB logs ingestion)
- Data Transfer: $0.09/tháng (1 GB outbound)

*Tổng*: $1.17 USD/tháng, $14.04 USD/năm


### 7. Đánh giá rủi ro  
*Ma trận rủi ro*  
- API Gateway không khả dụng: Ảnh hưởng cao, xác suất thấp
- Mất dữ liệu chi tiêu: Ảnh hưởng cao, xác suất thấp.  
- Vượt ngân sách: Ảnh hưởng trung bình, xác suất thấp.  

*Chiến lược giảm thiểu*  
- API Gateway: Lưu trữ tạm thời với LocalStorage và đồng bộ khi kết nối lại.  
- Vượt ngân sách: Thiết lập cảnh báo chi phí.  
- Mất dữ liệu chi tiêu: Bật DynamoDB Point-in-Time Recovery + Export định kỳ.  

### 8. Kết quả kỳ vọng  
*Cải tiến kỹ thuật*: Nhập liệu real-time,Phân tích tức thì với biểu đồ tự động cập nhật thay làm thủ công.  
*Giá trị dài hạn*: Áp dụng cho quản lý chi tiêu tránh sử dụng quá mức thu nhập hàng tháng, có thể tái sử dụng cho các dự án tương lai.