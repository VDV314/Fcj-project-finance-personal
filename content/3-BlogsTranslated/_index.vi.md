---
title: "Các bài blogs đã dịch"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---



###  [Blog 1 - Giới thiệu Cedar Analysis: Công cụ mã nguồn mở để xác minh các chính sách ủy quyền](3.1-Blog1/)
Blog này giới thiệu Cedar Analysis, một bộ công cụ mã nguồn mở mới giúp các nhà phát triển xác minh hành vi của các chính sách ủy quyền Cedar. Bạn sẽ tìm hiểu về Cedar - một hệ thống ủy quyền để triển khai kiểm soát truy cập chi tiết, những thách thức trong việc quản lý ủy quyền ở quy mô lớn, và cách Cedar Analysis sử dụng các kỹ thuật suy luận tự động để phân tích toàn diện các chính sách. Bài viết hướng dẫn bạn qua Cedar Symbolic Compiler, các khả năng của Cedar Analysis CLI để so sánh các tập hợp chính sách và phát hiện xung đột, đồng thời trình bày các ví dụ thực tế về tái cấu trúc chính sách với xác minh chính thức để đảm bảo các chính sách hoạt động như mong muốn trong mọi tình huống.

###  [Blog 2 - Cách Stellantis tối ưu hóa việc quản lý giấy phép dùng song song bằng điều phối không máy chủ trên AWS](3.2-Blog2/)
Blog này giới thiệu cách Stellantis chuyển đổi hệ thống quản lý giấy phép người dùng được chỉ định thủ công thành giải pháp giấy phép dùng song song tự động bằng các dịch vụ không máy chủ của AWS. Bạn sẽ tìm hiểu về những thách thức trong việc quản lý số lượng giấy phép phần mềm hạn chế trên các nhóm phát triển đang mở rộng, cách kiến trúc hướng sự kiện sử dụng Amazon EventBridge, AWS Lambda, Amazon DynamoDB và AWS Systems Manager để tự động gán và thu hồi giấy phép dựa trên trạng thái của các phiên workbench. Bài viết trình bày quy trình làm việc hoàn chỉnh từ tài khoản người dùng đến tài khoản máy chủ giấy phép, giải thích lợi ích của quản lý giấy phép tập trung, và cho thấy cách giải pháp không máy chủ này giảm gánh nặng quản trị đồng thời tối ưu hóa việc sử dụng giấy phép cho Virtual Engineering Workbench (VEW).

###  [Blog 3 - Sử dụng Strands Agents với khả năng tư duy đan xen của Claude 4](3.3-Blog3/)
Blog này giới thiệu cách sử dụng Strands Agents SDK với tính năng tư duy đan xen (interleaved thinking) đang ở giai đoạn beta của Claude 4 để xây dựng các AI agent giải quyết các tác vụ phức tạp bằng công cụ. Bạn sẽ tìm hiểu về phương pháp tiếp cận dựa trên mô hình, trong đó các nhà phát triển trang bị cho agent các công cụ và prompt thay vì định nghĩa các quy trình làm việc cứng nhắc, cách vòng lặp sự kiện quản lý các lời gọi mô hình và thực thi công cụ, cũng như khả năng tư duy đan xen mạnh mẽ cho phép Claude suy ngẫm và điều chỉnh kế hoạch một cách linh hoạt sau khi gọi công cụ. Bài viết trình bày các ví dụ thực tế như tính toán khoảng cách đến ISS, giải thích sự khác biệt giữa tư duy đan xen và phương pháp ReAct truyền thống, và cho thấy cách bật tính năng này trên Amazon Bedrock để xây dựng các AI agent tinh vi và hiệu quả hơn.
