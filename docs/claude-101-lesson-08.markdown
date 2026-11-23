---
layout: page
title: Bài 08 - Connectors & MCP
permalink: /claude-101-lesson-08/
parent: Claude
nav_order: 9
---

# Bài 08: Connectors — Kết nối công cụ qua MCP

**Module 4 — Mở rộng phạm vi Claude** · ~20 phút · Trung cấp

## Ý chính

Không có connector, bạn là "phần mềm trung gian": copy từ Slack/Gmail/CRM/Drive dán vào Claude rồi copy kết quả ra (≈ 10-15 phút/ngày + gánh nặng nhận thức). **Connector** là cầu nối giữa Claude và công cụ ngoài (Drive, Slack, Notion, Gmail, Jira, Salesforce...), cho phép Claude:

- **Đọc / tìm kiếm:** file, tài liệu, tin nhắn, thread, bản ghi
- **Thực hiện hành động:** tạo task, cập nhật record, gửi tin nhắn, sửa dữ liệu (tùy quyền bạn cấp)

## MCP — "USB-C cho AI"

**Model Context Protocol** là chuẩn mở (Anthropic mở mã nguồn) giúp AI kết nối nhiều ứng dụng qua một giao diện thống nhất.

- Trước: mỗi nhà cung cấp AI có API/SDK riêng cho từng công cụ → phân mảnh
- Sau: ai cũng có thể viết MCP server cho bất kỳ công cụ nào
- Lợi ích: hàng nghìn connector từ cộng đồng; trải nghiệm quản lý quyền nhất quán; dữ liệu giữ ở công cụ nguồn, chỉ truyền khi cần; công cụ mới hỗ trợ MCP là dùng được ngay

## 2 loại connector

| Loại | Mô tả | Ví dụ |
|---|---|---|
| **Web connectors** | Dịch vụ cloud, chạy trên web và desktop | Productivity: Drive, Calendar, Docs, Notion, Asana · Communication: Slack, Gmail, Outlook, Teams · Dev: GitHub, GitLab, Linear, Jira, Sentry · Business: Salesforce, HubSpot, Stripe, QuickBooks · Data: Airtable, PostgreSQL, Snowflake, BigQuery · Design: Figma · Content: WordPress, Ghost |
| **Desktop extensions** | Chạy cục bộ qua Claude Desktop (Bài 04), không dùng được trên web | File hệ thống local, app native, điều khiển trình duyệt |

## Cài đặt

Thư mục: `claude.ai/directory` (tab Web / Desktop extensions) hoặc trong chat: **Search and tools → Add connectors**.

**Web connector (5 bước):**

1. Tìm connector
2. **Connect**
3. **Xác thực** qua OAuth (Claude không lưu mật khẩu)
4. **Cấp quyền** tối thiểu (vd Drive: đọc, tìm kiếm; tùy chọn tạo/sửa doc) — bật/tắt lại được
5. **Test:** "Bạn truy cập được Google Drive của tôi không?", "Tìm trên Slack tin nhắn chứa 'launch' trong 7 ngày qua"

**Desktop extension (4 bước):** tải Claude Desktop → Settings → Extensions → Install → làm theo bước riêng (cấp quyền file system, cài extension trình duyệt, cấu hình).

## Dùng connector

Để Claude tự quyết định, hoặc chỉ định: "Dùng Google Drive để tìm...", "Lấy từ Slack của tôi...".

| Nhóm | Đọc | Ghi |
|---|---|---|
| Quản lý dự án (Asana, Linear, Jira) | Task ưu tiên tuần này; tình trạng dự án; ticket tag "performance" | Tạo task; đánh dấu hoàn thành; giao bug #1234 cho John (High) |
| Giao tiếp (Slack, Gmail) | Tìm thread về hợp đồng NCC; quyết định trong #product hôm qua | Soạn trả lời; gửi tin cho @jane; trả lời email khách theo bảng giá chuẩn |
| Tài liệu (Notion, Drive, Confluence) | Tìm brand voice guide; tóm tắt biên bản họp | Tạo trang Notion tổng kết; cập nhật Google Doc "Q4 OKRs" |
| Kinh doanh (Stripe, PayPal, Salesforce) | Xu hướng doanh thu; trạng thái deal Acme; giao dịch > $1.000 | Tạo opportunity $50K; cập nhật contact thành "qualified" |

## Ví dụ workflow

**Release notes tự động** (Linear + Drive): "Lấy ticket hoàn thành trong sprint tuần này từ Linear và tạo release notes theo mẫu trong Google Drive." → ~95 phút còn ~7 phút (tiết kiệm ~76 giờ/năm).

**Workflow liên công cụ:**

1. **Họp → Action items:** đọc transcript Zoom (Drive) → trích action item + owner → tạo task Asana → đăng tóm tắt lên Slack #team, mention người liên quan.
2. **Ticket → Giải quyết:** đọc ticket Zendesk → tra tài khoản Salesforce → check email 3 tháng (Gmail) → soạn trả lời đồng cảm, tập trung giải pháp, 150 từ.
3. **Weekly intel (scheduled task Cowork):** thứ Hai 8h: tin đối thủ (web) + đề cập trên Slack + phản hồi khách trong Gong → brief 1 trang → lưu Drive "Weekly Intel".

## Ví dụ theo vai trò

| Vai trò | Connectors | Workflow | Giá trị |
|---|---|---|---|
| Sales Manager | Salesforce, Gmail, Slack, Calendar | Thứ Sáu 16h: deal chuyển stage + thread email → tóm tắt thắng/thua/trở ngại → Slack #sales-leadership | Review pipeline 2 giờ → 10 phút |
| Marketing | HubSpot, GA, Slack, Drive | Hàng ngày 9h: số liệu chiến dịch vs baseline, cờ CTR giảm > 20% / CPA tăng > 30% → Slack #marketing-ops | Phát hiện bất thường < 24h |
| Tài chính | QuickBooks, Drive, Excel | Ngày 1: P&L tháng trước vs budget, cờ lệch > 15%, cập nhật dashboard, email CFO | Chốt sổ 1 ngày → 1 giờ review |
| Pháp lý | Drive, Outlook, Jira | Hàng tuần: quét hợp đồng mới, so template, tạo Jira ticket, gửi ma trận rủi ro | Không sót hợp đồng |
| Biên tập y tế | Drive, REDCap | Lấy dữ liệu mới, cập nhật phần CSR, đánh dấu phần cần chuyên gia | Tự động hóa cập nhật lặp lại |
| Curriculum | Drive, Classroom, Canvas | Lấy bài nộp, cờ trễ/< 70%, nháp feedback cho giáo viên duyệt | Feedback cá nhân hóa quy mô lớn |
| Data analyst | PostgreSQL, Slack, Notion | Yêu cầu ad-hoc trên Slack → SQL → chạy → Notion + trả lời thread | 2 giờ → 20 phút |
| Support | Zendesk, Salesforce, Confluence | Ticket mới → lấy tài khoản + top 3 bài KB → nháp trả lời | Giải quyết nhanh hơn 30-50% |

## Bảo mật & quyền

1. **Quyền có phạm vi:** vd Gmail bật/tắt riêng: đọc, gửi, sửa nhãn, xóa (thường tắt). Cấp tối thiểu, mở rộng sau.
2. **Claude thấy những gì bạn thấy:** không vượt quyền tài khoản của bạn → ranh giới bảo mật của bạn = của Claude.
3. **Thu hồi bất cứ lúc nào:** ở Settings của Claude (ưu tiên) hoặc trong cài đặt bảo mật của dịch vụ bên thứ ba (khẩn cấp).
4. **Custom/third-party connector:** chỉ nguồn tin cậy; đọc code trước khi cài; quyền tối thiểu; kiểm tra access log định kỳ.

**Lưu ý:** dùng tài khoản Claude cá nhân với công cụ công việc → xem chính sách AI của công ty; nhiều tổ chức yêu cầu Team/Enterprise (có BAA/DPA).

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Bật mọi quyền "để phòng hờ" | Cấp tối thiểu, chỉ khi cần ngay |
| Kết nối 30 công cụ cho "toàn diện" | Bắt đầu với 3 công cụ tốn thời gian nhất |
| Cài MCP server ngẫu nhiên từ GitHub | Chỉ nguồn tin cậy, đọc code, thử sandbox |
| Share chat công khai khi chứa dữ liệu lấy từ Gmail riêng | Kiểm tra và làm sạch trước khi chia sẻ |
| Nghỉ việc mà quên ngắt connector | Offboarding checklist |
| Giao hành động rủi ro cao ("chuyển $50.000") | Connector cho đọc/ghi rủi ro thấp; hành động rủi ro cao cần người duyệt |

## Bài tập

1. **Top 3 công cụ (5')**: dùng nhiều nhất? copy-paste với Claude nhiều nhất? kết nối sẽ tiết kiệm ≥ 30 phút/tuần?
2. **Kết nối đầu tiên (10')**: `claude.ai/directory` → connect → xác thực → quyền tối thiểu → test "Bạn truy cập được [công cụ] không? Cho tôi xem [câu hỏi đơn giản]".
3. **Workflow liên công cụ (15')**: viết prompt dạng "[Lấy dữ liệu từ A]. [So với B]. [Tổng hợp]. [Đăng lên C / lưu vào D]." → test → lặp → lưu prompt.

## Tóm tắt

- Connector biến Claude từ "AI để chat" thành "AI làm việc trong hệ thống của bạn".
- **MCP = USB-C cho AI**: chuẩn mở, hàng nghìn connector.
- 2 loại: **Web** (cloud) và **Desktop extensions** (local).
- Bảo mật: quyền có phạm vi, Claude thấy những gì bạn thấy, thu hồi được bất cứ lúc nào.
- Workflow liên công cụ nhân đôi ROI; bắt đầu với 3 công cụ hàng đầu.
- Tối thiểu trước, mở rộng sau.
