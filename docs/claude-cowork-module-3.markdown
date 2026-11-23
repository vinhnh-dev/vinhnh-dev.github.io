---
layout: page
title: Module 3 - Ở mọi nơi, chia sẻ và an toàn
permalink: /claude-cowork-module-3/
parent: Claude Cowork
grand_parent: Claude
nav_order: 3
---

# Module 3: Use Claude wherever you work + Sharing and safety (bài 9-14)

Gồm hai phần gốc: *Use Claude wherever you work* (bài 9-10) và *Sharing and safety in Claude Cowork* (bài 11-14).

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

---

## Bài 11: Thực hành làm việc an toàn (~8 phút)

Từ bài 2 bạn biết Claude luôn hỏi trước khi xóa, và (ở chế độ mặc định) trước khi gửi/chia sẻ. Đó là mức sàn. Bài này là phần bạn bổ sung để công cụ tự chủ có quyền file/app/connector không làm điều ngoài ý muốn.

**Thiết lập để lỗi không chạm vào thứ quan trọng**
- **Thư mục chuyên dụng, không phải "thùng rác chung":** trỏ vào Documents/Downloads/Desktop khác nào để đồng nghiệp mới lục mọi file. Tạo thư mục cho công việc, chuyển/copy vào đủ thứ cần, trỏ Claude vào đó.
- **Sao lưu thứ không thay thế được** (deliverable khách cũ, hợp đồng không thể cấp lại): giữ một bản ở nơi Cowork không với tới (cloud backup, thư mục khác, ổ không kết nối). Claude không xóa mà không hỏi, nhưng bấm nhầm xác nhận thì cái giá là chính file đó.
- **Thử workflow mới trên bản copy trước:** vd task định kỳ thứ Sáu hàng tuần thì lần chạy đầu trên bản sao dữ liệu; thấy ổn rồi mới trỏ vào thư mục thật.

**Viết prompt không chừa chỗ cho hành động sai**
- **Cụ thể với động từ phá hủy:** "cắt phần này" có thể là "bỏ khỏi bản xem" hoặc "xóa khỏi file"; "cập nhật file" có thể là "viết lại" hoặc "thêm vào". Nếu hiểu sai không khôi phục được thì nói rõ: "Bỏ phần này khỏi bản nháp nhưng giữ nguyên file", "Thêm phụ lục mới; không viết lại các phần hiện có".
- **Nêu phạm vi:** "Chỉ 3 file mới cập nhật nhất", "Chỉ hợp đồng chốt ở Q3", "Đừng nhắn ai, chỉ soạn nháp".
- **Scheduled task giai đoạn đầu chỉ soạn nháp:** chúng chạy khi bạn không nhìn; đến khi tin tưởng thì mới để gửi thay bạn.

**Ba lần kiểm tra ngay lúc làm**
1. **Đọc plan** (trong tab Progress): hợp lý không? thứ tự đúng không? nguồn đúng không? Chỉnh nếu cần.
2. **Để ý mẫu bất thường:** không cần xác thực mọi lệnh, nhưng nếu Claude đụng vào file/site bạn không nhắc, hoặc phạm vi phình ra, thì dừng task. "Có gì đó sai sai" là tín hiệu thật.
3. **Duyệt xác nhận có chủ đích:** giữ *Ask before acting* với mọi thứ gửi/đăng/chia sẻ, và khi hộp xác nhận hiện ra thì đọc. Phần lớn sai sót không phải do chốt chặn hỏng mà do bấm qua một xác nhận không đúng ý.

**Khi Cowork không phù hợp:** (1) quy trình bị quản lý cần audit trail (hoạt động Cowork không nằm trong audit log, Compliance API hay data export); (2) việc bạn không giao cho một đồng nghiệp giỏi mà không giám sát (gửi tài liệu pháp lý cho đối tác, đăng thông báo công khai, đẩy thay đổi hướng khách hàng): Claude chuẩn bị, bạn phát hành; (3) dữ liệu cá nhân rất nhạy cảm ngoài phạm vi IT đã duyệt rõ ràng. Tham khảo thêm *Use Claude Cowork safely*.

**Bài thực hành "tìm 5 điểm nguy hiểm":** task dọn file khách quý trước (lưu trữ + đăng ghi chú Slack cho mỗi khách), chạy mỗi thứ Sáu 5h chiều, trỏ vào `~/Documents/Work`, connector gồm M365, Asana, Claude in Chrome. Nhìn connector, thư mục, từ ngữ prompt và lịch chạy để thấy chỗ cần dừng. Tự hỏi: sẽ trỏ vào thư mục nào, có gì bên trong không nên chạm tới hoặc cần sao lưu trước? động từ phá hủy nào cần cụ thể hơn?

## Bài 12: Kiểm chứng skill cho plugin (~8 phút)

**Vì sao:** khi xây skill hoặc gói thành plugin, bạn đang xây một sản phẩm nhỏ cho người khác dùng. Khi tự dùng bạn biết cách né lỗi (hỏi gì, đưa file nào, đáp án trông thế nào); đồng nghiệp thì không: họ diễn đạt khác, đưa đầu vào hơi khác, hoặc gặp **edge case** (tình huống hiếm nhưng thật, như yêu cầu nằm ngay ngoài phạm vi thiết kế). Skill hay vấp ở đó và người dùng không biết vì sao. **Eval** (đánh giá) là cách bắt lỗi trước người khác: một lần thử thực tế (yêu cầu thật vào → xem kết quả → bảo Claude sửa gì), không cần code hay script test, chỉ cần phán đoán của bạn có đủ tốt để đặt tên mình lên không.

**Cách hệ thống eval hoạt động** (khi xây skill bằng **skill-creator**, trợ lý dựng skill có sẵn của Claude):
1. skill-creator đề xuất ≥ 2 prompt thực tế mà ai đó có thể dùng với skill.
2. Với mỗi prompt tạo **một cặp đầu ra: có skill** và **không có skill** (cùng prompt): để thấy skill thực sự tạo khác biệt gì, không chỉ "đầu ra có ổn không" mà "có tốt hơn Claude tự làm không".
3. Bạn xem từng cặp và phản hồi bằng tiếng thường trên trang review, trả lời hai câu: **bản có skill có phải bản tôi sẽ dùng không** (nếu có, ghi lại điều làm nó tốt hơn); **nếu không, thiếu/sai gì** (cụ thể: "giọng quá trang trọng", "bỏ qua phần executive summary" tốt hơn "chưa đúng ý").
4. Gửi phản hồi → Claude sửa skill (viết lại instructions, chỉnh ví dụ, siết yêu cầu).

**Lặp lại:** phản hồi của bạn chính là cách sửa. **Mỗi lần sửa một thứ** (vd vòng đầu thấy skill dài dòng và thiếu một mục, chọn cái quan trọng hơn, sửa, chạy lại) để biết cái gì thực sự có tác dụng. Đây là vòng lặp, không phải cổng một lần; đa số skill xong sau 1-2 vòng. **Ngưỡng ship** (cho bạn hay đồng nghiệp) không phải eval hoàn hảo, mà là các case bạn quan tâm cho kết quả tốt hơn baseline rõ rệt và bạn đã nêu tên các case chưa xử lý được. Nếu lần đầu đầu ra đã tốt thì xong; eval là khi cần tự tin, không phải nghi thức.

**Bài đọc mẫu (recap họp product sync):** với skill đạt 4/4 house rule (mở đầu bằng quyết định; mỗi action item có owner và ngày; có mục câu hỏi mở; dưới 150 từ), không skill chỉ 1/4 (văn xuôi dài, không owner/ngày, quyết định lẫn trong đoạn văn). Việc của bạn: chọn bản bạn thực sự gửi và viết một dòng phản hồi cho Claude.

## Bài 13: Chia sẻ thứ bạn xây với team (~7 phút)

Khi vài skill của team đã chứng tỏ giá trị (bắt đầu là cách làm cá nhân, qua eval, đúng với nhiều cách dùng), bạn gói thành plugin (bài 8). Bài này nói về bước tiếp theo: đưa plugin tới mọi người.

**Phân phối trong tổ chức:** cách khuyến nghị là qua **private marketplace** của tổ chức (danh mục plugin được công ty duyệt, do admin quản lý). Thực tế là một cuộc bàn giao: bạn đưa plugin cho người sở hữu marketplace (team lead, enablement/operations, hoặc IT) và họ publish. Họ chọn mức hiển thị:

| Mức | Ý nghĩa |
|---|---|
| **Available** | Có trong Directory công ty, ai muốn thì cài. Phù hợp phần lớn plugin team |
| **Installed by default** | Có sẵn khi mở Cowork; người dùng tắt được |
| **Required** | Cài sẵn và luôn bật: dành cho việc như kiểm tra tuân thủ mọi người phải chạy giống nhau |
| **Hidden** | Có trong marketplace nhưng không hiện trong Directory, để staging hoặc rollout giới hạn |

Đồng nghiệp thấy plugin trong Directory gắn nhãn của công ty, cạnh plugin công khai của Anthropic; dùng và tắt được (trừ Required) nhưng **không sửa được**; bản cập nhật đến từ người bảo trì. Tùy tổ chức: có nơi đã chạy marketplace và có chủ rõ ràng (chỉ cần bàn giao); có nơi chưa bật marketplace (cần tìm hiểu/đề xuất); nếu bạn là admin thì bạn là người thiết lập.

**Thói quen giữ plugin khỏe:**
- **Một chủ sở hữu có tên** cho mỗi plugin chia sẻ: duyệt thay đổi, chạy eval sau mỗi lần sửa, quyết định khi nào cập nhật hay nghỉ hưu.
- **Eval trước mỗi lần publish:** vòng eval là cổng; nếu các case quan trọng không giữ được sau thay đổi thì đừng đẩy tới mọi người.
- **Đặt tên cụ thể:** `meeting-prep` có thể trùng 3 skill khác trong toàn tổ chức; `sales-customer-renewal-prep` thì không.
- **Rà soát định kỳ** (gợi ý hằng quý): cái gì đã cài, cái gì thực sự được dùng, cái gì lỗi thời; nghỉ hưu cái không ai chạy, sửa cái người dùng đã chỉ ra cơ hội cải thiện.

**Tự nhìn lại:** 2-3 skill bạn và đồng đội dùng nhiều nhất là ứng viên gói thành plugin của team? Ai trong tổ chức là người bạn sẽ mang plugin đến để lên marketplace? Nếu chưa biết, đó là bước tiếp theo thật sự.

## Bài 14: Tổng kết và bước tiếp theo (~5 phút)

| Module | Bạn có gì |
|---|---|
| **1. Meet Claude Cowork** | Cowork là gì, khác Chat/Code thế nào, loại việc nó làm, task thật đầu tiên |
| **2. Make it yours** | Global instructions, projects, skills, plugins: Cowork mạnh hơn khi biết điều bạn biết, truy cập thứ bạn truy cập, làm theo cách team làm |
| **3. Use Claude wherever you work** | Cowork trong browser (Claude in Chrome) và trong Word/Excel/PowerPoint/Outlook (Claude in M365) |
| **4. Sharing and safety** | Làm việc tự chủ có trách nhiệm, kiểm chứng thứ bạn xây, biến workflow cá nhân thành hạ tầng team |

**Mạch xuyên suốt:** Cowork vượt ra ngoài chat để bạn *giao việc thật*; mọi thứ khác trong khóa xây trên nền đó.

**Chọn một việc để làm tuần này:**
1. **Thiết lập memory:** viết global instructions 5 câu, hoặc tạo project cho một mạch việc bạn đang làm.
2. **Lên lịch một task hoặc chạy Cowork trên cloud:** đưa một deliverable định kỳ lên lịch; hoặc dùng cloud (beta, gói đủ điều kiện) để lần sau nghĩ ra việc ngoài giờ có thể giao từ điện thoại.
3. **Cài một plugin:** *Customize → Plugins*, cài plugin gần vai trò của bạn nhất, chạy với việc của tuần này.
4. **Thử Claude in Chrome hoặc M365:** chọn bề mặt khớp nơi phần lớn công việc thật của bạn diễn ra, cài và dùng cho một task.
5. **Chia sẻ thứ bạn xây:** một use case có giá trị, một workflow ra kết quả tốt, hoặc một skill bạn đã làm: có thể là thứ giúp đồng đội thoải mái hơn khi làm việc với Claude.
