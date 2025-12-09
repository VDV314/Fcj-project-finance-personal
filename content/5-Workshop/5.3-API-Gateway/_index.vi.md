---
title: "Tạo API Gateway"
date: "`r Sys.Date()`" 
weight: 3 
chapter: false
pre: " <b> 5.3. </b> "
---



Trong phần này, bạn sẽ tạo một REST API sử dụng Amazon API Gateway để expose hàm Lambda của bạn thành các HTTP endpoint. API Gateway sẽ đóng vai trò như "cửa trước" cho ứng dụng của bạn, cho phép React frontend giao tiếp với hàm Lambda backend.

## Amazon API Gateway là gì?

Amazon API Gateway là một dịch vụ được quản lý hoàn toàn giúp các nhà phát triển dễ dàng tạo, xuất bản, duy trì, giám sát và bảo mật API ở bất kỳ quy mô nào. Nó hoạt động như một gateway giữa client của bạn (ứng dụng web/mobile) và các dịch vụ backend.

### Lợi ích chính:
- **Tạo API dễ dàng**: Tạo RESTful API chỉ với vài cú nhấp chuột
- **Khả năng mở rộng**: Tự động mở rộng để xử lý bất kỳ lượng traffic nào
- **Bảo mật**: Tích hợp sẵn authorization và authentication
- **Giám sát**: Tích hợp với CloudWatch để giám sát và ghi log
- **Hỗ trợ CORS**: Cấu hình dễ dàng cho các yêu cầu cross-origin

### Cấu trúc API

API Gateway sẽ có hai resource chính:
- `/updateBudget` - Phương thức POST để cập nhật thông tin ngân sách
- `/updateExpense` - Phương thức POST để thêm/cập nhật chi tiêu

## Bước 1: Tạo REST API

1. Điều hướng đến **API Gateway** trong AWS Console
2. Nhấp **Create API**
3. Chọn **REST API** (không phải REST API Private)
4. Nhấp **Build**
5. Cấu hình API:
   - **Choose the protocol**: REST
   - **Create new API**: New API
   - **API name**: `expenseManagerAPI`
   - **Description**: API for Finance Tracker application (tùy chọn)
   - **Endpoint Type**: Regional
6. Nhấp **Create API**

![Create REST API](/images/5-Workshop/5.3-API-Gateway/CreateRESTAPI.png)



## Bước 2: Tạo Resources

Bây giờ bạn sẽ tạo hai resource (endpoint) cho API của mình.

### 2.1 Tạo Resource /updateBudget

1. Trong API Gateway console, chọn API của bạn
2. Nhấp **Resources** trong thanh bên trái
3. Chọn root resource `/`
4. Nhấp **Create Resource**
5. Cấu hình resource:
   - **Resource Path**: `/`
   - **Resource Name**: `updateBudget`
   - **CORS (Cross Origin Resource Sharing)**: Đánh dấu vào ô này
6. Nhấp **Create resource**

![Budget Resource](/images/5-Workshop/5.3-API-Gateway/CreateResourcesupdateBudget.png)

### 2.2 Tạo Resource /updateExpense

1. Chọn lại root resource `/`
2. Nhấp **Create Resource**
3. Cấu hình resource:
   - **Resource Path**: `/`
   - **Resource Name**: `updateExpense`
   - **CORS (Cross Origin Resource Sharing)**: Đánh dấu vào ô này
4. Nhấp **Create resource**

![Expense Resource](/images/5-Workshop/5.3-API-Gateway/CreateResourcesupdateExpenses.png)



## Bước 3: Tạo POST Methods

Bây giờ bạn sẽ thêm phương thức POST vào cả hai resource để xử lý các yêu cầu đến.

### 3.1 Tạo POST Method cho /updateBudget

1. Chọn resource `/updateBudget`
2. Nhấp **Create method**
3. Trong **Method details**:
   - **Method type**: POST
   - **Integration type**: AWS Service
   - **AWS Region**: us-east-1 (hoặc region của bạn)
   - **AWS Service**: DynamoDB
   - **HTTP method**: POST
   - **Action name**: PutItem
   - **Execution role**: `arn:aws:iam::{account-id}:role/APIGateway-to-DynamoDB`
   - **Credential cache**: Do not add caller credentials to cache key
   - **Content handling**: Passthrough
   - **Integration timeout**: 29000 milliseconds
4. Nhấp **Create method**

![PostMethods](/images/5-Workshop/5.3-API-Gateway/Createmethod.png)

### 3.2 Tạo POST Method cho /updateExpense

1. Chọn resource `/updateExpense`
2. Nhấp **Create method**
3. Cấu hình với các cài đặt giống như trên:
   - **Method type**: POST
   - **Integration type**: AWS Service
   - **AWS Service**: DynamoDB
   - **Action name**: PutItem
4. Nhấp **Create method**

![PostMethods1](/images/5-Workshop/5.3-API-Gateway/Createmethod1.png)


## Bước 4: Cấu hình Method Integration

Đối với mỗi phương thức, bạn cần cấu hình cách API Gateway chuyển đổi yêu cầu trước khi gửi đến DynamoDB.

### 4.1 Cấu hình Integration Request

1. Chọn phương thức POST trong `/updateBudget`
2. Nhấp tab **Integration request**
3. Nhấp **Edit**

![ConfigMethod](/images/5-Workshop/5.3-API-Gateway/configMethod.png)
![ConfigHeaderMapping](/images/5-Workshop/5.3-API-Gateway/ConfigHeaderMapping.png)

### 4.2 Cấu hình Mapping Templates cho /updateBudget

1. Mở rộng **Mapping templates**
2. Nhấp **Add mapping template**
3. Nhập **Content-Type**: `application/json`
4. Nhấp vào dấu kiểm
5. Trong **Template body**, nhập:

```json
{
    "TableName": "budgetTrackingTable",
    "Item": {
        "id": {"S": "$input.params('budget')"},
        "actual-budget": {"S": "$input.params('budget')"}
    }
}
```

6. Nhấp **Save**

**Luồng thực thi phương thức cho /updateBudget:**

![Config Request to Budget](/images/5-Workshop/5.3-API-Gateway/configrequesttoBudget.png)

### 4.3 Cấu hình Mapping Templates cho /updateExpense

1. Chọn phương thức POST trong `/updateExpense`
2. Nhấp tab **Integration request**
3. Mở rộng **Mapping templates**
4. Thêm một mapping template mới với **Content-Type**: `application/json`
5. Trong **Template body**, nhập:

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

6. Nhấp **Save**

**Luồng thực thi phương thức cho /updateExpense:**

![Config Request to Update Expense](/images/5-Workshop/5.3-API-Gateway/configrequesttoupdateExpense.png)


## Bước 5: Bật CORS

CORS (Cross-Origin Resource Sharing) phải được bật để cho phép web frontend của bạn gọi API từ một domain khác.

### 5.1 Bật CORS cho /updateBudget

1. Chọn resource `/updateBudget`
2. Nhấp **Enable CORS**
3. Xem lại cấu hình CORS:
   - **Access-Control-Allow-Headers**: `Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token`
   - **Access-Control-Allow-Methods**: `DELETE,GET,HEAD,OPTIONS,PATCH,POST,PUT`
   - **Access-Control-Allow-Origin**: `*`
4. Nhấp **Save**

### 5.2 Bật CORS cho /updateExpense

Lặp lại các bước tương tự cho resource `/updateExpense`.

![Enable CORS](/images/5-Workshop/5.3-API-Gateway/enableCORS.png)

## Bước 6: Deploy API

Bây giờ API của bạn đã được cấu hình, bạn cần deploy nó để có thể truy cập được.

1. Nhấp nút **Deploy API**
2. Trong hộp thoại **Deploy API**:
   - **Stage**: Chọn `*New stage*`
   - **Stage name**: `Dev`
   - **Deployment description**: Initial deployment (tùy chọn)
3. Nhấp **Deploy**

![Deploy API](/images/5-Workshop/5.3-API-Gateway/DeloyApi.png)