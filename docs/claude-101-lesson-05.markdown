---
layout: page
title: Bài 05 - Projects
permalink: /claude-101-lesson-05/
parent: Claude 101
grand_parent: Claude
nav_order: 7
---

# Bài 05: Projects — Không gian làm việc chuyên dụng

**Module 3 — Tổ chức công việc** · ~20 phút · Trung cấp

## Ý chính

**Project = workspace** có lịch sử chat riêng, **knowledge base** (tải 1 lần, dùng mọi chat) và **instructions** (viết 1 lần, áp dụng mọi chat). Team/Enterprise có thể chia sẻ cho đồng nghiệp. Mỗi chat trong Project tự kế thừa knowledge + instructions, hết cảnh dán lại bối cảnh (≈ 8 giờ/năm, chưa kể rủi ro lệch tông).

## Khi nào dùng Project

1. Có tài liệu tham khảo dùng nhiều lần (brand guide, mẫu, dữ liệu lịch sử, personas, specs)
2. Cần Claude phản hồi nhất quán (tông, nguồn trích dẫn, template)
3. Làm việc nhóm trên cùng nền tảng

**Quy tắc "lần thứ 3":** dán cùng context lần thứ 3 → tạo Project.

## Tạo Project — 3 bước

**1. Thiết lập:** `claude.ai/projects` → **+ New Project** → tên rõ ràng (✅ "Chiến dịch Q4 — Flower Biz", ❌ "Dự án 1") → mô tả (cho người, không phải cho Claude) → Private / Public (Team/Enterprise).

**2. Instructions — 4 phần:**

| Phần | Ví dụ |
|---|---|
| Bối cảnh dự án | "Nội dung marketing cho startup B2B HR-tech, khách hàng là HR leader SMB 50-500 nhân viên" |
| Quy trình | "Khi tải biên bản họp: tóm tắt theo Quyết định / Việc cần làm (người phụ trách) / Câu hỏi mở / Bước tiếp" |
| Tông & phong cách | "Chuyên nghiệp nhưng gần gũi, câu chủ động, không dùng 'synergy', 'leverage'" |
| Yêu cầu cụ thể | "Luôn có CTA; tiêu đề email + preview < 200 từ" |

Mẹo: viết instructions như code (NẾU ... THÌ ...; LUÔN tránh ...). Áp dụng cùng Preferences và Styles trong Settings.

**3. Knowledge base:** nút **+** hoặc kết nối Google Drive. Định dạng: PDF, DOCX, CSV, TXT, HTML, MD...

- Nên tải: brand guide, OKR/chiến lược, nghiên cứu thị trường, ghi chú họp, ví dụ tốt (blog, báo cáo, slide), tài liệu kỹ thuật (specs, API, wiki)
- **Đặt tên file có ý nghĩa** (Claude truy xuất theo tên): `document1.pdf` → `Q4-2025-Brand-Guidelines-v2.pdf`. Mẫu: `[Topic]-[Date]-[Version/Owner].ext`

## RAG

- KB nhỏ (< 5 file, < 100 trang): nạp toàn bộ vào context.
- KB lớn (> 20 file hoặc > 500 trang): **RAG** tự bật — tìm các đoạn liên quan nhất rồi đưa vào context; mở rộng dung lượng ~10 lần, bạn không cần làm gì.
- Vẫn nên **nhắc tên tài liệu** trong prompt: "Dựa vào Báo cáo bán hàng Q3, ..."

## Knowledge base vs context của chat

| | Project knowledge | Chat context |
|---|---|---|
| Phạm vi | Mọi chat trong Project | Chỉ chat đó |
| Dùng cho | Tài liệu tham khảo, template, brand — dùng lặp lại | Bối cảnh 1 lần, dữ liệu nhạy cảm, file thử |

Quy tắc: dùng 1 lần → để ở chat; dùng > 1 lần → đưa vào KB.

## Chia sẻ (Team/Enterprise)

| Quyền | Làm được gì |
|---|---|
| **Can view** | Xem, truy cập KB, tham gia chat; không sửa |
| **Can edit** | Sửa instructions, cập nhật tài liệu |
| **Owner** | Toàn quyền, quyết định ai truy cập |

Chia sẻ: **Share project** → cá nhân (tên/email), hàng loạt (dán danh sách) hoặc cả tổ chức. Người nhận thấy ở "Shared with me".

Ví dụ **Brand Voice Central**: brand guide, 20 bài mẫu tốt nhất, danh sách từ cấm, personas → 10 người viết cùng một giọng, nhân sự mới onboard nhanh.

## Project mẫu

| Project | Knowledge | Instructions chính |
|---|---|---|
| Ra mắt sản phẩm Q4 | Specs, phân tích đối thủ, ghi chú brainstorm, nghiên cứu khách hàng | Tự tin, không "sales"; người mua am hiểu kỹ thuật |
| Hỗ trợ nghiên cứu | Đánh giá đối thủ, dữ liệu user research, feedback | Tổng hợp nhiều nguồn, nêu mâu thuẫn, trích `[Tên file]` |
| Client Account Hub | Brand khách, sản phẩm cũ, lịch sử giao tiếp, bối cảnh ngành | Theo tông khách, đúng thuật ngữ ngành |
| Lập kế hoạch sự kiện | Hợp đồng địa điểm, bio diễn giả, dữ liệu người tham dự | Bám chủ đề và tông sự kiện |
| Tài chính cá nhân | Sao kê 12 tháng, ngân sách, mục tiêu | Đánh dấu bất thường, tôn trọng riêng tư |

## Ví dụ theo vai trò

| Vai trò | Project | Kết quả |
|---|---|---|
| Sales lead | Enterprise Pipeline (ICP, đối thủ, win/loss, bảng giá; khung MEDDIC) | Nhân viên mới có context như người 2 năm kinh nghiệm |
| Tài chính | Monthly Close (SOP, chart of accounts, vendor list; dung sai ±$500, chỉ báo ngoại lệ) | Chốt sổ 5 ngày → 2 ngày |
| Pháp lý | Contract Review Hub (template, GDPR/CCPA, ma trận rủi ro; gắn "cần luật sư xem") | Review 4 giờ → 1 giờ |
| Curriculum | Giáo án Vật lý 11 (chuẩn, SGK, giáo án cũ) | Thiết kế 2 tuần → 4 ngày |
| Data analyst | Growth Metrics (data dictionary, SQL library, stakeholder personas) | Ad-hoc 2 giờ → 30 phút |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Project "kho rác" 200 file | Mỗi Project = 1 luồng công việc rõ |
| Instructions mơ hồ ("viết chuyên nghiệp") | Cụ thể: vai, đối tượng, tông, từ cấm |
| Không bao giờ cập nhật KB | Rà soát hàng quý, ghi "cập nhật lần cuối" |
| Tên file mơ hồ | `[Topic]-[Date]-[Version].ext` |
| Chia sẻ cả tổ chức khi có PII | Least privilege |
| Tải dữ liệu nhạy cảm (PII, y tế, tài chính) vào tài khoản cá nhân/Pro | Dùng gói Enterprise có BAA/DPA |

## 5 best practices

1. Bắt đầu tập trung, mở rộng sau (5 Project nhỏ > 1 "Everything Project")
2. Luôn cập nhật knowledge base
3. Viết instructions rõ ràng như lập trình
4. Đặt tên tài liệu có cấu trúc
5. Nhắc tên tài liệu trong chat

## Bài tập

1. Từ danh sách Bài 01, chọn 3 Project tiềm năng (việc lặp ≥ 3 lần/tháng cùng context): tên, KB, instructions, chat điển hình.
2. Tạo Project #1: tên + mô tả + quyền (3') → instructions ≥ 150 từ theo 4 phần (5') → tải 3-5 tài liệu đặt tên rõ (5') → mở chat mới, kiểm tra Claude có áp dụng instructions và tham khảo KB không (2').

## Tóm tắt

- **Project = knowledge + instructions + lịch sử chat**, cài một lần dùng mãi.
- 3 dấu hiệu: tài liệu lặp lại, cần nhất quán, làm nhóm.
- **Instructions** 4 phần, cụ thể > mơ hồ.
- **Tên file rõ** = truy xuất tốt; RAG tự động khi KB lớn.
- Team: 3 quyền (view / edit / owner); dữ liệu nhạy cảm → gói Enterprise.
