---
layout: page
title: Bài 01 - Claude là gì?
permalink: /claude-101-lesson-01/
parent: Claude 101
grand_parent: Claude
nav_order: 3
---

# Bài 01: Claude là gì?

**Module 1 — Làm quen với Claude** · ~15 phút · Cơ bản

## Ý chính

Claude là trợ lý AI do **Anthropic** xây dựng, hoạt động như một **thinking partner**: giữ ý tưởng, nghiên cứu, soạn thảo và rút ngắn khoảng cách từ "ý tưởng" đến "bản đầu tiên" (từ tuần → ngày → giờ). Bối cảnh, phán đoán và trách nhiệm vẫn thuộc về bạn.

Claude được thiết kế để:

- Xử lý lượng thông tin lớn (context window rất lớn)
- Làm theo hướng dẫn về phong cách, giọng điệu, ràng buộc (**steerable**)
- Giữ giá trị đạo đức (**aligned**)
- Dùng công cụ khi cần (web search, connectors, chạy code)
- Làm việc nhiều bước, tự chia nhỏ nhiệm vụ (**agentic**)

## Constitutional AI

Claude được huấn luyện theo một "bộ nguyên tắc": **hữu ích – vô hại – trung thực**. Với bạn nghĩa là:

- Từ chối rõ ràng khi yêu cầu độc hại/bất hợp pháp
- Thừa nhận không biết thay vì bịa
- Giải thích lý do khi từ chối hoặc kết quả có giới hạn

## Steerable

Bạn điều khiển được: **giọng điệu, định dạng, độ dài, vai trò, ngôn ngữ**. Kết quả không đúng ý thường do hướng dẫn chưa đủ cụ thể, không phải "AI lỗi".

## Claude vs chatbot thông thường

| Tiêu chí | Chatbot thường | Claude |
|---|---|---|
| Mục tiêu | Trả lời câu hỏi ngắn | Cộng tác dài hạn việc phức tạp |
| Context | Vài câu | 200K+ token (~500 trang), tối đa 1M trên Opus 4.6 |
| Đầu vào | Văn bản | Văn bản + file (PDF, DOCX, CSV, ảnh, code) |
| Đầu ra | Văn bản | Văn bản + file + app nhỏ + sơ đồ + code chạy được |
| Công cụ | Không | Web search, Drive, Slack, Notion... |
| Tự động | Không | Cowork, Research, Subagents |

## 5 năng lực cốt lõi

1. **Viết & sáng tạo** — email, báo cáo, social, marketing. Tải 3-4 bài cũ → "viết theo phong cách này".
2. **Nghiên cứu & phân tích** — đọc nhiều PDF, so sánh đối thủ, phân tích Excel, search web có trích dẫn.
3. **Lập trình** — công thức Excel, debug, script Python (không cần là dev).
4. **Suy luận** — chế độ nhanh và **Extended thinking** (suy nghĩ từng bước, xem được thinking block).
5. **Học tập** — Learning mode (hỏi ngược, dẫn dắt), tự thích ứng trình độ.

## Các cách truy cập Claude

Cùng một mô hình, nhiều giao diện:

| Giao diện | Dùng cho |
|---|---|
| **Claude.ai** (web / desktop / mobile) | Công việc chung; đồng bộ chat, Projects, Memory. Desktop: Quick Entry, screenshot, file cục bộ; mobile: Dispatch |
| **Claude Code** | Lập trình agentic; cũng dùng để đổi tên/sắp xếp file, chạy script phân tích |
| **Claude trong Slack** | `@Claude` trong thread, tìm kiếm kênh/tin nhắn theo quyền truy cập |
| **Claude cho Excel** | Sidebar: đọc/sửa workbook, sửa lỗi `#REF!`, theo dõi công thức |
| **Claude cho Chrome** | Điền form, điều hướng, trích xuất dữ liệu (research preview, chỉ nên dùng trang tin cậy, việc rủi ro thấp) |

## Ví dụ theo vai trò

| Vai trò | Trước | Sau |
|---|---|---|
| Sales | 30-45 phút chuẩn bị cuộc gọi (CRM + email + web) | ~5 phút (CRM + Gmail + web search) |
| Marketing | 3 giờ chuyển 1 blog thành 5 định dạng | ~20 phút (brand trong Project) |
| Tài chính | 1 ngày đối chiếu ERP/ngân hàng/hóa đơn | ~45 phút kiểm tra |
| Pháp lý | 3-4 giờ review hợp đồng | ~40 phút (bảng chênh lệch + mức rủi ro) |
| L&D | 1 tuần soạn module | 1-2 ngày (Project + quy trình) |

## Sai lầm thường gặp

| Sai lầm | Cách đúng |
|---|---|
| Coi Claude như Google (câu hỏi 1 dòng) | Cung cấp bối cảnh: vai trò, ngân sách, đội ngũ, đã thử gì |
| Dán 500 dòng text thay vì tải file | Tải thẳng PDF/DOCX/CSV để giữ cấu trúc |
| Dùng Opus cho việc đơn giản | Mặc định **Sonnet**, chỉ lên Opus khi phức tạp |
| Không xác minh kết quả | Coi như bản nháp của đồng nghiệp cấp dưới, luôn kiểm tra |

## Bài tập

1. **Audit tuần này:** liệt kê 5 cuộc họp/nhiệm vụ; với mỗi cái, xác định cần *phán đoán* hay chỉ *thực hiện*, có lặp > 3 lần/tháng không, phần nào có thể giao.
2. **Prompt đầu tiên (optional):** "Tôi là [vai trò] trong [ngành]. Việc tốn thời gian nhất tuần này là [nhiệm vụ]. Đề xuất 3 cách Claude giúp tôi, kèm ưu/nhược và công sức thiết lập."

## Tóm tắt

- Claude là **thinking partner**, không phải chatbot FAQ.
- **Constitutional AI:** hữu ích, vô hại, trung thực.
- **Steerable:** kết quả sai ý = hướng dẫn chưa cụ thể.
- **5 năng lực:** Viết, Nghiên cứu, Lập trình, Suy luận, Học tập.
- **Claude = trí tuệ; giao diện = cách dùng.**
- Bắt đầu từ vấn đề thật: "tuần này tôi vướng ở đâu?"
