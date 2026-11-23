---
layout: page
title: Bài 04 - Claude Desktop
permalink: /claude-101-lesson-04/
parent: Claude 101
grand_parent: Claude
nav_order: 6
---

# Bài 04: Claude Desktop — Chat, Cowork, Code

**Module 2 — Môi trường làm việc** · ~20 phút · Trung cấp

## Ý chính

Web không truy cập được file local, không có scheduled task, không có quick entry. App Desktop bổ sung **3 chế độ, 1 trí tuệ**:

- **Chat** — hỏi nhanh, nháp, brainstorm (đồng nghiệp ghé bàn hỏi 1 câu)
- **Cowork** — ủy quyền cả dự án nhiều bước (đồng nghiệp nhận việc dài ngày)
- **Code** — môi trường phát triển phần mềm đầy đủ

Cowork và Code chạy trên cùng engine (Claude Code): chạy cục bộ, làm việc độc lập, tạo sub-agents, xử lý tác vụ dài.

## Chat mode — 4 tính năng riêng của Desktop

| Tính năng | Mô tả |
|---|---|
| **Quick Entry** (Mac: nhấn đúp `Option`) | Cửa sổ Claude nổi trên mọi app, `Esc` để đóng |
| **Screenshot / window sharing** | Chụp vùng hoặc chia sẻ cửa sổ: dashboard lạ, lỗi terminal, `#REF!` trong Excel |
| **Dictation** | Nói thay vì gõ |
| **Local connectors** | Apple Notes, Finder, app cài trên máy (chi tiết Bài 08) |

## Cowork mode — ủy quyền dự án

| Chat | Cowork |
|---|---|
| 1 lượt = 1 phản hồi | 1 mục tiêu = nhiều giờ làm việc |
| Bạn điều khiển từng bước | Claude tự lập kế hoạch và thực thi |
| Ngữ cảnh = 1 chat | Ngữ cảnh = thư mục + công cụ + lịch sử |
| Đầu ra: văn bản | Đầu ra: file hoàn chỉnh (docx, xlsx...) |

**Quy trình:** nhập mục tiêu → Claude hỏi làm rõ → đề xuất kế hoạch → **bạn duyệt/chỉnh** → thực thi (có thể song song bằng sub-agents) → xem tiến độ ở sidebar → nhận file kết quả trong thư mục.

### 8 tính năng cần biết

1. **Folder access** — chỉ đọc/ghi trong thư mục bạn chia sẻ
2. **Scheduled tasks** — thiết lập 1 lần, chạy hàng ngày/tuần/tháng (máy tắt thì chạy bù khi mở lại app)
3. **Sub-agents** — chia nhỏ, xử lý song song (15 phút thay vì 75)
4. **Dispatch** — tiếp tục Cowork từ điện thoại; desktop phải đang mở
5. **Projects trong Cowork** — gom file, bối cảnh, hướng dẫn, memory (cục bộ)
6. **Browser (Claude in Chrome)** — điều hướng, trích xuất dữ liệu web
7. **Computer use** — Pro/Max, macOS, beta; ưu tiên: connectors → Chrome → điều khiển màn hình; luôn xin quyền, có blocklist app
8. **Plugins** — mở rộng (dữ liệu tài chính realtime, KB nội bộ, compliance)

**Ranh giới an toàn:** Claude *không* truy cập file ngoài thư mục cấp quyền, app chưa cho phép, hay lưu state ngoài thư mục làm việc.

**Dùng khi:** tra cứu đa nguồn, nghiên cứu/đối thủ, thư mục 50+ tài liệu, việc hành chính lặp mỗi sáng. Gói Pro/Max/Team/Enterprise.

## Code mode

- Truy cập codebase, sửa code, chạy lệnh; **diff** trước khi áp dụng; terminal tích hợp; Git
- **Local** (code không rời máy, chạy dev server) vs **Remote** (kết nối GitHub, chạy trên cloud, tiếp tục khi tắt app)
- 3 chế độ tương tác:

| Mode | Hành vi |
|---|---|
| **Ask** | Đề xuất từng thay đổi, chờ duyệt (kiểm soát cao, chậm) |
| **Code** | Tự áp dụng thay đổi file, kiểm tra trước khi chạy lệnh (mặc định) |
| **Plan** | Lập kế hoạch tổng thể trước, bạn duyệt rồi mới làm (việc phức tạp) |

- Chạy nhiều session song song, lọc theo Active/Archived và Local/Cloud. Chi tiết Bài 12.

## So sánh 3 chế độ

| | Chat | Cowork | Code |
|---|---|---|---|
| Tối ưu cho | Hỏi nhanh, bản nháp | Nghiên cứu, phân tích, tài liệu dài | Phát triển phần mềm |
| Thời gian | Giây → phút | Phút → giờ | Phút → giờ |
| Đầu ra | Văn bản | File hoàn chỉnh | Thay đổi code, commit |
| Công cụ | Connectors, Skills, Chrome | + Plugins, computer use, scheduled tasks | + Hooks, Git |

## Một ngày với 3 chế độ (ví dụ)

- **8:00 Chat:** 3 sự kiện lịch hôm nay + bối cảnh
- **9:00 Quick Entry:** tra nhanh thuật ngữ khi đang viết email
- **10:00 Cowork:** review 30 hợp đồng vs template → `risk-matrix.xlsx` + `summary-memo.docx`, duyệt kế hoạch rồi đi họp
- **11:00:** xem tiến độ qua Dispatch trên điện thoại
- **14:00 Code:** test fail trên branch → Claude tìm nguyên nhân, đề xuất 2 cách sửa, commit & push
- **7:00 hôm sau:** scheduled task gửi email tóm tắt lịch + email ưu tiên + Slack

## Ví dụ theo vai trò

| Vai trò | Chat | Cowork | Code |
|---|---|---|---|
| Sales | Chuẩn bị cuộc gọi | Tổng kết tuần (CRM + email + Slack), lên lịch thứ Sáu | — |
| Tài chính | Hỏi công thức | Đối chiếu ngày 1 hàng tháng | Script Python ETL |
| Pháp lý | Tra quy định | Review hợp đồng theo lô | Tool theo dõi nghĩa vụ |
| Giảng viên | Tạo câu hỏi | Chấm 30 bài luận theo rubric (nháp) | Ít dùng |
| Data analyst | Giải thích SQL | Cập nhật dashboard hàng tuần | Pipeline ETL, visualization |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Dán 10 file vào Chat để "phân tích tất cả" | Dùng Cowork + folder access |
| Mở Cowork chỉ để hỏi 1 câu | Chat cho task < 3 bước; Cowork cho nhiều bước/cần công cụ |
| Nhập lại "daily brief" mỗi sáng | Dùng scheduled task (5 phút/ngày ≈ 25 giờ/năm) |
| Dùng Code để viết content marketing | Chat hoặc Cowork |
| Duyệt kế hoạch Cowork không đọc | Dành 1-2 phút chỉnh kế hoạch, tiết kiệm 30 phút làm lại |

## Bài tập

1. Lập bản đồ công việc tuần (từ Bài 01) → gán Chat / Cowork / Code + lý do.
2. Tạo 1 scheduled task trong Cowork (ví dụ "Báo cáo sáng" 7:00: 3 sự kiện lịch + Slack DM chờ xử lý + email ưu tiên → email ~200 từ cho bản thân); hôm sau xem lại và chỉnh prompt.

## Tóm tắt

- **3 chế độ, 1 trí tuệ** — chọn theo loại công việc.
- Desktop Chat > Web nhờ Quick Entry, screenshot, dictation, local connectors.
- **Cowork = ủy quyền dự án**: mục tiêu → kế hoạch → thực thi → kết quả.
- **Code** có Ask / Code / Plan, local hoặc remote.
- **Scheduled tasks** ít người dùng đủ, tiết kiệm 25+ giờ/năm.
- **Môi trường giới hạn**: chỉ thư mục được cấp quyền, hỏi quyền trước khi thao tác máy.
