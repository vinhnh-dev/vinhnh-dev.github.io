---
layout: page
title: Bài 2.1 - Cách Claude Code hoạt động
permalink: /claude-code-101-lesson-2-1/
parent: Claude Code 101
grand_parent: Claude
nav_order: 3
---

# Bài 2.1: Cách Claude Code hoạt động

**Module 1 — Nền tảng** · ~20 phút · Cơ bản

## Ý chính

Autocomplete (Copilot/Cursor) là "bạn gõ, AI gợi ý". Claude Code thì **tự làm việc**: đọc file, grep, sửa code, chạy test, sửa lỗi, rồi mới hỏi bạn. Đây là thay đổi về bản chất ("from punch cards to prompts"), và là công cụ mà đội ngũ Anthropic dùng hằng ngày.

## Agentic loop

Claude không chỉ trả lời mà **hành động theo vòng lặp** đến khi xong:

1. **Gather context** — hỏi mô hình "cần biết gì?", rồi gọi tool: đọc file, glob, grep, find. Dùng *agentic search* giống dev mới làm quen codebase (không dùng indexing/embedding).
2. **Take action** — sửa/tạo file, chạy bash, gọi API; mỗi hành động là một tool call có kết quả thật.
3. **Verify** — chạy test, đọc output, so với mục tiêu. **Fail thì quay lại bước 1**, không báo thất bại rồi dừng.

Dừng khi: (a) đạt yêu cầu → báo xong, chờ prompt; hoặc (b) gặp trở ngại cần bạn quyết định.

**Vai trò của bạn trong loop:** thêm context ("endpoint này dùng auth v2"), điều hướng ("chỉ tập trung `routes.ts`"), interrupt bất cứ lúc nào.

## Context window

Bộ nhớ làm việc của Claude: prompt, file đã đọc, kết quả tool call, lịch sử hội thoại, system config (CLAUDE.md, permissions).

- **200.000 token** (~150.000 từ, ~80-100 file cỡ trung bình); mọi hành động trong loop đều tiêu thụ context
- Gần đầy (~80%) → **auto compact** (tóm tắt phần không còn cần)
- Chủ động: **`/compact`** (giữ tóm tắt task hiện tại) hoặc **`/clear`** (xóa sạch, phiên mới)
- Chi tiết ở Bài 2.5

## Tools

Chat AI thường chỉ "văn bản vào, văn bản ra". **Tools** cho Claude quyết định khi nào cần thực thi để tiến gần mục tiêu, dựa trên hiểu ngữ nghĩa (vd "cần biết framework test" → Read `package.json`; "tìm mọi chỗ dùng hàm deprecated" → Grep).

| Nhóm | Tool |
|---|---|
| File | Read, Write, Edit, MultiEdit |
| Search | Glob (tìm file), Grep (tìm nội dung) |
| Execute | Bash (lệnh, test, script) |
| Web | WebSearch, WebFetch |
| Agent | Spawn subagent (forked context) |
| MCP | Slack, Linear, GitHub, custom (Bài 2.10) |

## Permission modes (`Shift+Tab` để chuyển)

| Mode | Hành vi | Dùng khi |
|---|---|---|
| **Default** | Hỏi trước **mọi** file edit và shell command | Task lạ, code quan trọng (auth, thanh toán, migration) |
| **Auto Accept** (edit only) | Edit không hỏi; shell vẫn hỏi | Đã thống nhất kế hoạch, task routine, refactor nhỏ |
| **Plan Mode** (read-only) | Chỉ tool đọc, không sửa, không chạy lệnh | Lên kế hoạch trước task trung bình đến lớn |

Mẫu phổ biến: Plan mode → duyệt kế hoạch → `Shift+Tab` sang Auto Accept để thực thi (Bài 2.3).

⚠️ Tránh auto-accept cả lệnh shell trừ khi tin tưởng hoàn toàn: một lệnh sai có thể xóa file, reset DB, deploy nhầm môi trường.

## Claude Code vs chat AI thường

| | Chat AI | Claude Code |
|---|---|---|
| Tương tác | Phản ứng từng bước | Tự chạy loop đến khi xong |
| Truy cập file | Không (phải dán) | Đọc/ghi trực tiếp trong codebase |
| Tools | Không/rất hạn chế | Bash, edit, glob, grep, web, MCP |
| Xác minh | Không, bạn tự kiểm | Tự chạy test, đọc kết quả, lặp lại |
| Nhiều bước | Bạn làm từng bước, copy-paste | Tự nối nhiều bước |
| Bộ nhớ | Context ngắn, hay bị reset | 200k token + `/compact` |
| Đầu ra | Văn bản/đoạn code để bạn dán | File đã sửa, lệnh đã chạy, test đã qua |

**Dễ nhớ:** Cursor/Copilot là cây bút thông minh (vẫn bạn cầm bút); Claude Code là pair programmer (bạn nêu mục tiêu, nó làm, báo khi xong hoặc cần quyết định). Boris Cherny: *"Claude là con ngựa, kỹ sư là người cưỡi"* — ngựa mạnh nhưng cần giàn giáo (system prompt, tools, quản lý context, quyền) và người biết chỉ đường.

## Ví dụ: thêm chuyển đổi WebP vào luồng upload ảnh

- **Gather:** Read `src/upload/handler.ts` (multer + S3) → Grep `sharp|imagemin|jimp` (đã có `sharp` v0.33) → Read `package.json` (không cần cài thêm)
- **Action:** Edit `handler.ts` (thêm `convertToWebP()` trước khi upload S3), Edit `types.ts` (thêm WebP mime type)
- **Verify:** `pnpm test src/upload/` → 2 test fail (mime type)
- **Loop lần 2:** đọc test (kỳ vọng JPEG, code trả WebP) → sửa test + content-type header → test lại: 8/8 pass
- Báo cáo: sửa 2 file, cập nhật 2 test, tất cả pass. Bạn có thể ngắt bất cứ lúc nào.

## Case study theo vai trò

| Vai trò | Tình huống | Điểm đáng chú ý |
|---|---|---|
| **Backend** | Chuyển module auth 3 năm tuổi sang refresh-token flow | Loop tự chạy: đọc 7 file, grep `jwt.sign/verify`, test fail 3 cái → viết test mới, sửa edge case hết hạn → pass. Với chat: mỗi vòng 3-5 phút copy-paste |
| **DevOps** | `payments-api` trả 503 lúc 2h sáng | Đọc 1000 dòng log, tìm pattern, đọc config → tóm tắt "3 nguyên nhân khả dĩ". Bạn quyết định, rồi yêu cầu "tăng connection pool 10 → 25 trong `config/db.yaml`"; xong trong ~8 phút |
| **Designer** (Megan) | Muốn tự ship "2% hoàn thiện" (margin, hover blue-500 → blue-600, focus ring) | Claude tìm component, đọc config Tailwind, sửa class, kiểm tra không bị override; Megan gửi 8 PR trong một ngày, không chờ kỹ sư |
| **Open-source contributor** | Repo 200k LOC chưa quen | "Khám phá và tóm tắt kiến trúc trong 5 điểm" → nắm tổng quan trong 5 phút thay vì 2 ngày đọc mù |
| **Data engineer** | ETL Postgres → BigQuery | Mỗi bước verify được (số hàng vào/ra, kiểu cột, null); Claude chạy thử trên mẫu nhỏ rồi mới báo xong |

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Dùng như chat: dán đoạn code, copy kết quả ra tay | Để Claude tự đọc file; chỉ nói file nào + vấn đề gì |
| Nhấn "yes" mọi đề xuất mà không đọc kế hoạch | Plan mode cho task quan trọng, đọc kế hoạch trước khi thực thi |
| Bật auto-accept toàn bộ vì "phiền" | Default cho task lạ/có side effect; Auto Accept (chỉ edit) cho task routine |
| Tư duy autocomplete ("tiếp tục code từ đây", "viết function tiếp theo") | Mô tả kết quả, không phải các bước (vd "bộ lọc khoảng ngày cho báo cáo, nhận biết múi giờ, xử lý cuối tháng") |
| Tin mù quáng vào "xong" của Claude | Verify của Claude là bước đầu, không phải duy nhất: đọc diff, chạy lại test, thử thật — nhất là auth, thanh toán, migration |

## Bài tập

1. **Quan sát loop (10')**: trong dự án quen thuộc, prompt *"Khám phá codebase và tóm tắt kiến trúc trong 5 điểm: (1) entry point, (2) nơi chứa business logic, (3) mẫu test, (4) dependency quan trọng nhất, (5) 'bẫy' cho dev mới."* Ghi lại: file nào đọc trước? tool nào gọi? có lặp không? bao nhiêu vòng?
2. **Task nhiều bước (15')**: chọn task nhỏ (vd thêm validation form đăng ký + cập nhật test; hoặc refactor `formatDate()` từ moment.js sang date-fns). Quan sát cách Claude verify và có vòng fail → fix → retry không; sau đó tự verify (đọc diff, chạy test, thử tay) và so sánh.

## Tóm tắt

- **Agentic loop Gather → Action → Verify** là nền tảng: agent tự chạy, không phải chatbot gợi ý code.
- **3 permission modes:** Default / Auto Accept (edit) / Plan (read-only).
- Khác chat AI ở 3 điểm: tự truy cập file, tự verify, tự nối nhiều bước.
- Vai trò của bạn: từ người gõ code → người mô tả mục tiêu và đánh giá kết quả.
- **Tư duy autocomplete cản trở bạn:** dùng Claude Code như Copilot là bỏ lỡ ~80% sức mạnh.
