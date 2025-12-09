---
title: "Triển khai Frontend với Amplify"
date: "`r Sys.Date()`" 
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Triển khai Frontend với AWS Amplify

Trong phần này, bạn sẽ triển khai ứng dụng React Finance Tracker lên AWS Amplify, cung cấp giải pháp hosting hoàn chỉnh với khả năng CI/CD.

## Tổng quan

AWS Amplify sẽ:
- Host ứng dụng React frontend của bạn
- Cung cấp tự động build khi commit code
- Bật HTTPS theo mặc định
- Cung cấp tên miền tùy chỉnh (tùy chọn)
- Cung cấp CI/CD pipeline từ GitHub

## Yêu cầu Trước khi Bắt đầu

Trước khi bắt đầu, đảm bảo bạn có:
- Tài khoản GitHub
- Repository Finance Tracker đã được fork vào tài khoản của bạn
- URL Repository: `https://github.com/VDV314/financeTracker`

## Các Bước Triển khai với Amplify

### 1. Điều hướng đến AWS Amplify Console

1. Mở AWS Management Console
2. Tìm kiếm **AWS Amplify** trong thanh tìm kiếm dịch vụ
3. Nhấp vào **AWS Amplify**
4. Nhấp **Create new app**

![Start Building](/images/5-Workshop/5.5-Amplify/Create-new-app.png)

### 2. Chọn Source Code Provider

Trên trang **Start building with Amplify**:

1. Trong **Deploy your app**, chọn **GitHub**
2. Nhấp **Next**

![Choose GitHub](/images/5-Workshop/5.5-Amplify/buildAmplify.png)

### 3. Thêm Repository và Branch

1. Trong ô tìm kiếm, nhập: `VDV314/financeTracker`
2. Chọn repository của bạn từ dropdown
3. Đối với **Branch**, chọn `main`
4. Nhấp **Next**

![Add Repository](/images/5-Workshop/5.5-Amplify/add-repository.png)

### 4. Cấu hình App Settings

Trên trang **App settings**:

1. **App name**: Nhập `financeTracker`
2. **Build settings**: Amplify sẽ tự động phát hiện framework của bạn

Build settings sẽ hiển thị:
- **Framework**: None (hoặc React nếu được phát hiện)
- **Frontend build command**: (để trống hoặc tự động phát hiện)
- **Build output directory**: `/` (thư mục gốc)

![App Settings](/images/5-Workshop/5.5-Amplify/app-settings.png)

3. Nhấp **Next**

### 5. Xem lại và Deploy

1. Xem lại tất cả các cài đặt của bạn:
   - **Repository details**: github / VDV314/financeTracker
   - **Branch**: main
   - **App settings**: financeTracker
   - **Framework**: None
   - **Advanced settings**: Standard build, default image

![Review](/images/5-Workshop/5.5-Amplify/review.png)

2. Nhấp **Save and deploy**

### 6. Theo dõi Quá trình Triển khai

Amplify bây giờ sẽ:
1. **Provision**: Thiết lập môi trường hosting
2. **Build**: Cài đặt dependencies và build ứng dụng của bạn
3. **Deploy**: Triển khai ứng dụng của bạn lên CDN
4. **Verify**: Chạy kiểm tra sau triển khai

![Deployment Progress](/images/5-Workshop/5.5-Amplify/deployment-progress.png)

Quá trình này thường mất 3-5 phút.

{{% notice info %}}
Bạn có thể nhấp vào từng giai đoạn để xem log chi tiết và khắc phục sự cố nếu có.
{{% /notice %}}

### 7. Tổng quan financeTracker

Trong Amplify console overview, bạn có thể thấy:

**Phần Get to production** với các tùy chọn để:
1. **Add a custom domain**: Sử dụng domain riêng của bạn với HTTPS
2. **Enable firewall protections**: Sử dụng AWS WAF cho bảo mật
3. **Connect new branches**: Thiết lập nhiều môi trường

**Phần Branches** hiển thị:
- Branch **main** (Production branch)
- **Domain**: URL do Amplify cung cấp của bạn
- **Last deployment**: Dấu thời gian
- **Last commit**: Thông điệp Git commit mới nhất

![Overview](/images/5-Workshop/5.5-Amplify/Overview.png)