---
layout: page
title: Bài 02 - Cuộc trò chuyện đầu tiên
permalink: /claude-101-lesson-02/
parent: Claude 101
grand_parent: Claude
nav_order: 4
---

# Bài 02: Cuộc trò chuyện đầu tiên với Claude

**Module 1 — Làm quen với Claude** · ~20 phút · Cơ bản

## Ý chính

Khó nhất khi làm việc trí óc là "tờ giấy trắng". Claude giúp bạn bắt đầu, nhưng cần biết cách **giao việc (brief)** như với một đồng nghiệp giỏi.

## Giao diện claude.ai

- **Sidebar:** New chat, Recent chats, Projects (Bài 05), Artifacts (Bài 06), Settings
- **Chat area:** ô nhập prompt, 📎 tải file, 🔧 Search & tools (web search, connectors), 🧠 Extended thinking, chọn model
- Phím tắt chat mới: `Cmd/Ctrl + K`

## Công thức prompt C-T-R

| Phần | Nội dung | Yếu → Mạnh |
|---|---|---|
| **Context** | Bạn là ai, ngành, mục tiêu, đối tượng cuối | "Viết email về chậm trễ" → "Tôi là CS Manager SaaS B2B, khách doanh nghiệp chờ tích hợp 2 tuần, lần chậm thứ 2 trong quý..." |
| **Task** | Động từ cụ thể: viết, phân tích, so sánh, tóm tắt, đánh giá | "Giúp tôi" → "Soạn email thừa nhận chậm trễ, nêu nguyên nhân (không đổ lỗi), cam kết mốc mới, đề nghị bồi thường nhỏ" |
| **Rules** | Giọng điệu, định dạng, độ dài, ngôn ngữ, điều cần tránh | "Viết hay nhé" → "150-180 từ, chuyên nghiệp nhưng chân thành, có CTA, tiếng Anh" |

Ví dụ nâng cấp prompt: "Nghiên cứu thị trường phim indie streaming" → (thêm task: phân tích xu hướng, đối thủ, cơ hội cho Series A) → (thêm context + rules: web search có trích dẫn, báo cáo ≤ 5 trang, 4 phần, tiếng Anh).

## Khung 4D (AI Fluency)

Của GS. Rick Dakan và Joseph Feller:

- **Delegation** — giao gì cho AI, giữ gì
- **Description** — giao tiếp với AI (= C-T-R, bài này)
- **Discernment** — đánh giá output
- **Diligence** — dùng AI có trách nhiệm

Ba D còn lại ở Bài 03.

## Tải file làm bối cảnh

- Hỗ trợ: PDF, DOCX, TXT, MD · CSV, XLSX · PNG/JPEG/GIF · code (.py, .js...) · PPTX
- Dùng 📎 hoặc kéo thả, rồi nhập prompt về file.
- **Đặt tên file mô tả** (`Q4-2025-Sales-Report-APAC.pdf`, không phải `document1.pdf`).

## Search & Tools

- Mặc định Claude dùng kiến thức huấn luyện (cũ 6-12 tháng) → bật **web search** cho giá, tin tức, công ty mới.
- **Connectors:** Drive, Slack, Notion, Gmail, Linear... (Bài 08).
- Có thể yêu cầu thẳng: "Dùng web search để xác minh số liệu này".

## Chọn model

| Model | Khi nào dùng |
|---|---|
| **Opus 4.6** | Phức tạp: nhiều bước, code khó, nghiên cứu |
| **Sonnet 4.6** | Mặc định, cân bằng chất lượng/chi phí |
| **Haiku 4.5** | Việc đơn giản cần nhanh: phân loại, tóm tắt ngắn |

Đổi model sẽ bắt đầu chat mới.

**Extended thinking (🧠):** Claude suy luận từng bước (xem được) rồi mới trả lời. Bật cho toán/logic, quyết định nhiều ràng buộc, debug khó, lập kế hoạch phức tạp. Không cần cho câu đơn giản, tóm tắt, brainstorm. Đánh đổi: chậm hơn, chất lượng cao hơn.

## Lặp lại (iterate)

4 cách:

1. **Hỏi tiếp** — "Mở rộng ý 2", "Cho ví dụ cụ thể"
2. **Phản hồi cụ thể** — "Quá trang trọng, thân mật hơn", "Phần 3 rút còn 2 câu"
3. **Định hướng lại** — "Tôi muốn tập trung vào X, không phải Y"
4. **Sửa prompt gốc (🖊️)** — tạo phản hồi mới thay vì nối thêm

**Nên mở chat mới khi:** > 15 tin nhắn và bắt đầu lặp/mất bối cảnh · đổi chủ đề · muốn góc nhìn mới.

## Cá nhân hóa

| Tính năng | Mô tả |
|---|---|
| **Memory** | Tự lưu vai trò, sở thích, quyết định; xem/sửa/xóa ở Settings → Memory |
| **Styles** | Concise / Formal / Explanatory, hoặc Custom Style (tải 3-4 bài viết + mô tả phong cách) |
| **Preferences** | Settings → General; ví dụ "Luôn trả lời tiếng Việt", "Mặc định Markdown bullet" |

## Ví dụ theo vai trò (điểm chung)

Prompt tốt luôn nêu rõ: vai trò + tình huống, nguồn dữ liệu/file, và output cụ thể.

- **Sales:** chuẩn bị cuộc gọi → tóm tắt 1 trang (công ty, người liên hệ, 3 điểm thảo luận, 2 câu hỏi khám phá)
- **Marketing:** 1 blog → 5 định dạng, giữ thông điệp nhất quán
- **Tài chính:** file chi phí 3 sheet → memo 2 trang cho CFO, giọng khách quan
- **Pháp lý:** so hợp đồng với template → bảng 4 cột (Mục | Khác biệt | Rủi ro | Đề xuất sửa)
- **Giáo viên:** kế hoạch bài 45 phút, lớp trình độ hỗn hợp, nhiều ví dụ đời thường

## Lỗi thường gặp

| Lỗi | Cách đúng |
|---|---|
| Prompt 1 dòng | Đầu tư 2 phút viết context + rules (tiết kiệm 20-30 phút) |
| Nhồi nhiều nhiệm vụ 1 prompt | Chia bước: 1 prompt = 1 kết quả chính |
| Mơ hồ về độ dài ("dài một chút") | Nói số cụ thể: "150-200 từ", "3 điểm" |
| Không phản hồi tiếp, mở chat mới liên tục | Chỉ rõ chỗ sai trong cùng chat |
| Dán text dài thay vì tải file | Tải CSV/XLSX/PDF |

## Bài tập

1. Viết 3 prompt C-T-R cho 3 việc tuần này (từ Bài 01). Ghi: phản hồi đầu dùng được không? Đã lặp thế nào? Tổng thời gian so với tự làm?
2. (Optional) Bật Memory, giới thiệu bản thân; tạo Custom Style từ 2-3 bài viết; thử prompt ở chat mới và so sánh.

## Tóm tắt

- **C-T-R** → phản hồi dùng được ngay.
- Nói chuyện tự nhiên như với đồng nghiệp.
- **Tải file thay vì dán.**
- **Sonnet mặc định**, Opus cho việc khó, Haiku cho tốc độ; Extended thinking khi cần suy luận sâu.
- **Lặp lại** là bước quan trọng.
- **Memory + Styles + Preferences:** thiết lập một lần, dùng mãi.
