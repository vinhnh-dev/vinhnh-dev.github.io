---
layout: page
title: Bài 2.7 - CLAUDE.md
permalink: /claude-code-101-lesson-2-7/
parent: Claude Code 101
grand_parent: Claude
nav_order: 9
---

# Bài 2.7: Tệp CLAUDE.md

**Module 3 — Persistent context & extension** · ~30 phút · Trung cấp

## Ý chính

Claude Code không có bộ nhớ giữa các phiên. **CLAUDE.md** là bộ nhớ dài hạn: file Markdown thường, tự động được đọc và **prepend vào system prompt** mỗi phiên, nên Claude "đã biết" dự án trước khi bạn gõ prompt (2 giờ giải thích lại → 30 giây). Claude 4 tuân thủ CLAUDE.md sát hơn nhiều so với 3.7.

## 4 biến thể

| Biến thể | Đường dẫn | Chia sẻ | Nội dung | Khi nào |
|---|---|---|---|---|
| **Project** | `./CLAUDE.md` | ✅ commit lên Git, cả team | Stack, lệnh build, kiến trúc, convention, gotcha chung | Khi setup dự án hoặc team bắt đầu dùng Claude Code |
| **User** | `~/.claude/CLAUDE.md` | ❌ cá nhân, mọi dự án | Style cá nhân, format commit, ngôn ngữ, workflow | Ngay khi cài Claude Code |
| **Local** | `./CLAUDE.local.md` | ❌ (phải tự thêm vào `.gitignore`) | Đường dẫn/URL local, ghi chú debug tạm | Sở thích riêng không ảnh hưởng team |
| **Nested** | `./api/CLAUDE.md`, `./apps/web/CLAUDE.md` | ✅ | Quy tắc riêng module (monorepo, microservices) | Dự án đủ lớn |

Tất cả được **merge**; quy tắc cụ thể thắng quy tắc chung (project > user khi xung đột). Nested được tự phát hiện khi làm việc trong thư mục con.

## Cấu trúc một CLAUDE.md tốt (ví dụ Next.js 15 + Tailwind + Drizzle)

Các phần: **Dự án** (stack: App Router, Tailwind, Drizzle, Neon Postgres, BetterAuth, TanStack Query + Zustand), **Lệnh** (`pnpm dev`; `pnpm test --run`, không `pnpm test` vì watch mode không thoát; `pnpm lint`; `pnpm typecheck`; `pnpm db:migrate`, **không** gọi `drizzle-kit migrate` trực tiếp vì bỏ qua seed), **Code style** (2 space; named export, trừ `page.tsx`; ưu tiên Server Actions; TypeScript strict, không `any`), **Kiến trúc** (schema ở `db/schema/` mỗi domain một file; fetch phía server; UI chung `components/ui/`, feature `components/features/`; env), **Gotchas** (token session hết hạn 7 ngày, đừng hardcode; `db.query.*` vs `db.select()` không trộn; chỉ `pnpm`, không commit `package-lock.json`/`yarn.lock`), **Tham chiếu** (`@README.md`, `@docs/architecture.md`, `@docs/api-spec.md`).

Phần quan trọng nhất và hay bị bỏ quên là **Gotchas** (điều phản trực giác chỉ người làm dự án mới biết) và **Lệnh** (Claude hay chạy sai, ví dụ watch mode). Dùng `@file` thay vì dán nội dung (tránh trùng lặp, nhanh lỗi thời); file gốc là nguồn sự thật.

## Thiết lập từng bước

1. **Đừng tạo ngay từ ngày 1.** Làm 3-5 ngày, ghi lại: phải nhắc gì mỗi phiên, Claude hay sai gì, lệnh nào hay giải thích lại, gotcha nào. CLAUDE.md tốt xuất phát từ vấn đề thực tế.
2. **Chọn 5-10 điều cần nhắc lại** (2-3 lệnh hay sai, 1-2 quyết định kiến trúc, 3-5 convention, 1-2 gotcha).
3. **`/init`** tạo bản nháp (quét codebase, `package.json`, config; thường 20-30 dòng, đúng về stack nhưng thiếu gotcha/quyết định).
4. **Chỉnh bản nháp:** thêm lệnh sai, pattern kiến trúc, gotcha; xóa thứ hiển nhiên; mục tiêu **40-60 dòng** (> 150 dòng bắt đầu tốn context vô ích).
5. **Test với phiên mới:** giao task thường bị làm sai; nếu vẫn sai, thêm quy tắc cụ thể hơn.
6. **Commit** `CLAUDE.md`, rồi tạo `~/.claude/CLAUDE.md` cho sở thích cá nhân (ngôn ngữ trả lời, format commit `<type>(<scope>): <desc>`, TS strict, đề xuất kế hoạch trước khi thay đổi lớn, chạy test sau mỗi thay đổi, flag vấn đề bảo mật).

## Case study

| Team | Cách dùng CLAUDE.md |
|---|---|
| Backend (5 dev, 8 service, pnpm monorepo) | Root: cấu trúc monorepo, package chung, CI, deploy; `services/payments/CLAUDE.md` (PCI, tiền tệ, idempotency key); `services/notifications/CLAUDE.md` (queue, retry); mỗi dev có user-level riêng. Kết quả: không truy cập DB trực tiếp, không bỏ idempotency |
| Mobile (React Native) | `npm run ios/android` (không `npx react-native run-*`); `pod install` sau khi đổi native module; file `.ios.tsx`/`.android.tsx` thay vì `Platform.OS`; `react-native-config`; ảnh trong `assets/images/` |
| Data (Python + Poetry) | Luôn `poetry run ...`, không `pip install`; data raw gitignored; model đặt tên theo timestamp; `nbstripout` trước commit |
| COBOL modernization (demo AWS, 94 file) | CLAUDE.md đặt vai "chuyên gia tài liệu COBOL", phương pháp, định dạng output; nhiều subagent phân tích song song, ra > 100 trang tài liệu trong ~1 giờ |
| Maintainer thư viện OSS | Chính sách version (v3.x chỉ bugfix), semver, JSDoc cho API công khai, MIGRATION.md cho breaking change, changeset, test + coverage ≥ 80% |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| CLAUDE.md quá dài (> 200 dòng, ~3-5k token/lần) | 40-80 dòng; thứ chỉ cho một loại task (checklist PR, format commit) → **Skill** (Bài 2.9) |
| Nhét sở thích cá nhân vào CLAUDE.md của project (đồng nghiệp bị ảnh hưởng) | Cá nhân → `~/.claude/CLAUDE.md` hoặc `CLAUDE.local.md` |
| Không cập nhật khi stack đổi (đã sang Vitest mà vẫn ghi Jest) | Cập nhật ngay trong cùng PR; thêm "cập nhật CLAUDE.md nếu cần" vào checklist PR |
| `/init` xong không chỉnh | `/init` là điểm khởi đầu; thêm gotcha, convention, quyết định kiến trúc |
| Chỉ dùng project-level, bỏ user-level (copy-paste cùng đoạn vào 8 dự án) | Viết `~/.claude/CLAUDE.md` một lần |
| Đưa checklist PR 20 mục vào CLAUDE.md | Đó là Skill |

## Mẹo nâng cao

- **Phím `#`:** gõ `# ghi chú...`; Claude hỏi lưu vào project / user / nested. Sửa Claude 2-3 lần cùng một điều → gõ `#`.
- **`@filename`** để tham chiếu README, `docs/architecture.md`, `docs/api-contracts/*.yaml`, `.env.example`.
- **Monorepo:** `apps/web/CLAUDE.md`, `apps/mobile/CLAUDE.md`, `packages/ui/CLAUDE.md`... Claude tải root + file của thư mục đang làm + user-level, tự thay khi chuyển thư mục.
- **Sau khi sửa lỗi cho Claude:** "Hãy lưu quy tắc này vào CLAUDE.md" (ví dụ cấm `any`, dùng `unknown` rồi narrow).
- **Rà soát mỗi quý:** một file 40 dòng chính xác hơn 120 dòng có 30% lỗi thời.

## Bài tập

1. **Tạo, chỉnh, kiểm (20'):** `/init` → ghi đúng/thiếu → thêm 3-5 gotcha, quyết định kiến trúc, bỏ nội dung hiển nhiên (≤ 40-60 dòng) → đóng và mở phiên mới → giao task hằng ngày, xem Claude áp dụng convention ngay chưa.
2. **User-level CLAUDE.md (10'):** `mkdir -p ~/.claude`; nội dung gợi ý: ngôn ngữ trả lời, format commit, style cá nhân, quy tắc workflow, những điều phải nhắc ở mọi dự án.

## Tóm tắt

- **CLAUDE.md = bộ nhớ dài hạn** (nạp vào system prompt mỗi phiên).
- **4 biến thể:** project (team), user (cá nhân), local (gitignored), nested (module).
- Bắt đầu từ **đau thực tế** (quan sát 1 tuần); `/init` chỉ là bản nháp.
- **`@filename`** thay vì copy-paste.
- **Ngắn gọn 40-80 dòng**; việc theo tác vụ → Skill.
