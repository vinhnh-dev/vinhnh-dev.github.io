---
layout: page
title: Bài 03 - Tối ưu kết quả & AI Fluency
permalink: /claude-101-lesson-03/
parent: Claude
nav_order: 4
---

# Bài 03: Tối ưu hóa kết quả — AI Fluency & Lặp lại

**Module 1 — Làm quen với Claude** · ~20 phút · Cơ bản → Trung cấp

## Ý chính

Phản hồi chưa ưng ý là **tín hiệu**, không phải lỗi của AI. Người thành thạo không phải vì Claude tốt hơn, mà vì họ giao tiếp có hệ thống hơn.

## 5 thách thức phổ biến

| Triệu chứng | Nguyên nhân | Cách khắc phục |
|---|---|---|
| Quá chung chung | Thiếu bối cảnh | Thêm đối tượng, vai trò, ràng buộc, lịch sử |
| Quá dài / ngắn | Claude tự ước lượng | Nói rõ: "200 từ", "3 điểm", "tối đa 1 trang" |
| Sai định dạng | Hiểu nội dung, không rõ cách trình bày | Đưa ví dụ / mô tả cấu trúc |
| Tự tin nhưng sai (hallucination) | Sai với sự kiện cụ thể, trích dẫn | Xác minh, yêu cầu nguồn + mức tin cậy, bật web search |
| Sai tông giọng | Mặc định chuyên nghiệp chung | Mô tả tông, gửi văn bản mẫu |

**Công thức sửa:** thêm đối tượng cụ thể + lịch sử/bối cảnh + yêu cầu (độ dài, tông) + cấu trúc mong muốn.

## Tư duy lặp lại

1. **Bản đầu = bản nháp của đồng nghiệp mới** — 80% giá trị ở lần đầu, 20% từ lặp.
2. **Phản hồi cụ thể:**

| Yếu | Mạnh |
|---|---|
| "Viết ngắn hơn" | "Cắt hai đoạn đầu; kết luận mang tính hành động hơn" |
| "Cải thiện giọng điệu" | "Bỏ thuật ngữ như 'synergy'; viết như tư vấn cấp cao, uy tín nhưng thân thiện" |
| "Thêm chi tiết" | "Mở rộng phần 3 lên 200 từ, nêu 3 đối thủ cụ thể" |
| "Sửa lại" | "Giá ở đoạn 2 sai, phải là $4.99; cập nhật và nêu tác động tới biên lợi nhuận" |

3. **Biết khi nào bắt đầu lại:** > 15 tin nhắn và lặp, đổi chủ đề, cần góc nhìn mới. Mẹo: trước khi đóng, hỏi *"Tóm tắt bối cảnh chính thành 5 điểm để tôi dán vào chat mới."*

## Khung 4D (chi tiết)

| D | Câu hỏi | Điểm chính |
|---|---|---|
| **Delegation** | Giao gì? | Giao: việc có mẫu, kiểm tra được, rủi ro thấp. Không giao: quyết định rủi ro cao (tuyển/sa thải, chiến lược), việc cần bối cảnh AI không có. Mẹo: nếu đồng nghiệp giỏi làm giúp 60% mà bạn chấp nhận → ứng viên tốt |
| **Description** | Giao thế nào? | C-T-R (Bài 02); nâng cao: thêm ví dụ nên/không nên |
| **Discernment** | Output đúng chưa? | Chính xác (số liệu, trích dẫn, nhân quả vs tương quan) · Phù hợp (tông, độ sâu, văn hóa) · Đầy đủ (thiếu sót, ngoại lệ) · Nhất quán (số liệu, thuật ngữ, mâu thuẫn) |
| **Diligence** | Ai chịu trách nhiệm? | Bạn. Minh bạch việc dùng AI (bắt buộc ở học thuật/pháp lý), ký tên = chịu trách nhiệm, tự cân nhắc đạo đức |

## Vòng lặp Delegation–Diligence

Ví dụ **Rio** (Valley Veterans Services) — phân tích tỷ lệ tham gia vs việc làm:

1. Xác định task cụ thể (không chung chung)
2. Chọn dữ liệu quá khứ đã có "đáp án" (quý 1, làm thủ công)
3. Cho AI tái tạo kết quả
4. So sánh với thực tế (AI đúng ở tương quan, sai ở nhóm chương trình nhà ở + việc làm)
5. Hoàn thiện prompt, ghi lại điểm thiếu ("luôn nêu rõ loại chương trình")
6. Thử câu khó hơn (AI thiếu ngày đăng ký → quý sau bổ sung dữ liệu)

→ Lặp lại bước 3 hoặc **dừng** khi đủ tin cậy hoặc xác định *không nên giao*. **Cả hai kết quả đều có giá trị.**

**Nếu chưa đủ chuyên môn để kiểm tra:** yêu cầu AI giải thích từng bước trước khi làm; hỏi "Tại sao chọn X? Giả định gì?"; bắt đầu với tập con nhỏ (1 tuần thay vì cả quý).

## Eval nhẹ (lightweight eval)

1. **Thu thập 5-10 ví dụ** bạn từng làm (email, báo cáo, phân tích) làm "đáp án".
2. **Viết prompt thử** kèm bối cảnh như khi bạn làm thật.
3. **So sánh** theo thang 1-5: độ chính xác · cấu trúc · giọng điệu.
4. **Tinh chỉnh:** lỗ hổng lặp lại → sửa mẫu prompt; thêm ví dụ phong cách; đánh dấu việc cần người duyệt.

Ví dụ báo cáo tuần: Claude đúng task nhưng dài dòng, dùng "delivered comprehensive analysis" thay vì "shipped analysis" → tải `status-update-examples.md` làm tham chiếu phong cách → chất lượng tăng rõ. **Đánh giá lại mỗi 1-2 tháng/quý.**

| Vai trò | Tiêu chí eval |
|---|---|
| Data analyst (phát hiện bất thường) | Recall + Precision |
| Content marketer (blog → LinkedIn) | Câu mở, độ rõ, CTA so với hiệu suất cũ |
| Tài chính (đối chiếu) | Độ chính xác so với đối chiếu thủ công, tỷ lệ đánh dấu sai |
| Giáo viên (giáo án) | Hợp lý sư phạm, ngôn ngữ theo lứa tuổi, chất lượng bài tập |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Chữa triệu chứng ("cụ thể hơn" lặp mãi) | Chẩn đoán thiếu gì trong bối cảnh và bổ sung |
| Lặp > 5 lần cho 1 phản hồi | Tối đa 3-5 lần; không ưng → sửa prompt gốc, bắt đầu lại |
| Eval một lần rồi quên | Đánh giá lại hàng quý (model và tiêu chuẩn đều đổi) |
| Nhầm "không thích phong cách" với "không chính xác" | Tự hỏi: độ chính xác hay phong cách? Sửa tương ứng |
| Chủ quan vì "Claude đáng tin" | Mức duyệt tỷ lệ với rủi ro: memo nội bộ ~20%, gửi khách 100% |

## Bài tập

1. **Gỡ chat "bị kẹt":** lấy chat cũ không ưng, chẩn đoán triệu chứng → nguyên nhân → sửa prompt, gửi lại, ghi mức cải thiện.
2. **Vòng lặp delegation–diligence** cho 1 task lặp: chọn ca đã làm thủ công → prompt → so sánh → tinh chỉnh → chạy lần 2 → kết luận (giao / giao có điều chỉnh / không giao).

## Tóm tắt

- 5 lỗi phổ biến đều sửa được; **chữa nguyên nhân, không chữa triệu chứng**.
- **4D** đủ cả bốn, không chỉ Description.
- Lặp lại có phản hồi cụ thể; biết khi nào bắt đầu lại.
- **Xác thực bằng dữ liệu quá khứ** trước khi tin.
- Eval nhẹ với 5-10 ví dụ, lặp lại định kỳ.
- **AI làm 95%, bạn vẫn chịu 100% trách nhiệm.**
