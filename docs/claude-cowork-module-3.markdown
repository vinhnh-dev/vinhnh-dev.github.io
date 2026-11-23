---
layout: page
title: Module 3 - Claude ở mọi nơi bạn làm việc
permalink: /claude-cowork-module-3/
parent: Claude Cowork
grand_parent: Claude
nav_order: 3
---

# Module 3: Use Claude wherever you work (bài 9-10)

## Bài 9: Claude in Chrome (~10 phút)

**Ý chính:** Claude in Chrome là **cầu nối cho công cụ không có connector**: mọi thứ chạy trong trình duyệt, Claude đọc và thao tác được. Chrome và Cowork phối hợp: Claude thu thập thông tin/hành động trong browser, rồi lấy kết quả dựng sản phẩm trong Cowork (một hội thoại, hai bề mặt). Bạn vẫn kiểm soát: mặc định Claude in Chrome hỏi trước hành động nhạy cảm và bạn duyệt/từ chối từng cái.

**Mở khóa được gì:**
- **Dashboard nội bộ:** view Tableau của tài chính, dashboard Looker của sales ops, BI tool có chỉ số bạn xem mỗi thứ Hai: Claude lấy số, tải về máy, dùng làm ngữ cảnh cho task Cowork.
- **Portal vendor/hệ thống khách hàng:** portal mua hàng không có API, CRM sau SSO, công cụ support cần triage từng ticket.
- **Web app sau đăng nhập:** kể cả tool team tự làm (vd "mở hệ thống mua hàng, tìm mọi PO của 10 nhà cung cấp lớn nhất ở Q3, đưa line item vào bảng tính").
- **Research web ra sản phẩm:** mở 10 tab, lấy nội dung từng tab, ra brief mà không copy-paste.

Mẫu nhận biết: khi bạn nghĩ "muốn đưa ngữ cảnh này cho Claude nhưng nó nằm trên web", đáp án là Claude in Chrome.

**Dùng cùng Cowork:** ví dụ dashboard sức khỏe khách hàng nằm sau đăng nhập, không có connector, cần bản tóm tắt 1 trang mọi tài khoản vàng/đỏ trước thứ Sáu. Prompt trong Cowork: *"Mở dashboard customer health trong Chrome, lấy mọi tài khoản vàng hoặc đỏ; với mỗi cái lấy hoạt động 30 ngày qua từ thư mục khách trong Drive và các thread gần đây trong #customer-success trên Slack; dựng bản tóm tắt 1 trang để tôi xem trước cuộc gọi thứ Sáu."* Claude giao phần browser cho Claude in Chrome, nhận dữ liệu về, lấy ngữ cảnh từ Drive và Slack, dựng file (vd `at-risk-accounts-summary.docx`) trong thư mục của bạn: một lần giao việc, ba nguồn ngữ cảnh.

**Lưu ý:**
- **Phải đã đăng nhập:** Claude không đăng nhập thay bạn; đăng nhập một lần trong browser, Claude làm việc trong phiên đó.
- **Cẩn trọng với quyền trên web:** giống connector, Claude thấy những gì bạn thấy, nhưng trên web mở điều đó gồm mọi thứ bạn truy cập được; với site nhạy cảm, thu hẹp phạm vi Claude được thao tác và xem lại hành động trước khi duyệt.
- Hướng dẫn mới nhất: *Get started with Claude in Chrome*.

**Thử ngay:** chọn một việc nằm trong công cụ web không có connector; mô tả trong Cowork, để Claude làm trong Chrome, rồi bàn giao insight về Cowork.

## Bài 10: Claude for Microsoft 365 (~5 phút)

**Ý chính:** Claude là **add-in nằm trong Word, Excel, PowerPoint, Outlook**, làm việc trên chính file/không gian bạn đang mở. Một hội thoại mang ngữ cảnh qua các app. Claude trong Cowork và trong M365 là hai công cụ cho hai khoảnh khắc: Cowork dựng sản phẩm hoàn chỉnh từ nhiều nguồn; M365 dùng để tinh chỉnh, debug, định hình file đang làm và mang ngữ cảnh từ file đó sang tài liệu khác.

| App | Claude làm được | Ví dụ "nước đi mạnh nhất" |
|---|---|---|
| **Excel** | Phân tích dữ liệu, viết công thức, debug `#REF!` và tham chiếu vòng, thử kịch bản không phá model, dựng sheet từ template, dẫn về ô cụ thể | "Lấy số thực tế từ sheet Q3, so với kế hoạch Q3 trong cùng workbook, viết nhận xét phương sai ở cột F cạnh mỗi dòng" |
| **PowerPoint** | Đọc slide master, font, bảng màu rồi dựng slide khớp; tạo biểu đồ native, sửa được (không phải ảnh dán); làm trên slide đang chọn | "Lấy phân tích vừa làm trong Excel thành deck 3 slide cho QBR theo template của chúng ta" |
| **Word** | Soạn, sửa, định dạng tại chỗ; làm việc với comment và tracked changes; lấy ngữ cảnh từ nguồn đã kết nối để soạn có căn cứ | "Soạn executive summary dựa trên thân memo và dữ liệu nguồn nêu ở phụ lục" |
| **Outlook** | Triage mail với ngữ cảnh từ phần còn lại của công việc; soạn trả lời phản ánh thread cũ, lịch, quyết định gần đây | |

**Chuyển việc giữa các app trong một hội thoại:** Outlook → Word (brief của khách đến inbox: "mở brief trong Word, bắt đầu memo từ template công ty"; thread email đã nằm trong sidebar) → Excel ("dựng model quy mô thị trường cho option 2"; lấy giả định từ brief, ra model nhiều tab có công thức) → PowerPoint ("làm deck cho ban chỉ đạo theo template khách"; biểu đồ native với số mới từ Excel) → quay lại Outlook ("tìm 30 phút với team trước thứ Năm"; lời mời soạn sẵn chờ bạn bấm Send).

**Khi nào dùng cái nào:** Cowork khi việc kéo từ nhiều nguồn và kết thúc bằng sản phẩm (dựng brief từ 20 file nguồn, báo cáo từ Salesforce + 3 kênh Slack, workflow theo lịch). Claude trong M365 khi bạn làm *trong chính file Office* (sửa tại chỗ, mang ngữ cảnh app này sang app khác). Thực tế dùng cả hai: Claude dựng deck nháp, bạn mở trong PowerPoint tinh chỉnh với Claude; Claude kéo dữ liệu vào model, bạn mở trong Excel hỏi về outlier. Hai bề mặt bàn giao mượt, file vẫn là file.

**Thử ngay:** cài add-in Claude cho app M365 bạn dùng nhiều nhất (xem *Work across Excel, PowerPoint, and Word*; Claude for Outlook là beta riêng; nếu IT quản lý add-in thì hỏi IT); mở tài liệu thật tuần này, thử một thao tác (sửa một đoạn, debug công thức, dựng slide từ một đoạn Word).
