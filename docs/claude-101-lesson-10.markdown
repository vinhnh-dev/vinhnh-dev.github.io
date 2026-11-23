---
layout: page
title: Bài 10 - Research mode
permalink: /claude-101-lesson-10/
parent: Claude 101
grand_parent: Claude
nav_order: 12
---

# Bài 10: Chế độ Research — Nghiên cứu sâu rộng

**Module 4 — Mở rộng phạm vi Claude** · ~15 phút · Trung cấp → Nâng cao

## Ý chính

**Research** biến Claude từ trợ lý trả lời 1 câu hỏi thành "nhà điều tra có hệ thống": khám phá câu hỏi từ nhiều góc độ, tổng hợp từ web và các tích hợp. Ví dụ so sánh 5 địa điểm offsite cho 30 người: thủ công ~1 ngày → Research ~10 phút công sức (chờ 15-25 phút), ra báo cáo ~5 trang có trích dẫn.

## Research vs Web search

| | Web search | Research |
|---|---|---|
| Phạm vi | 1 truy vấn, 1 vòng | Nhiều vòng, mỗi vòng dựa trên kết quả trước |
| Độ sâu | Lướt ~10 kết quả | Đọc kỹ 20-50+ nguồn, đối chiếu |
| Kết quả | Danh sách link | Báo cáo có cấu trúc, phân tích |
| Thời gian | Tức thì | 5-45 phút |
| Trích dẫn | Ít/không | Mọi khẳng định đều có link nguồn |
| Lập luận | Không | Extended thinking, giải thích phương pháp |

## Khi nào dùng / không dùng

**Dùng khi:** báo cáo tổng hợp nhiều nguồn; phân tích web + tích hợp (Google Workspace, Slack...); điều tra thường mất hàng giờ; so sánh (đối thủ, nhà cung cấp, phương án); cần trích dẫn kiểm chứng được. Ví dụ: phân tích thị trường, lập kế hoạch dự án phức tạp, tổng quan tài liệu học thuật, due diligence (M&A, đầu tư, chọn vendor), brief cần thông tin cập nhật.

**Chọn công cụ khác khi:**

| Câu hỏi | Công cụ |
|---|---|
| Nhanh, cụ thể, 1-2 nguồn (giá cổ phiếu, địa chỉ) | **Web search** (1-10 giây) |
| Suy luận sâu, không cần thông tin ngoài (toán, debug, logic) | **Extended thinking** |
| Kiến thức nội bộ tổ chức (Slack, email, tài liệu, chính sách) | **Enterprise Search** (~30 giây) |
| Điều tra đa nguồn, kỹ lưỡng | **Research** (5-45 phút) |

| | Research | Web search | Extended Thinking | Enterprise Search |
|---|---|---|---|---|
| Nguồn | Web + tích hợp, sâu | Web, bề mặt | Chỉ suy luận nội bộ | Công cụ nội bộ tổ chức |
| Kết quả | Báo cáo có cấu trúc | Link | Lập luận từng bước | Tổng hợp dữ liệu tổ chức |
| Trích dẫn | Luôn có | Tùy | Không | Luôn có |

## Cách hoạt động — 4 bước

1. **Lập kế hoạch:** Extended thinking tự bật, phân rã yêu cầu, xác định nhu cầu thông tin
2. **Tìm kiếm nhiều lần:** mỗi lần quyết định dựa trên kết quả trước, theo manh mối, lấp khoảng trống
3. **Tổng hợp:** web + tích hợp (Gmail, Calendar, Drive) → báo cáo có cấu trúc
4. **Trích dẫn:** mọi khẳng định gắn nguồn, dễ kiểm chứng

Bạn thấy: chỉ báo "thinking" kèm đồng hồ, phần suy nghĩ mở rộng được (minh bạch, không hộp đen).

## Cách dùng

- Nút **Research** ở góc dưới trái khung chat → bật (xanh) → nhập prompt → gửi
- **Bắt buộc bật Web Search** (Search and tools)
- Chạy nền, xong sẽ thông báo; trong lúc chờ có thể làm việc khác

| Độ phức tạp | Thời gian |
|---|---|
| Đơn giản (1 chủ đề) | 5-10 phút |
| Trung bình (báo cáo 3-4 phần) | 10-20 phút |
| Phức tạp (phân tích thị trường, so sánh chi tiết) | 25-45 phút |

## Viết prompt Research hiệu quả

Dành 3-5 phút soạn prompt → tiết kiệm 30-60 phút chờ kết quả kém.

1. **Cụ thể về mục tiêu:** ❌ "Cho tôi biết về thị trường xe điện" → ✅ "Phân tích thị trường pin EV: doanh nghiệp chủ chốt, xu hướng công nghệ, thách thức chuỗi cung ứng ảnh hưởng quyết định đầu tư"
2. **Chỉ định các phần/cấu trúc** (mỗi phần = một phần Claude đảm bảo bao phủ)
3. **Nêu ràng buộc:** ngân sách, thời gian, địa lý, đối tượng, quy định (vd 40-60K USD, 30 người, 15-22/6/2026, trong 90 phút lái xe từ SFO, có món chay/halal)
4. **Nhờ Claude cải thiện prompt** trước khi bật Research
5. **Mẫu prompt:**

```
[BỐI CẢNH/MỤC TIÊU]  Tôi là [vai trò], đang điều tra [chủ đề] cho [mục đích/quyết định].
[PHẠM VI]            Địa lý / thời gian / phân khúc: ...
[CÁC PHẦN]           1. ...  2. ...  3. ...
[RÀNG BUỘC]          Ngân sách, thời gian, loại trừ
[ĐỊNH DẠNG]          Độ dài, phong cách, đối tượng đọc
[TRÍCH DẪN]          Ưu tiên nguồn trong khoảng thời gian..., loại nguồn (học thuật/ngành/tin tức)
```

## Kết hợp với tích hợp

Khi nối Google Workspace/Slack..., Claude lấy ngữ cảnh nội bộ song song với research web. Nên chỉ rõ nguồn: "Lấy bối cảnh từ Drive về Dự án Y", "Gồm chi tiết từ email gần đây", "So với ghi chú họp tuần trước".

- **Tình trạng dự án vs chuẩn ngành:** tóm tắt thảo luận Dự án X trong email/Slack → research best practice ngành → nêu khoảng cách.
- **Chuẩn bị họp:** đọc lịch tuần tới → tra từng công ty (tin tức, gọi vốn, sản phẩm, thay đổi nhân sự) → 1 trang/cuộc họp.
- **Vị thế cạnh tranh:** tìm tài liệu nội bộ về chiến lược giá → so với cách đối thủ định vị (web) → khoảng trống và cơ hội.

**Research nội bộ:** tắt Web Search để chỉ tìm trong công cụ đã kết nối (thảo luận launch Q3 trên Slack/Docs, tổng hợp feedback khách trong các cuộc họp, quyết định về hệ thống xác thực 6 tháng qua).

## Ví dụ theo vai trò

| Vai trò | Prompt Research | Thời gian |
|---|---|---|
| Phân tích đầu tư | Company X góc nhìn cổ phiếu, 12 tháng gần nhất; 10-K, earnings, analyst, tin ngành → ~5 trang + executive summary | 30-45' (thủ công 1-2 ngày) |
| Marketing strategy | Vị thế cạnh tranh công cụ viết AI cho SMB, ~3 trang + bảng so sánh | 20-30' (thủ công 1 ngày) |
| Due diligence | Target Corp (private), 24 tháng; press release, SEC, LinkedIn, G2, Crunchbase | 30-40' |
| Pháp lý | Yêu cầu lưu trữ dữ liệu cho SaaS tại Việt Nam; web chính phủ, hãng luật; 4-5 trang | 20-30' |
| Y khoa/khoa học | Literature review GLP-1 agonists trong béo phì; tạp chí bình duyệt từ 2020; trích dẫn Tác giả, Năm, Tạp chí | 25-40' |
| Học thuật | Literature review AI tutoring và kết quả Toán K-12; 4000 từ, APA | 30-45' |
| BĐS | Đầu tư multifamily Austin TX, 24 tháng, so Nashville và Raleigh | 20-30' |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Dùng Research cho câu hỏi nhanh ("Thủ đô Pháp?") | Web search cho tra cứu; Research cho điều tra đa nguồn |
| Prompt mơ hồ ("Research startup AI") | Chỉ định phạm vi, phần, ràng buộc, đối tượng |
| Không chỉ định cấu trúc output | Nêu các phần ngay từ đầu |
| Coi báo cáo là quyết định cuối | Research = đầu vào; phán đoán vẫn là của bạn |
| Không kiểm tra trích dẫn | Mở link, xác minh số liệu, trích dẫn trực tiếp |
| Phạm vi quá rộng ("toàn bộ thị trường AI, 5 năm tới") | Chia nhiều đợt tập trung rồi tổng hợp |
| Chấp nhận kết quả lần đầu | Lặp: "Đào sâu Phần 3 về rủi ro quy định, thêm biến thể cấp bang Mỹ" |

## Bài tập

1. **Soạn prompt (10'):** chọn chủ đề (phân tích đối thủ, thị trường, literature review) → viết prompt theo mẫu (Bối cảnh / Phạm vi / Phần / Ràng buộc / Đầu ra) → nhờ đồng nghiệp hoặc Claude phản biện → chỉnh 1-2 lần, **chưa chạy**.
2. **Research đầu tiên (20-40'):** bật Research, làm việc khác trong lúc chờ. Kiểm tra: phủ đủ các phần? có trích dẫn? tổng hợp có insight? có lỗi/thiếu sót? Lặp với 1 prompt: "Phần X chưa đủ chi tiết, mở rộng..."
3. **Research + tích hợp (15'):** chuẩn bị họp (lịch + web), QBR khách (CRM + ngành) hoặc kickoff (tài liệu nội bộ + best practice ngoài) → so chất lượng khi có tích hợp.

## Tóm tắt

- **Research = điều tra đa nguồn chủ động**, tự quyết tìm gì tiếp theo, vượt xa web search.
- **Dùng:** báo cáo tổng hợp, so sánh, phân tích sâu (5-45 phút). **Không dùng:** thông tin nhanh, suy luận thuần, kiến thức nội bộ → Web search / Extended thinking / Enterprise Search.
- **Prompt = đòn bẩy ROI:** mục tiêu cụ thể, cấu trúc các phần, ràng buộc.
- **Extended thinking tự bật**, có thể mở ra xem.
- **Research + tích hợp** = tổng hợp góc nhìn nội bộ và bên ngoài.
- **Luôn kiểm tra trích dẫn** với nội dung quan trọng.
