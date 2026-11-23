---
layout: page
title: Bài 2.6 - Code review & Git workflow
permalink: /claude-code-101-lesson-2-6/
parent: Claude Code 101
grand_parent: Claude
nav_order: 8
---

# Bài 2.6: Code review và quy trình Git

**Module 2 — Workflow chuyên nghiệp** · ~25 phút · Trung cấp

## Ý chính

Review thường là nút thắt (PR chờ 2-3 ngày, lọt bug). Với **subagent reviewer + GitHub Actions**, ví dụ team 5 người giảm thời gian merge từ 5-7 ngày xuống 4-6 giờ.

## Subagent code reviewer

Claude vừa viết code thì thiên vị (như tác giả tự hiệu đính). Reviewer chạy trong **context mới**, chỉ có tool **read-only** (Bash đọc, Grep, Glob), không Write/Edit/Execute; flag vấn đề kèm `file:line` và trả tóm tắt, không tự sửa.

**Tạo qua `/agents`**, tên `code-reviewer`. Prompt: người review nghiêm khắc nhưng mang tính xây dựng, kiểm theo thứ tự: (1) bảo mật (injection, secret hardcode, bypass auth, giải mã không an toàn), (2) lỗi logic (off-by-one, null, race, edge case), (3) test coverage, (4) style/nhất quán với CLAUDE.md, (5) hiệu năng (N+1, rò rỉ bộ nhớ). Định dạng: `[CRITICAL|WARNING|INFO] file.ts:42 — mô tả + đề xuất` và 3-5 câu tổng kết.

Lưu tại `.claude/agents/code-reviewer.md` và **commit vào repo** để cả team dùng cùng một reviewer (`git add .claude/agents/code-reviewer.md && git commit -m "chore: ..."`).

Dùng trước khi push: *"Khởi chạy subagent code-reviewer kiểm tra thay đổi của nhánh hiện tại so với main, tập trung bảo mật và lỗi logic."*

| | Subagent reviewer | Người review | Lint/CI |
|---|---|---|---|
| Tốc độ | 2-5 phút | 2-3 ngày | 30 giây |
| Độ sâu | Cao (logic, bảo mật, ngữ cảnh) | Rất cao (kiến trúc, nghiệp vụ) | Thấp (cú pháp, format) |
| Hiểu nghiệp vụ | Trung bình | Cao | Không |
| Chi phí | Token (vài cent/lần) | Thời gian senior | ~0 |
| Dùng lý tưởng | Vòng kiểm tra đầu | Quyết định kiến trúc, duyệt cuối | Áp dụng style, thay đổi gây lỗi |

Ba loại bổ sung nhau: Lint/CI chạy trước, subagent sâu hơn, người chỉ làm bước cuối (giá trị cao, nguồn lực khan hiếm).

## `/commit-push-pr`

Thay chuỗi thủ công `git add -p` / `commit` / `push` / `gh pr create` (5-10 phút, hay vội). Một lệnh (~20-30 giây): xem diff → tạo conventional commit (nêu *lý do*) → stage chỉ file liên quan → push đúng branch → tạo PR (tiêu đề, mô tả, link issue) → (nếu có Slack MCP và khai báo kênh trong CLAUDE.md, ví dụ "đăng link PR vào #engineering-prs") đăng Slack.

## `--from-pr`: quay lại PR không mất context

Khi Claude tạo PR bằng `gh pr create`, phiên được liên kết với số PR. Sau đó: `claude --from-pr 247` → lấy PR, diff, mô tả, comment và lịch sử hội thoại → làm tiếp. Dùng cho: xử lý review comment ("sửa các CRITICAL trước"), CI fail vài ngày sau, yêu cầu thêm của reviewer (vd thêm structured logging theo `logger.ts`).

## GitHub Actions: `@claude` trong PR

`/install github-action` (chọn repo, trigger: comment PR/issue assign/PR review, quyền đọc repo + ghi PR/commit) → tạo `.github/workflows/claude.yml`. Sau đó ai cũng có thể comment `@claude ...`: Claude đọc context PR, sửa code, commit vào nhánh PR và trả lời comment.

| Claude làm được | Không làm được |
|---|---|
| Đọc code, tạo PR từ issue, thêm commit vào PR, trả lời comment, review | Merge PR, xóa branch, đổi cài đặt repo, truy cập secret ngoài cấu hình |

**Demo Quiz app:** 2 issue ("50/50 power-up", "timer 30 giây") → `@claude` ở cả hai → Claude tạo 2 PR riêng, tự viết test, maintainer review và merge: < 1 giờ.

## Ví dụ team 5 người

1. **Setup (một lần):** lead tạo `code-reviewer`, commit, `/install github-action`.
2. **Dev A:** code xong → gọi reviewer → báo CRITICAL (`upload.ts:34` thiếu kiểm tra kích thước) và WARNING (`:67` chỉ kiểm tra extension, không kiểm tra MIME thật) → sửa ngay (5MB, `file-type`) → `/commit-push-pr` (PR liên kết issue #89, thông báo Slack).
3. **Dev B** comment `@claude` (hiển thị % trên ProgressBar; thêm analytics event `profile_avatar_upload_success`) → Claude sửa trong ~4 phút → B approve.
- **Tổng ~2h40:** A code 1h30, reviewer 10', B review 20', Claude fix 5', approve 5'. Cách cũ: 3-5 ngày, bảo mật lọt qua.

## Case study theo vai trò

| Vai trò | Cách dùng |
|---|---|
| Indie dev | Reviewer là "đôi mắt thứ hai"; `/commit-push-pr` sau mỗi feature; `@claude` thêm test khi tự review PR |
| Team 5+ | Reviewer là bước bắt buộc trên CI; người chỉ lo kiến trúc, nghiệp vụ; `@claude` sửa kỹ thuật vặt; thời gian review của senior giảm ~60% |
| Maintainer OSS | `@claude` review PR contributor theo CONTRIBUTING.md; contributor có phản hồi trong vài phút |
| Designer | Reviewer chuyên a11y (WCAG 2.1 AA: aria-label, contrast, focus ring); 8-10 PR/ngày |
| DevOps/Platform | Reviewer có checklist hạ tầng (Terraform không hardcode secret, K8s resource limit/probe/securityContext); `@claude` giải thích thay đổi Terraform cho người không chuyên |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Reviewer có tool ghi/sửa | Reviewer chỉ đánh dấu; agent chính quyết định sửa; chạy lại reviewer để verify |
| Bỏ hẳn human review ("Claude duyệt rồi, merge") | Reviewer = vòng đầu; người = vòng cuối (Claude không biết ràng buộc nghiệp vụ bất thành văn, hướng kiến trúc, bối cảnh team) |
| Không commit `.claude/agents/` | Commit để cả team nhất quán |
| Auto-merge sau khi Claude review | Vẫn cần người approve trước khi vào main/production |
| `@claude` cho việc phi-code (phân tích số liệu Q3, chiến lược) | `@claude` cho code, review, sửa lỗi, giải thích code; research/phân tích dùng Claude Code trong terminal |

## Mẹo nâng cao

- **Reviewer chuyên biệt:** `security-reviewer.md` (SQLi, XSS, secret hardcode, IDOR, thiếu authz, giải mã không an toàn, path traversal), `accessibility-reviewer.md`, `performance-reviewer.md`; chạy **song song** rồi gộp: CRITICAL phải sửa, WARNING nên sửa, INFO để sau.
- **Hook PostToolUse chạy ESLint** sau mỗi lần Edit/Write (Bài 2.11): reviewer khỏi phải flag lỗi style.
- **`--from-pr` khi CI fail không liên quan:** "CI đã ổn, chạy lại test; nếu fail thì debug."
- **Quy trình PR trong CLAUDE.md:** trước `/commit-push-pr` luôn chạy code-reviewer, sửa CRITICAL; chạy security-reviewer nếu đụng auth/thanh toán.

## Bài tập

1. **Tạo reviewer (15'):** `/agents` → `code-reviewer` (chỉ read-only) → chạy trên diff hiện tại → commit file. Ghi: bao nhiêu issue, có CRITICAL không, có đồng ý không.
2. **GitHub Actions (20'; cần quyền admin repo):** `/install github-action` → push `claude.yml` → comment trên PR `@claude giải thích PR này trong 3 gạch đầu dòng cho stakeholder không chuyên` → (nâng cao) `@claude thêm test đơn giản cho chức năng chính`. Ghi thời gian phản hồi, commit có được thêm không, chất lượng.

## Tóm tắt

- **Subagent reviewer = góc nhìn mới, không thiên vị**; read-only, commit vào repo.
- **`/commit-push-pr`:** một bước từ code đến PR.
- **`--from-pr`:** tiếp tục không mất context.
- **`@claude` trong GitHub Actions:** giao việc code ngay trong PR, không cần mở terminal.
- **Người review vẫn là bước cuối.**
