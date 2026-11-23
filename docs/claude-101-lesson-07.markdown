---
layout: page
title: Bài 07 - Skills
permalink: /claude-101-lesson-07/
parent: Claude 101
grand_parent: Claude
nav_order: 9
---

# Bài 07: Skills — Đóng gói quy trình

**Module 3 — Tổ chức công việc** · ~15 phút · Trung cấp

## Ý chính

Biết kiến thức ≠ làm đúng phương pháp mỗi lần. **Skill** gói toàn bộ quy trình (các bước, template, file tham chiếu, quy tắc phong cách) thành một gói mà Claude **tự động dùng** khi task khớp. Tạo một lần, dùng ở claude.ai, Claude Code và API.

**Skill = thư mục** chứa instructions, scripts, tài nguyên:

```
quarterly-variance-analysis/
├── SKILL.md          # entry point: name + description + hướng dẫn chính
├── references/       # variance-categories.md, thresholds.md, memo-style-guide.md
├── assets/           # memo-template.docx, variance-report-template.xlsx
└── scripts/          # (tùy chọn) calculate_variance.py
```

## Cách hoạt động — progressive disclosure

1. **Khởi động:** chỉ nạp name + description của mỗi skill vào system prompt (~30-50 token/skill).
2. **Khi prompt khớp description:** Claude tải toàn bộ `SKILL.md`.
3. **Khi cần:** tải/chạy dần `references/`, `assets/`, `scripts/`.

→ Cài 50 skill vẫn không quá tải context.

## 2 loại Skill

| Loại | Mô tả |
|---|---|
| **Anthropic Skills** (built-in) | Do Anthropic duy trì, cho mọi người dùng trả phí, tự kích hoạt: `xlsx`, `pptx`, `docx`, `pdf` (việc tạo file Excel/PowerPoint/Word/PDF thực chất chạy bằng skill) |
| **Custom Skills** | Do bạn/tổ chức tạo: `brand-voice-review`, `quarterly-variance-analysis`, `customer-call-prep`, `compliance-checklist`, `meeting-summary`, `blog-seo-optimizer` |

## Bật Skills (preview cho Pro, Max, Team, Enterprise)

- **Pro/Max:** Settings → Capabilities → bật *Code execution and file creation* → cuộn xuống Skills, bật/tắt từng skill.
- **Enterprise:** Owner bật Code execution + Skills trong Admin settings trước.
- **Team:** bật mặc định ở cấp tổ chức.

## Dùng trong thực tế

Không cần chọn skill, Claude tự chọn theo yêu cầu. Ví dụ prompt: "Tạo Excel theo dõi chi tiêu có công thức tổng và % theo danh mục", "Chuyển ghi chú họp thành PowerPoint cho quản lý", "Báo cáo PDF Q4 kèm biểu đồ", "Mô hình tài chính base/bull/bear".

**File execution:** upload xlsx/pptx/docx/pdf → Claude đọc, phân tích, thêm công thức/slide/đề xuất sửa, tạo **phiên bản mới** (không sửa bản gốc). Bật *Allow limited network access* khi cần dữ liệu ngoài.

## Bảo mật

Skill có thể chứa code thực thi.

- ✅ Chỉ dùng skill từ nguồn tin cậy: built-in, tự tạo, được admin duyệt; đọc nội dung trước khi dùng nếu từ ngoài
- ⚠️ Cảnh báo: nguồn ẩn danh, code bị che giấu, request mạng không giải thích, quyền rộng không cần thiết
- Tránh cài skill ngẫu nhiên từ internet khi chưa tự tin đọc code

## Tạo Custom Skill — qua chat, không cần code

1. **Nêu ý định:** "Tôi muốn tạo skill viết báo cáo kinh doanh hàng quý."
2. **Trả lời phỏng vấn của Claude:** skill làm gì, thế nào là kết quả tốt, ví dụ, các bước, ràng buộc.
3. **Upload tài liệu tham chiếu:** template, style guide, brand assets, sản phẩm mẫu đã hài lòng.
4. **Lưu** → Claude sinh cấu trúc skill. Xem/sửa tại tab **Customize** ở sidebar.

**Cải tiến:** dùng 1-2 tuần rồi bảo Claude cập nhật, ví dụ *"Cập nhật skill quarterly-variance-analysis: thêm so sánh 12 tháng gần nhất ngoài QoQ; memo có bảng sensitivity."*

### Case study: quarterly-variance-analysis

- Claude hỏi: nguồn dữ liệu (P&L từ NetSuite + budget Excel từ FP&A), ngưỡng (doanh thu > 15%, chi phí > 10%, cả hai chiều), đầu ra (memo, đôi khi tóm tắt 1 trang), đối tượng (CFO → board), phân loại (Volume, Price, Mix, Timing, One-time), 3 memo mẫu Q2-Q4/2024.
- Quý sau chỉ cần: *"Chạy phân tích chênh lệch Q1 2025."* → `Q1-2025-variance-memo.docx` + `Q1-2025-variance-details.xlsx`, không dán lại SOP 15 trang, nhất quán mỗi quý.

## Skills vs Projects

**Projects lưu kiến thức (what). Skills thực hiện nhiệm vụ (how).** Bổ sung cho nhau.

| | Projects | Skills |
|---|---|---|
| Mục đích | Kho kiến thức Claude tham chiếu | Quy trình Claude thực hiện |
| Phù hợp | Bối cảnh dài hạn, tài liệu, làm nhóm | Việc lặp lại, nhiều bước, phương pháp nhất quán |
| Kích hoạt | Khi bạn ở trong Project | Tự động khi prompt khớp description |
| Phạm vi | Chỉ claude.ai | claude.ai, Claude Code, API |

Kết hợp: Project "Customer Hub" (hồ sơ khách, QBR cũ, call logs) + skill `customer-qbr-prep` (tóm tắt 90 ngày, đánh dấu vấn đề, tạo QBR theo template). Nói "Chuẩn bị QBR cho Acme Corp" → skill lấy dữ liệu từ Project.

## Ví dụ theo vai trò

| Vai trò | Skill | Đầu vào → Đầu ra |
|---|---|---|
| Sales | `discovery-call-notes-processor` | Transcript → tài liệu MEDDIC |
| Marketing | `brand-voice-reviewer` | Bản nháp → điểm lệch brand + đề xuất sửa |
| Tài chính | `monthly-close-reconciliation` | Bank + ERP + hóa đơn → workbook matched/unmatched + báo cáo ngoại lệ |
| Pháp lý | `contract-review-v2` | Hợp đồng NCC → đề xuất sửa + ma trận rủi ro |
| Biên tập y tế | Soạn CSR | Dữ liệu + protocol → bản nháp CSR theo ICH E3 |
| Curriculum | `lesson-plan` | Chủ đề + lớp → giáo án theo mẫu |
| Data analyst | `weekly-progress-report` | Dữ liệu thô → báo cáo KPI + cảnh báo bất thường |
| HR | `onboarding-kit` | Vị trí + phòng ban → bộ onboarding 4 giai đoạn |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| 15 skill cho 15 biến thể nhỏ | 1 skill cho quy trình cốt lõi, biến thể qua tham số trong prompt |
| Skill quá chung ("viết nội dung") | Skill phải có phương pháp/ràng buộc riêng; không có → dùng prompt tốt |
| Cài skill ngẫu nhiên từ internet | Kiểm tra code, tin nguồn, thử trong sandbox |
| Không bao giờ cập nhật | Rà soát hàng tháng, cập nhật khi quy tắc đổi |
| Dùng "skill" để chứa 50 PDF tham khảo | Đó là Project |
| Chia sẻ skill chứa phương pháp độc quyền công khai | Giữ nội bộ, admin quản lý chia sẻ |

## Bài tập

1. **Chọn ứng viên Skill:** với mỗi task ở Bài 01, tick: có phương pháp cụ thể? lặp ≥ 1 lần/tuần hoặc 3 lần/tháng? có template đầu ra? có quy tắc phong cách? Từ 3 dấu tích → ứng viên. Chọn top 3.
2. **Tạo skill đầu tiên:** chat mới *"Tôi muốn tạo skill cho [quy trình]. Hãy phỏng vấn tôi."* → trả lời cụ thể → upload 2-3 tài liệu → xem lại → lưu → dùng ở task kế tiếp, lặp nếu cần.

## Tóm tắt

- **Skill = gói chuyên môn**, tự tải khi khớp; progressive disclosure nên cài nhiều vẫn nhẹ.
- 2 loại: **built-in** (xlsx/pptx/docx/pdf) và **custom**.
- Tạo bằng chat: ý định → trả lời → upload tài liệu → lưu.
- **Projects = kiến thức, Skills = quy trình**; dùng cùng nhau.
- **Bảo mật trước:** chỉ dùng skill từ nguồn tin cậy.
- Skill dùng được trên claude.ai, Claude Code, API.
