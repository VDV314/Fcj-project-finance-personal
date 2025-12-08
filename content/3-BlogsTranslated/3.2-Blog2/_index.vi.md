---
title: "Blog 2"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---


# Cách Stellantis tối ưu hóa việc quản lý giấy phép dùng song song bằng điều phối không máy chủ trên AWS

*bởi Göksel SARIKAYA và Milosz Stawarski | 12/06/2025*

Bài viết này được viết bởi Goeksel Sarikaya, Chuyên viên tư vấn triển khai cao cấp tại AWS, và Milosz Stawarski, Kiến trúc sư phần mềm cao cấp tại Stellantis.

Giấy phép phần mềm là một khía cạnh quan trọng trong hoạt động của nhiều tổ chức, với nhiều mô hình khác nhau có sẵn để phù hợp với các nhu cầu khác nhau. Hai loại phổ biến là **giấy phép người dùng được chỉ định**, được gán cho các cá nhân cụ thể, và **giấy phép dùng song song**, có thể được chia sẻ giữa một nhóm người dùng. Một số nhà cung cấp phần mềm độc lập (ISV) cung cấp cả hai tùy chọn, trong khi những nhà cung cấp khác có thể có những giới hạn, đặc biệt là trong các môi trường đám mây.

Trong bài viết này, chúng tôi khám phá một kịch bản đặc biệt, trong đó một ISV, không thể cung cấp tùy chọn giấy phép dùng song song cho việc sử dụng trên đám mây, đã hợp tác với Stellantis để phát triển một giải pháp thay thế. Cách tiếp cận này, được triển khai với sự cho phép của ISV, xử lý các giấy phép người dùng được chỉ định như thể chúng là giấy phép dùng song song, bằng cách tự động gán và thu hồi chúng dựa trên trạng thái của các phiên làm việc người dùng.

Giải pháp này không nhằm mục đích lách các điều khoản cấp phép hoặc giảm chi phí gây bất lợi cho các ISV. Thay vào đó, đây là một cách tiếp cận hợp tác để giải quyết nhu cầu cụ thể của khách hàng khi các giấy phép dùng song song truyền thống không khả dụng. Chúng tôi sẽ trình bày cách mà giải pháp này sử dụng các dịch vụ không máy chủ (serverless) của AWS như **Amazon EventBridge**, **AWS Lambda**, **Amazon DynamoDB**, và **AWS Systems Manager**, đồng thời lưu ý rằng bất kỳ việc triển khai tương tự nào cũng chỉ nên được thực hiện khi có sự cho phép rõ ràng từ nhà cung cấp phần mềm.

---

## Tổng quan về Stellantis

**Stellantis N.V.**, được hình thành từ sự hợp nhất giữa FCA và PSA Group, đang dẫn đầu quá trình chuyển đổi hướng tới các **phương tiện được định nghĩa bằng phần mềm (Software Defined Vehicles – SDV)**.

Là một phần của quá trình chuyển đổi này, AWS và Stellantis đã cùng nhau tạo ra **Virtual Engineering Workbench (VEW)** — một khung mô-đun để phát triển, tích hợp và kiểm thử phần mềm cho xe trên nền tảng đám mây, với mục tiêu cuối cùng là kết nối các phương tiện của họ với đám mây.

VEW cung cấp các môi trường được định nghĩa sẵn được tùy chỉnh cho những trường hợp sử dụng cụ thể. Những môi trường này được trang bị đầy đủ các công cụ, môi trường phát triển tích hợp (IDE) và giấy phép cần thiết để giúp các nhà phát triển bắt đầu dự án của họ một cách nhanh chóng.

Để biết thêm chi tiết về VEW, hãy tham khảo bài viết [Stellantis' SDV transformation with the Virtual Engineering Workbench on AWS](https://aws.amazon.com/blogs/automotive/stellantis-sdv-transformation-virtual-engineering-workbench/).

---

## Tổng quan về giải pháp

Khi số lượng nhà phát triển và dự án tăng lên, Stellantis đã phải đối mặt với thách thức trong việc quản lý số lượng hạn chế các giấy phép người dùng được chỉ định cho các công cụ phần mềm của họ. Quy trình thủ công trong việc gán và thu hồi giấy phép ngày càng trở nên tốn thời gian và kém hiệu quả, có khả năng làm giảm tính linh hoạt và năng suất của các nhóm phát triển.

Stellantis và AWS đã trực tiếp giải quyết thách thức này bằng cách hợp tác xây dựng một giải pháp quản lý giấy phép động, sáng tạo, sử dụng các dịch vụ không máy chủ của AWS. Giải pháp này chuyển đổi mô hình giấy phép người dùng cố định truyền thống thành hệ thống giấy phép dùng song song linh hoạt hơn, tự động gán và thu hồi giấy phép dựa trên trạng thái của các phiên làm việc người dùng.

Các giấy phép và giải pháp được đề cập trong bài viết này chỉ áp dụng cho việc sử dụng các công cụ phần mềm độc lập, chẳng hạn như những công cụ được sử dụng trong lĩnh vực ô tô, và không liên quan đến việc chia sẻ dữ liệu hoặc nội dung người dùng khi giấy phép được tái sử dụng.

Trước khi đi sâu vào quy trình hoạt động chi tiết của giải pháp, hãy cùng xem xét kiến trúc tổng thể ở cấp độ cao. Sơ đồ sau đây minh họa cách các dịch vụ AWS khác nhau phối hợp với nhau để tạo nên một hệ thống quản lý giấy phép hiệu quả.

![blog2 Architecture](/images/3-Blog/Architecture-blog2.png)

---

## Kiến trúc

Kiến trúc này sử dụng các dịch vụ chủ chốt của AWS như **EventBridge**, **Lambda**, **DynamoDB**, và **Systems Manager** để tạo ra một giải pháp không máy chủ có khả năng mở rộng, giúp giảm đáng kể khối lượng công việc quản trị và tối ưu hóa việc sử dụng giấy phép.

Trong các phần tiếp theo, chúng ta sẽ tìm hiểu chi tiết từng thành phần trong kiến trúc này, giải thích cách chúng tương tác với nhau để mang lại trải nghiệm quản lý giấy phép liền mạch cho Virtual Engineering Workbench (VEW) của Stellantis.

---

## Trong các tài khoản Workbench (tài khoản người dùng)

Thiết kế này là không máy chủ và dựa trên mô hình **hướng sự kiện**. Luồng công việc trong các tài khoản người dùng được mô tả như sau:

### Quy trình từng bước:

1. **Sự kiện EC2 Instance**
   - Các phiên làm việc Workbench là các instance của **Amazon Elastic Compute Cloud (Amazon EC2)**
   - Khi chúng khởi động hoặc dừng, chúng tự động gửi các sự kiện AWS

2. **Kích hoạt quy tắc EventBridge**
   - Một quy tắc EventBridge sẽ kích hoạt một hàm Lambda khi sự kiện đó xảy ra
   - Hàm này kiểm tra các thẻ trên instance EC2 để phân biệt các phiên Workbench với các instance EC2 khác
   - Hai thẻ quan trọng để xác định các instance workbench:
     - `vew:workbench:ownerId`
     - `vew:workbench:type`

3. **Tạo sự kiện tùy chỉnh**
   - Hàm Lambda sẽ tạo một sự kiện tùy chỉnh (custom event) chứa các dữ liệu sau:
     - `user-id`
     - `workbench-type`
     - `workbench-state`
     - `instance-id`
   - Và gửi sự kiện này đến event bus mặc định

4. **Chuyển tiếp sự kiện**
   - Một quy tắc EventBridge khác sẽ chuyển tiếp sự kiện tùy chỉnh đó đến event bus tùy chỉnh trong tài khoản máy chủ quản lý giấy phép

---

## Trong tài khoản máy chủ giấy phép

Các bước sau đây diễn ra trong tài khoản máy chủ cấp phép:

### 1. Xử lý sự kiện

Một quy tắc EventBridge kích hoạt một hàm Lambda tương tác với một bảng **DynamoDB**, nơi lưu trữ ánh xạ giữa các sản phẩm được cấp phép và người dùng.

### 2. Logic gán giấy phép

Hàm này thực hiện các thao tác sau:

- **Suy ra** các sản phẩm được cấp phép có trong workbench dựa trên loại workbench
- Đối với mỗi sản phẩm được cấp phép, hàm sẽ kiểm tra xem cặp kết hợp giữa sản phẩm và người dùng đã tồn tại trong DynamoDB hay chưa

**Nếu workbench đang khởi động:**
- Nếu tổ hợp này đã tồn tại → hàm sẽ tăng số lượng workbench trong bảng cho mục này lên 1
- Nếu tổ hợp này chưa tồn tại → hàm sẽ tạo một mục mới trong bảng (`product`, `user-id`, `workbench-count`, `timestamp`)

**Nếu workbench đang dừng:**
- Giảm giá trị `workbench-count` trong bảng của mục này đi 1
- Nếu giá trị trở về 0 → mục đó sẽ bị xóa khỏi bảng

---

### 3. Xử lý DynamoDB Stream

Bất kỳ bản cập nhật nào đối với bảng DynamoDB cũng sẽ kích hoạt một hàm Lambda khác:

- Nếu thay đổi trong bảng là việc **tạo một mục mới** hoặc **xóa một mục**:
  - Hàm này sẽ ghi thời điểm hiện tại (current timestamp) vào một tham số trong Systems Manager trong cả hai trường hợp
  - Mục đích là để nếu không có thay đổi nào được phát hiện trong cơ sở dữ liệu, chúng ta sẽ không thực hiện hàm gọi xLC (License Client) một cách không cần thiết

---

### 4. Đồng bộ hóa giấy phép định kỳ

Một hàm Lambda khác được kích hoạt **mỗi phút**:

1. So sánh thời điểm (timestamp) được ghi trong tham số của Systems Manager (phản ánh việc tạo hoặc xóa một mục trong DynamoDB) với thời điểm lần cuối cùng hàm này gọi xLC CLI để gán người dùng cho một giấy phép

2. **Nếu timestamp trong DynamoDB cũ hơn** → hàm sẽ dừng lại

3. **Nếu timestamp trong DynamoDB mới hơn**:
   - Hàm sẽ truy vấn bảng để lấy `user-id` cho từng sản phẩm
   - Đối với mỗi sản phẩm được cấp phép, hàm sử dụng **Run Command** (một tính năng của Systems Manager) để gọi API xLC CLI trên máy chủ giấy phép
   - Gán các người dùng được chỉ định cho giấy phép của sản phẩm đó
   - Cung cấp danh sách người dùng được gán cho sản phẩm tới API
   - Cập nhật danh sách người dùng được chỉ định trên máy chủ giấy phép (danh sách này được ghi đè hoàn toàn, bao gồm việc thêm các user ID mới và xóa những user ID không còn cần thiết)

---

### 5. Dấu vết kiểm toán

Để duy trì một bản ghi đầy đủ về các hoạt động gán giấy phép, bạn có thể bật **các sự kiện data plane cho DynamoDB trong AWS CloudTrail**.

---

## Lợi ích và các tính năng chính

Giải pháp mang lại các lợi ích sau:

| Lợi ích | Mô tả |
|---------|-------|
| **Tự động gán và thu hồi giấy phép** | Người dùng tự động được gán giấy phép khi các phiên workbench của họ khởi động, và giấy phép được trả lại vào pool khi các phiên dừng, đảm bảo tối ưu hóa việc sử dụng giấy phép |
| **Kiến trúc mở rộng và không máy chủ** | Được xây dựng trên các dịch vụ serverless của AWS, cho phép mở rộng liền mạch khi số lượng người dùng và phiên workbench tăng, mà không cần phải cung cấp hay quản lý máy chủ |
| **Quản lý giấy phép tập trung** | Tài khoản máy chủ giấy phép đóng vai trò là trung tâm quản lý các giấy phép trên nhiều tài khoản workbench, đơn giản hóa việc quản trị và cung cấp cái nhìn tổng thể về việc sử dụng giấy phép |
| **Giảm gánh nặng quản trị** | Bằng cách tự động hóa quy trình gán và thu hồi giấy phép, giải pháp có thể giảm đáng kể khối lượng công việc quản trị liên quan đến việc quản lý giấy phép thủ công |
| **Tối ưu hóa việc sử dụng giấy phép** | Giấy phép chỉ được gán khi cần thiết và trả lại vào pool khi không còn sử dụng, tối đa hóa khả dụng của giấy phép và giảm thiểu giấy phép nhàn rỗi |
| **Giám sát và số liệu** | Cung cấp khả năng giám sát và các chỉ số về việc sử dụng giấy phép, giúp cải thiện tầm nhìn và đưa ra quyết định thông minh hơn về việc mua sắm và phân bổ giấy phép |

---

## Kết luận

Bằng cách triển khai giải pháp không máy chủ này, có thể chuyển đổi hệ thống quản lý giấy phép người dùng cố định thủ công thành một hệ thống giấy phép linh hoạt tự động cho các công cụ phần mềm. Kiến trúc hướng sự kiện và các thành phần serverless cung cấp quy trình gán và thu hồi giấy phép hiệu quả và có khả năng mở rộng, dựa trên trạng thái của các phiên workbench.

Giải pháp này đã tinh gọn quy trình quản lý giấy phép, giảm gánh nặng quản trị và tối ưu hóa việc sử dụng giấy phép. Hiện nay, việc cung cấp các công cụ phần mềm trở nên hiệu quả hơn, nâng cao năng suất và phân bổ tài nguyên trên toàn tổ chức. Thêm vào đó, quản lý giấy phép tập trung và khả năng giám sát cung cấp tầm nhìn và kiểm soát tốt hơn về việc sử dụng giấy phép, giúp đưa ra quyết định thông minh và tối ưu hóa chi phí.

Tóm lại, giải pháp giấy phép linh hoạt dựa trên AWS này đã giúp các tổ chức sử dụng công cụ phần mềm hiệu quả hơn, đồng thời giảm thiểu gánh nặng vận hành liên quan đến quản lý giấy phép.

---

## Bắt đầu với Serverless

Để biết thêm tài nguyên học tập về serverless, vui lòng truy cập [Serverless Land](https://serverlessland.com/).

---
