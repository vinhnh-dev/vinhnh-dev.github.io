---
layout: page
title: Bài 2.4 - Workflow EPCC
permalink: /claude-code-101-lesson-2-4/
parent: Claude Code 101
grand_parent: Claude
nav_order: 6
---

# Bài 2.4: Explore → Plan → Code → Commit

**Module 2 — Workflow chuyên nghiệp** · ~35 phút · Trung cấp

## Ý chính

*"Nếu chỉ nhớ một điều từ khóa học này, hãy nhớ workflow này"* (Boris Cherny). Ví dụ: cùng ticket "thêm chuyển đổi WebP vào luồng upload". Minh lao vào code: Claude đoán sai điểm vào và thư viện (`sharp` thay vì `jimp`), sau 2 giờ vẫn vá chắp vá. An dùng Plan mode: Claude tìm ra pipeline ở `middleware/storage.ts` và dự án đã có `jimp`; kế hoạch → duyệt → xong trong ~25 phút. Khác biệt nằm ở quy trình, không phải ở người.

## 4 giai đoạn

| Giai đoạn | Vai trò | Tỷ lệ thời gian (ước tính) |
|---|---|---|
| **Explore** | Claude hiểu codebase trước khi chạm code (context gathering) | ~20% |
| **Plan** | Điểm thống nhất giữa bạn và Claude, nơi **rẻ nhất để đổi hướng** | ~30% |
| **Code** | Thực thi theo plan, verify bằng test (execution loop) | ~40% |
| **Commit** | Review với "con mắt mới" trước khi push (confidence gate) | ~10% |

Explore + Plan ≈ 50% thời gian: đó là khoản đầu tư. Task nhỏ thì 10-15%, task kiến trúc lớn Plan có thể 50%+.

## Explore: đừng để Claude đoán

- **Cách 1:** Plan mode (`Shift+Tab`, read-only: Claude không thể "lỡ tay" sửa file).
- **Cách 2:** prompt tường minh: "Khám phá codebase và cho tôi biết... Đừng thay đổi gì."
- **Cần biết sau Explore:** chức năng nằm ở đâu, pattern hiện tại, dependency có sẵn, test ở đâu và chạy bằng lệnh gì, ràng buộc ẩn (logic nghiệp vụ, hiệu năng, bảo mật).
- Bỏ Explore tiết kiệm 5-10 phút nhưng thường mất thêm 30-90 phút sửa lỗi.

## Plan: nơi rẻ nhất để đổi hướng

Chi phí thay đổi: sửa plan (vài từ) < sửa code (revert, viết lại) < sau commit (đóng PR, review và test lại).

- Vào Plan mode: `Shift+Tab` 1-2 lần đến khi thấy "Plan Mode".
- Prompt Plan trả lời 3 câu: nhiệm vụ là gì, ràng buộc (không được/phải), thành công trông thế nào (test pass, endpoint trả đúng, UI đúng).
- Đọc plan **trước khi approve**: thiếu edge case? thứ tự hợp lý? mâu thuẫn quyết định kiến trúc của team?

| Plan tốt | Plan kém |
|---|---|
| Liệt kê từng file/hàm cần sửa | "Sửa luồng upload để hỗ trợ WebP" |
| Bước có thứ tự và phụ thuộc rõ | Danh sách không thứ tự |
| Có tiêu chí thành công | "Tính năng chạy được" |
| Xử lý edge case (GIF động, giới hạn kích thước, lỗi) | Chỉ happy path |
| Không mở rộng phạm vi | "Nhân tiện refactor luôn module upload" |
| Khả thi với codebase hiện tại | Giả định thư viện/pattern không tồn tại |

Có thể lặp 2-3 vòng (mỗi vòng 1-2 phút), vẫn rẻ hơn debug code sai.

## Code: thực thi có kiểm soát

- Thoát Plan mode (`Shift+Tab`). **Auto Accept** khi đã review plan (task trung bình); **Manual/Default** cho auth, thanh toán, bảo mật.
- **3 yếu tố giúp Code mượt:**
  1. **Tiêu chí thành công rõ** (vd: test trong `tests/upload/` pass + không lỗi TypeScript + build OK)
  2. **Công cụ verify:** extension Claude cho Chrome để kiểm UI, lệnh test rõ (`npm test`, `pnpm test --run`, `pytest -v`), lệnh lint
  3. **Test suite là nguồn sự thật:** Claude làm → chạy test → fail → đọc lỗi → sửa → chạy lại, tự lặp. Không có test thì Claude phải hỏi bạn sau mỗi bước
- Mẹo: bảo Claude **viết test trước**, bạn duyệt, rồi mới implement (TDD với AI).
- Claude lặp lại cùng một lỗi → thiếu context quan trọng → "Hãy lưu giải pháp vào CLAUDE.md" (Bài 2.7).
- **Chạy song song:** 4-6 instance Claude, mỗi cái một `git worktree` riêng (`git worktree add ../project-feature-a feature/webp-conversion` rồi `cd ... && claude`).
- **Async Accept** (`Shift+Enter` khi Claude đang chạy): Claude chạy nền, bạn làm việc khác, được báo khi xong.

## Commit: cổng kiểm tra trước khi push

Claude có thiên kiến vì chính nó viết code. Dùng **subagent reviewer** (context mới): đọc `git diff --staged`, test liên quan, `CLAUDE.md`; trả về tóm tắt thay đổi, vấn đề/bug, edge case thiếu, vi phạm style, khuyến nghị commit hay sửa (chi tiết Bài 2.6). Sau đó để Claude tạo commit message theo style của `git log`. GitHub Actions cho phép `@claude` ngay trong PR comment.

## Khi nào bỏ qua giai đoạn nào

| Task | Explore | Plan | Code | Commit | Thời gian |
|---|---|---|---|---|---|
| Tiny (typo, 1 hằng số, đổi tên biến) | Bỏ | Bỏ | Nhanh | Tối thiểu | 2-5 phút |
| Easy (1 field form, bug 1 file) | Tùy | Tùy | Bình thường | Tối thiểu | 5-15 phút |
| Medium (feature mới, refactor 1 module) | **Bắt buộc** | **Bắt buộc** | + test | Review kỹ | 20-60 phút |
| Hard (đổi kiến trúc, đa lĩnh vực) | Mở rộng | Nhiều vòng | Theo giai đoạn | Nhiều commit | 2-8 giờ |
| Rất lớn (hiện đại hóa hệ thống cũ) | Nhiều ngày | Nhiều ngày | Theo giai đoạn qua nhiều tuần | Commit theo giai đoạn | Tuần |

Task Hard: nhiều commit nhỏ (mỗi giai đoạn 1 commit) thay vì 1 commit khổng lồ, dễ review và rollback.

## Ví dụ đầy đủ: WebP (Node.js + Express, `jimp`, Jest, S3), ~28 phút

1. **Explore (5-7'):** Plan mode, hỏi file upload, thư viện ảnh, test, tích hợp S3 → pipeline `middleware/storage.ts` → `services/upload.service.ts`; `jimp` đã có; 12 test; `services/s3.service.ts`.
2. **Plan (8-10'):** yêu cầu JPEG/PNG → WebP trước khi lên S3, GIF động giữ nguyên, ≤ 2MB, API không đổi, dùng `jimp`, viết test. Plan 5 bước (hàm `convertToWebP`, sửa middleware, đổi tên/Content-Type, test, verify). Bạn nhận xét: jimp không xuất WebP mặc định → Claude xác minh cần `@jimp/plugin-webp` (~50KB) và cập nhật Bước 1.
3. **Code (15-20'):** Auto Accept, "Thực thi plan, chạy `npm test` sau mỗi bước"; test "large file" fail → Claude tự sửa → 26 test pass, TypeScript 0 lỗi, build OK.
4. **Commit (3-5'):** subagent review phát hiện thiếu edge case buffer rỗng → sửa + thêm test → Claude viết commit `feat(upload): ...` → push.

## Case study theo vai trò

| Vai trò | Cách áp dụng EPCC |
|---|---|
| Backend (fintech, tách monolith thanh toán, ~2 ngày) | Explore: 4 subagent song song map 4 module (30' thay vì 1 ngày); Plan nhiều vòng, lưu `.md` trong repo; Code theo giai đoạn; 8 commit nhỏ; xong trong 1,5 ngày, không regression |
| Frontend (dark mode, ~45') | Explore `theme/` + `tailwind.config.ts`; 1 vòng Plan (10'); Auto Accept + Chrome extension test UI; subagent review nhanh |
| Maintainer OSS (review PR contributor) | Bỏ Explore/Plan; checkout nhánh PR, chạy test, Claude verify logic/edge case; chu kỳ review nhanh hơn 2-3 lần |
| Anthropic (đổi 22.000 dòng mã RL) | Nhiều ngày lập kế hoạch bởi người; tập trung vào "leaf node" (không ai phụ thuộc, nợ kỹ thuật chấp nhận được); review tập trung phần mở rộng; stress test; tin cậy như thay đổi bình thường nhưng nhanh hơn nhiều |
| Indie / vibe coding | Explore ngắn, Plan để *nghĩ về tính năng*; tin test, tập trung hành vi; chỉ dùng cho prototype/tính năng không quan trọng, auth/thanh toán vẫn phải review code |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Bỏ Plan để "tiết kiệm thời gian" | 10 phút Plan tiết kiệm 30-90 phút debug (task Medium trở lên) |
| Analysis paralysis (sửa plan lần thứ 6) | Plan "đủ tốt" (happy path + edge case chính) → approve; tinh chỉnh khi Code |
| Code không có test suite | Đầu tư test từ đầu; bảo Claude viết test trước |
| Commit không review | Luôn có review (nhanh 2 phút hay kỹ 5 phút) |
| EPCC đầy đủ cho task nhỏ | Co giãn theo quy mô; workflow là công cụ, không phải nghi thức |

## Mẹo nâng cao

- **`/rewind`** (Esc Esc): quay lại trạng thái trước (git worktree/`git stash` vẫn là lưới an toàn tốt hơn).
- **`/compact` giữa các giai đoạn:** sau Explore nén lại trước khi vào Plan (Bài 2.5).
- **Lưu plan thành tài liệu** (`docs/plans/xxx-plan.md`): version control, không mất khi phiên gián đoạn, dùng lại sau.
- **Test-first:** "Viết test cho tính năng X trước, tôi duyệt, rồi implement cho test pass."
- **Quy tắc workflow trong CLAUDE.md:** luôn chạy test trước khi tuyên bố xong, không thêm dependency mà không xin phép, hàm async phải xử lý lỗi, dùng conventional commits.

## Bài tập

1. **EPCC trong 30' (task vừa):** ghi thời gian từng giai đoạn, giai đoạn nào lâu nhất, có lúc nào muốn bỏ qua không, reviewer phát hiện gì. Nhiều người ngạc nhiên vì Plan nhanh hơn dự kiến.
2. **A/B test trong 1 sprint:** 2 task tương tự, 1 làm EPCC, 1 làm kiểu cũ. Đo: thời gian đến merge, số lần rollback/viết lại, bug sau merge, mức căng thẳng.

## Tóm tắt

1. **EPCC giúp nhanh hơn**, Explore + Plan ≈ 50% là đầu tư.
2. **Plan mode** là chỗ rẻ nhất để chỉnh hướng.
3. **Test suite là nhân tố nhân đôi:** có test thì Code tự vận hành.
4. **Subagent reviewer = con mắt mới** (Claude thiên vị với code của chính nó).
5. **Co giãn theo quy mô task.**

Phím tắt: `Shift+Tab` (Plan Mode), `Shift+Enter` (Async Accept), `Esc Esc` (`/rewind`).
