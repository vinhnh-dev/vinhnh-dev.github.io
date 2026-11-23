---
layout: page
title: Claude Code 101 - Tổng quan
permalink: /claude-code-101-overview/
parent: Claude Code 101
grand_parent: Claude
nav_order: 2
---

# Claude Code 101 — Tổng quan khóa học

**Phiên bản:** v1.0 (tiếng Việt, cho developer và builder) · ~5-7 giờ đọc + 3-5 giờ thực hành = 8-12 giờ

## Ý chính

Nhiều developer dùng AI như "công cụ tìm kiếm thông minh": copy code vào tab chat, dán kết quả ngược lại IDE, lặp lại bối cảnh mỗi phiên. Thiếu hai thứ:

- **Bối cảnh liên tục:** AI hiểu dự án, nhớ quyết định đã thống nhất, làm việc trực tiếp trên codebase thay vì copy-paste.
- **Agentic loop:** AI tự chạy lệnh, đọc file, kiểm tra, sửa lỗi, commit mà không cần bạn theo từng bước.

**Claude Code** là công cụ lập trình tự động chạy trên terminal: đọc toàn bộ codebase, chạy bash, gọi tool, làm việc liên tục trên nhiều file.

## Sau khóa học bạn sẽ

| Nhóm | Nội dung |
|---|---|
| **Tư duy** | Giải thích agentic loop · áp dụng **EPCC** (Explore → Plan → Code → Commit) thay vì lao vào code · phân biệt giao quyền có trách nhiệm và liều lĩnh |
| **Kỹ năng** | Cài Claude Code (terminal, VS Code, JetBrains, Desktop, Web) · viết prompt cụ thể, tiết kiệm context · dùng Plan mode · quản lý context chủ động (`/context`, `/compact`, `/clear`) |
| **Mở rộng** | `CLAUDE.md` · subagent chạy song song · slash command cho việc lặp lại · MCP (Linear, Slack, GitHub) · hook tự động hóa |

## Lộ trình 11 bài (+ quiz)

| Module | Bài |
|---|---|
| **1. Nền tảng** | 2.1 Cách Claude Code hoạt động (agentic loop, tools, permissions) · 2.2 Cài đặt · 2.3 Prompt đầu tiên (permission modes, Plan mode) |
| **2. Workflow chuyên nghiệp** | 2.4 **EPCC** (bài quan trọng nhất) · 2.5 Quản lý context (`/compact`, `/clear`, 5 chiến thuật) · 2.6 Code review và Git workflow (subagent reviewer, GitHub Actions) |
| **3. Persistent context & extension** | 2.7 `CLAUDE.md` (3 tầng, `/init`, `@`) · 2.8 Subagents (forked context, parallel tasks) |
| **4. Hệ sinh thái** | 2.9 Skills & Slash commands · 2.10 MCP (HTTP vs stdio, chi phí context, `claude mcp add`) · 2.11 Hooks (lifecycle events, exit codes) |
| **Final** | 2.12 Quiz tổng hợp (15-20 câu) |

## Cách học: quy tắc 70/20/10

- **70% thực hành:** mở terminal, gõ lệnh, làm ít nhất 1 việc thật mỗi bài
- **20% học từ người khác:** case study theo vai trò (backend, DevOps, designer, indie hacker)
- **10% lý thuyết:** agentic loop, context window, MCP, chỉ cần hiểu đủ để áp dụng

**Lộ trình theo nhóm:**

| Bạn là | Nên đọc |
|---|---|
| Developer mới với agentic coding | Tuần tự từ 2.1; đừng bỏ 2.1-2.2; đọc 2.4 (EPCC) ít nhất 2 lần |
| Đã dùng Cursor/Copilot/Codeium | 2.1 (đổi mô hình) → 2.4 (EPCC) → 2.5 (context); 2.7 (CLAUDE.md) và 2.8 (subagents) là thứ công cụ cũ chưa có |
| Tech lead triển khai cho cả đội | 2.4, 2.7, 2.8, 2.11 (hook để đảm bảo tuân thủ); thêm 2.10 nếu dùng tool nội bộ, 2.9 để chuẩn hóa quy trình; quiz 2.12 dùng kiểm tra onboarding |

## Chuẩn bị

- **Bắt buộc:** terminal bất kỳ · Node.js ≥ 18 (`node --version`) · tài khoản Claude **Pro/Max hoặc API key** (Free không hỗ trợ; chi phí thực tế ~$20-50/tháng khi dùng nhiều, hoặc Claude Max)
- **Nên có:** Git cơ bản · IDE quen thuộc · một dự án nhỏ để thực hành (~100-1000 file)
- **Không cần:** kiến thức AI/ML, kinh nghiệm prompt engineering, kinh nghiệm công cụ AI khác

## 3 nguyên tắc xuyên suốt

1. **"Đừng viết code vội."** Dùng Claude Code để *hỏi về codebase* trước (kiến trúc, pattern, convention), rồi mới viết (xem 2.4).
2. **"Hãy là PM của Claude."** Bạn chỉ định yêu cầu rõ ràng, review output, đưa feedback, quyết định hướng đi. Mơ hồ thì Claude sẽ đoán, và đoán không phải lúc nào cũng đúng.
3. **"Nếu việc đó phải luôn xảy ra, gắn vào hook."** Chọn đúng công cụ mở rộng:

| Công cụ | Dùng cho |
|---|---|
| **CLAUDE.md** | Điều Claude cần biết mọi lúc (convention, lệnh build) |
| **Skill** | Quy trình lặp lại theo yêu cầu (format commit, checklist PR) |
| **Hook** | Hành động phải tự động, không cần nhắc (lint trước commit, báo Slack khi xong) |
| **Prompt** | Việc làm một lần |

## 3 sai lầm của người mới

| Sai lầm | Cách tránh |
|---|---|
| Nhảy thẳng vào code, bỏ Plan mode (20 phút sau mới thấy Claude đi sai hướng) | `Shift+Tab` vào Plan mode, thống nhất kế hoạch trước; 3 phút ở đây tiết kiệm 30 phút sửa |
| Để auto-compact tự chạy (context đầy 80% → Claude "quên" yêu cầu ban đầu → code lệch spec) | Mỗi 30-45 phút gõ `/context`, chủ động `/compact` (đang dở task) hoặc `/clear` (đổi task) |
| Không có CLAUDE.md, mỗi phiên giải thích lại ("dùng pnpm", "test là `pnpm test --run`", "named export") | Nhắc cùng một điều 3 lần → đưa vào CLAUDE.md |

## Bài tập khởi động (5 phút)

Ghi ra: (1) dự án dùng để thực hành (tên repo + đường dẫn), (2) điểm khó khăn lớn nhất trong workflow hiện tại (1 câu), (3) ngày muốn hoàn thành khóa (gợi ý 2-3 tuần, 1-2 giờ/tuần).
