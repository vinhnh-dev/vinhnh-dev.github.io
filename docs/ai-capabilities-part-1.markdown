---
layout: page
title: Phần 1 - Bắt đầu
permalink: /ai-capabilities-part-1/
parent: AI Capabilities & Limitations
grand_parent: Claude
nav_order: 1
---

# Phần 1: Bắt đầu (bài 1-3)

## Bài 1: Chào mừng đến với khóa học (~15 phút)

**Ý chính:** Khung 4D dạy **bạn** cách cộng tác với AI; khóa này dạy **AI làm việc với bạn được đến đâu**. Hai thứ là một hệ thống: năng lực của con người ở một bên, thuộc tính của máy ở bên kia.

**Lộ trình khóa học:**

| Phần | Câu hỏi |
|---|---|
| Ta nói "AI" là gì | AI tạo sinh là gì, khác các loại AI khác thế nào? |
| AI được huấn luyện thế nào | Pretraining và fine-tuning tạo "tính cách" ra sao? |
| Thuộc tính của AI | Next token prediction, knowledge, working memory, steerability |
| Ghép lại | Chuyện gì xảy ra khi các thuộc tính va nhau ngoài đời thật? |
| Bước tiếp theo | Dùng hiểu biết này để dùng AI an toàn, hiệu quả, có đạo đức thế nào? |

**Điểm cần nhớ**
- Khung 4D mô tả năng lực người; khóa này mô tả thuộc tính máy mà các năng lực ấy đáp lại.
- AI tạo sinh có **4 thuộc tính cốt lõi:** Next Token Prediction, Knowledge, Working Memory, Steerability.
- Nội dung này **bền** vì các thuộc tính ổn định ngay cả khi mô hình cải thiện: ranh giới dịch chuyển nhưng thuộc tính vẫn vậy.

**Bài tập: lập bản đồ việc bạn đang dùng AI** (nền cho mọi bài sau)
1. Liệt kê 4-6 việc bạn thực sự đã dùng AI trong 2 tuần qua (hoặc muốn dùng); cụ thể ("soạn email giải thích việc trễ dự án cho khách" chứ không phải "viết").
2. Mỗi việc ghi một dòng: đầu ra dùng được ngay lần đầu hay phải làm lại?
3. Chia sẻ danh sách với Claude: *"Với mỗi việc này, nếu tôi không để ý thì có thể sai ở đâu?"*; nếu không khớp trải nghiệm, phản bác lại. **Giữ danh sách này**, bạn sẽ quay lại ở mọi bài.

**Tự nhìn lại:** việc nào thấy "an toàn" khi giao cho AI, việc nào rủi ro, giải thích được chưa? Có hành vi AI nào bạn từng thấy mà chưa giải thích được?

## Bài 2: AI tạo sinh là gì (~20 phút)

**Mục tiêu:** phân biệt AI tạo sinh với AI phân loại/dự đoán bạn gặp hằng ngày; hiểu thuộc tính của nó nằm trên một **dải liên tục** từ khả năng đến giới hạn; xem trước 4 thuộc tính.

**Ý chính:** phần lớn AI trong đời sống (bộ lọc spam, gợi ý, phát hiện gian lận) **không phải tạo sinh**. Khóa này nói về loại tạo sinh: mô hình văn bản dựa trên **transformer** sinh nội dung mới **từng token một**.

### Khung 4 thuộc tính

Mỗi thuộc tính là một dải: càng lệch về phía **giới hạn** thì càng nên **kiểm chứng và bù đắp**.

| Thuộc tính | Câu hỏi | Vùng khả năng | Vùng giới hạn |
|---|---|---|---|
| **Next Token Prediction** | Câu trả lời của AI đến từ đâu? | Lối mòn quen thuộc: tóm tắt, định dạng lại, giải thích khái niệm phổ biến | Lãnh thổ mới, mẫu thưa, "đúng" so với "nghe có vẻ đúng" |
| **Knowledge** | AI thực sự biết gì? | Thường xuyên, mới (trong dữ liệu huấn luyện), nhất quán: chủ đề phổ biến, ngôn ngữ lập trình thông dụng | Hiếm, sau cutoff, ngách, địa phương hoặc gây tranh cãi |
| **Working Memory** | AI đang chú ý vào gì ngay bây giờ? | Tài liệu vừa vặn, phiên còn mới, bạn cung cấp ngữ cảnh liên quan | Tài liệu/hội thoại rất dài, mong nhớ xuyên phiên ("vách đá") |
| **Steerability** | Tôi kiểm soát được đến đâu? | Chỉ dẫn ngắn, cụ thể, kiểm chứng được ("trả lời dạng bảng", "dưới 100 từ") | Chuỗi lập luận dài, yêu cầu trừu tượng, độ chính xác "bản địa" (tính toán) |

**Điểm cần nhớ**
- AI tạo sinh **tạo nội dung mới** thay vì phân loại nội dung có sẵn.
- AI không "đều mạnh" hay "đều không đáng tin": mạnh và yếu theo **4 trục dự đoán được**.
- Mỗi thuộc tính là một dải liên tục; **cùng một cơ chế vừa cho khả năng vừa cho giới hạn**.
- **Niềm tin có hiệu chỉnh** = xác định việc của bạn nằm ở đâu trên dải, không cấp hay rút niềm tin hàng loạt.

**Bài tập "Generative or Not?"**
1. Liệt kê 5 tính năng AI bạn dùng tuần này (autocomplete, gắn thẻ ảnh, lọc spam, chatbot, dịch, gợi ý sản phẩm, trợ lý giọng nói).
2. Với mỗi cái, phán đoán: **tạo nội dung mới** hay **sắp xếp/xếp hạng/phân loại** nội dung có sẵn?
3. Nhờ AI kiểm tra; với cái sai/chưa chắc, nhờ giải thích khác biệt trong một câu; hỏi: *"Trong 5 cái, cái nào dễ có kiểu lỗi mà khóa này sẽ giúp tôi hiểu?"*
4. Quay lại danh sách việc ở bài 1, gắn mỗi việc với câu hỏi thuộc tính phù hợp nhất lúc này: nguồn câu trả lời (NTP)? biết gì (Knowledge)? chú ý gì (Working Memory)? kiểm soát được đến đâu (Steerability)? Không cần đúng; bạn đang tạo **dự đoán** để kiểm chứng ở 4 bài sau.

**Tự nhìn lại:** phân biệt tạo sinh/phân loại có đổi cách bạn nghĩ về công cụ nào không? Có việc nào có thể thuộc nhiều thuộc tính?

## Bài 3: AI có "tính cách" từ đâu (~25 phút)

**Ý chính:** sự lịch sự, hữu ích, thận trọng của AI **không phải phép màu tự nổi lên** mà được huấn luyện vào từng lớp; mỗi giai đoạn để lại "dấu vân tay" dự đoán được trong cách nó tương tác.

### Hai giai đoạn huấn luyện

| Giai đoạn | Làm gì | Kết quả |
|---|---|---|
| **1. Pretraining** | Mô hình đọc lượng văn bản khổng lồ và học một việc: **dự đoán cái gì đến tiếp theo** | "Bộ hoàn thành tài liệu" mạnh nhưng **không có khái niệm giúp bạn** |
| **2. Fine-tuning** | Phán đoán của con người định hình nó thành **trợ lý**: coi đầu vào là yêu cầu, trả lời hữu ích, từ chối yêu cầu có hại | Hữu ích (helpful), trung thực (honest), vô hại (harmless) |

### Dấu vân tay do fine-tuning để lại

Vì fine-tuning dùng phán đoán của người về "phản hồi tốt", nên có 4 thiên hướng:
1. **Sycophancy** (xu nịnh): kéo về phía đồng tình với bạn.
2. **Verbosity** (dài dòng): mặc định nói nhiều.
3. **Over-caution** (quá thận trọng): thỉnh thoảng rào đón thừa.
4. **Loose confidence calibration:** độ tự tin nói ra lệch với độ tin cậy thật.

**Bài tập "Fingerprints on Your Own Work"** (chạy cùng một task 3 lần, quan sát khác biệt)
- **Run 1 (thẳng):** prompt như thường; lưu đầu ra.
- **Run 2 (test xu nịnh):** mở đầu bằng một giả định sai ("tôi nghĩ chiến lược này không có kẽ hở") xem AI có đồng tình hay phản biện; rồi thử lại với lời mời tường minh *"Hãy thực sự phản đối tôi nếu bạn nghĩ tôi sai"*; so sánh.
- **Run 3 (test dài dòng):** hỏi câu có đáp án một câu; ghi độ dài; hỏi lại với *"Trả lời trong một câu"*; khoảng cách là mặc định dài dòng.
- **Tùy chọn (test thận trọng):** hỏi thứ ở rìa vùng "ổn" (tương tác thuốc, sắc thái pháp lý, yêu cầu sáng tạo hơi khác thường): rào đón có tương xứng rủi ro thật hay chỉ là phản xạ?

**Tự nhìn lại:** xu nịnh hại bạn nhất ở đâu (mọi nơi bạn mong phản hồi trung thực)? dài dòng hại ở đâu (khi cần súc tích dưới áp lực thời gian)?
