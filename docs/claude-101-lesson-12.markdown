---
layout: page
title: Bài 12 - Các cách khác dùng Claude
permalink: /claude-101-lesson-12/
parent: Claude 101
grand_parent: Claude
nav_order: 14
---

# Bài 12: Các cách khác để làm việc với Claude

**Module 5 — Áp dụng thực tế** · ~15 phút · Trung cấp

## Ý chính

**Claude là trí tuệ; claude.ai chỉ là một giao diện.** Mỗi "flavor" chuyên biệt đưa Claude vào đúng môi trường bạn đang làm việc, bỏ việc chuyển ngữ cảnh (mỗi lần ~30 giây + mất tập trung):

| Môi trường | Flavor |
|---|---|
| Terminal / IDE | **Claude Code** |
| Slack workspace | **Claude in Slack** |
| Microsoft Excel | **Claude for Excel** |
| Chrome | **Claude for Chrome** |

## Claude Code

Công cụ lập trình agentic: thao tác trực tiếp trên codebase (sửa file, chạy test, tạo commit), không phải copy-paste. Bạn giao việc, Claude thực thi.

**Dùng khi:**
1. **Xây tính năng bằng ngôn ngữ tự nhiên** — vd "Thêm xác thực email + mật khẩu theo pattern trong /auth, viết test, đừng commit"
2. **Sửa lỗi** — dán lỗi, Claude phân tích nguyên nhân, đề xuất và áp dụng nếu bạn duyệt
3. **Khám phá codebase lạ** — giải thích luồng đăng ký, file liên quan
4. **Tự động hóa việc nhàm** — fix lint 50 file, giải quyết merge conflict, viết release notes từ commit, đổi tên biến toàn dự án
5. **Giữ nguyên workflow terminal**

**Cài đặt:** `claude.com/code` → đăng nhập → mở trong thư mục dự án. Chạy trên: terminal (chính), IDE extension (VS Code, JetBrains), tab Code trong Desktop (Bài 04), trình duyệt (`claude.ai/code`), Slack (@Claude).

**Lưu ý:** không dùng cho việc phi-dev (marketing → Chat). Local mode: code không rời máy; cloud mode có sync repo. Bạn chịu trách nhiệm triển khai, đặc biệt thay đổi production.

## Claude in Slack

Hỗ trợ ngay trong channel/thread, có ngữ cảnh từ hội thoại Slack.

**2 cách truy cập:** (1) **AI Assistant** header của channel/DM — chat riêng với Claude, có sẵn context kênh; (2) **@Claude** trong thread — phản hồi hiển thị cho mọi người.

**Dùng khi:** soạn phản hồi cho thread phức tạp · tóm tắt quyết định + action items · bắt kịp thread khi vào muộn · chuẩn bị họp (tổng hợp hội thoại + tài liệu về Dự án X) · onboarding ("review #engineering 30 ngày qua") · chuyển giao việc code (`@Claude khởi chạy phiên Claude Code để điều tra bug này`).

**Ưu/nhược:** không chuyển ngữ cảnh, minh bạch với team; nhưng phản hồi hiển thị trong kênh (trừ DM) → đừng đăng thông tin nhạy cảm.

**Cài đặt:** admin cài app Claude từ Slack Marketplace; người dùng xác thực.

## Claude for Excel

Sidebar trong Excel: phân tích, hiểu, sửa bảng tính qua chat — làm trực tiếp trên file đang mở, thấy thay đổi ngay, lưu như bình thường (không phải upload → tải về → ghép lại thủ công).

**Dùng khi:**
1. Hiểu workbook nhiều tab, công thức tham chiếu chéo ("Sheet 3 P&L phụ thuộc Sheet 1, 2 thế nào?")
2. Cập nhật giả định không phá mô hình (vd tăng trưởng Q4 15% → 20%, giữ công thức, đánh dấu ô bị ảnh hưởng)
3. Sửa lỗi (`#REF!`, tham chiếu vòng)
4. Tạo bảng mới / điền template (tracker chi phí 3 sheet, có công thức và biểu đồ)
5. Pivot table + biểu đồ (doanh số theo khu vực × danh mục, bar top 5)
6. Giải thích công thức phức tạp (`INDEX/MATCH/MATCH`)

**Cài đặt:** cần Microsoft 365 → tải add-in Claude for Excel → cài qua Office Add-ins → đăng nhập.

## Claude for Chrome

Extension hiện sidebar trong Chrome, đọc trang hiện tại và thao tác trên web.

**Dùng khi:** tóm tắt trang đang đọc · soạn trả lời email trong Gmail · tự động điền nhiều form từ CSV (bạn duyệt trước khi gửi) · test luồng web (đăng ký như user thử, báo lỗi/UX, có screenshot) · research so sánh giá (Claude tự duyệt từng site) · trích dữ liệu từ tool nội bộ không có API (top 10 khách hàng từ CRM → Excel).

**Research preview — lưu ý:** chỉ dùng cho tác vụ rủi ro thấp, trang đáng tin cậy; extension xin quyền trước hành động rủi ro cao (mua hàng, chia sẻ dữ liệu cá nhân); một số danh mục bị chặn mặc định (tài chính, nội dung người lớn). **Không dùng** cho giao dịch ngân hàng/tài chính, nhập dữ liệu nhạy cảm, quyết định rủi ro cao.

**Cài đặt:** Chrome Web Store → "Claude for Chrome" → cài → ghim → đăng nhập.

## So sánh

| Công cụ | Phù hợp nhất | Nơi chạy | Ai dùng |
|---|---|---|---|
| **Claude.ai** | Việc chung, nghiên cứu, viết, phân tích, tạo file | Web, desktop, mobile | Mọi người |
| **Claude Code** | Phát triển phần mềm, codebase, Git | Terminal, IDE, browser | Developer |
| **Claude Cowork** | Tác vụ nhiều bước: research, nhiều tài liệu, file, dữ liệu | Desktop (mobile qua Dispatch) | Người làm việc tri thức |
| **Claude in Slack** | Cộng tác, chuẩn bị họp, trả lời nhanh theo context | Slack workspace | Teams |
| **Claude for Excel** | Phân tích bảng tính, mô hình tài chính, debug công thức | Sidebar Excel | Tài chính, analyst |
| **Claude for PowerPoint** | Tạo/sửa slide, định dạng, thiết kế | Sidebar PowerPoint | Marketing, Sales, Leadership |
| **Claude for Chrome** | Research web, email, tự động hóa trình duyệt | Sidebar Chrome | Mọi người (preview) |

**Chọn nhanh:** coding → Claude Code · hội thoại Slack → Claude in Slack · đang trong Excel → Claude for Excel · duyệt web → Claude for Chrome · còn lại → Claude.ai.

Cân nhắc thêm: nhiều bước kéo dài → **Cowork**; cần context chung cho nhóm → **Slack** hoặc **Shared Projects**; sửa tài liệu có sẵn → **Excel/PowerPoint**; không muốn rời terminal → **Claude Code**.

## Ví dụ một ngày đa công cụ

| Vai trò | Luồng |
|---|---|
| Sales Manager | Sáng Desktop quick entry (3 cuộc họp quan trọng) → giữa ngày Excel (pipeline, forecast) → trong họp Slack (`@Claude` tóm tắt câu hỏi của sếp) → sau họp Cowork (cập nhật CRM, soạn follow-up, lên lịch báo cáo thứ Sáu) |
| Content Marketing | Chrome (blog đối thủ) → Claude.ai (viết đúng brand voice) → Excel (cập nhật content calendar) → Slack (đăng thông báo #company-announcements) |
| Tài chính | Excel (hiểu file kế thừa) → Claude.ai + Artifacts (bảng so sánh) → Cowork scheduled (báo cáo/closing tháng) → Slack (trả lời câu hỏi ad-hoc #finance) |
| Developer | Claude Code terminal (chính) → Slack (`@Claude` mở phiên xử lý bug #1234) → Claude.ai (brainstorm kiến trúc) → Excel (hỗ trợ data analyst) |
| HR | Cowork + Project (bộ onboarding cá nhân hóa) → Slack (hỏi đáp chính sách 24/7) → Excel (phân tích phúc lợi) → Chrome (trích thông tin ứng viên từ LinkedIn) |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Dùng 1 flavor cho mọi việc (vd upload Excel lên web chat rồi ghép lại tay) | Chọn flavor theo ngữ cảnh (Excel → Claude for Excel) |
| Mở nhiều flavor cùng làm một việc | 1 flavor/tác vụ, chỉ chuyển khi đổi loại việc |
| Dùng preview (Chrome) cho việc quan trọng | Preview chỉ cho việc rủi ro thấp; việc quan trọng → bản ổn định |
| Đăng thông tin nhạy cảm vào kênh có `@Claude` | Dùng DM với Claude hoặc Claude.ai; nắm mức hiển thị |
| Để Claude Code tự commit không review | Dùng chế độ Ask/Plan, xem diff, chạy test |

## Bài tập

1. **Audit độ phủ (10')**: liệt kê tác vụ tuần qua → công cụ hiện tại → có flavor Claude nào tốt hơn không? Chọn 2 để thử.
2. **Cài 1 flavor mới (15-30')**: Claude Code / Slack / Excel / Chrome → thử 2-3 tác vụ thật → ghi lại công sức setup, ấn tượng đầu, trở ngại đã bỏ chưa, có muốn dùng tiếp không.
3. **Xếp hạng theo vai trò (5')**: #1 giá trị nhất → #4 ít nhất → triển khai theo thứ tự.

## Tóm tắt

- **Claude = trí tuệ, flavor = cách tiếp cận**; flavor chuyên biệt loại bỏ chuyển ngữ cảnh.
- 4 flavor chính: **Code** (dev), **Slack** (teamwork), **Excel** (bảng tính), **Chrome** (web).
- **Chọn flavor theo tác vụ hiện tại.**
- **Preview ≠ production:** Chrome chỉ cho việc rủi ro thấp.
- Một ngày làm việc thường kết hợp 2-3 flavor.
- 15-30 phút setup mỗi flavor đền lại hàng giờ mỗi tháng.
