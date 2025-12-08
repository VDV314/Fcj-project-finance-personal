---
title: "Blog 1"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Giới thiệu Cedar Analysis: Bộ công cụ mã nguồn mở giúp xác minh các chính sách phân quyền

*bởi Spencer Erickson và Liana Hadarean | 16/06/2025*

Hôm nay, chúng tôi rất vui mừng được giới thiệu **Cedar Analysis** — một bộ công cụ mã nguồn mở mới dành cho các nhà phát triển, giúp việc xác minh hành vi của các chính sách Cedar trở nên dễ dàng hơn bao giờ hết.

---

## Cedar là gì?

**Cedar** là một hệ thống phân quyền mã nguồn mở (open source authorization system) cho phép các nhà phát triển triển khai kiểm soát truy cập chi tiết (fine-grained access controls) trong ứng dụng của họ. Với hơn **1,17 triệu lượt tải** và mức độ sử dụng ngày càng tăng, Cedar nhanh chóng thu hút được sự quan tâm của cộng đồng lập trình viên. Cedar vừa là một ngôn ngữ để viết các chính sách phân quyền (authorization policies), vừa là một hệ thống để đánh giá các chính sách đó nhằm đưa ra quyết định kiểm soát truy cập.

Thay vì nhúng trực tiếp logic phân quyền vào mã nguồn của ứng dụng, Cedar cho phép các nhà phát triển định nghĩa các chính sách độc lập, xác định rõ ai có thể làm gì trong ứng dụng của họ. Cedar đã nhận được đóng góp từ các tổ chức nổi bật như **MongoDB** và **StrongDM**, và các công ty này cùng nhiều doanh nghiệp khác đã sử dụng Cedar trong môi trường sản xuất (production). Việc được áp dụng rộng rãi này là minh chứng cho độ tin cậy và hiệu quả của Cedar trong các kịch bản thực tế.

---

## Thách thức của việc phân quyền khi mở rộng quy mô

Khi ứng dụng mở rộng quy mô, việc phân quyền trở nên phức tạp hơn, khiến cho các công cụ mạnh mẽ để quản lý và xác minh chính sách truy cập trở nên vô cùng quan trọng. Hầu hết các nhóm phát triển hiện nay thường kiểm thử các tình huống cụ thể, chẳng hạn như:
- "Alice có thể xem tài liệu này không?"
- "Bob có thể chỉnh sửa thư mục đó không?"

Tuy nhiên, cách tiếp cận này chỉ phát hiện được các vấn đề rõ ràng trong các trường hợp mẫu, và không đảm bảo độ bao phủ toàn hệ thống.

Các tổ chức cần một phương pháp toàn diện hơn để hiểu rõ tác động của các thay đổi trong chính sách đối với toàn bộ hệ thống, đặc biệt khi ứng dụng phát triển và yêu cầu bảo mật liên tục thay đổi.

Ngay từ đầu, Cedar đã được thiết kế với khả năng phân tích (analysis) trong tư duy, sử dụng các **kỹ thuật suy luận tự động (automated reasoning techniques)** để hiểu các chính sách và kiểm tra mọi kịch bản truy cập có thể xảy ra, chứ không chỉ dựa vào các trường hợp kiểm thử đơn lẻ. Cách tiếp cận này giúp phát hiện các thay đổi ngoài ý muốn về quyền truy cập trước khi chúng gây ra sự cố trong môi trường thực tế.

---

## Giới thiệu Bộ công cụ Cedar Analysis

Bộ công cụ Cedar Analysis mới cung cấp các công cụ mạnh mẽ và khả năng suy luận tự động để phân tích và xác thực toàn diện các chính sách phân quyền, đảm bảo rằng chúng hoạt động đúng như mong đợi trong mọi trường hợp.

### Với Cedar Analysis, bạn có thể trả lời các câu hỏi như:

- Hai chính sách này có tương đương nhau không?
- Việc thay đổi này trong chính sách của tôi có cấp thêm quyền không mong muốn nào không?
- Việc tái cấu trúc (refactoring) chính sách này có làm gián đoạn các mô hình truy cập hiện có không?
- Có chính sách nào trong hệ thống của tôi không hiệu quả hoặc mâu thuẫn không?
- Chính sách mới được thêm vào có vô tình chặn toàn bộ quyền truy cập không?

Dù bạn đang tái cấu trúc một chính sách phức tạp, thêm các điều kiện mới, hay chỉ đơn giản là muốn hiểu sâu hơn về cách chính sách của mình hoạt động, Cedar Analysis mang đến cho bạn bộ công cụ cần thiết để phát triển và hoàn thiện các chính sách phân quyền cùng với sự tăng trưởng của ứng dụng.

---

## Chúng tôi phát hành những gì dưới dạng mã nguồn mở?

Bản phát hành hôm nay bao gồm hai thành phần chính:

### 1. Cedar Symbolic Compiler

Một trình biên dịch chuyển đổi các chính sách Cedar thành các công thức toán học có thể được phân tích tự động. Chúng tôi đã chứng minh tính chính xác của quá trình chuyển đổi này bằng phương pháp toán học, đảm bảo rằng kết quả phân tích khớp chính xác với cách các chính sách của bạn sẽ hoạt động trong môi trường sản xuất.

### 2. Cedar Analysis CLI

Một công cụ dòng lệnh minh họa cách tận dụng Cedar Symbolic Compiler để phân tích chính sách. Nó triển khai hai khả năng phân tích ví dụ:
- **So sánh các tập hợp chính sách** để hiểu các thay đổi về quyền truy cập
- **Phân tích các tập hợp chính sách riêng lẻ** để xác định các điểm không nhất quán, dư thừa và lỗi logic

Các triển khai này cho thấy cách xây dựng các công cụ phân tích bằng cách sử dụng cấu trúc mã hóa biểu tượng (symbolic encoding) do trình biên dịch cung cấp.

> **Lưu ý:** Công cụ CLI đóng vai trò như một bản triển khai tham chiếu nhằm minh họa các cách tiếp cận phân tích khả thi bằng cách sử dụng symbolic encoding của Cedar. Mặc dù nó triển khai các kiểm tra cơ bản hữu ích, nhưng nó chỉ đại diện cho một phần nhỏ của các khả năng phân tích tiềm năng có thể được xây dựng dựa trên symbolic encoding này. Chúng tôi khuyến khích các nhà phát triển sử dụng CLI để học tập thực hành, khám phá và phát triển các bản thử nghiệm (proof-of-concept), đồng thời hy vọng cộng đồng sẽ tạo ra các công cụ phân tích tinh vi hơn, được tùy chỉnh phù hợp với nhu cầu cụ thể của họ.

---

## Công nghệ đằng sau Cedar Analysis

### Sử dụng các bộ giải SMT Solvers

Cốt lõi của Cedar Analysis là việc sử dụng **SMT – Satisfiability Modulo Theories** (tính thỏa mãn theo các mô hình lý thuyết) để suy luận về các chính sách.

Công cụ này chuyển đổi các chính sách Cedar của bạn thành các công thức toán học mô tả mọi yêu cầu có thể được phép theo chính sách đó. Sau đó, các bộ giải SMT chuyên biệt như **CVC5** sẽ phân tích các công thức này.

Ví dụ, với chính sách đơn giản sau:

```cedar
// Allow users to view and comment on resources they own.
permit(
    principal,
    action in [Action::"view", Action::"comment"],
    resource
) when {
    principal == resource.owner
};
```

Trình biên dịch biểu tượng Cedar (Cedar Symbolic Compiler) sẽ chuyển đổi chính sách này thành một công thức toán học mô tả chính xác các yêu cầu được phép thực hiện.

Sau đó, bộ giải SMT có thể trả lời các câu hỏi phức tạp về hành vi của chính sách, chẳng hạn như: *"Liệu chính sách này có bao giờ cho phép một người không phải là chủ sở hữu xem tài nguyên không?"*

---

### Xác minh hình thức với Lean (Formal Verification with Lean)

Trình biên dịch biểu tượng Cedar (Cedar Symbolic Compiler) được triển khai bằng **Lean**, một ngôn ngữ lập trình hàm đồng thời là công cụ hỗ trợ chứng minh (proof assistant). Tính chất "kép" này cho phép chúng ta vừa triển khai trình biên dịch, vừa chứng minh tính đúng đắn của nó một cách toán học.

Nhờ đó, chúng ta có thể đảm bảo hai yếu tố quan trọng:

| Thuộc tính | Mô tả |
|-----------|-------|
| **Tính đúng đắn (Soundness)** | Khi Cedar Analysis xác nhận rằng chính sách của bạn đáp ứng một thuộc tính nhất định (ví dụ: "không có truy cập trái phép"), điều đó được đảm bảo đúng trong mọi trường hợp có thể xảy ra. Bộ giải SMT bên dưới sẽ xây dựng một chứng minh toán học để xác thực điều này. |
| **Tính đầy đủ (Completeness)** | Nếu Cedar Analysis báo rằng chính sách của bạn không đáp ứng một thuộc tính, điều đó có nghĩa là tồn tại ít nhất một trường hợp mà thuộc tính đó bị vi phạm. Phân tích này cung cấp kết quả chính xác mà không có cảnh báo sai. |

Những chứng minh toán học này mang lại sự tin cậy cao rằng kết quả phân tích phản ánh chính xác cách các chính sách của bạn sẽ hoạt động trong môi trường thực tế. Đối với các nhà nghiên cứu và giới học thuật, khía cạnh xác minh hình thức này khiến Cedar Analysis trở thành nền tảng đặc biệt giá trị cho nghiên cứu sâu hơn về phân tích chính sách phân quyền.

---

## Các khả năng của Cedar Analysis CLI

### 1. So sánh các tập chính sách (Comparing Policy Sets)

Cedar Analysis CLI giúp bạn hiểu mối quan hệ giữa các tập chính sách bằng cách phân loại cách chúng cho phép các quyền truy cập, bao gồm:

- **Equivalent (Tương đương)**: Cả hai tập chính sách cho phép chính xác cùng một tập yêu cầu
- **More Permissive (Cho phép nhiều hơn)**: Tập chính sách mới cho phép tất cả các yêu cầu mà tập cũ cho phép, và còn cho phép thêm
- **Less Permissive (Hạn chế hơn)**: Tập chính sách mới giới hạn quyền nhiều hơn so với tập chính sách cũ
- **Incomparable (Không thể so sánh)**: Mỗi tập chính sách cho phép một số yêu cầu mà tập còn lại không cho phép

---

### 2. Phát hiện xung đột và trùng lặp trong chính sách (Detecting Policy Conflicts and Redundancies)

Cedar Analysis CLI cũng có thể xác định các vấn đề phổ biến trong một tập hợp chính sách, bao gồm:

- **Shadowed Permits**: Khi một câu lệnh permit không có tác dụng vì đã có một permit khác cho phép tất cả các yêu cầu tương tự
- **Impossible Conditions**: Khi một permit không bao giờ có thể cho phép bất kỳ yêu cầu nào do các điều kiện mâu thuẫn nhau
- **Forbid Overrides**: Khi một permit không có tác dụng, vì một forbid từ chối tất cả các yêu cầu mà permit đó cho phép
- **Complete Denials**: Khi một tập chính sách từ chối tất cả các yêu cầu đối với một số hành động nhất định

---

## Cedar Analysis trong thực tế: Tái cấu trúc chính sách (Policy Refactoring)

Hãy cùng xem xét một ví dụ thực tế về cách Cedar Analysis có thể giúp bạn tái cấu trúc các chính sách một cách tự tin.

Giả sử bạn có một ứng dụng chia sẻ ảnh với chính sách sau:

```cedar
permit(
    principal,
    action in [Action::"view", Action::"comment"],
    resource
) when {
    principal == resource.owner ||
    ((resource.filename like "*.png" ||
    resource.filename like "*.jpg") && !resource.private)
};
```

Chính sách này cho phép người dùng:
- Xem hoặc bình luận về những bức ảnh mà họ sở hữu
- Xem hoặc bình luận về các tệp PNG hoặc JPEG công khai

---

### Tái cấu trúc chính sách

Khi ứng dụng của bạn phát triển, bạn có thể muốn tách chính sách này thành nhiều chính sách riêng biệt để cải thiện khả năng đọc và bảo trì:

```cedar
// Allow owners to view and comment on their resources
permit(
    principal,
    action in [Action::"view", Action::"comment"],
    resource
) when {
    principal == resource.owner
};

// Allow access to image files
permit(
    principal,
    action in [Action::"view", Action::"comment"],
    resource
) when {
    resource.filename like "*.png" ||
    resource.filename like "*.jpg"
};

// Block access to private files
forbid(
    principal,
    action in [Action::"view", Action::"comment"],
    resource
) when {
    resource.private
};
```

Những chính sách này có vẻ tương đương, nhưng liệu chúng có thực sự như vậy không?

Hãy sử dụng Cedar Analysis CLI để kiểm tra:

```bash
cedar-lean-cli analyze compare \
     refactored_policy_set.cedar \
     original_policy_set.cedar \
     photo_app.cedarschema
```

---