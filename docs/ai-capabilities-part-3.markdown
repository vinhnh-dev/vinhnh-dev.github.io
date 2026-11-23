---
layout: page
title: Phần 3 - Tổng hợp
permalink: /ai-capabilities-part-3/
parent: AI Capabilities & Limitations
grand_parent: Claude
nav_order: 3
---

# Phần 3: Tổng hợp (bài 8-9)

## Bài 8: Chẩn đoán lỗi của AI (~25 phút)

**Ý chính:** bốn thuộc tính không hoạt động riêng lẻ. **Phần lớn lỗi thực tế là hai thuộc tính gặp nhau.** Khi gọi tên được đó là hai thuộc tính nào, bạn biết nên dùng cách sửa nào.

**Cặp chẩn đoán cần nhận ra**

| Cặp | Biểu hiện |
|---|---|
| **Next Token Prediction + Knowledge** | Chi tiết cụ thể bị bịa (hallucinated citations, tên, số liệu) |
| **Working Memory + Steerability** | Trôi dần trong hội thoại dài (long-conversation drift) |
| Cũng gặp | Tính toán sai mà tự tin (steerability + NTP); chấp nhận tiền đề xấu quá dễ (sycophancy + steerability) |

**Cách sửa theo thuộc tính giới hạn:** kiểm chứng chi tiết cụ thể; cấp lại ngữ cảnh; chuyển phép tính sang **thực thi code**; mời AI **phản biện**.

**Liên hệ 4D:** chẩn đoán này là **Discernment** áp dụng: bạn đánh giá tốt hơn khi biết mình đang nhìn kiểu "sai" nào.

### Tương tác: When Properties Collide

Kéo hai thuộc tính lại gần nhau để xem điều gì xảy ra khi chúng va chạm.

### Bài tập "The Failure Diagnosis"

1. Nghĩ lại 2-3 lần đầu ra AI làm bạn thất vọng hay bất ngờ; mỗi lần mô tả: hỏi gì, nhận gì, điều gì đáng thất vọng.
2. Cùng AI duyệt từng sự kiện: *"Dựa trên 4 thuộc tính (Next Token Prediction, Knowledge, Working Memory, Steerability), thuộc tính nào có khả năng liên quan và vì sao?"*
3. Đối chiếu chẩn đoán với hiểu biết hiện tại; nếu không đồng ý thì phản bác. (Nhớ dấu vân tay **xu nịnh**: AI có thể đồng tình với cách đóng khung của bạn quá dễ.)
4. Với mỗi chẩn đoán hỏi *"Với chẩn đoán đó, cách sửa nhắm trúng nhất là gì?"* và thử ngay trên task tương tự.
5. Xem danh sách việc ở bài 1 với mọi chú thích tích lũy (nhãn thuộc tính, điểm kiểm chứng, cờ tri thức, nhu cầu ngữ cảnh, câu mục tiêu); với những việc gây rắc rối nhất, ghi **cặp thuộc tính đã va chạm**.

**Tự nhìn lại:** gọi tên cặp thuộc tính có đổi cách sửa bạn chọn không? Cặp nào bạn sẽ gặp nhiều nhất trong công việc hằng ngày?

## Bài 9: Áp dụng khung 4D để có đầu ra AI tốt hơn (~20 phút)

**Ý chính:** dùng AI thành thạo không phải học thuộc mọi kiểu lỗi mà là **giữ một mô hình nhỏ, rõ ràng về cỗ máy trong đầu**, để khi có gì sai bạn nhận ra là kiểu sai nào và phản ứng tương ứng.

**Điểm cần nhớ**
- Bạn đã có mô hình thực dụng: **4 thuộc tính là các dải liên tục**, lỗi đặc trưng là **giao điểm của thuộc tính**.
- Khung này và **khung 4D là hai nửa của một hệ thống**: các thuộc tính giải thích điều mà các năng lực 4D đang đáp lại.
- **Niềm tin có hiệu chỉnh** = định vị task của bạn trên từng dải và khớp thói quen kiểm chứng và cấp ngữ cảnh với vị trí đó.
- Mô hình sẽ còn đổi; **hình dạng của các thuộc tính vẫn hữu ích** dù ranh giới dịch chuyển.

**Bài tập "Your Commitment"**
1. Quay lại danh sách việc ở bài 1; mỗi việc ghi nhanh: nằm ở đâu trên dải của từng thuộc tính, cần biện pháp giảm thiểu nào?
2. Chọn **một task và một thay đổi** sẽ làm tuần này (bước kiểm chứng, thiết lập ngữ cảnh thường trực, checkpoint, thói quen nêu mục tiêu chứ không chỉ định dạng) và ghi lại.

**Tự nhìn lại:** thay đổi lớn nhất trong cách bạn nghĩ về hành vi AI từ bài 1 đến nay là gì? Trong 4D, năng lực nào được mài sắc nhất ngay lúc này?

**Bước tiếp theo:** nếu chưa học, khóa **AI Fluency: Framework & Foundations** là bước tự nhiên (đi sâu vào các năng lực của con người mà khóa này đã cho bạn ngữ cảnh phía máy). Tiếp tục thử các ranh giới: thuộc tính ổn định, còn vị trí của các đường ranh sẽ tiếp tục dịch chuyển khi mô hình cải thiện.
