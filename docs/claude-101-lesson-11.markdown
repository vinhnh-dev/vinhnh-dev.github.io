---
layout: page
title: Bài 11 - Claude theo vai trò
permalink: /claude-101-lesson-11/
parent: Claude 101
grand_parent: Claude
nav_order: 13
---

# Bài 11: Claude theo vai trò — Use case theo vai trò

**Module 5 — Áp dụng thực tế** · ~15 phút · Ứng dụng

## Ý chính

Khoảng cách giữa "biết tính năng" và "áp dụng vào công việc" được lấp bằng **use case cụ thể theo vai trò**. Mỗi use case dưới đây ghi: khó khăn → giải pháp (Project / Skills / Connectors / Research / Artifacts / Scheduled task) → tác động. Quét nhanh, tìm vai trò gần nhất, chọn 1 use case thử trong tuần này.

Use Case Gallery: `claude.com/resources/use-cases`

## Use case theo vai trò

### Tổng hợp (đa vai trò)

| Use case | Giải pháp | Tác động |
|---|---|---|
| Báo cáo tình trạng dự án | Project "Status Report Hub" (mẫu, báo cáo cũ, gu stakeholder) + connectors Asana/Jira, Slack, Gmail + skill `weekly-status-generator` | 3 giờ → 20 phút |
| Phân tích mẫu feedback người dùng (Zendesk, NPS, review) | Research: lấy feedback 90 ngày, nhóm theo chủ đề, xếp theo tần suất, trích dẫn, tách vấn đề mới (30 ngày) và lâu dài | 2 ngày → 3 giờ |
| Đóng gói brand guide thành Skill | `brand-voice-review`: bản nháp → điểm lệch + đề xuất sửa | Nhất quán thương hiệu, onboarding nhanh hơn |

### 💼 Sales

| Use case | Giải pháp | Tác động |
|---|---|---|
| Thư viện battle card | Project hồ sơ từng đối thủ + Research (tính năng, giá, review, phản đối) + skill `battle-card-generator` | Tăng win rate trước đối thủ chính |
| Chuẩn bị deal | Connectors Salesforce + Gmail + LinkedIn; skill lấy lịch sử tài khoản, contact, tin tức, talking points → brief 1 trang | 45 phút → 5 phút/deal |
| Báo cáo pipeline | Scheduled task thứ Sáu 16h: Salesforce → format → email/Slack lãnh đạo + skill chuẩn hóa | 2 giờ/tuần → 10 phút |

### 📣 Marketing

| Use case | Giải pháp | Tác động |
|---|---|---|
| Phân tích hiệu suất chiến dịch | Connectors HubSpot/Marketo, GA, Meta/LinkedIn Ads; scheduled task 9h hàng ngày: so baseline, cờ bất thường, tóm tắt Slack | Phát hiện vấn đề < 24 giờ |
| Điều chỉnh nội dung đa kênh | Skill content repurposer (quy tắc từng kênh) + Project brand voice + ví dụ từng kênh | 4 giờ → 30 phút |

### 💰 Tài chính

| Use case | Giải pháp | Tác động |
|---|---|---|
| Xây mô hình tài chính | Artifact Excel với input động + skill DCF/scenario theo phương pháp công ty | Vài ngày → vài giờ |
| Soạn investment memo | Project (memo cũ, framework) + skill memo drafter + Research thị trường/đối thủ | 8 giờ → 2 giờ |
| Hiểu bảng tính được kế thừa (8 sheet) | Claude for Excel: "Giải thích quy trình, sheet 3 phụ thuộc gì, truy lỗi công thức" | Hiểu trong 20 phút |

### 👥 HR

| Use case | Giải pháp | Tác động |
|---|---|---|
| Onboarding cá nhân hóa | Project "Onboarding Hub" (tài nguyên theo vị trí) + skill `onboard-role` + checklist HTML có progress | 2-3 giờ → 20 phút/người |

### ⚖️ Pháp lý

| Use case | Giải pháp | Tác động |
|---|---|---|
| Dòng thời gian & mẫu trong discovery | Project riêng cho từng vụ án + Research: dựng timeline sự kiện, tìm mẫu (bên lặp lại, đổi quyết định), đánh dấu mâu thuẫn | Vài tuần → vài ngày |

### 🔍 Nghiên cứu

| Use case | Giải pháp | Tác động |
|---|---|---|
| Literature review 80+ bài | Project thư mục bài + Research theo giai đoạn (danh mục → chủ đề → khoảng trống) + skill `lit-review-assist` (PRISMA, tường thuật...) | 3 tuần → 1 tuần |
| Kiểm chứng thống kê từ dữ liệu thô | Claude for Excel / Claude Code: "Tái tạo phân tích, đánh dấu khác biệt" | Phát hiện lỗi trước khi triển khai |

### 🛠️ Product

| Use case | Giải pháp | Tác động |
|---|---|---|
| Tổng hợp user research (50 transcript) | Project + Research nhóm chủ đề + Artifact bản đồ chủ đề tương tác | 3 ngày → 3 giờ |
| Viết PRD | Project (mẫu, PRD cũ) + skill `prd-drafter` + Jira và công cụ research | 3 giờ → 30 phút |
| Báo cáo chỉ số sản phẩm | Connectors (Amplitude, Mixpanel, GA) + scheduled task → dashboard hợp nhất + cảnh báo | 4 giờ → 15 phút |

### 🎧 Support

| Use case | Giải pháp | Tác động |
|---|---|---|
| Trả lời ticket theo KB | Zendesk + Confluence/Notion + skill triage (quét, lấy KB, nháp trả lời) | 15 → 5 phút/ticket |
| Theo dõi sentiment | Scheduled task: phân loại ticket, nhóm chủ đề, cờ xu hướng mới | 1 ngày → 30 phút |

### 🎓 Giáo dục

| Use case | Giải pháp | Tác động |
|---|---|---|
| Chấm luận theo rubric | Skill chấm sơ bộ; giáo viên chỉnh và cá nhân hóa 30% cuối | 6 giờ → 2 giờ |
| Thiết kế module | Project (chuẩn, module cũ) + skill backward design | 2 tuần → 3-4 ngày |

### 🏢 Bất động sản

| Use case | Giải pháp | Tác động |
|---|---|---|
| Listing presentation | Skill (comps, dữ liệu khu vực, cá nhân hóa) + Artifact tương tác | 4 giờ → 30 phút |
| Sàng lọc deal (50+ BĐS) | Cowork + subagents sàng song song → top 10 + phân tích | 3 ngày → 4 giờ |

### 📊 Data / Analytics

| Use case | Giải pháp | Tác động |
|---|---|---|
| Yêu cầu SQL ad-hoc | Skill `analyze-request` (câu hỏi → SQL → chạy → định dạng + giải thích) + PostgreSQL/Snowflake/BigQuery | 2 giờ → 20 phút |
| Tài liệu hóa dashboard | Skill `dashboard-docs` (query, quan hệ, logic nghiệp vụ) | 3 ngày → 6 giờ |

### 🏥 Healthcare

| Use case | Giải pháp | Tác động |
|---|---|---|
| Tổng hợp hồ sơ bệnh nhân | Skill: timeline + cờ bất thường. **Liên quan HIPAA — cần gói Enterprise có BAA** | 30 → 5 phút/bệnh nhân |
| Soạn CSR | Skill theo template, đánh dấu phần chuyên gia duyệt | 6 tuần → 3 tuần |

### 🏭 Operations / Supply chain

| Use case | Giải pháp | Tác động |
|---|---|---|
| Đánh giá nhà cung cấp (50+) | Skill khung tiêu chí → scorecard + xếp hạng + kế hoạch khắc phục | 1 tuần → 2 ngày |
| Tổng hợp tình trạng 5+ dự án hàng tuần | Scheduled task Cowork + Asana/Jira, Slack, Gmail | 4 giờ → 30 phút review |

### 👔 Trợ lý điều hành

| Use case | Giải pháp | Tác động |
|---|---|---|
| Daily brief cho lãnh đạo | Scheduled task 7h + skill `exec-morning-brief` | 2 giờ → 15 phút |
| Chuẩn bị họp hội đồng | Project (mẫu báo cáo + form nộp) + skill tổng hợp tự động | 1 tuần → 2 ngày |

## Chọn use case để bắt đầu

Đừng làm 10 cái cùng lúc: chọn 1 → triển khai đầy đủ → đo → mở rộng.

**Chấm điểm (1-5 mỗi tiêu chí):**

| Tiêu chí | Trọng số |
|---|---|
| Tần suất (lần/tuần) | × 3 |
| Thời gian tiết kiệm/lần | × 2 |
| Công sức thiết lập (1 = cao, 5 = thấp) | × 1 |
| Độ ổn định (1 = đổi nhiều, 5 = nhất quán) | × 2 |

Ví dụ: Morning brief (5 lần/tuần, 30 phút, setup 4, ổn định 5) = 15 + 60 + 4 + 10 = **89**; Q-end variance memo (0,25 lần/tuần, 2 giờ, setup 3, ổn định 5) = 0,75 + 4 + 3 + 10 = **17,75** → làm morning brief trước.

*(Ví dụ gốc dùng số thực để nhân, không chỉ thang 1-5.)*

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Thử mọi tính năng cùng lúc | 1 use case, 1-2 tính năng, làm chủ rồi mở rộng |
| Mục tiêu chung chung ("dùng Claude cho email") | Use case cụ thể + kết quả đo được (vd "pipeline report: review < 30 phút thay vì 2 giờ") |
| Bắt đầu từ quy trình khó nhất | Chọn tần suất cao + độ phức tạp vừa để có quick win |
| Giữ Skill/Project hữu ích cho riêng mình | Chia sẻ với team (Team/Enterprise) để nhân tác động |

## Bài tập

1. **Chấm điểm 3 use case** từ danh sách Bài 01 theo khung trên → chọn điểm cao nhất.
2. **Kế hoạch triển khai use case #1:** liệt kê tính năng cần (Project, Skills, Connectors, Artifacts, Research, Scheduled tasks), ước tính giờ setup và giờ tiết kiệm/tuần; mục tiêu tuần 1 (setup cơ bản), tuần 2 (lặp prompt), tuần 4 (đo thời gian tiết kiệm, mức hài lòng); chỉ số thành công.
3. **Khám phá Use Case Gallery (10'):** lọc theo vai trò, chọn thêm 2 use case phù hợp, lưu lại.

## Tóm tắt

- Kiến thức tính năng chỉ có tác động khi gắn với **use case theo vai trò**.
- **1 use case làm sâu > 10 use case hời hợt.**
- Khung chấm điểm: **Tần suất ×3 + Thời gian tiết kiệm ×2 + Setup ×1 + Độ ổn định ×2.**
- **Kết hợp** Project + Skills + Connectors tạo ra thay đổi lớn.
- Use Case Gallery là nguồn cảm hứng; **team > cá nhân** (chia sẻ để nhân tác động).
