---
layout: page
title: Bài 09 - Enterprise Search
permalink: /claude-101-lesson-09/
parent: Claude 101
grand_parent: Claude
nav_order: 11
---

# Bài 09: Enterprise Search — Tìm kiếm trên toàn tổ chức

**Module 4 — Mở rộng phạm vi Claude** · ~15 phút · Trung cấp

## Ý chính

Kiến thức tổ chức nằm rải rác (Slack, Gmail, Jira, Notion, Drive, SharePoint) và không ai nắm hết. **Enterprise Search** là một Project dựng sẵn cho cả tổ chức, hiện ở sidebar dưới tên **"Ask [Tên tổ chức]"**: hỏi 1 câu → Claude tìm đồng thời nhiều nguồn → tổng hợp câu trả lời có trích dẫn.

## Khác gì Chat + connectors

| | Chat + connectors | Enterprise Search |
|---|---|---|
| Phạm vi | Chỉ connector bạn tự kết nối | Bộ công cụ toàn tổ chức cấu hình sẵn |
| Instructions | Mặc định / theo project | Tùy chỉnh, tối ưu cho knowledge base công ty |
| Cài đặt | Từng người tự làm | Admin thiết lập 1 lần, mọi người dùng |
| Khám phá | Phải biết dữ liệu ở công cụ nào | Tự tìm nhiều nguồn, không cần chỉ định |
| Tốt nhất cho | Truy vấn nhanh, phạm vi nhỏ | Kiến thức tổ chức, liên phòng ban, dữ liệu phân tán |

Chat + connectors = hộp công cụ cá nhân; Enterprise Search = hộp công cụ cả tổ chức đã sắp xếp.

## 5 loại use case

1. **Catch-up sau vắng mặt:** "Hôm qua tôi vắng, có chuyện gì?", "Quyết định nào được thông qua trong 5 ngày qua?", "Blocker của dự án Platform?"
2. **Chính sách & quy trình:** làm việc từ xa, nộp chi phí, xin nghỉ phép, lưu trữ dữ liệu
3. **Nghiên cứu & phân tích:** lý do khách chọn đối thủ, thảo luận roadmap Q4, xu hướng churn
4. **Onboarding:** hệ thống xác thực hoạt động thế nào, hỏi ai về billing, team dùng công cụ deploy nào, brand voice
5. **Theo dõi dự án:** thảo luận/tài liệu về chiến dịch, đóng góp của team cho sáng kiến Infrastructure, tình trạng Project Osprey

## Cơ chế

1. **Phân tích** câu hỏi (dự án, người, thời gian, chủ đề)
2. **Tìm đồng thời** SharePoint, Slack, Gmail, Drive, Teams...
3. **Lọc theo quyền** của bạn trong công cụ gốc
4. **Tổng hợp** thành câu trả lời mạch lạc
5. **Trích dẫn** nguồn để bạn xác minh

Không sao chép dữ liệu, không có index riêng: tìm trực tiếp, trả về những gì bạn có quyền xem.

## Cài đặt 2 bước

**1. Admin (Owner)** — bật mặc định ở Team/Enterprise nhưng Owner phải setup:

- Sidebar → **Ask Your Org** → **Set up for your org** (hoặc **Disable**)
- Kết nối: **Documents** (bắt buộc: Drive hoặc SharePoint) · **Chat** (bắt buộc: Slack hoặc Teams) · **Email** (khuyến nghị: Gmail hoặc Outlook) · **+ Add more** (Jira, Notion, Confluence...)
- Đặt tên Project (hiện thành "Ask [Tên]"), thêm mô tả → **Finish setup**

**2. Người dùng:** mở project "Ask {Tên tổ chức}" (có dấu sao) → làm theo hướng dẫn đăng ký → **xác thực từng dịch vụ bằng tài khoản cá nhân** → bắt đầu hỏi. Kết nối càng nhiều, kết quả càng đầy đủ; thêm sau qua *Connect* trong Instructions của Project.

## 4 mẫu câu hỏi hiệu quả

| Mẫu | Ví dụ |
|---|---|
| **Giới hạn thời gian** | "Điều gì được quyết định về giá trong 2 tuần qua?" |
| **Gợi ý nguồn** | "Xem kênh #product trên Slack về tính năng X", "Tìm trong thư mục Legal trên SharePoint về GDPR" |
| **Thực thể cụ thể** | "Tình trạng Project Osprey ở mọi team", "Jane Doe làm gì trong tháng này?" |
| **Yêu cầu tổng hợp** | "Tổng hợp phản hồi khách về onboarding, nêu 3 pain point hàng đầu + trích dẫn tiêu biểu" |

## Ví dụ: PM mới vào team Platform

5 câu hỏi ngày đầu: (1) ai trong team, đang làm gì → (2) các dự án đang chạy + tình trạng → (3) quyết định quan trọng 30 ngày qua → (4) hỏi ai về auth/DB migration/API design → (5) blocker hiện tại theo mức nghiêm trọng. **~15 phút so với ~2 ngày nghiên cứu thủ công.**

## Ví dụ theo vai trò

| Vai trò | Câu hỏi mẫu |
|---|---|
| Sales leadership | Deal chốt tuần này và lý do thắng; ARR pipeline theo stage + rủi ro lớn nhất; competitive intel về [Đối thủ X] |
| Marketing | Hiệu quả chiến dịch tháng này; tiến độ nội dung vs kế hoạch; chủ đề feedback từ support/social/review |
| Tài chính/Ops | Cảnh báo chi phí vượt; hợp đồng NCC hết hạn trong 60 ngày; tình trạng tuyển dụng |
| Pháp lý | Vấn đề tuân thủ 30 ngày qua; hàng đợi review hợp đồng; cập nhật chính sách cần thông báo |
| Product/Eng | Feature đã ship + trạng thái rollout; sự cố tuần qua; yêu cầu tính năng hàng đầu |
| Customer Success | Tài khoản rủi ro (health score giảm, ticket tăng); success story cho case study; câu hỏi onboarding thường gặp |
| HR | Chủ đề feedback từ 1:1; tỷ lệ hoàn thành đào tạo; tình trạng vị trí mở |

## Bảo mật & quyền riêng tư

1. **Chỉ thấy những gì bạn đã xem được** (không có email CEO, không có kênh Slack riêng tư chưa được mời).
2. **Chat riêng tư** với bạn, không chia sẻ với tổ chức mặc định.
3. **Không index / lưu trữ dữ liệu riêng**, tìm trực tiếp qua công cụ đã kết nối.
4. **Admin có thể:** tắt hẳn, giới hạn connector, cấu hình retention, audit log sử dụng.

**Tuân thủ:** tùy gói Team/Enterprise mà mức tuân thủ khác nhau. Với dữ liệu quy định (y tế, tài chính, PII) → xác nhận với security/legal và sales của Anthropic (BAA, DPA, data residency, retention) trước khi kết nối công cụ nhạy cảm. Đừng giả định.

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Coi như "Google nội bộ" (gõ từ khóa, chờ link) | Hỏi bằng ngôn ngữ tự nhiên; yêu cầu tổng hợp/so sánh |
| Không kiểm tra nguồn | Với quyết định quan trọng, mở trích dẫn và xác minh |
| Tìm chủ đề nhạy cảm (lương, M&A, nhân sự) | Dùng kênh phù hợp, hệ thống có kiểm soát truy cập (truy vấn bị ghi log, admin xem được) |
| Tin thông tin cũ là mới | Kiểm tra ngày nguồn; yêu cầu Claude đánh dấu nội dung > 6 tháng |
| Bỏ cuộc sau câu trả lời đầu | Hỏi tiếp: "Tôi bỏ sót gì về phản ứng của khách?", "Đào sâu mối lo của team kỹ thuật" |
| Thay cuộc trò chuyện với đồng nghiệp bằng tra cứu | Dùng để nghiên cứu nền; vẫn trao đổi trực tiếp |

## Bài tập

1. **Xác thực (5'):** kiểm tra sidebar có "Ask [Tên tổ chức]" không → làm hướng dẫn → xác thực các connector → thử "Team mình dùng công cụ nào để quản lý dự án?".
2. **Câu hỏi định hướng thứ Hai (10'):** "Tóm tắt cập nhật quan trọng tuần qua toàn công ty: quyết định sản phẩm, thành công/lo ngại của khách, thông báo team, ưu tiên sắp tới." Ghi lại: thời gian phản hồi, độ phủ, thiếu gì, câu hỏi tiếp theo; lặp lại tuần sau.
3. **Tài liệu onboarding cá nhân (15'):** hỏi 5-7 câu (ai làm gì, ưu tiên công ty, feedback khách gần đây, công cụ/quy trình) → lưu chat làm tài liệu tham khảo cho đồng nghiệp mới.

## Tóm tắt

- **Enterprise Search = Project dựng sẵn cho tổ chức** ("Ask [Your Org]"), tổng hợp kiến thức nhiều nguồn.
- **Setup 2 bước:** admin cấu hình, người dùng tự xác thực; quyền theo công cụ gốc.
- Mạnh nhất với câu hỏi **nhiều nguồn**: catch-up, chính sách, nghiên cứu, onboarding, theo dõi dự án.
- **4 mẫu prompt:** thời gian, nguồn, thực thể, tổng hợp.
- **Bảo mật:** Claude thấy những gì bạn thấy; chat riêng tư; không index riêng.
- Rút ngắn onboarding từ tuần xuống ngày; chuyển phòng ban cũng dùng được từ ngày đầu.
