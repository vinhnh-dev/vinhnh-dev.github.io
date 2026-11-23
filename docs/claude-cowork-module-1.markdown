---
layout: page
title: Module 1 - Làm quen với Cowork
permalink: /claude-cowork-module-1/
parent: Claude Cowork
grand_parent: Claude
nav_order: 1
---

# Module 1: Làm quen với Claude Cowork (bài 1-4)

## Bài 1: Cowork là gì? (~8 phút)

**Cowork** là một chế độ của Claude trong app desktop, nơi Claude làm việc **cùng bạn trên môi trường của bạn**: file trên máy, app đã kết nối (Gmail, Slack, Google Drive, lịch), trình duyệt. Bạn trỏ Claude vào một thư mục, kết nối app, mô tả kết quả mong muốn; Claude lập kế hoạch, thực hiện từng bước, lưu **sản phẩm thật** về thư mục. Cũng có thể chạy Cowork **trên cloud** (beta, một số gói) để giao/kiểm tra việc từ web và app mobile.

**Tư duy then chốt: Cowork là để *giao việc* (delegate), không chỉ trò chuyện.** Chat vẫn là nơi suy nghĩ thành tiếng, soạn nháp, hỏi đáp. Cowork thêm khả năng giao nguyên một đầu việc (thu thập ngữ cảnh, phân tích, tạo file, dùng công cụ) và nhận lại kết quả hoàn chỉnh. Đừng dùng theo phản xạ "gõ câu hỏi → hỏi tiếp"; hãy dùng cho việc bạn *thường tự làm*, không phải việc bạn *thường hỏi*.

**Bạn luôn kiểm soát:** Claude cho xem kế hoạch trước khi làm, mặc định hỏi trước các hành động quan trọng (gửi, xóa, chia sẻ), và bạn có thể điều hướng bất kỳ lúc nào.

**Cowork hoạt động trong môi trường của bạn:**
- **Trên file của bạn:** đọc thư mục bạn chỉ định, ghi kết quả về đó.
- **Trong app của bạn:** lấy ngữ cảnh từ email, lịch, chat, drive, CRM đã kết nối.
- **Trong trình duyệt:** với web tool không có connector, Claude in Chrome đọc/thao tác trên trang (dashboard, portal, trang cần đăng nhập).
- **Với công cụ của bạn:** hành động thật, không chỉ mô tả cách làm.

**Việc hợp với Cowork:** nhiều bước, nhiều công cụ, file thật, kết thúc bằng sản phẩm thật. Ví dụ prompt: *"Chúng ta đã quyết định gì về giá quý trước? Xem ghi chú họp trong thư mục Q3 Sales, email của team sales, Slack lãnh đạo và các deck đề xuất, tổng hợp thành bản tóm tắt 1 trang cho ban điều hành."*

| Chế độ | Dùng để | Phù hợp |
|---|---|---|
| **Chat** | **Suy nghĩ cùng** Claude (hội thoại từng lượt) | Hỏi đáp, brainstorm, soạn nháp, việc vừa trong một phản hồi |
| **Cowork** | **Giao việc** cho Claude (workspace) | Việc nhiều bước qua file và công cụ, ra tài liệu/deck/sheet thật, việc muốn giao rồi quay lại sau |
| **Code** | **Xây phần mềm** cùng Claude | Làm việc trong repo, refactor nhiều file nguồn, việc của developer |

Ví dụ: "đọc 5 PDF nhà cung cấp trong Downloads, so sánh giá và SLA, đưa vào bảng tính" → **Cowork** (file local, nhiều bước, có sản phẩm, vượt khả năng của Chat). Phần lớn người làm tri thức sẽ sống chủ yếu ở Chat và Cowork cho hai mục đích khác nhau.

**Tự nhìn lại:** nghĩ về một việc gần đây mà giá trị nằm ở *việc làm*, không chỉ *việc nghĩ*; giữ lại cho bài 3 và 4.

## Bài 2: Cài đặt Cowork (~8 phút)

- **Cài:** app Claude Desktop (Mac, Windows) từ `claude.com/download` → đăng nhập → chọn **Cowork**. Không thấy Cowork: có thể cần gói trả phí hoặc cập nhật app. Cloud: chạy từ web và app mobile (beta).
- **Chọn thư mục:** bấm *Work in a project or folder* ở thanh prompt. Đây là lựa chọn quan trọng nhất cho mỗi task vì thư mục là nơi công việc diễn ra: Claude **đọc mọi file bên trong và ghi kết quả về đó**. Chọn thư mục nhỏ nhất đủ chứa thứ task cần (không phải cả Documents); có thể thêm thư mục sau. Khác Chat ở chỗ Chat đọc được file bạn upload nhưng không lưu gì về máy; Cowork thì có.
- Thư mục nên có sẵn ngữ cảnh: tài liệu nguồn, tài liệu liên quan, template.
- **File cloud khác biệt:** nhiều connector mặc định (Google Drive, M365) chỉ **đọc và tìm kiếm**, một số có thể tạo/sửa (xem mô tả connector khi bật). Phiên cloud chỉ chạm tới connector, không chạm thư mục trên máy: việc cần file local thì bắt đầu từ desktop.

**Connectors:** cầu nối tới app nơi công việc diễn ra, cài một lần ở *Customize*, bật/tắt theo từng task. Thường cài trước: email + lịch (Outlook/Gmail), chat (Slack/Teams), lưu trữ (SharePoint/OneDrive/Google Drive/Box), CRM và công cụ dự án (Notion, Salesforce, HubSpot, Asana, Linear). Sau khi bật, chỉ cần nói tự nhiên: "xem team nói gì trên Slack về launch". Web tool không có connector (dashboard nội bộ, portal vendor) → **Claude in Chrome** (module 3; có thể không có trên một số gói enterprise).

**Mô hình quyền:** chọn thư mục = cấp quyền đọc/ghi. Hai chế độ cho từng task:

| Chế độ | Hành vi | Dùng khi |
|---|---|---|
| **Ask before acting** (mặc định) | Claude dừng chờ bạn duyệt mỗi hành động chạm bên ngoài (gửi email, đăng tin, chia sẻ file) | Công cụ/file mới, cần theo dõi sát |
| **Act without asking** | Không dừng, nhanh hơn nhưng rủi ro hơn | Bạn đang giám sát, file/site đáng tin |

Ở **cả hai chế độ**: Claude **luôn hỏi trước khi xóa vĩnh viễn file** (không thể bỏ qua). Bạn cũng kiểm soát connector/MCP nào Claude được dùng và tần suất hỏi quyền; admin có thể tắt web search; bạn giới hạn site Claude in Chrome được vào.

**Thử ngay:** (1) trỏ vào thư mục có việc thật (hoặc bản copy), (2) kết nối một app (thường email và cloud storage), (3) nhờ: *"Xem mọi thứ trong thư mục này và viết brief: có gì, các tài liệu liên quan nhau thế nào, điều bất ngờ, thông tin đáng chia sẻ"*, (4) đối chiếu brief với hiểu biết của bạn: Claude bắt được gì, bỏ sót gì.

## Bài 3: Cowork làm được gì (~11 phút)

**3 mẫu việc hợp với Cowork**
1. **Nhiều bước:** một prompt, Claude thu thập → so sánh → nghiên cứu thêm → soạn → định dạng. Ví dụ: phân loại một tuần email phản hồi khách thành chủ đề kèm trích dẫn; lấy số liệu từ 3 báo cáo + bảng tính dựng thành dashboard tương tác. Bạn thấy **panel Progress** chia các bước (đọc nguồn, so sánh, soạn, định dạng, tự review).
2. **File thật trên máy:** đầu ra là tài liệu thật (Word, bảng tính, deck, PDF) đúng định dạng, đầu vào cũng là file có sẵn mà Claude đọc/sửa/lưu lại. Chat tạo tài liệu mới; Cowork làm việc *trên file bạn đang có*. Ví dụ: đề xuất khách hàng từ template + ghi chú họp; báo cáo chỉ số tháng từ bảng tính thô kèm biểu đồ.
3. **Nhiều công cụ:** Claude lên kế hoạch xuyên các công cụ và chạy cả chuỗi như một lần giao việc (Chat cũng kết nối công cụ, nhưng bạn tự ghép từng bước). Ví dụ: soạn email follow-up trong Outlook từ lịch + danh sách tham dự + ghi chú; tìm mọi thảo luận Slack về launch và tổng hợp.

**Scheduled tasks:** thiết lập một lần, Claude chạy theo lịch. Hai cách: gõ `/schedule`, mô tả việc + tần suất, duyệt prompt Claude soạn; hoặc (tự nhiên hơn) **làm task một lần, xác nhận kết quả ổn, rồi `/schedule`** để biến đúng quy trình đó thành định kỳ (hourly, daily, weekdays, manual).
- Chạy ở đâu phụ thuộc nơi Cowork chạy: **cloud** → chạy trên cloud đúng giờ, không cần bật máy, dùng connector cloud; **desktop** → chạy khi máy bật và Claude đang chạy (máy tắt/ngủ thì chạy bù khi bạn quay lại và báo trễ). Task giữ nguyên nơi nó được tạo.
- Gợi ý lịch: tổng kết chiều thứ Sáu (đã ship gì, quyết định lớn trên Slack/Teams, xem trước tuần sau); roll-up chỉ số ngày 1 hàng tháng; briefing sáng cho các cuộc họp (dựa trên ghi chú cũ và email).

**Cowork trên cloud (beta):** dùng Cowork trên web và app Claude mobile: giao việc từ điện thoại, xem tiến độ từ trình duyệt bất kỳ, nhận sản phẩm ở bàn làm việc; không cần để máy bật. Hai lưu ý: (1) phiên cloud chỉ với tới connector, không tới thư mục trên máy; (2) **nơi bắt đầu là nơi chạy** (bắt đầu từ web/mobile → chạy cloud; từ desktop → chạy trên desktop và cần máy bật); muốn đổi nơi, tạo phiên mới. Ví dụ: trên đường đi làm, nhờ Cowork lấy ticket quý này từ công cụ hỗ trợ và soạn bản 1 trang; hoặc ở offsite, nhờ so sánh điều khoản hợp đồng vendor mới với hợp đồng năm trước trong Drive. Tùy gói và tổ chức.

**Tóm lại:** nhiều bước / file thật / nhiều công cụ cho biết *việc gì đáng giao*; scheduled task và cloud cho biết *khi nào và ở đâu* giao. Phán đoán vẫn ở bạn: Claude lên kế hoạch, thực hiện, hỏi; bạn quyết định gửi, chia sẻ, phát hành gì.

**Tự nhìn lại:** chọn một việc tuần này khớp ≥ 1 mẫu và ghi lại để giao ở bài 4.

## Bài 4: Giao task đầu tiên (~15 phút)

**Điểm chính:** Claude **hỏi trước khi cam kết** (câu hỏi làm rõ là cách lấp khoảng trống ngữ cảnh, không phải rào cản); bạn **điều hướng giữa chừng** được; **sản phẩm cuối là file**, không phải đoạn chat; bạn review như review việc của đồng nghiệp.

**Prompt giao việc tốt gồm 3 phần:**
1. **Nêu sản phẩm** ("brief 1 trang", "slide cho QBR", "danh sách xếp hạng kèm ghi chú"): chỉ rõ định dạng và độ dài.
2. **Nêu đầu vào:** thư mục nào, kênh nào, khoảng thời gian, ứng dụng nào.
3. **Nêu sắc thái/ngữ cảnh Claude không đoán được** (vd "muốn thấy kịch bản base/best/worst, tính cả 3 cửa hàng mở ở Q3 năm ngoái"; "cho ban điều hành tại offsite, giúp quyết định về gói giá mới; mở đầu bằng khuyến nghị; đánh dấu thứ không xác minh được").

Việc này tốn công hơn lúc đầu so với chat, nhưng thay vì 5 vòng "à mà thêm cái này nữa", phần trao đổi diễn ra trước khi Claude bắt đầu. Bỏ trống phần nào thì Claude thường sẽ hỏi lại.

**Trả lời câu hỏi làm rõ:** phần lớn là lựa chọn nhanh (vd "thấy 2 memo `Q2-board-memo.docx` và `Q2-board-memo-FINAL.docx`, khớp theo bản nào?"); không có lựa chọn nào hợp thì trả lời bằng lời của bạn hoặc bỏ qua.

**Điều hướng giữa chừng:** xem plan và tiến độ; nếu sai nguồn, định dạng, giọng điệu thì **ngắt ngay** (hoặc xếp hàng tin nhắn để Claude chỉnh kế hoạch khi đang chạy), đừng đợi xong rồi regenerate như thói quen ở Chat. Nếu lệch nhiều, dừng, sửa prompt và chạy lại.

**Review sản phẩm cuối** (xem preview trong app, file nằm trong thư mục): kiểm tra (1) có đúng mục tiêu thật hay lệch tinh vi? (2) sự kiện/số liệu chính xác không (nhờ Claude liệt kê tài liệu đã dùng rồi tự đối chiếu)? (3) có chi tiết nghe như bịa (ngày, tên, trích dẫn không truy ra được từ đầu vào) không? Bản nháp gần đúng → nói rõ cần sửa gì thay vì làm lại; sai ở điểm cốt lõi → prompt thiếu ngữ cảnh then chốt, hãy bổ sung ngữ cảnh đó.

**Thử ngay:** giao task đã chọn ở bài 3: nêu sản phẩm, đầu vào, sắc thái; trả lời câu hỏi cụ thể; điều hướng nếu lệch; review với tinh thần phản biện. Lần chạy đầu dạy bạn nhiều nhất.
