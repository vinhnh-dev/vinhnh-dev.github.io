---
layout: page
title: Phần 2 - 4 thuộc tính của AI
permalink: /ai-capabilities-part-2/
parent: AI Capabilities & Limitations
grand_parent: Claude
nav_order: 2
---

# Phần 2: 4 thuộc tính của AI (bài 4-7)

Khung 4 thuộc tính và dải khả năng ↔ giới hạn xem ở **Phần 1, bài 2**. Mỗi bài dưới đây đi sâu một thuộc tính theo cùng khuôn: ý chính → vùng khả năng và vùng giới hạn → lỗi đặc trưng → tính năng sản phẩm khắc phục → liên hệ 4D → bài tập.

## Bài 4: Next Token Prediction, câu trả lời đến từ đâu? (~30 phút)

**Ý chính:** AI tạo sinh gần với **autocomplete cực kỳ tinh vi** hơn là công cụ tìm kiếm: nó viết câu trả lời **từng từ một** dựa trên cái thường theo sau cái. Một thuộc tính duy nhất này cho bạn cả **sự trôi chảy** lẫn **ảo giác (hallucination)**.

- **Vùng khả năng:** task giống mẫu mô hình đã thấy nhiều lần (tóm tắt, định dạng lại, giải thích khái niệm phổ biến).
- **Vùng giới hạn:** lãnh thổ mới/thưa, và mọi chỗ cần phân biệt "đúng" với "nghe có vẻ đúng".
- **Bịa đặt tập trung ở độ cụ thể:** tên, ngày, thống kê, trích dẫn, URL, câu trích. Càng chính xác thì càng cần kiểm chứng.
- **Tính năng sản phẩm khắc phục:** trích dẫn nguồn (citations), báo hiệu độ không chắc chắn, sinh có ràng buộc (constrained generation), mẫu generator-verifier.
- **Liên hệ 4D:** NTP là nền của **Discernment**: biết đầu ra là *được sinh ra* cho bạn biết cần soi kiểu gì.

### Tương tác: "Text Your Friend Markov"

Bộ sinh next-token **hoàn toàn diễn giải được** giúp thấy cơ chế cốt lõi. Ý tưởng: "huấn luyện" trên vài tin nhắn bằng cách **đếm các kết nối từ→từ** (bảng tần suất/ma trận chuyển tiếp); chuẩn hóa mỗi hàng thành **phân phối xác suất** của từ kế tiếp; chọn từ kế tiếp theo xác suất gọi là **sampling** (cùng tên với tiến trình ở LLM như Claude). Với 5 tin nhắn: 17 từ riêng biệt, 13 ngữ cảnh, 24 chuyển tiếp (vd sau "probably": ship 50%, wait 50%).

**"Núm vặn" của sampling:** luôn chọn xác suất cao nhất; chọn ngẫu nhiên theo xác suất; chọn theo xác suất nhưng tăng cường lựa chọn khả dĩ nhất; bỏ qua mọi thứ dưới một ngưỡng; chỉ xét top N; hoặc chọn bừa. Các chiến lược này gần như giống ràng buộc sampling mà developer truyền cho Claude.

| | Markov chain | LLM |
|---|---|---|
| Đọc ngữ cảnh | Từ cuối cùng | Toàn bộ hội thoại đến giờ |
| Tính phân phối | Tra một hàng trong bảng | Forward pass qua hàng tỷ tham số (attention, embedding, feedforward...) |
| Chọn token kế | Sampling | Sampling, cùng quy trình |

Đầu ra cùng dạng (phân phối xác suất từ/token kế tiếp); khác ở huấn luyện: LLM đổi tính diễn giải đơn giản của việc đếm từ lấy ngữ cảnh và năng lực lớn hơn nhiều. Ý tưởng không mới: Markov công bố 1906; mô hình n-gram chạy gợi ý từ trên điện thoại ~2010 (SwiftKey, QuickType); ~2015 mạng neural (RNN rồi transformer 2017) thay bảng tra bằng hàm học được.

### Bài tập "The Verification Test"

Chọn task bạn là chuyên gia (để kiểm chứng được); ghi 5 sự kiện cụ thể kiểm chứng được (chức danh, ngày xuất bản, thống kê, thông số sản phẩm, trích dẫn, URL). Ba phép thử:
1. **Vùng khả năng:** nhờ giải thích khái niệm phổ biến trong lĩnh vực; chú ý độ trôi chảy; soát nội dung.
2. **Độ cụ thể dưới áp lực:** yêu cầu 5 chi tiết kiểm chứng được (3 nguồn, tên tác giả, số liệu chính xác, URL); kiểm chứng từng cái, chấm trên 5; chú ý độ tự tin khi bịa.
3. **Sampling:** chạy lại đúng yêu cầu trong hội thoại mới; cái gì giữ nguyên, cái gì đổi (khác biệt là sampling).
- **Mở rộng:** chạy lại phép 2 với công cụ có trích dẫn (như Research mode); điểm có đổi không?

**Tự nhìn lại:** bạn có bắt được bịa đặt ở lĩnh vực không rành? Việc nào của bạn chủ yếu ở vùng khả năng, việc nào đẩy sang độ cụ thể cần kiểm chứng?

## Bài 5: Knowledge, AI thực sự biết gì? (~30 phút)

**Ý chính:** mô hình biết **những gì nó đã được tiếp xúc khi huấn luyện, và chỉ vậy**: không duyệt web thời gian thực theo mặc định, không có trải nghiệm sống, và **dừng cứng ở knowledge cutoff**. Câu hỏi thực tế không phải "AI có biết không?" mà là **"nội dung này được đại diện tốt cỡ nào trong những gì nó đã đọc?"**

- **Vùng khả năng:** chủ đề xuất hiện **thường xuyên, gần đây (trong dữ liệu), nhất quán**.
- **Vùng giới hạn:** **hiếm, sau cutoff, ngách, địa phương, gây tranh cãi**.
- **Lỗi đặc trưng:** lỗi thời (staleness), độ phủ không đều, **thiên kiến thừa kế** về cái gì là "mặc định"/"bình thường", và không truy được nguồn tri thức (source amnesia).
- **Khắc phục bằng sản phẩm:** web search, retrieval/RAG/MCP, tool use.
- **Liên hệ 4D:** tính không đều của tri thức là cốt lõi của **Delegation**: biết chỗ nào đầy đủ chỗ nào mỏng cho bạn biết lúc nào giao, lúc nào tự cung cấp ngữ cảnh, lúc nào đi chỗ khác.

### Tương tác: không gian 1024 chiều (embedding)

Cách retrieval hoạt động: tìm kiếm cũ so khớp **chuỗi ký tự** ("car" không tìm ra "automobile"); **embedding** coi **ý nghĩa là một vị trí**: chuyển văn bản thành tọa độ, khái niệm giống nhau nằm gần nhau; **không do con người gán** mà nổi lên từ dữ liệu huấn luyện.
- Ví dụ 2D (độ liên quan "khủng long" × "tàu lượn siêu tốc"): vẽ nguồn lên đồ thị; vẽ câu hỏi cùng hệ trục; **tìm k điểm gần nhất** = similarity search.
- Thêm chiều (vd sinh học) → 3D; thực tế mô hình embedding dùng ~**1.000 chiều** (ví dụ 1.024 với VoyageAI); toán vẫn chạy dù ta không hình dung được. **Các trục không có nhãn**: bạn không thể nói "chiều 847 là trục khủng long".
- Mô hình embedding: chuỗi bất kỳ vào, **vector độ dài cố định** ra (dù 3 từ hay 3 đoạn). Độ tương đồng đo bằng **cosine similarity** (theo *hướng* của vector chứ không theo khoảng cách): -1 ngược nhau, 0 không liên quan, 1 giống hệt.

### Bài tập "The Outsider Test"

Chọn một task; ghi: 2 chủ đề phổ biến-ổn định; 2 chủ đề ngách/địa phương/mới/đang thay đổi nhanh; 1 "giả định mặc định" người ngoài hay hiểu sai. Ba phép thử:
1. **Độ phủ:** hỏi một chủ đề phổ biến và một ngách; so độ sâu và độ chính xác; AI có báo hiệu độ không chắc chắn khác nhau không?
2. **Lỗi thời:** hỏi điều bạn biết vừa đổi (quy định, bản phát hành công cụ, thay đổi lãnh đạo): AI có thừa nhận cutoff, đưa thông tin cũ như hiện tại, hay từ chối?
3. **Giả định mặc định:** hỏi câu bộc lộ AI có mặc định theo góc nhìn người ngoài không (vd "khách hàng điển hình").
Gắn nhãn thứ hai cho danh sách việc: dựa vào tri thức của mô hình, hay phải tự mang tri thức vào (ngữ cảnh, tài liệu, search). **Mở rộng:** chạy lại phép 2 với web search bật (retrieval hoạt động).

**Tự nhìn lại:** chỗ nào trong công việc bạn nay nhận ra phải tự cấp ngữ cảnh? Phép thử "giả định mặc định" có gì bất ngờ?

## Bài 6: Working Memory, AI đang chú ý vào gì? (~30 phút)

**Ý chính:** mọi thứ AI chú ý nằm trong **cửa sổ ngữ cảnh (context window)** kích thước cố định; nó chú ý được thứ bên trong, không chú ý được thứ bên ngoài. Thuộc tính này có **vách đá** (cliff): chạy tốt cho đến khi không còn tốt, khác các thuộc tính suy giảm dần.

- **Vùng khả năng:** tài liệu vừa vặn, phiên còn mới, bạn cung cấp ngữ cảnh liên quan.
- **Vùng giới hạn:** tài liệu/hội thoại rất dài, mong liên tục xuyên phiên, chôn thông tin quan trọng giữa đầu vào dài.
- **Lỗi nguy hiểm:** **cắt bớt âm thầm** (không phải lúc nào cũng được cảnh báo). **Mô hình không học từ chỉnh sửa của bạn**, chỉ phản hồi những gì đang trong ngữ cảnh.
- **Khắc phục bằng sản phẩm:** memory, compaction, projects/workspace, cửa sổ lớn hơn, workflow đa agent.
- **Liên hệ 4D:** Working Memory là thứ **Description** tác động lên: biết cửa sổ hoạt động thế nào cho biết cách cấu trúc ngữ cảnh, lúc nào đặt lên đầu, lúc nào bắt đầu lại.
- **Chiến lược "ngữ cảnh là đòn bẩy":** đặt tài liệu quan trọng lên đầu, chia việc dài thành phần, cấp lại ngữ cảnh then chốt.

### Tương tác: Context Degradation (nhiều ngữ cảnh chưa chắc tốt hơn)

Bản năng "đưa hết cho AI" có thể phản tác dụng. **Hiệu ứng vị trí chuỗi (serial position effect):** đầu danh sách được nhớ tốt nhờ *primacy*, cuối được nhớ tốt nhờ *recency*, **giữa bị "lạc"**. LLM cũng vậy: nghiên cứu Stanford 2023 cho thấy độ chính xác cao nhất khi thông tin ở đầu hoặc cuối cửa sổ, **giảm hơn 30%** khi bị chôn giữa; attention của transformer vốn đặt trọng số lên hai rìa.

| Mẫu nguy hiểm | Mẫu an toàn hơn |
|---|---|
| Chỉ dẫn quan trọng bị chôn giữa các tin nhắn dài | Chỉ dẫn then chốt **đặt đầu** (system prompt) và **nhắc lại gần cuối** |

Mỗi mẩu ngữ cảnh thêm vào đẩy mẩu khác vào "vùng chết attention": cốt lõi của context engineering là *nên đưa gì, đặt ở đâu, bỏ gì*. **Nhiều ngữ cảnh ≠ kết quả tốt hơn:** chú ý của mô hình hữu hạn; chọn lọc thật gắt, đặt đúng chỗ, nhắc lại điều quan trọng.

### Bài tập "The Before-and-After"

Chọn task cần ngữ cảnh chỉ bạn có (style guide, ví dụ việc tốt trước đây, ràng buộc riêng); mô tả "tốt" trong 2-3 dòng để người lạ đánh giá được. Ba phép thử:
1. **Khởi đầu nguội vs có ngữ cảnh:** chạy không ngữ cảnh; rồi hội thoại mới với ngữ cảnh cấp trước; so với định nghĩa "tốt", đo khoảng cách.
2. **Lạc ở giữa:** chôn một chỉ dẫn quan trọng giữa tài liệu dài, hỏi câu phụ thuộc vào nó; rồi chuyển lên đầu và hỏi lại.
3. **Tờ giấy trắng:** dạy AI điều cụ thể về công việc/sửa nó, rồi mở hội thoại mới và hỏi như thể nó nhớ.
Gắn nhãn thứ ba: việc nào cần ngữ cảnh thường trực (project, chỉ dẫn lưu sẵn, tài liệu upload) mới đáng chạy, việc nào chạy nguội vẫn ổn. **Mở rộng:** nếu công cụ có memory/project, thiết lập ngữ cảnh ở phép 1 và chạy lại.

**Tự nhìn lại:** nạp ngữ cảnh lên đầu đổi chất lượng nhiều cỡ nào? Bạn sẽ thiết lập ngữ cảnh thường trực nào tuần này để khỏi giải thích lại?

## Bài 7: Steerability, bạn kiểm soát được đến đâu? (~30 phút)

**Ý chính:** mô hình làm theo chỉ dẫn **theo cách nó làm mọi thứ khác: tiếp nối một mẫu**. Vì vậy nó điều khiển được rất tốt, nhưng luôn có **khoảng cách giữa điều bạn định nói và điều đến được**; phần lớn lỗi thú vị nằm ở đó.

- **Vì sao hoạt động:** fine-tuning dạy mô hình làm theo chỉ dẫn. **Vì sao có giới hạn:** chỉ dẫn được làm theo bằng **khớp mẫu**, không phải hiểu.
- **Vùng khả năng:** chỉ dẫn **ngắn, cụ thể, kiểm chứng được** (đặc tả định dạng, giới hạn độ dài, vai rõ ràng).
- **Vùng giới hạn:** chuỗi lập luận dài, yêu cầu trừu tượng/mơ hồ, mọi việc cần độ chính xác số/logic "bản địa".
- **Lỗi đặc trưng:** **reasoning drift** (lỗi nhỏ tích lũy dọc chuỗi lập luận), **letter-over-spirit** (làm đúng câu chữ nhưng trật ý định), **số học mong manh**.
- **Khắc phục bằng sản phẩm:** system prompt, thực thi code, lập luận hiển thị, đầu ra có cấu trúc (giữ ý định khỏi bị loãng).
- Chỉ dẫn được làm theo đúng chữ nhưng vô dụng → **nêu lại mục tiêu**; lặp lại chỉ dẫn mạnh hơn không thu hẹp khoảng cách.
- **Liên hệ 4D:** steerability là thứ làm **Description** mạnh và cũng là thứ giới hạn nó.

### Bài tập "The Goal Rewrite"

Chọn task nhiều bước hoặc có định dạng riêng; ghi mục tiêu thật bằng một câu ("thuyết phục team rằng timeline này khả thi" là *mục tiêu*; "ba gạch đầu dòng" là *định dạng*). Ba phép thử:
1. **Kiểm soát chặt:** chỉ dẫn ngắn, cụ thể, kiểm chứng được ("bảng ba cột", "đúng năm gạch đầu dòng", "ngôi thứ hai"); có giữ đúng không? (vùng khả năng.)
2. **Reasoning drift:** yêu cầu phiên bản 4-5 bước phụ thuộc; soát từng bước xem lỗi sớm có kéo đến cuối không; thử lại với yêu cầu **dừng ở bước 2 cho xem kết quả** rồi mới tiếp; so sánh có/không checkpoint.
3. **Letter vs spirit:** chỉ dẫn có thể thỏa theo chữ nhưng vô dụng ("làm ngắn hơn" cho bản nháp mà vấn đề là cấu trúc; "chuyên nghiệp hơn" cho email chôn mất yêu cầu); rồi nhắc lại kèm mục tiêu: *"Làm ngắn hơn. Mục tiêu: giữ sự chú ý của giám đốc đến phát hiện chính ở trang hai."*
Quay lại danh sách: việc nhiều bước thì ghi chỗ đặt checkpoint; việc chỉ nêu định dạng thì soạn câu mục tiêu để thêm lần sau.

**Tự nhìn lại:** bạn thường nêu định dạng mà không nêu mục tiêu cỡ nào? Việc lặp lại nào bạn sẽ thêm checkpoint giữa quy trình từ tuần này?
