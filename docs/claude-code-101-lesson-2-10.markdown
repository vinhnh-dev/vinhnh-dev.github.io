---
layout: page
title: Bài 2.10 - MCP
permalink: /claude-code-101-lesson-2-10/
parent: Claude Code 101
grand_parent: Claude
nav_order: 12
---

# Bài 2.10: MCP — Kết nối Claude Code với thế giới bên ngoài

**Module 4 — Hệ sinh thái mở rộng** · ~30 phút · Trung cấp

## Ý chính

Context không chỉ nằm trong codebase mà còn ở Linear, Slack, Notion, Figma, Postgres production, Datadog, Jira... **MCP (Model Context Protocol)** là chuẩn mở để Claude Code gọi tool có cấu trúc, thời gian thực, kiểm tra được thay vì copy-paste.

Ví dụ: trước MCP, Khoa copy ticket Linear MEN-12 (400 từ) vào prompt (~5 phút, và ticket đổi thì bản copy lỗi thời). Sau khi thêm Linear MCP: `xem xét PR #238 theo yêu cầu của ticket MEN-12` → Claude tự lấy mô tả, tiêu chí chấp nhận, comment mới nhất của PM, đối chiếu với diff trong ~30 giây.

**Tools** là nền tảng của agentic AI (grep, đọc file, bash, web search đều là tool); MCP mở rộng ra ngoài máy bạn (gọi API Linear, truy vấn Postgres, post Slack). Giao tiếp qua JSON-RPC: Claude gọi tool → MCP server thực thi → trả kết quả.

## 2 loại server

| | HTTP server | Stdio server |
|---|---|---|
| Chạy ở đâu | Cloud của nhà cung cấp | Process cục bộ trên máy bạn |
| Setup | Thấp (URL + xác thực) | Trung bình (cài package/binary) |
| Mạng | Cần internet | Không cần |
| Bảo mật | OAuth2 / API key | Chỉ truy cập tài nguyên local |
| Độ trễ | Cao hơn | Thấp (IPC local) |
| Phù hợp | SaaS (Linear, Slack, Notion, Context7, Figma, GitHub) | DB local, tool nội bộ, tool dev |

## Thêm server

```bash
claude mcp add linear https://mcp.linear.app/sse                   # HTTP
claude mcp add postgres -- npx @modelcontextprotocol/server-postgres postgresql://localhost/mydb  # stdio
claude mcp add --scope project linear https://mcp.linear.app/sse   # chọn scope
```

Wizard hỏi xác thực (OAuth2 khuyến nghị, hoặc API key), liệt kê các tool (vd Linear: 12 tool, ~8k token).

**Quản lý bằng `/mcp`:** xem server, số tool, ước lượng token, trạng thái; `/mcp disable <tên>`, `/mcp enable <tên>`, `/mcp reconnect <tên>`. Cũng có `claude mcp list` / `claude mcp remove`.

## 3 scope

| Scope | Lưu ở | Ai dùng | Secret |
|---|---|---|---|
| **Local** | `~/.claude/mcp.json` (mục theo dự án) | Chỉ bạn, dự án này | API key trong cấu hình local |
| **User** | `~/.claude/mcp.json` (toàn cục) | Chỉ bạn, mọi dự án | API key local |
| **Project** | `.mcp.json` ở root repo | Cả team (qua version control) | **Dùng biến môi trường**, không hardcode key |

Gợi ý: Local = MCP DB dev cục bộ; User = Linear/Slack cá nhân dùng ở mọi dự án; Project = Linear, Context7, GitHub MCP cả team cần. Ví dụ `.mcp.json`: `linear` (type `http`, `env: { LINEAR_API_KEY: "${LINEAR_API_KEY}" }`), `context7` (http), `postgres` (type `stdio`, command `npx`, args `["@modelcontextprotocol/server-postgres", "${DATABASE_URL}"]`).

## Chi phí context của MCP (quan trọng nhất bài này)

Khi khởi động, Claude Code tải **toàn bộ định nghĩa tool** của mọi server đang bật, dù bạn không dùng.

| Server | Tools | Token | Dùng thực tế |
|---|---|---|---|
| Linear | 12 | ~8k | Hằng ngày |
| Context7 | 3 | ~2k | Khi cần docs |
| Slack | 6 | ~4k | Vài lần/tuần |
| Figma | 15 | ~12k | Chỉ khi thiết kế |
| Jira | 10 | ~7k | Team không dùng nữa |
| **Tổng** | 46 | **~33k (~16%)** | |

Nếu hôm nay chỉ làm backend: Linear + Context7 (~10k) là đủ; Figma + Jira là 19k lãng phí (> 57%). `/mcp` để audit, `/mcp disable figma` / `jira` rồi khởi động lại phiên → overhead còn ~14k (7%).

**Tool search tự động:** khi MCP > 10% context, Claude Code chuyển sang chế độ tìm kiếm tool theo nhu cầu, nhưng **chưa ổn định** (có thể bỏ sót tool phù hợp) → đừng dựa vào nó, chủ động tắt (Bài 2.5, chiến thuật 5).

## MCP vs CLI vs Skill

- Dịch vụ có CLI tốt, có tài liệu (`gh`, `aws`, `gcloud`, `kubectl`, `heroku`) → **dùng CLI** (ưu tiên số một: không tốn context, Claude đã hiểu rõ).
- Không có CLI nhưng có MCP chính thức (Linear, Figma, Notion, Datadog) và bạn dùng thường xuyên → **thêm MCP** (scope project hoặc user); không thường xuyên hoặc việc một lần → Skill hoặc gọi API thủ công.
- Không có CLI lẫn MCP → có mẫu lặp lại thì viết Skill với mẫu gọi API; tạm thời thì nhờ Claude viết lệnh `curl`/script tạm.

| | CLI | MCP server | Skill |
|---|---|---|---|
| Chi phí context | 0 | Cao (2-15k/server, luôn có) | Thấp (chỉ khi dùng) |
| Cài đặt | Có sẵn | Cần `claude mcp add` + xác thực | Viết 1 file Markdown |
| Dữ liệu realtime | Qua lệnh CLI | Tự động qua tool call | Cần bạn cung cấp |
| Phù hợp | GitHub, AWS, k8s, DB migration | Linear, Slack, Figma, Notion | Quy trình lặp lại, checklist |

**Ví dụ nội bộ Anthropic:** thay vì viết MCP cho tool CLI nội bộ `cu`, chỉ ghi vào CLAUDE.md các lệnh chính (`cu task list`, `cu task create "title" --project X`, `cu deploy --env staging`) → Claude tự biết khi nào gọi, không tốn token định nghĩa tool ("CLI > MCP" khi CLI đã đủ tốt).

## Ví dụ: Linear + Context7 + Slack cho team 5 người (Next.js + Postgres)

1. `claude mcp add --scope project linear https://mcp.linear.app/sse` (OAuth qua trình duyệt) → lấy ticket, tiêu chí chấp nhận, comment; chuyển trạng thái; comment lên ticket.
2. `claude mcp add --scope project context7 https://mcp.context7.com/sse` (không cần xác thực) → docs Next.js, React, Postgres, package npm luôn cập nhật, không bịa API cũ.
3. `claude mcp add --scope user slack https://mcp.slack.com/sse` (user scope vì token Slack là cá nhân).
4. Linear và Context7 ở project scope tự vào `.mcp.json` → `git add .mcp.json && git commit -m "chore: ..."`. Slack không vào file đó, mỗi người tự đăng nhập.
5. Thành viên mới clone repo và chạy `claude`: Claude Code đọc `.mcp.json`, hỏi cài các server, họ xác thực Linear một lần là xong.
6. Cuối sprint chỉ còn viết code: `/mcp` → `/mcp disable slack` → giải phóng ~4k token.

## Case study theo vai trò

| Vai trò | Dùng MCP thế nào |
|---|---|
| Backend | Postgres MCP (stdio, scope local) + Plan mode: `describe_table`, `list_indexes` → đề xuất composite index để tối ưu query chậm, không cần copy schema |
| DevOps/SRE | Datadog + PagerDuty MCP (scope user): p99 tăng đột biến 01:43 UTC trùng deploy v2.3.1, nghi cạn connection pool; ~2 phút, không phải mở 3 tab |
| Frontend | Figma MCP lấy frame "Button variants" trực tiếp (màu, spacing, radius, hover) → khớp thiết kế 100%, không đo pixel |
| PM | Linear + Notion MCP: lấy issue hoàn thành của sprint + mẫu tóm tắt → báo cáo hoàn chỉnh |
| Marketing | Google Analytics + Slack MCP: số liệu tuần so với tuần trước → đăng #marketing-metrics |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Bật mọi MCP "phòng khi cần" (12 server = 60k = 30% context) | `.mcp.json` chỉ chứa thứ cả team cần; tắt riêng cái không dùng trong phiên |
| Dùng MCP khi CLI đã đủ (github-mcp ~10k token trong khi có `gh`) | GitHub → `gh`, AWS → `aws`, K8s → `kubectl`; MCP chỉ khi không có CLI tốt |
| Hardcode API key trong `.mcp.json` (lộ cho cả team, vào git history) | `"LINEAR_API_KEY": "${LINEAR_API_KEY}"` + key thật trong `.env` gitignored hoặc CI secret |
| Không làm mới OAuth token (401 giữa phiên) | `/mcp reconnect linear` trước phiên dài; nhắc xác thực lại hàng tháng (token ~30-90 ngày) |
| MCP "zombie" (Jira MCP vẫn bật 6 tháng sau khi chuyển sang Linear) | `/mcp` mỗi tháng, tắt cái không dùng |
| Thêm MCP cho việc một lần (Notion MCP chỉ để lấy ghi chú họp hôm qua) | Skill nhỏ gọi API bằng `curl`, hoặc copy-paste |

## Mẹo nâng cao

- **MCP stdio cho tool nội bộ** không có API công khai: `claude mcp add --scope project internal-crm -- python3 ./scripts/crm-mcp-server.py`.
- **Repo mẫu theo stack** (`nextjs-starter/.mcp.json`, `python-api/.mcp.json`, `mobile-react-native/.mcp.json`...) để project mới có MCP chuẩn của tổ chức.
- **Tách môi trường dev/prod** bằng hai server (`postgres-dev`, `postgres-prod`) với biến môi trường khác nhau để không nhầm.
- **MCP + Skill** (vd skill `/sprint-review` điều phối Linear → GitHub → Slack → viết review → đăng #engineering): 10 phút thủ công → 30 giây.
- **Giao việc nặng MCP cho subagent** (vd báo cáo quý từ Linear + GitHub + Datadog + Slack + GA) → context chính chỉ nhận 1 trang tóm tắt (Bài 2.8).
- **Audit hằng tháng (5 phút):** `/mcp` → tắt server không dùng → thêm server cho workflow mới → gia hạn OAuth sắp hết → xóa mục lỗi thời trong `.mcp.json`.

## Bài tập

1. **Khám phá MCP (15'):** vào `claude.com/connectors`, chọn 2-3 server hợp stack (web: Context7 + Linear/Jira + GitHub; backend/data: Postgres/MySQL; DevOps: Datadog/Grafana + PagerDuty; mobile: Figma + Linear); `claude mcp add --scope local <tên> <url>`; thử trong phiên (vd với Context7: "Tìm API mới nhất của React Hook useCallback, dùng context7"); ghi thời gian thêm và tool có hoạt động không.
2. **Audit context cost (10'):** `/mcp`, điền bảng (Server | Tools | Token | Dùng lần cuối | Giữ?), tắt ≥ 1 server rồi so overhead trước/sau.

## Tóm tắt

- **MCP là chuẩn mở:** để Claude Code hành động trong dịch vụ ngoài, không chỉ trả văn bản.
- **2 loại server:** HTTP (cloud, cần xác thực) và stdio (local, không cần mạng).
- **3 scope:** local / user / project (`.mcp.json`); key luôn qua biến môi trường.
- **MCP tốn context kể cả khi không dùng** (2-15k/server): audit `/mcp` định kỳ, tắt zombie.
- **CLI > MCP khi CLI đã tốt**; MCP hợp dịch vụ không có CLI mà bạn dùng hằng ngày.
