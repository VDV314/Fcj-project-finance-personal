---
title: "Blog 3"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---


# Sử dụng Strands Agents với khả năng tư duy đan xen của Claude 4

*bởi Ryan Coleman, Jawhny Cooke, Mark Roy, và Vadim Omeltchenko | 13/06/2025*

Khi chúng tôi giới thiệu Strands Agents SDK, mục tiêu của chúng tôi là đơn giản hóa và linh hoạt hóa việc phát triển agent bằng cách áp dụng phương pháp tiếp cận dựa trên model-driven approach. Hôm nay, chúng tôi rất vui được giới thiệu cách bạn có thể kết hợp tính năng "tư duy đan xen" (interleaved thinking) hiện đang ở giai đoạn beta của Claude 4 với Strands, để đơn giản hóa hơn nữa quá trình viết các AI agent có khả năng giải quyết các tác vụ phức tạp bằng cách sử dụng công cụ.

Với phương pháp dựa trên mô hình, các nhà phát triển không còn cần phải định nghĩa các quy trình làm việc cứng nhắc để gọi công cụ và phân tích phản hồi của mô hình ở từng bước nhằm hoàn thành một nhiệm vụ. Với Strands Agents, bạn chỉ cần trang bị cho mô hình các công cụ và một prompt sau đó mô hình có thể lập kế hoạch, nối chuỗi suy nghĩ, gọi công cụ, và tự phản tư. Strands sẽ quản lý một vòng lặp sự kiện xung quanh các lời gọi mô hình cho đến khi xác định rằng nhiệm vụ đã được hoàn thành, rồi trả kết quả phản hồi lại cho client.

Hãy cùng xem cách hoạt động của cơ chế này thông qua ví dụ đơn giản dưới đây (giả sử bạn đã hoàn thành phần quickstart):

```python
from strands import Agent
from strands_tools import http_request, python_repl

agent = Agent(
    model="us.anthropic.claude-sonnet-4-20250514-v1:0",
    tools=[http_request, python_repl]
)

prompt="""
Which of the following cities is closest to the ISS?
Portland, Vancouver, Seattle, or New York?
 
Include the current altitude of the ISS, and the distance and vector from the closest city to the ISS.
"""

agent(prompt)
```

Strands có đầy đủ mọi thứ cần thiết để tạo ra một câu trả lời được trang bị với mô hình Claude 4 Sonnet, prompt, và các công cụ để tạo mã Python cũng như thực hiện các yêu cầu HTTP. Dưới đây là cách vòng lặp sự kiện hoạt động.

Trước hết, Strands sẽ cấu trúc lại prompt của bạn và bất kỳ lịch sử hội thoại nào trước đó thành một định dạng mà mô hình ngôn ngữ có thể hiểu được. Sau đó, Strands tự động tải các công cụ có sẵn—đây có thể là các công cụ từ MCP Server, hoặc các hàm Python tùy chỉnh được đánh dấu bằng @tool. Phần docstring trong mã Python sẽ được dùng làm mô tả của công cụ, còn type hints sẽ xác định cấu trúc của các tham số. Trong ví dụ này, chúng ta sử dụng hai công cụ tích hợp sẵn từ gói strands-agents-tools.

Bộ SDK sẽ quản lý lỗi (ví dụ như giới hạn tốc độ – rate limiting hoặc tràn ngữ cảnh – context overflow), tự động thử lại khi cần, và ghi lại chi tiết nhật ký, dấu vết và số liệu để hỗ trợ quan sát và giám sát.

Khi thiết lập xong, Strands sẽ quản lý vòng lặp sự kiện gọi mô hình, xử lý việc gọi công cụ, và duy trì cuộc hội thoại cho đến khi mô hình tạo ra câu trả lời cuối cùng.

---

## Cách vòng lặp sự kiện hoạt động

**Gọi mô hình và suy luận**: Vòng lặp sự kiện sẽ gọi mô hình ngôn ngữ với trạng thái hội thoại hiện tại, prompt, và các công cụ. Mô hình sẽ truyền trực tiếp các phản hồi của nó, bao gồm cả các bước suy luận từng phần mà bạn có thể quan sát được khi mô hình "thinks out loud".

**Phát hiện và thực thi việc sử dụng công cụ**: Nếu mô hình quyết định rằng nó cần gọi một công cụ (ví dụ: để lấy dữ liệu, thực hiện phép tính, v.v.), thì vòng lặp sự kiện sẽ phát hiện yêu cầu này, xác thực nó, và thực thi hàm Python hoặc công cụ MCP Server tương ứng, sử dụng các tham số mà mô hình cung cấp.

**Cập nhật ngữ cảnh**: Kết quả của quá trình thực thi công cụ sẽ được thêm vào cuộc hội thoại đang diễn ra, cho phép mô hình sử dụng thông tin mới này trong lần suy luận tiếp theo.

---

## Ví dụ: Tính toán khoảng cách đến ISS

Bạn sẽ thấy vòng lặp này hoạt động khi chạy ví dụ ISS bằng Python. Mô hình lập kế hoạch sử dụng các công cụ của nó, bao gồm các lệnh gọi API để lấy dữ liệu thời gian thực về ISS và sử dụng Python REPL (Read–Evaluate–Print Loop) để tính toán khoảng cách và vectơ.

Agent sẽ trả lời câu hỏi với một phản hồi bao gồm nội dung như sau: "Hiện tại, Trạm Vũ trụ Quốc tế (ISS) đang nằm phía trên Tây Thái Bình Dương, khiến cho các thành phố ở miền Tây Bắc của Bắc Mỹ (Vancouver, Seattle, Portland) gần hơn nhiều so với New York. Vĩ độ hơi cao hơn của Vancouver mang lại cho nó lợi thế so với Seattle và Portland."

Mô hình lập kế hoạch sử dụng các công cụ của mình, bao gồm các lệnh gọi API để lấy dữ liệu thời gian thực về Trạm Vũ trụ Quốc tế (ISS) và sử dụng Python REPL để tính toán khoảng cách và vectơ. Sau đó, mô hình tạo ra một câu trả lời và hiển thị quá trình thực hiện của nó. Đoạn sau đây là ví dụ về kết quả đầu ra của agent này khi chạy cục bộ.

```
Để trả lời câu hỏi này, tôi cần thực hiện các bước sau:
1. Lấy vị trí hiện tại của Trạm Vũ trụ Quốc tế (ISS)
2. Lấy tọa độ của bốn thành phố: Portland, Vancouver, Seattle và New York
3. Tính khoảng cách từ mỗi thành phố đến ISS
4. Xác định thành phố nào gần nhất và cung cấp thông tin được yêu cầu

Trước hết, tôi cần tìm một API cung cấp vị trí hiện tại của ISS. Có một số API miễn phí cho việc này, chẳng hạn như:
- http://api.open-notify.org/iss-now.json (cung cấp vị trí hiện tại của ISS)
- Hoặc các API khác có thể cung cấp thông tin chi tiết hơn.

Hãy bắt đầu bằng việc lấy vị trí hiện tại của ISS.
```

```
Tôi sẽ giúp bạn xác định thành phố nào gần ISS nhất bằng cách lấy vị trí hiện tại của Trạm Vũ trụ Quốc tế và tính toán khoảng cách. Bắt đầu bằng việc truy xuất vị trí hiện tại của ISS.

Công cụ #1: http_request
```

```
Tuyệt vời! Tôi đã lấy được vị trí hiện tại của ISS:
- Vĩ độ (Latitude): -24.4214°
- Kinh độ (Longitude): 14.4347°
- Dấu thời gian (Timestamp): 1749832513

Bây giờ, tôi cần lấy độ cao (altitude) của ISS. API cơ bản không cung cấp thông tin này, nhưng tôi biết rằng ISS bay ở độ cao khoảng 400–420 km. Hãy thử tìm một API chi tiết hơn hoặc dùng giá trị trung bình đã biết.

Thực tế, hãy thử một API khác có thể cung cấp thông tin chi tiết hơn về ISS:
```

```
Bây giờ hãy lấy thêm dữ liệu chi tiết về ISS, bao gồm cả độ cao.

Công cụ #2: http_request
```

```
Hoàn hảo! Giờ tôi đã có thông tin chi tiết hơn về ISS:
- Tên: ISS
- Vĩ độ: -24.844251408166°
- Kinh độ: 14.692696204258°
- Độ cao: 422.41885597014 km
- Vận tốc: 27,570.247286017 km/h
- Dấu thời gian: 1749832520

Bây giờ tôi cần tính khoảng cách từ mỗi thành phố đến ISS. Để làm điều này, tôi cần tọa độ của từng thành phố, sau đó tính khoảng cách 3D có tính đến độ cao của ISS.

Các thành phố cần tọa độ:
1. Portland (giả định là Portland, Oregon)
2. Vancouver (giả định là Vancouver, British Columbia, Canada)
3. Seattle, Washington
4. New York, New York

Hãy sử dụng Python để tính các khoảng cách này.
```

```
Tuyệt vời! Giờ tôi đã có dữ liệu ISS hiện tại. Tiến hành tính khoảng cách từ từng thành phố đến ISS bằng Python:

Công cụ #3: python_repl
```

```
Vị trí hiện tại của ISS:
Vĩ độ: -24.844251408166°
Kinh độ: 14.692696204258°
Độ cao: 422.42 km

Kết quả tính toán:

Portland:
  Tọa độ: 45.5152°, -122.6784°
  Khoảng cách 3D đến ISS: 15,588.43 km
  Khoảng cách bề mặt: 15,582.71 km

Vancouver:
  Tọa độ: 49.2827°, -123.1207°
  Khoảng cách 3D đến ISS: 15,484.80 km
  Khoảng cách bề mặt: 15,479.03 km

Seattle:
  Tọa độ: 47.6062°, -122.3321°
  Khoảng cách 3D đến ISS: 15,493.28 km
  Khoảng cách bề mặt: 15,487.52 km

New York:
  Tọa độ: 40.7128°, -74.006°
  Khoảng cách 3D đến ISS: 11,680.55 km
  Khoảng cách bề mặt: 11,672.91 km

Thành phố gần nhất: New York
Khoảng cách: 11,680.55 km
```

```
Tuyệt vời! Giờ tôi đã có tất cả các khoảng cách. New York là thành phố gần ISS nhất. Tiếp theo, tôi cần tính vectơ từ New York đến ISS. Một vectơ bao gồm hướng và độ lớn. Tôi sẽ tính:
1. Góc phương vị (bearing) từ New York đến ISS
2. Góc cao (elevation angle)
3. Khoảng cách (đã có ở trên)

Hãy tính các thông tin này:
```

```
Công cụ #4: python_repl
```

```
══════════════════════════════════════════════════
KẾT QUẢ CUỐI CÙNG
══════════════════════════════════════════════════

Trạng thái hiện tại của ISS:
• Độ cao: 422.42 km trên Trái Đất
• Vị trí: -24.8443°, 14.6927°
• Khu vực: Trên Đại Tây Dương phía Nam

Phân tích khoảng cách:
• Portland, OR: 15,588.43 km
• Vancouver, BC: 15,484.80 km
• Seattle, WA: 15,493.28 km
• New York, NY: 11,680.55 km

Thành phố gần nhất: NEW YORK
• Khoảng cách: 11,680.55 km
• Góc phương vị (bearing): 110.1° (Đông – Đông Nam)
• Góc cao (elevation): 2.1° trên đường chân trời

Vectơ từ New York đến ISS:
• Hướng: 110.1° (Đông – Đông Nam)
• Góc cao: 2.1°
• Khoảng cách: 11,680.55 km
```

```
Tuyệt vời! Tôi đã có toàn bộ thông tin được yêu cầu. Tóm tắt kết quả chính:

1. Độ cao hiện tại của ISS: 422.42 km so với Trái Đất
2. Thành phố gần nhất: New York
3. Khoảng cách và vectơ:
   - Khoảng cách: 11,680.55 km
   - Hướng: 110.1° (Đông – Đông Nam)
   - Góc cao: 2.1° trên đường chân trời

ISS hiện đang ở trên khu vực Đại Tây Dương phía Nam, tại tọa độ -24.8443°, 14.6927°. Trong bốn thành phố, New York gần ISS hơn đáng kể so với ba thành phố còn lại ở bờ Tây (Vancouver, Seattle, Portland). New York gần hơn khoảng 4,800 km so với thành phố bờ Tây gần nhất (Vancouver).
```

### Câu trả lời: **New York** là thành phố gần ISS nhất

#### Trạng thái hiện tại của ISS:
- **Độ cao:** 422.42 km trên Trái Đất
- **Vị trí:** 24.84°S, 14.69°E (trên Đại Tây Dương phía Nam)

#### So sánh khoảng cách:
- **New York, NY:** 11,680.55 km ✅ **GẦN NHẤT**
- **Vancouver, BC:** 15,484.80 km
- **Seattle, WA:** 15,493.28 km
- **Portland, OR:** 15,588.43 km

#### Vectơ từ New York đến ISS:
- **Khoảng cách:** 11,680.55 km
- **Hướng:** 110.1° góc phương vị (Đông – Đông Nam)
- **Góc cao:** 2.1° trên đường chân trời

ISS hiện đang ở trên khu vực Đại Tây Dương phía Nam, khiến New York gần hơn đáng kể so với ba thành phố ở miền Tây Bắc Thái Bình Dương. New York gần hơn khoảng 4,800 km so với thành phố bờ Tây gần nhất (Vancouver).

---

## Tăng cường vòng lặp sự kiện của Strands bằng khả năng tư duy xen kẽ của Claude 4

Claude 4 giới thiệu một tính năng beta có tên là "interleaved thinking", tính năng này phù hợp hoàn hảo với cách tiếp cận dựa trên mô hình của Strands. Tính năng này cho phép Claude suy ngẫm lại sau mỗi lần gọi công cụ (tool call) và điều chỉnh kế hoạch của mình một cách linh hoạt, mà không cần phải chờ hoàn tất toàn bộ một vòng lặp sự kiện (event loop) hiện tại. Tư duy xen kẽ mở rộng khả năng của mô hình trong việc tự phản tỉnh, sửa lỗi, và tổ chức một quy trình kết hợp giữa suy luận và sử dụng công cụ.

Nếu bạn đang sử dụng Amazon Bedrock làm nhà cung cấp mô hình cho Strands, bạn có thể bật tính năng interleaved thinking bằng cách chỉ định thêm tham số additional request fields trong yêu cầu gửi đến Bedrock:

```python
from strands import Agent
from strands_tools import http_request, python_repl
from strands.models import BedrockModel

model = BedrockModel(
    model_id="us.anthropic.claude-sonnet-4-20250514-v1:0",
    additional_request_fields={
        "anthropic_beta": ["interleaved-thinking-2025-05-14"],
        "thinking": {"type": "enabled", "budget_tokens": 8000},
    },
)

agent = Agent(
    model=model,
    tools=[http_request, python_repl]
)
```

Nếu bạn bật tracing with Strands, bạn sẽ thấy các khối bổ sung có tên là "reasoningContent" xuất hiện trong bản ghi vết, giống như trong hình dưới đây—bao gồm cả phần suy luận được ghi lại khi Claude 4 quyết định thực hiện tư duy xen kẽ sau khi gọi công cụ.

> *Hình minh họa ví dụ về reasoning content*

Ví dụ, trong phản hồi này có một phép tính sai từ lệnh gọi công cụ, mà Claude có thể nhận biết và sửa ngay lập tức, trước khi tiếp tục sang vòng lặp tiếp theo của event loop.

```
Xếp hạng các thành phố (từ gần đến xa):
1. Vancouver: 12027.19 km
2. Seattle: 12144.49 km
3. Portland: 12195.86 km
4. New York: 15432.29 km

Phép tính hiệu số kinh độ có vẻ sai—nó hiển thị 243.6443° vượt quá 180°. Điều này xảy ra vì chúng ta đang xử lý đường đổi ngày quốc tế. Hãy sửa lại phép tính này để hiển thị đường đi ngắn nhất thực tế.
```

```
Hãy sửa lại phép tính kinh độ để hiển thị đường đi ngắn nhất thực tế:

Công cụ #6: python_repl
```

```
PHÉP TÍNH VECTƠ ĐÃ ĐƯỢC SỬA LẠI:
═══════════════════════════════════════════════════════════════

Độ cao hiện tại của ISS: 418.15 km trên bề mặt Trái Đất

Thành phố gần nhất: Vancouver, BC
Khoảng cách đến ISS: 12027.19 km

Vectơ từ Vancouver đến ISS:
- **Khoảng cách:** 12027.19 km
- **Hướng:** 110.1° góc phương vị (Đông – Đông Nam)
- **Góc cao:** 2.1° trên đường chân trời
```

---

## So sánh tư duy xen kẽ với phương pháp ReAct truyền thống

So với phương pháp ReAct truyền thống, cách tiếp cận tư duy xen kẽ nhanh hơn và mượt mà hơn. Bạn có thể hình dung ReAct giống như một thám tử ghi chép và suy luận từng bước một, trong khi tư duy xen kẽ giống như một chuyên gia trong lĩnh vực đang cân nhắc các sự kiện trong đầu trong khi giải thích một khái niệm. Nói cách khác, suy nghĩ và hành động diễn ra trong một khối tư duy duy nhất, chứ không phải một vòng lặp hoàn chỉnh khác.

Đây là một ví dụ khác trong đó Claude có thể giảm số lượng lệnh gọi công cụ bằng cách nhận ra rằng nó có thể tính toán câu trả lời từ thông tin đã truy xuất được trong lần gọi API đầu tiên, điều mà nếu không thì chỉ có thể được xác định trong vòng lặp sự kiện thứ hai.

```
Hãy bắt đầu bằng cách lấy vị trí của ISS. Tôi sẽ giúp bạn tìm thành phố nào gần ISS nhất bằng cách lấy vị trí hiện tại của ISS và tính toán khoảng cách. Hãy bắt đầu bằng việc truy xuất vị trí hiện tại của ISS.

Công cụ #1: http_request
```

```
Tuyệt vời! Tôi đã lấy được vị trí ISS. Bây giờ tôi cần lấy độ cao của ISS. API Open Notify không cung cấp độ cao trong endpoint vị trí cơ bản, nhưng tôi biết rằng ISS thường bay ở độ cao khoảng 400-420 km. Hãy kiểm tra xem có endpoint nào khác cung cấp thông tin chi tiết hơn không.

Thực tế, hãy sử dụng Python để tính khoảng cách giữa ISS và từng thành phố. Tôi sẽ cần:
- Xác định tọa độ của từng thành phố
- Tính khoảng cách từ mỗi thành phố đến vị trí ISS
- Tính đến độ cao của ISS trong phép tính

Vị trí ISS hiển thị:
• Vĩ độ: 44.7381°
• Kinh độ: 169.1833°

Điều này đặt ISS ở đâu đó trên Thái Bình Dương, có thể giữa Nga và Alaska.
```

---

## Xây dựng các Agent phức tạp hơn

Các ví dụ này chỉ là phần nổi của tảng băng về những gì bạn có thể xây dựng với Strands và Claude 4 sử dụng tư duy xen kẽ. Chúng tôi đã công bố các mẫu agent bổ sung giải quyết các vấn đề phức tạp hơn và được trang bị nhiều công cụ, như mẫu này minh họa tư duy xen kẽ. Sử dụng các ví dụ này, bạn sẽ thấy suy luận năng động hơn từ Claude, như học hỏi từ một lệnh gọi công cụ thất bại và thử lại với các tham số được tinh chỉnh, hoặc đưa ra các chiến lược mới một cách linh hoạt thay vì lặp lại cùng một chiến lược qua nhiều lần gọi công cụ.

---

## Kết luận

Chúng tôi xây dựng Strands Agents để đơn giản hóa việc phát triển agent bằng cách tận dụng các mô hình như Claude 4 có khả năng phân tích vấn đề thành các quy trình làm việc với công cụ để đạt được kết quả. Chúng tôi rất mong được xem những gì bạn xây dựng với Strands. Tham gia thảo luận tại https://github.com/strands-agents/sdk-python

*Jawhny Cooke, Vadim Omeltchenko, và Mark Roy đã đóng góp cho bài viết này.*

---