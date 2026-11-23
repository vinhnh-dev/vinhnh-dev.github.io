---
layout: page
title: Bài 2.8 - Subagents
permalink: /claude-code-101-lesson-2-8/
parent: Claude Code 101
grand_parent: Claude
nav_order: 10
---

# Bài 2.8: Subagents

**Module 3 — Persistent context & extension** · ~30 phút · Trung cấp

## Ý chính

*"Bạn nhận được câu trả lời mà không cần trải qua quá trình tìm kiếm."* Hỏi "auth endpoint ở đâu?": không subagent thì Claude đọc ~15 file, grep nhiều lần, tốn ~25.000 token và giữ chúng suốt phiên. Với subagent, quá trình đó diễn ra trong context riêng và bị loại bỏ; main chỉ nhận tóm tắt (~500 token).

## Subagent là gì

Agent chuyên biệt chạy trong **context window riêng (forked)**, độc lập với main. Nhận **system prompt tùy chỉnh** (từ file cấu hình) + **task description** (do main viết), làm việc tự chủ rồi trả **summary**; toàn bộ tool call trung gian bị discard.

**Đánh đổi:** main không thấy "hành trình" nên khó debug nếu subagent đi sai.

## 3 subagent tích hợp sẵn

| Subagent | Tool | Dùng cho | Context điển hình |
|---|---|---|---|
| **general-purpose** | Bash, Read, Write, Grep, Web search, MCP | Task nhiều bước, vừa khám phá vừa thực hiện; phạm vi chưa rõ | 50-150k |
| **Explore** | Read, Grep, Glob (read-only) | Tìm kiếm thuần: "hàm này ở đâu?", "ai dùng API này?" | 10-40k |
| **Plan** | Read, Grep, Glob (read-only) | Nghiên cứu trong Plan mode (Claude tự dùng khi `Shift+Tab`) | 20-60k |

## Tạo subagent tùy chỉnh (`/agents`)

Wizard: **Scope** (Project → `.claude/agents/`, User → `~/.claude/agents/`), **Purpose**, **Tools** (tick), **Color**; Claude sinh tên, mô tả, system prompt. Kết quả là file Markdown với YAML frontmatter (`name`, `description`, `tools`, `color`) rồi đến system prompt. Ví dụ `security-checker`: tools Read/Grep/Glob; prompt kiểm SQLi, XSS, secret hardcode, dependency CVE, lỗ hổng auth, IDOR; trả báo cáo theo mức độ (Critical/High/Medium/Low) + file:line + mô tả + cách sửa, tập trung kết quả hành động được, bỏ qua rủi ro lý thuyết không có bằng chứng.

**Project scope** (commit vào repo, cả team dùng) vs **User scope** (chỉ bạn, mọi dự án).

## Subagent vs main agent

| | Subagent | Main |
|---|---|---|
| Context | Forked, bỏ sau khi xong | Chia sẻ, tích lũy cả phiên |
| Khả năng hiển thị | Chỉ thấy tóm tắt | Thấy mọi tool call, mọi file đọc |
| Hợp với | Khám phá, task chỉ cần câu trả lời, đọc nhiều | Tương tác nhiều lượt, cần điều hướng |
| Can thiệp giữa chừng | Thấp (one-shot) | Cao |
| Debug | Khó | Dễ |

**Quy tắc nhanh:** chỉ cần kết quả cuối, không cần quá trình → subagent; đọc nhiều file nhưng câu trả lời ngắn → subagent; cần nhiều bước qua lại → main.

## Case study chính: COBOL modernization (AWS Mainframe demo)

Hệ thống thẻ tín dụng ~100 file (94 file COBOL từ thập niên 80), gần như không có tài liệu, không ai biết COBOL; mục tiêu: hiểu và chuyển sang Java mà không mất quy tắc nghiệp vụ.

- **Giai đoạn 1 (tài liệu):** tạo subagent `cobol-documentation-expert` (Read/Glob/Grep) — trích quy tắc nghiệp vụ, luồng dữ liệu WORKING-STORAGE ↔ PROCEDURE DIVISION, dependency (`CALL`), sơ đồ Mermaid, giải thích plain English. Giao: "phân tích 94 file trong `/src/cobol/`, xuất ra `/docs/cobol-analysis/`". Chạy ~1 giờ tự động: 94 file, hơn 100 trang, 23 sơ đồ, 847 quy tắc nghiệp vụ, 12 module tính toán rủi ro cao.
- **Giai đoạn 2 (migration):** dựa vào tài liệu, triển khai từng module Java; test kép (GNU COBOL chạy song song với bản mới) để xác minh kết quả từng bit. Kết quả: chính xác tuyệt đối từng bit.
- Chỉ khả thi vì subagent giữ 94 file COBOL ngoài context chính.

## Case study theo vai trò

| Vai trò | Ví dụ |
|---|---|
| Backend | Explore tìm mọi chỗ gọi `sendLegacyEmail()` trong 200k LOC, nhóm theo module, trả bảng `file:line` + context; main nhận 2 trang thay vì 47 file |
| DevOps | general-purpose đọc log 50MB, tìm pattern lỗi từ 02:47, theo luồng request, trả root cause + timeline 10 phút trước; main chỉ +800 token |
| Architect | 5 subagent song song đánh giá 5 service (coupling, cohesion, dependency graph, tech debt hotspot) → 1 trang mỗi service; ~15 phút thay vì 75 |
| Frontend | Explore kiểm a11y 30 component (aria, keyboard, contrast, focus) → 8 component FAIL kèm vi phạm cụ thể |
| Maintainer OSS | general-purpose + GitHub MCP phân loại issue chưa nhãn (bug/feature/question/duplicate, độ nghiêm trọng, module); **chỉ đề xuất, không tự gán nhãn** |

## Tùy chỉnh nâng cao

- **Persistent memory:** giữ trạng thái qua các phiên (di chuyển nhiều ngày); frontmatter dạng `persistent: true` nhưng tên key có thể đổi theo version, luôn kiểm tra `claude agents --help`.
- **Preload skills:** khóa `skills:` trong frontmatter; khác main, subagent **tải toàn bộ** skill vào context ngay khi khởi động (skill 500 dòng = 500 dòng chiếm context), nên cân nhắc.
- **Giới hạn tool:** read-only (Read/Grep/Glob) cho phân tích, audit, tài liệu; CLI-only (chỉ Bash); không web search cho phân tích offline.
- **System prompt chuyên biệt:** security (OWASP, "kẻ tấn công khai thác thế nào?"), performance (Big-O, N+1, cache, định lượng tác động), accessibility (WCAG 2.1 AA, trích dẫn tiêu chí cụ thể như 1.4.3).

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Subagent cho task cần tương tác qua lại (subagent one-shot, không ngắt giữa chừng được) | Để main xử lý để bạn điều hướng |
| Subagent có **tất cả** tool như main (mất ý nghĩa tách biệt) | Bộ tool tối thiểu đủ cho công việc |
| Không commit `.claude/agents/` | Commit; đây là công cụ của team |
| Spawn cho task < 5 phút (main làm xong trong 1-2 tool call) | Spawn có overhead; chỉ dùng cho việc đáng |
| Quên main không thấy hành trình → khó debug | Yêu cầu xác minh mẫu cụ thể sau khi nhận kết quả; task quan trọng thì chạy ở main lần đầu để xem hành trình |
| System prompt mơ hồ | Viết cụ thể → kết quả cụ thể |

## Mẹo nâng cao

- **4 subagent Explore song song:** API endpoints (`src/api/`), models (`src/models/`), background jobs (`src/jobs/`), event handlers (`src/events/`), mỗi cái trả `file:line` → gộp thành bản đồ kiến trúc: ~10 phút thay vì 40.
- **Chuỗi A nghiên cứu → B triển khai** (vd Explore đọc tài liệu Stripe API → general-purpose implement `src/payments/stripe.ts`).
- **Chọn đúng loại:** chưa chắc phạm vi → general-purpose; tìm kiếm thuần → Explore; Plan mode → Plan.
- **Subagent + skills** (vd `pr-security-reviewer` preload `owasp-top10`, `secure-code-review`) → review nhất quán, không bỏ sót danh mục.
- **Xác minh output khi có phát hiện nghiêm trọng:** "Đọc `src/api/users.ts` dòng 85-95 và xác nhận SQL injection có thật không" để tránh false positive.

## Bài tập

1. **Explore (15'):** *"Khởi chạy subagent Explore mô tả kiến trúc codebase trong 5 điểm: module chính, luồng dữ liệu chính, pattern chính (MVC? event-driven? microservices?)."* Ghi thời gian, `/context` trước và sau, độ chính xác của tóm tắt.
2. **Subagent tùy chỉnh (25'):** tạo qua `/agents` một trong hai: **Code reviewer** (Read/Grep/Glob; nhất quán với pattern hiện có, đặt tên, test coverage) hoặc **Security checker** (OWASP Top 10, validate input, auth). Đọc file ở `.claude/agents/`, thử "Spawn [tên] để review file [vừa sửa]", chỉnh system prompt nếu kết quả chưa đủ cụ thể.

## Tóm tắt

- **Subagent = forked context:** chạy riêng, chỉ trả tóm tắt, context chính sạch.
- **3 built-in:** Explore (tìm kiếm), Plan (nghiên cứu trong Plan mode), general-purpose (khám phá + hành động).
- **Tùy chỉnh qua `/agents`:** YAML frontmatter + system prompt, lưu ở `.claude/agents/` (commit) hoặc `~/.claude/agents/`.
- **One-shot:** không hợp task cần điều hướng nhiều lượt.
- **Song song = nhân tố tăng throughput** và giữ context chính gọn.
