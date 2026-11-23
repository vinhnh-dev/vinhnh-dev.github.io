---
layout: page
title: Module 2 - Biến Cowork thành của bạn
permalink: /claude-cowork-module-2/
parent: Claude Cowork
grand_parent: Claude
nav_order: 2
---

# Module 2: Make Claude Cowork yours (bài 5-8)

## Bài 5: Cowork tốt hơn theo thời gian (~5 phút)

Càng biết nhiều về bạn, công việc và cách team làm, sản phẩm lần sau càng tốt và nhanh hơn. **4 building block** để Claude mang ngữ cảnh đi tiếp:

| Block | Claude học được gì | Mở khóa gì |
|---|---|---|
| **Global instructions** | Bạn là ai, làm việc thế nào | Mọi task bắt đầu đã hiệu chỉnh theo vai trò, định dạng, sở thích của bạn |
| **Projects** | Ngữ cảnh của một mạch công việc | Trong project, Cowork như người đã ở trong team: có file, lịch sử, quyết định đến nay |
| **Skills** | Cách làm một quy trình cụ thể | Khi gặp task khớp, chạy đúng theo template, tiêu chuẩn, bước của team |
| **Plugins** | Chuyên môn của vai trò/lĩnh vực | Từ generalist thành specialist, có workflow, connector, know-how cho vị trí của bạn |

Chúng độc lập và cộng dồn. Đừng làm cả bốn ngay ngày đầu: dần dần bạn sẽ thấy điều gì nên là global instruction, mạch việc nào nên là project, quy trình nào bạn giải thích lặp lại (ứng viên skill đầu tiên); khi hoàn thiện quy trình đủ tốt để chia sẻ thì tới plugin.

## Bài 6: Standing context: global instructions và projects (~7 phút)

Khác Chat (memory tự xây), trong Cowork phần ngữ cảnh mang từ task này sang task khác chủ yếu là **ngữ cảnh bạn tự thiết lập**: global instructions (áp dụng mọi phiên) và projects (Claude tự xây memory từ các hội thoại trong project).

**Global instructions** = bản brief thường trực, mọi session đều tham chiếu (cả scheduled task). Cài: *Settings → Cowork → Edit* cạnh Global instructions → viết → lưu. Nên có: bạn là ai/làm gì; viết tắt và acronym ("QBR deck"...); cách bạn muốn nhận đầu ra (định dạng, độ dài, giọng). Ví dụ PM: "dẫn dắt sản phẩm cho công ty B2B SaaS 200 người; mặc định văn phong ngắn gọn, hướng quyết định, mở đầu bằng vấn đề của người dùng; khi dẫn số liệu nêu tài liệu nguồn; luôn đánh dấu giả định cần kiểm chứng". Không cần đầy đủ từ ngày đầu: những chỉnh sửa bạn lặp lại ("nói kết luận trước", "không dùng Oxford comma") chính là ứng viên.

**Project** = workspace gắn với một mạch công việc (khách hàng, deliverable định kỳ, launch). Gồm 3 thứ bạn thiết lập + 1 thứ Claude tự xây:
- **Instructions:** như global nhưng phạm vi project.
- **Scheduled tasks:** việc định kỳ thuộc project (kéo status thứ Sáu, brief trước họp, rollup cuối tháng), tạo từ hội thoại trong project, chạy với ngữ cảnh project.
- **Context:** một hoặc nhiều thư mục/link Claude làm việc từ đó (mọi hội thoại trong project đều truy cập được).
- **Memory:** Claude học từ các hội thoại trong project, tích lũy dần (bạn không phải viết). Đây là khác biệt của project: ngoài project mỗi session bắt đầu mới (trừ global instructions); trong project mỗi hội thoại làm giàu thêm hiểu biết.

Ví dụ project cho một khách hàng: instructions (giọng trang trọng, gọi tên người quyết định, kết bằng "draft for review"), scheduled (thứ Hai 8h lấy hoạt động cuối tuần từ CRM soạn update 3 dòng; −30 ngày trước gia hạn soạn brief), context (thư mục `Clients/Northwind/`), memory (sở thích của EB, ngày gia hạn, độ nhạy giá), links (Salesforce, Notion).

**Mạch việc hợp làm project:** khách hàng/tài khoản; deliverable định kỳ (mỗi kỳ là một hội thoại mới trong cùng project, tự kế thừa insight kỳ trước); launch/initiative (brief, quyết định, status trong một chỗ đến khi ship).

**3 cách tạo project** (*Projects → New project* ở sidebar Cowork): **từ đầu**; **từ thư mục có sẵn** (thư mục thành working directory); **từ project trong Chat** (chuyển instructions và knowledge sang Cowork; chỉ một chiều, sửa ở Cowork không đồng bộ ngược). Có thể đổi working folder, instructions, connector bất cứ lúc nào.

**Thử ngay:** (1) viết global instructions ban đầu, vài câu: *"Tôi là [vai trò] ở [công ty], làm [1-2 mạch việc chính]. Thuật ngữ thường dùng: ... Deliverable thường là [doc, deck, brief, model]. Update và brief ngắn gọn: mở đầu bằng khuyến nghị, nền tảng gói trong một đoạn. Tôi thích xem bản `.md` preview trước khi tạo file chính thức."* (2) Tạo một project cho mạch việc định kỳ, trỏ vào thư mục có sẵn tài liệu.

## Bài 7: Skills: dạy Cowork cách làm của bạn (~12 phút)

**Skill** = playbook tái sử dụng (một thư mục file và tài nguyên) dạy Claude làm một loại việc theo đúng ý bạn. Task khớp skill đã cài thì Claude **tự tải và làm theo**, không cần gọi tên (vẫn có thể nói rõ "dùng skill soạn board memo").

**4 loại file trong skill** (dùng tổ hợp nào tùy quy trình, "chỉ gồm thứ cần thiết"):
1. **Instructions (`SKILL.md`):** brief nói skill làm gì, khi nào dùng, làm thế nào; viết như runbook cho đồng nghiệp mới.
2. **Assets:** logo, template thương hiệu, slide master, font: nguyên liệu để sản phẩm trông chuyên nghiệp.
3. **References:** ví dụ đầu ra tốt, style guide, thư viện điều khoản, việc cũ làm chuẩn: cách Claude học "tốt" là thế nào.
4. **Scripts:** đoạn code nhỏ cho phần phải luôn giống nhau mỗi lần (tính phương sai, so sánh có cấu trúc, định dạng biểu đồ, định dạng lại tài liệu).

Ví dụ skill: `meeting-recap`, `board-memo`, `variance-analysis`. Skill là cách mã hóa cách team làm việc: instructions nói làm gì, assets là nguyên liệu, references cho thấy "tốt" là gì, scripts chạy phần lặp lại đồng nhất.

**Tạo skill với Claude** (cách nhanh nhất): hội thoại mới trong Cowork: *"Tôi muốn tạo skill cho [quy trình tôi chán phải giải thích lại]. Hãy dẫn tôi qua những gì bạn cần biết."* Claude hỏi: skill làm gì, kích hoạt khi nào, đầu ra tốt trông thế nào, dùng tài nguyên nào. Trả lời cụ thể, chỉ vào ví dụ thật, template thật, đầu ra cũ thật. Kết quả là thư mục skill (`SKILL.md` + assets/references/scripts cần thiết), sẵn sàng cài. Sau khi cài, xem trong **Customize**; muốn sửa chỉ cần nói với Claude ("thêm bước đánh dấu mọi deal > $100K tụt 2 giai đoạn") và Claude cập nhật tại chỗ. Skill dùng được trong mọi hội thoại, kể cả trong project.

**Tự nhìn lại:** ghi một quy trình bạn lặp lại (báo cáo, định dạng quen dùng, checklist): đó là ứng viên skill đầu tiên.

## Bài 8: Plugins: mã hóa chuyên môn của team (~12 phút)

**Plugin** = gói các skill xoay quanh một công việc, kèm **connector** và **subagent** chúng phụ thuộc (subagent = helper chuyên biệt mà skill có thể sinh ra xử lý một phần việc trong context riêng, vd research subagent, drafting subagent). Skill là một playbook; plugin là nhiều playbook. Plugin dạy Claude cách làm của team: cài plugin finance, Claude biết cách team phân tích cổ phiếu; plugin legal, Claude biết playbook hợp đồng. Chuyên môn đi theo bản cài, không đi theo người. Anthropic có plugin cho các vai trò phổ biến (finance, legal, sales, marketing, customer support, product management...): cài dùng ngay, tùy chỉnh, hoặc tự xây.

**2 hình dạng plugin**
- **Quy trình end-to-end:** việc có nhiều bước tuần tự; mỗi bước là một skill, gói lại thành một quy trình. Ví dụ plugin chốt sổ tháng gồm skill kéo số thực tế, dựng bảng phương sai, soạn board memo.
- **Bộ skill dùng nhiều nhất của team:** các việc lặp lại độc lập, gói chung (vd plugin finance: variance analysis, financial modeling, investment memo, quarterly report); người mới cài một lần là có cả bộ công cụ của team.

Ví dụ plugin Legal: `/nda-review` (redline NDA theo playbook), `/contract-summary`, `/clause-library`, `/regulatory-check`, `/counterparty-research`; connector Box, Egnyte, Slack, M365, Atlassian. Plugin vận hành workflow, nhưng đầu ra vẫn do bạn review.

**Cài từ marketplace Anthropic:** *Customize → Plugins* → tìm plugin hợp công việc → **Install** → duyệt connector plugin dùng → skill dùng được ngay.

**Tùy chỉnh theo team:** plugin marketplace là mặc định tốt, không phải đáp án cuối (dùng quy trình tổng quát). Sau khi cài: *Customize → Plugins → [tên] → **Customize*** mở một task Cowork mới để bạn và Claude tinh chỉnh: chỉ vào tài sản, chia sẻ ngữ cảnh, tải ví dụ. Vd: *"Đây là 3 NDA đã redline gần nhất; cập nhật skill `/nda-triage` để định dạng và giọng điệu khớp với chúng."*

**Tự xây plugin:** khi quy trình không khớp plugin nào, làm cùng Cowork: Cowork gói skill cần thiết, connector phụ thuộc, đóng gói để cài vào Cowork. Thường bắt đầu nhỏ: một skill cho việc lặp lại nhất, rồi thêm; khi có 3-4 skill và connector quan trọng thì đáng chia sẻ (bài 13). Admin có thể đã publish plugin của tổ chức: kiểm tra Directory (*Customize → Plugins*) trước khi tự xây.

**Thử ngay:** gõ **`/setup-cowork`** trong hội thoại mới; skill phỏng vấn ngắn về công việc rồi gợi ý plugin hợp nhất; cài ngay từ chat, thử, rồi tùy chỉnh cho team.
