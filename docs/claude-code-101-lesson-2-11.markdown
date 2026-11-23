---
layout: page
title: Bài 2.11 - Hooks
permalink: /claude-code-101-lesson-2-11/
parent: Claude Code 101
grand_parent: Claude
nav_order: 13
---

# Bài 2.11: Hooks — Kiểm soát hành vi của Claude Code

**Module 4 — Hệ sinh thái mở rộng** · ~30 phút · Trung cấp → Nâng cao

## Ý chính

Viết "Sau mỗi lần sửa file TypeScript, hãy chạy Prettier" vào CLAUDE.md chỉ đúng ~80%: đến chiều thứ Sáu trước release CI báo 47 file sai format, mất 40 phút chạy tay. CLAUDE.md và Skill đều **probabilistic** (model quyết định có làm hay không). **Hook** là **deterministic**: hệ thống Claude Code chạy script của bạn tại sự kiện chỉ định, 100% mỗi lần, không phụ thuộc "trí nhớ" của model.

> *"Nếu có điều gì cần xảy ra mọi lúc mà không được phép thất bại, đừng đặt nó vào prompt. Hãy đặt nó vào hook."*

| Cơ chế | Tính chất | Ai quyết định chạy |
|---|---|---|
| Hook | Deterministic, luôn chạy | Hệ thống Claude Code |
| CLAUDE.md | Xác suất | Model đọc, model quyết định |
| Skill | Xác suất | Model khớp yêu cầu, model quyết định gọi |
| MCP | Sẵn sàng khi cần | Model quyết định gọi tool |

## Vòng đời và 5 sự kiện

`UserPromptSubmit` → Claude xử lý, quyết định gọi tool → `PreToolUse` (exit 0 cho phép / exit 2 chặn) → tool chạy (Edit, Bash, Read...) → `PostToolUse` → Claude lặp hoặc kết thúc → `Stop`. `Notification` chạy khi Claude gửi thông báo.

| Sự kiện | Khi nào | Chặn được? | Ví dụ dùng | Matcher |
|---|---|---|---|---|
| **PreToolUse** | Trước khi tool chạy | Có (exit 2) | Chặn ghi file production, chặn `rm -rf`, validate input | Khuyến nghị |
| **PostToolUse** | Sau khi tool hoàn tất | Không | Auto-format, lint, audit log, chạy test | Khuyến nghị |
| **UserPromptSubmit** | Khi user gửi prompt, trước khi Claude xử lý | Không trực tiếp | Log prompt, chèn thêm context, validate | Không bắt buộc |
| **Stop** | Khi Claude trả lời xong | Không | Thông báo Slack, dọn file tạm, kích hoạt deploy | Không bắt buộc |
| **Notification** | Khi Claude gửi thông báo | Không | Định tuyến thông báo (macOS, Slack) | Không bắt buộc |

## Cấu hình

Trong `.claude/settings.json` (project, commit được) hoặc `~/.claude/settings.json` (user). Cấu trúc: `hooks` → tên sự kiện → danh sách `{ matcher, hooks: [{ type: "command", command }] }`.

```json
{
  "hooks": {
    "PostToolUse": [
      { "matcher": "Edit|MultiEdit|Write",
        "hooks": [{ "type": "command", "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/format-on-edit.sh" }] }
    ],
    "PreToolUse": [
      { "matcher": "Bash",
        "hooks": [{ "type": "command", "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/block-dangerous-commands.sh" }] }
    ],
    "Stop": [
      { "matcher": "",
        "hooks": [{ "type": "command", "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/notify-done.sh" }] }
    ]
  }
}
```

- **matcher:** regex khớp tên tool (`"Edit|MultiEdit|Write"`); rỗng `""` = khớp mọi tool call.
- **type:** hiện chỉ có `"command"`. **command:** lệnh shell hoặc đường dẫn script; dùng **`$CLAUDE_PROJECT_DIR`** (Claude Code inject), không đường dẫn tuyệt đối.
- Script nhận **JSON qua stdin**, vd `{"tool_name":"Edit","tool_input":{"file_path":"...","old_string":"...","new_string":"..."}}`; tín hiệu quan trọng nhất là **exit code**.

| Exit code | Hành vi | Ví dụ |
|---|---|---|
| **0** | Tiếp tục bình thường (cho phép tool call / PostToolUse tiếp) | Prettier chạy xong |
| **2** | **Chặn** tool call (chỉ PreToolUse); stderr được trả lại cho Claude làm phản hồi | Chặn ghi vào `infra/prod/*` |
| Khác (1, 3...) | Lỗi không chặn: hiện cho user nhưng Claude vẫn tiếp tục | Prettier không tìm thấy, bỏ qua |

Khi PreToolUse exit 2, Claude đọc stderr như lời giải thích bị từ chối rồi tự điều chỉnh (vd chuyển sang `infra/staging/`): một vòng phản hồi thông minh.

## Ví dụ 1: auto-format sau mỗi lần Edit (PostToolUse)

1. Tạo `.claude/hooks/format-on-edit.sh`: đọc stdin, lấy `tool_input.file_path` bằng `python3 -c`, thoát 0 nếu rỗng/không tồn tại; theo phần mở rộng chạy formatter (`ts|tsx|js|jsx|json|css|html|md` → `prettier --write`; `go` → `gofmt -w`; `py` → `ruff format` hoặc `black`); luôn `exit 0`.
2. `chmod +x .claude/hooks/format-on-edit.sh`.
3. Thêm vào `settings.json` (như mẫu ở trên), commit cả script và settings để cả team có cùng hook.
4. Kiểm tra: "Sửa `src/components/Button.tsx`, đổi className 'btn' thành 'button'" → file được định dạng ngay sau Edit, không cần nhắc, không còn tỷ lệ fail 20%.

## Ví dụ 2: chặn lệnh nguy hiểm (PreToolUse)

Script `block-dangerous-commands.sh` đọc `tool_name` và `tool_input.command` từ stdin; chỉ xử lý Bash; **exit 2 + thông điệp stderr** khi gặp: (1) `rm -rf`, (2) thao tác vào `infra/prod`, `production.env`, `.env.prod`, (3) `git push --force`/`-f`; còn lại exit 0. Có thể thêm matcher `Write|Edit|MultiEdit` trỏ tới `block-prod-writes.sh`. Khi Claude cố chạy `rm -rf /tmp/cache`, Claude nhận stderr "BLOCKED: 'rm -rf' không được phép..." và tự chuyển sang `rm -r /tmp/cache`.

## Hook vs Skill vs CLAUDE.md vs MCP

| | Hook | Skill | CLAUDE.md | MCP |
|---|---|---|---|---|
| Tính chất | Deterministic | Xác suất | Xác suất (có thể quên) | Sẵn sàng khi cần |
| Trigger | Sự kiện vòng đời | Model khớp yêu cầu | Tải khi phiên bắt đầu | Model quyết định gọi |
| Trong context? | Script nằm ngoài | Tải khi khớp | Luôn có | Định nghĩa tool luôn có |
| Tốt nhất cho | Đảm bảo 100% | Quy trình tái sử dụng | Quy tắc toàn dự án | Tích hợp ngoài |
| Ví dụ | Auto-format mỗi Edit | `/commit` theo chuẩn message | "Dùng pnpm, không npm" | GitHub, Linear, Slack |

**Quy tắc:** cần đảm bảo điều gì đó xảy ra → **Hook**; muốn Claude biết cách làm khi bạn hỏi → **Skill**; bối cảnh chung → **CLAUDE.md**; truy cập hệ thống ngoài → **MCP** (Bài 2.7, 2.9).

## Case study theo vai trò

| Vai trò | Hook |
|---|---|
| Backend | PostToolUse chạy `eslint --fix` mỗi khi sửa `.ts` → hết lỗi lint CI dù phiên dài 4 giờ |
| DevOps/Bảo mật | PreToolUse chặn mọi ghi vào `infra/prod/*`; stderr rõ ràng để Claude tự chọn `infra/staging/*` |
| Team kỹ thuật | `Stop` gửi Slack khi Claude xong (curl webhook `$SLACK_WEBHOOK_URL`): giao task 30+ phút, làm việc khác |
| Compliance | PreToolUse ghi mọi lệnh Bash vào audit log (timestamp, `$USER`, lệnh): bắt buộc quy định, không "quên" |
| Maintainer OSS | PostToolUse chạy test sau mỗi lần sửa file nguồn; fail thì ghi stderr, Claude tự sửa trước khi tiếp |
| Solo founder | Hook `say "Claude đã xong rồi..."` trên macOS, rời máy rồi nghe thông báo |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Script chạy quá lâu (> 10 giây; hook đồng bộ, chạy full test mỗi lần sửa thì chặn mỗi tool call) | Chạy test nhanh, hoặc tiến trình nền (`&`) + hook `Stop` thay vì PostToolUse |
| PreToolUse không xử lý input trống (tool call thiếu field → script treo/chặn sai) | Luôn `|| echo ""` và kiểm tra rỗng trước khi xử lý |
| Hardcode đường dẫn tuyệt đối (`/Users/john/...`) | `$CLAUDE_PROJECT_DIR/.claude/hooks/format.sh` |
| Không commit `settings.json` + script | Commit để cả team có cùng hook, mất lợi ích "deterministic cho cả team" nếu chỉ có mình bạn |
| Hardcode secret trong script (`SLACK_TOKEN=xoxb-...`) | Biến môi trường `$SLACK_WEBHOOK_URL` (local/CI secret) |
| Dùng hook cho việc nên là Skill (nhắc viết commit message đúng chuẩn) | Hook chỉ khi cần đảm bảo mỗi lần; còn lại Skill/CLAUDE.md |
| Matcher quá rộng (`""` chạy trên mọi tool call kể cả Read/Glob/Grep) | `Edit|MultiEdit|Write` cho formatter |

## Mẹo nâng cao

- Dùng `$CLAUDE_PROJECT_DIR` cho mọi đường dẫn trong script (log, `.env.local`...): chạy đúng dù cwd của Claude ở đâu.
- **Pipeline:** PreToolUse (validate/chặn) → PostToolUse (format, lint, log) → Stop (thông báo Slack).
- Script nên **idempotent** (Prettier, gofmt, ruff đều vậy); nếu ghi log thì tránh dòng trùng gây nhầm lẫn.
- **Test hook độc lập** trước khi bật: `echo '{"tool_name":"Edit","tool_input":{"file_path":"/tmp/test.ts"}}' | .claude/hooks/format-on-edit.sh`.
- **Điều kiện trong script**, không chỉ dựa matcher (vd chỉ format file trong `*/src/*`, không format test fixture).
- **Debug:** đổi matcher thành chuỗi không bao giờ khớp (`"DISABLED_Edit|MultiEdit"`) để so sánh hành vi và xác nhận hook có phải thủ phạm.
- Với sửa file nên bao gồm cả 3 tool: `Edit`, `MultiEdit`, `Write`. Gõ `/hooks` trong phiên để xem/sửa hook tương tác.

## Bài tập

1. **Auto-format hook (~25'):** xác định formatter (Prettier, eslint, ruff, gofmt) → tạo `.claude/hooks/format-on-edit.sh` → thêm vào `.claude/settings.json` → test stdin giả trước → bảo Claude sửa 5 file khác nhau, kiểm tra file nào cũng được format → commit script + settings.
2. **PreToolUse chặn (~20'):** chọn một mẫu nguy hiểm (`rm -rf`, ghi `.env`, sửa `infra/prod/`, `git push --force`) → tạo `.claude/hooks/block-dangerous.sh` exit 2 khi vi phạm → test case từ chối (exit 2 + đúng stderr) và case cho phép (exit 0) → bật trong settings, thử với Claude thật. Bonus: nhờ Claude làm hành động bị chặn để xem nó tự điều chỉnh dựa trên stderr.

## Tóm tắt

- **Hook là cơ chế duy nhất deterministic:** quy tắc cứng, không phải gợi ý.
- **PostToolUse:** đảm bảo việc sau hành động (format, lint, log, test) chạy mọi lần.
- **PreToolUse:** chặn bằng `exit 2` + stderr → Claude tự điều chỉnh.
- **Stop / Notification:** thông báo bất đồng bộ.
- **Commit `settings.json` + script** để cả team có cùng hook.
