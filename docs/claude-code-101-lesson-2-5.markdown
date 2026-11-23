---
layout: page
title: Bài 2.5 - Quản lý Context
permalink: /claude-code-101-lesson-2-5/
parent: Claude Code 101
grand_parent: Claude
nav_order: 7
---

# Bài 2.5: Quản lý Context

**Module 2 — Workflow chuyên nghiệp** · ~25 phút · Cơ bản → Trung cấp

## Ý chính

Context window là tài nguyên quan trọng nhất. Nếu không quản lý: sau ~30 phút (đọc 18 file, 12 lần test, 4 lần web search) Claude tự nén, "quên" ràng buộc bạn nói lúc đầu (vd JWT secret xoay mỗi 7 ngày), và khi đổi sang task khác còn bị "ám" bởi mẫu của task cũ.

## Context window

- Mặc định **200.000 token** (~150k từ, ~80-100 file cỡ trung bình). Gồm: system prompt (~5k), CLAUDE.md (~2-10k), định nghĩa tool MCP (~5-30k), hội thoại + tool call + kết quả.
- Đầu vào càng dài → mỗi câu trả lời đắt hơn và chậm hơn; gần đầy thì chi tiết ở xa bị "loãng".
- Giống trí nhớ ngắn hạn của người: chi tiết 7 phút trước rõ, 2 giờ trước mơ hồ.
- Khi ~80% đầy: **auto-compact** (tóm tắt quyết định/file đang sửa/blocker, bỏ tool result chi tiết). ⚠️ Có thể mất chi tiết, đừng phó mặc.

## 3 lệnh

| Lệnh | Tác dụng |
|---|---|
| **`/context`** | **Xem** dung lượng dùng và nguồn (system, CLAUDE.md, MCP, hội thoại, tool result, skills). Chẩn đoán trước khi quyết định |
| **`/compact`** | **Tóm tắt** hội thoại, giữ bản tóm tắt task hiện tại (~142k → ~38k) |
| **`/clear`** | **Xóa** về zero, chỉ giữ system prompt, CLAUDE.md, định nghĩa MCP, tên + mô tả skill |

**Quy tắc vàng:** mỗi 30-45 phút gõ `/context`. **Đang làm dở → `/compact`; chuyển task khác → `/clear`.** Nếu MCP chiếm quá lớn, tắt bớt server thay vì compact mù.

| | `/compact` | `/clear` | Auto-compact |
|---|---|---|---|
| Kích hoạt | Bạn | Bạn | Hệ thống khi gần đầy |
| Giữ tóm tắt task | ✅ | ❌ | ✅ (có thể mất chi tiết) |
| Dùng khi | Đang làm, cần tiếp tục | Xong, chuyển task | Phương án dự phòng |
| Rủi ro mất sắc thái | Thấp | N/A | Trung bình → cao |

## Ví dụ một buổi sáng (3 task, 0 lần auto-compact)

- **9:00 Bug webhook thanh toán** (gửi `notifyUser()` 2 lần): prompt cụ thể (file, nghi vấn, 50 dòng log cuối) → 25 phút xong; `/context` 19% (38k).
- **10:30 Tính năng export CSV:** task hoàn toàn khác → **`/clear`** (4,7%) → prompt rõ endpoint, streaming, chỉ admin; đọc `src/admin/reports.ts` → 44%.
- **11:30 Review PR KYC:** `/clear` + giao **subagent** đọc ~30 file, kiểm CLAUDE.md/bảo mật/test, trả tóm tắt → context chính chỉ 7%.

So với một phiên 3 giờ liên tục (lên 180k, auto-compact 2 lần, mất chi tiết bảo mật): 3 phiên tập trung không mất ngữ cảnh, code chất lượng hơn.

## 5 chiến thuật tiết kiệm context

1. **Prompt cụ thể.** "Sửa lỗi auth" → Claude đọc `src/auth/*`, grep, đọc test, hỏi lại: ~25k token. Prompt nêu file/dòng/nguyên nhân/cách sửa/test cần cập nhật: ~3k. Một prompt ~50 từ có thể tiết kiệm ~20k token "thám tử".
2. **Đưa ngữ cảnh lặp lại vào CLAUDE.md** (pnpm, `pnpm test --run`, thư mục migration, named export). Quy tắc: chỉ những gì bạn lặp lại ≥ 3 lần (CLAUDE.md cũng tốn context).
3. **Subagent để "thuê ngoài" khám phá** khi chỉ cần câu trả lời: "Endpoint auth ở đâu?", "File nào dùng `oldFn()`?", "Gom TODO theo module." Subagent có 200k riêng, chỉ trả tóm tắt (Bài 2.8).
4. **Skill thay vì nhét vào CLAUDE.md** các checklist chỉ dùng cho một loại task (PR review, commit format): Skill tải theo yêu cầu (Bài 2.9).
5. **Dọn MCP không dùng:** mỗi server tốn ~2-15k token chỉ cho định nghĩa tool. Dùng `/mcp` xem chi phí (vd linear ~8k, slack ~4k, figma ~12k, jira ~7k = ~33k, 16,5% context), `/mcp disable <server>` rồi khởi động lại phiên. Nếu MCP > 10%, Claude Code có thể chuyển sang chế độ tool search nhưng chưa ổn định, nên chủ động tắt.

## Case study theo vai trò

| Vai trò | Cách quản lý |
|---|---|
| Backend (tính năng refund 4-6 giờ) | Prompt rất chi tiết; mỗi giờ `/context`, > 60% thì `/compact`; subagent đọc tài liệu thư viện; sang giai đoạn viết test thì `/compact` với tóm tắt "xong A, B, C, giờ test"; không auto-compact, giữ spec ban đầu |
| Designer-as-developer | `/clear` mỗi khi đổi component; CLAUDE.md có design token; skill `/polish-design`; subagent kiểm WCAG; 8 PR/ngày, context không quá 30% |
| DevOps (sự cố 2h sáng) | Prompt cực ngắn nhưng cụ thể; subagent đọc log vài MB; `/clear` trước khi sửa (vd tăng pool 10 → 25); `/clear` lần nữa để viết postmortem; xong trong 35 phút |
| Sinh viên đóng góp OSS (200k LOC) | Không đọc hết; 4 subagent song song (auth, api-routing, db-layer, frontend), mỗi cái trả 1 trang; context chính ~10k; hiểu kiến trúc trong 30 phút thay vì 2 ngày |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| "Cứ tiếp tục" 4 giờ không `/context` (auto-compact 3 lần → mất sắc thái) | Mỗi 30-45 phút `/context`, rồi `/compact` hoặc `/clear` |
| "Đọc hết `src/`, `tests/`, `docs/`" | CLAUDE.md mô tả kiến trúc; subagent khám phá module khi cần |
| Gộp nhiều task trong một phiên | Một phiên = một task; xong thì `/clear` |
| Bật mọi MCP "phòng khi cần" (12 server = 50k token = 25% context) | Cấu hình theo dự án (`.mcp.json`), chỉ bật cái cần |
| `/clear` giữa lúc làm dở (mất quyết định, file đang sửa, edge case) | Đang làm = `/compact`; chỉ `/clear` khi xong hẳn |

## Mẹo nâng cao

- **Ghim quyết định vào CLAUDE.md trước khi `/compact`** ("JWT secret xoay mỗi 7 ngày, không hardcode thời gian").
- **Phím `#`:** gõ `# ghi chú...` để Claude hỏi lưu vào project / user / nested CLAUDE.md. Sửa lỗi cho Claude 2-3 lần cùng một chuyện → đó là tín hiệu nên gõ `#`.
- **Nhiều phiên song song:** mỗi tab terminal một `git worktree` riêng, context riêng, không "ám" lẫn nhau.
- **Ước lượng trước:** mỗi file ~500-2000 token, grep toàn dự án ~1-3k, web search ~2-5k, output test 5-50k. Dự kiến > 60k thì tách task hoặc dùng subagent.

## Bài tập

1. **Audit context (10'):** sau 15-30 phút làm việc gõ `/context`; ghi tổng %, danh mục lớn nhất, MCP có > 10% không; giảm một yếu tố (tắt MCP thừa, cắt CLAUDE.md, `/compact`) rồi so sánh trước/sau.
2. **Compact vs clear (15'):** liệt kê 3 task buổi sáng; sau mỗi task quyết định liên quan → `/compact`, không → `/clear`. Cuối ngày ghi số lần `/compact`, `/clear`, auto-compact (mục tiêu: 0).
3. **Ngân sách context (nâng cao):** cho task 3-5 giờ ước tính CLAUDE.md + system (~10k), số file × 1k, web search × 3k, tool call, hội thoại; lên kế hoạch compact, subagent, MCP cần/tắt; so với thực tế.

## Tóm tắt

- Context 200k token là tài nguyên giới hạn; quản lý chủ động > phó mặc auto-compact.
- **`/context` (chẩn đoán) → `/compact` (đang làm dở) → `/clear` (đổi task).**
- Prompt cụ thể tiết kiệm context.
- 5 chiến thuật: prompt cụ thể → CLAUDE.md → subagent → skill → tắt MCP thừa.
