---
title : "Giới thiệu"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

# Workshop Finance Tracker - Giới thiệu

## Tổng quan

Chào mừng bạn đến với **Workshop Finance Tracker**! Trong workshop thực hành này, bạn sẽ xây dựng một ứng dụng web serverless hoàn chỉnh để theo dõi tài chính cá nhân bằng các dịch vụ AWS. Ứng dụng này minh họa các phương pháp phát triển cloud-native hiện đại và thể hiện sức mạnh của công nghệ serverless của AWS.

## Finance Tracker là gì?

Finance Tracker là một ứng dụng web giúp người dùng quản lý tài chính cá nhân thông qua:

- 💰 **Theo dõi Chi tiêu**: Ghi lại và phân loại chi tiêu hàng ngày
- 📊 **Quản lý Ngân sách**: Đặt giới hạn ngân sách hàng tháng và theo dõi chi tiêu so với mục tiêu
- 📈 **Thông tin Tài chính**: Xem các mẫu chi tiêu và mức sử dụng ngân sách
- 🔒 **Lưu trữ An toàn**: Lưu trữ dữ liệu tài chính một cách an toàn trên đám mây

## Tại sao chọn Kiến trúc Serverless?

Workshop này sử dụng **kiến trúc serverless** mang lại nhiều lợi ích:

### Lợi ích chính:

**Không cần Quản lý Máy chủ**
- AWS quản lý toàn bộ hạ tầng
- Không cần cung cấp, mở rộng hoặc bảo trì máy chủ
- Tập trung vào logic ứng dụng thay vì hạ tầng

**Tự động Mở rộng**
- Tự động mở rộng theo nhu cầu
- Xử lý lưu lượng tăng đột biến mà không cần can thiệp thủ công
- Đảm bảo hiệu suất ổn định

**Tiết kiệm Chi phí**
- Chỉ trả tiền cho những gì bạn sử dụng
- Không có chi phí cho tài nguyên nhàn rỗi
- Hoàn hảo cho khối lượng công việc biến đổi

**Tính Sẵn sàng Cao**
- Khả năng dự phòng và chịu lỗi tích hợp sẵn
- AWS xử lý độ tin cậy của hạ tầng
- Triển khai đa AZ theo mặc định

**Phát triển Nhanh hơn**
- Giảm chi phí vận hành
- Thời gian đưa ra thị trường nhanh hơn
- Tập trung vào giá trị kinh doanh

## Công nghệ Bạn sẽ Sử dụng

### Dịch vụ AWS:

| Dịch vụ | Mục đích | Tại sao sử dụng |
|---------|---------|---------------|
| **Amazon DynamoDB** | Cơ sở dữ liệu NoSQL | Lưu trữ dữ liệu nhanh, linh hoạt cho chi tiêu và ngân sách |
| **AWS Lambda** | Serverless Compute | Thực thi logic nghiệp vụ mà không cần quản lý máy chủ |
| **Amazon API Gateway** | Quản lý API | Tạo và quản lý RESTful API |
| **AWS Amplify** | Lưu trữ Frontend | Triển khai và lưu trữ ứng dụng React với CI/CD |
| **AWS IAM** | Bảo mật & Kiểm soát Truy cập | Quản lý quyền giữa các dịch vụ |

### Công nghệ Frontend:

- **React.js** - Thư viện JavaScript hiện đại để xây dựng giao diện người dùng
- **JavaScript (ES6+)** - Ngôn ngữ lập trình cho logic frontend
- **HTML5 & CSS3** - Cấu trúc và định dạng

### Công nghệ Backend:

- **Python 3.14** - Ngôn ngữ lập trình cho hàm Lambda
- **Boto3** - AWS SDK cho Python
- **REST API** - Giao thức giao tiếp giữa frontend và backend

### Luồng Dữ liệu:

1. **Tương tác Người dùng**: Người dùng nhập dữ liệu chi tiêu hoặc ngân sách trong giao diện
2. **Yêu cầu API**: Frontend gửi yêu cầu HTTPS đến API Gateway
3. **Thực thi Lambda**: API Gateway kích hoạt hàm Lambda
4. **Xử lý Dữ liệu**: Lambda xác thực và xử lý dữ liệu
5. **Thao tác Cơ sở dữ liệu**: Lambda đọc/ghi dữ liệu vào DynamoDB
6. **Phản hồi**: Kết quả được trả về frontend và hiển thị cho người dùng
