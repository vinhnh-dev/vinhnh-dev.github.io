---
layout: page
title: Bài 06 - Artifacts
permalink: /claude-101-lesson-06/
parent: Claude 101
grand_parent: Claude
nav_order: 8
---

# Bài 06: Artifacts — Sáng tạo tương tác

**Module 3 — Tổ chức công việc** · ~20 phút · Cơ bản → Trung cấp

## Ý chính

**Artifact** = sản phẩm độc lập, tương tác được, hiển thị trong cửa sổ riêng bên cạnh Chat (dashboard chạy được, biểu đồ, tài liệu tải xuống được) thay vì 300 dòng code chôn trong chat. Việc mất nửa ngày → ~30 phút, không cần setup, build hay tải về.

**Claude tự tạo Artifact khi** nội dung độc lập, có ý nghĩa (thường > 15 dòng), bạn sẽ muốn sửa/tái sử dụng. Muốn ép tạo: *"Tạo nội dung này dưới dạng Artifact."*

## 6 loại Artifact

| Loại | Dùng cho | Ví dụ |
|---|---|---|
| **Documents** | Markdown, text, .docx, PDF, .pptx, .xlsx | Báo cáo Q4 cho CEO (Word, 2 trang) |
| **Code** | Python, JS, TS, Go, Rust, Java, C++... | Script quét file trùng bằng MD5 → `duplicates.csv` |
| **HTML** | Trang web HTML+CSS+JS một file, có preview | Landing page cho app năng suất |
| **SVG** | Logo, icon, minh họa, sơ đồ | Logo quán cà phê "Brew & Books" |
| **Mermaid** | Flowchart, sequence, Gantt, org chart | Quy trình onboarding khách hàng |
| **React** | UI tương tác có logic thật | Máy tính lương theo giờ cho freelancer |

## Thao tác với Artifact

- Chuyển **preview ↔ code**, **copy**, **download** (.md, .html, .py, .docx, .pptx, .xlsx)
- Mỗi phiên bản được lưu, có thể khôi phục
- Prompt tốt nên nêu rõ: chức năng, dữ liệu vào/ra, giới hạn, danh mục, phong cách (ví dụ dashboard ngân sách: 7 danh mục, biểu đồ tròn, cảnh báo đỏ khi > 8 triệu VND)

## Lặp lại trên Artifact

1. **Mỗi lần một thay đổi** → xem lại → thay đổi tiếp (dễ chẩn đoán, dễ hoàn tác)
2. **Phản hồi cụ thể:** ❌ "Đẹp hơn" → ✅ "Tăng khoảng cách giữa các thẻ, thêm bóng nhẹ, font-weight 500"
3. **Xây theo phiên bản:** V1 cấu trúc → V2 tương tác → V3 phong cách → V4 edge case/lỗi; test từng bản

Nếu Claude trả nội dung đáng lẽ là artifact trong chat: *"Tạo thành artifact / tài liệu để tôi chỉnh sửa."*

## Chia sẻ & xuất bản

| | Copy/Download | Chia sẻ trong tổ chức | Publish công khai |
|---|---|---|---|
| Đối tượng | Chỉ bạn | Đồng nghiệp Team/Enterprise | Ai có link |
| Cần đăng nhập | Không | Có (tổ chức) | Không |
| Gói | Tất cả | Team / Enterprise | Free / Pro / Max |
| Thu hồi | — | Có | Có (unpublish) |
| Google index | Không | Không | Không |

Khi publish: chỉ phiên bản được chọn công khai (chat vẫn riêng tư); người khác có thể **remix** (mở trong Claude của họ và sửa tiếp).

## 5 mẹo tăng ROI

1. **Cụ thể** (quyết định ~60% chất lượng bản đầu)
2. **Mô tả người dùng cuối** (nhân viên mới vs đội kỹ thuật → thiết kế khác)
3. **Từng bước một**, đừng gửi 10 yêu cầu cùng lúc
4. **Yêu cầu tạo artifact** khi Claude trả lời dài trong chat
5. **Nghĩ đến mẫu tái sử dụng** (proposal, tóm tắt họp, kickoff checklist): tạo 1 lần, dùng hàng chục lần

## Ví dụ theo vai trò

| Vai trò | Artifact | Giá trị |
|---|---|---|
| Sales | React: ROI calculator cho khách (nhân sự, chi phí giờ, giờ tiết kiệm → ROI 3 năm, payback, bar chart) | Khách tự tính → chốt số trước cuộc gọi |
| Content marketer | Excel content calendar 3 sheet, conditional formatting theo trạng thái | Dùng lại cả đội, nhiều năm |
| Tài chính | Excel DCF: NPV, IRR, sensitivity 2 chiều, 3 kịch bản | Thay 2 ngày dựng model |
| Pháp lý | HTML checklist GDPR 15 mục, % hoàn thành, xuất PDF | Team tự phục vụ |
| Giáo viên | HTML quiz 10 câu, phản hồi tức thì, làm lại | Học sinh luyện tại nhà, không cần LMS |
| Data analyst | React: upload CSV → chọn trục X/Y → 4 loại biểu đồ → tải PNG | Đồng nghiệp tự phân tích cơ bản |
| HR | HTML onboarding checklist 4 giai đoạn, progress bar, lưu `localStorage` | Onboarding nhất quán |
| Bất động sản | HTML tính thế chấp (+ thuế, bảo hiểm) | Nhúng vào trang listing |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Mỗi lần cần template lại yêu cầu tạo mới | Tạo mẫu 1 lần, lưu và tái sử dụng |
| Nhồi cả "hệ thống CRM" vào 1 artifact | Chia nhỏ, bắt đầu phần lõi |
| Publish nội dung nhạy cảm (số liệu thật) | Chỉ copy/download hoặc chia sẻ nội bộ (link public vẫn có thể bị lộ) |
| Không test artifact | Thử 3-5 kịch bản đầu vào; công thức có thể sai |
| Hard-code giá trị (ngân sách 8 triệu) | Cho nhập/cấu hình qua input |

## Khắc phục sự cố

| Vấn đề | Cách xử lý |
|---|---|
| Không tạo artifact | Yêu cầu rõ: "Tạo dưới dạng artifact" |
| Bố cục lỗi | Phản hồi cụ thể: "Dùng grid auto-fit, min-width 280px trên mobile" |
| Logic sai | Đưa test case: "Nhập 5, mong 25, ra 30. Debug phép tính" |
| Quá lớn / chậm | "Bỏ tính năng X, Y, chỉ giữ luồng cốt lõi" |
| Bản publish không tải | Publish lại, kiểm tra phụ thuộc ngoài bị hỏng |

## Bài tập

1. **Template tái sử dụng (20')**: chọn 1 mẫu (báo cáo tuần, tóm tắt họp, kickoff checklist, proposal...) → tạo artifact → lặp 3-5 lần → tải bản cuối vào thư mục `reusable-claude-artifacts`.
2. **Mini app (20')**: chọn 1 tool (theo dõi ngân sách, tính giá freelancer, ma trận quyết định, habit tracker...) → React component → test 5 kịch bản → chỉnh → publish hoặc lưu local.

## Tóm tắt

- Artifact là sản phẩm tương tác độc lập: documents, code, HTML, SVG, Mermaid, React.
- Claude tự tạo khi nội dung đáng kể và tái sử dụng được; có thể yêu cầu trực tiếp.
- **Lặp từng bước**, mỗi lần một thay đổi.
- 3 cách chia sẻ: copy/download, nội bộ tổ chức, publish công khai (có remix).
- **Mẫu tái sử dụng = ROI cao nhất.**
- Prompt cụ thể + mô tả người dùng cuối + lặp từng bước = artifact tốt ngay lần đầu.
