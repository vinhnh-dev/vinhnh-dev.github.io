---
layout: page
title: Bài 2.2 - Cài đặt Claude Code
permalink: /claude-code-101-lesson-2-2/
parent: Claude Code 101
grand_parent: Claude
nav_order: 4
---

# Bài 2.2: Cài đặt Claude Code

**Module 1 — Nền tảng** · ~20 phút · Cơ bản

## Ý chính

Cài đặt chỉ ~30 giây: một dòng lệnh, gõ `claude`, chọn theme, đăng nhập. Có **5 giao diện** (surface) tùy cách làm việc.

## 5 surface

| Surface | Cài bằng | Phù hợp | Ghi chú |
|---|---|---|---|
| **Terminal** (khuyến nghị) | curl / npm / brew / winget | Dev mọi cấp | Tính năng mới ra ở đây **đầu tiên** |
| **VS Code** | Extension Marketplace | Dev dùng VS Code | Inline diff, sidebar |
| **JetBrains** | Plugin Marketplace | IntelliJ, Android Studio, GoLand, PyCharm... | Terminal Claude Code chạy trong IDE |
| **Desktop** | Tải từ claude.ai (Mac/Windows) | Người không quen terminal, designer | Task nền, nhiều cửa sổ |
| **Web** (`claude.ai/code`) | Trình duyệt | Team phân tán, chạy song song | Chạy trên hạ tầng Anthropic; **chỉ repo GitHub**; có tích hợp Slack (12/2025) |

## Cài ở Terminal

| Hệ điều hành | Lệnh | Tự động cập nhật |
|---|---|---|
| macOS / Linux / WSL | `curl -fsSL https://claude.ai/install.sh \| sh` (khuyến nghị) | ✅ |
| Có Node.js | `npm install -g @anthropic-ai/claude-code` | ✅ |
| macOS Homebrew | `brew install claude-code` | ❌ phải `brew upgrade claude-code` |
| Windows PowerShell | `Invoke-RestMethod https://claude.ai/install.ps1 \| Invoke-Expression` | ✅ |
| Windows CMD | `curl -fsSL https://claude.ai/install.bat -o install.bat && install.bat` | ✅ |
| Windows winget | `winget install Anthropic.ClaudeCode` | ❌ phải `winget upgrade Anthropic.ClaudeCode` |

Kiểm tra: `claude --version`. Nếu lệnh `claude` không nhận, mở terminal mới để nạp lại PATH.

## VS Code và JetBrains

- **VS Code:** `Ctrl/Cmd+Shift+X` → tìm "Claude Code" → chọn extension của **Anthropic có dấu tích xanh xác minh** → Install → restart → `Ctrl/Cmd+Shift+P` → "Claude Code: Open in New Tab". Mẹo: tắt UI trong settings để chạy trong terminal tích hợp.
- **JetBrains:** Settings → Plugins → Marketplace → "Claude Code" (của Anthropic) → restart → biểu tượng Claude ở thanh bên.

## Lần đầu trên macOS

1. Cài bằng curl, kiểm tra `claude --version`
2. **`cd` vào thư mục dự án rồi mới `claude`**
3. Chọn theme và phương thức đăng nhập
4. Prompt thử: *"mô tả cơ sở mã này trong 5 câu"*

⚠️ Claude có quyền truy cập thư mục bạn chạy lệnh và mọi thư mục con. Chạy từ `$HOME` = lộ được `.ssh/`, `.aws/credentials`, `.env`... Luôn chạy trong thư mục dự án cụ thể.

## Xác thực và gói

| Lựa chọn | Dùng cho |
|---|---|
| **Pro** (~$20/tháng) | Cá nhân, indie, người mới, đủ cho học tập |
| **Max** (~$100/tháng trở lên) | Dùng nhiều, nhiều giờ/ngày; thường rẻ hơn API key khi dùng nặng (~$50/tháng) |
| **API key** | Kiểm soát chi phí chi tiết, tích hợp quy trình tự động |
| **Enterprise (SSO)** | Công ty có tài khoản Enterprise: **bắt buộc** dùng, không dùng Pro/Max cá nhân |

Đang học và chưa quyết định → bắt đầu với Pro.

## Case study

| Vai trò | Cách cài | Lý do |
|---|---|---|
| Indie hacker | curl + Pro, dùng terminal | Đơn giản, rẻ (~$20/tháng) |
| Tech lead (fintech 8 người) | Enterprise cho cả team, terminal, CLAUDE.md ở root repo (Bài 2.7) | Audit log, thanh toán theo tổ chức, compliance |
| Designer (như Megan) | Claude Desktop + Pro, chọn thư mục repo | Không cần gõ lệnh; tinh chỉnh CSS/spacing/màu |
| Android dev | Plugin JetBrains trong Android Studio | Không rời IDE: giải thích code, tạo ViewModel theo mẫu có sẵn |
| Team remote | Web `claude.ai/code` + GitHub, các phiên song song | Không cần môi trường local, máy yếu vẫn dùng được, giao việc từ Slack |

## Mẫu sai cần tránh

| Sai | Hậu quả / Đúng |
|---|---|
| Cài bằng Homebrew/winget mà không biết không tự cập nhật | Dùng bản cũ, thiếu tính năng. Ưu tiên curl/npm; nếu dùng brew thì đặt nhắc `brew upgrade` hằng tuần |
| Chạy `claude` từ `$HOME` | Lộ file nhạy cảm. Luôn `cd ~/projects/my-app && claude` |
| Dùng Pro cá nhân khi công ty có Enterprise | Code công ty đi qua tài khoản cá nhân, không audit log, không thu hồi được. Xin thông tin Enterprise từ IT/CTO, chọn SSO |
| Cài extension VS Code không phải của Anthropic | Có thể giả mạo, gửi prompt tới server lạ. Chỉ cài khi author = Anthropic + tích xanh |
| Chạy terminal + IDE cùng lúc trên một repo | Xung đột file, context lỗi thời. Dùng một surface; muốn song song thì dùng **git worktree**: `git worktree add ../my-app-feature-b feature-b` rồi chạy `claude` trong thư mục đó |

## Bài tập

1. **Cài và prompt đầu (10')**: chọn surface (không chắc thì Terminal) → cài → `cd` vào repo nhỏ (hoặc `git clone https://github.com/anthropics/anthropic-quickstarts`) → `claude` → *"mô tả cơ sở mã này trong 5 câu"*. Ghi: đọc bao nhiêu file? mô tả đúng không? mất bao lâu?
2. **Khám phá mode bằng `Shift+Tab` (5')**: hỏi *"what files are in this project?"*, nhấn `Shift+Tab` một lần, hỏi lại, so sánh (xem chi tiết ở Bài 2.3).

## Tóm tắt

- Cài **30 giây**: curl (macOS/Linux), PowerShell (Windows).
- **5 surface:** Terminal (nhanh nhất), VS Code, JetBrains, Desktop, Web (chỉ GitHub, song song).
- Chọn xác thực đúng: Pro / Max / API key / Enterprise, không trộn lẫn.
- **Luôn `cd` vào thư mục dự án**, tuyệt đối không chạy từ `$HOME`.
- Dùng curl/npm để tự cập nhật; brew/winget phải cập nhật tay.
