---
layout: page
title: Bài 2.9 - Skills
permalink: /claude-code-101-lesson-2-9/
parent: Claude Code 101
grand_parent: Claude
nav_order: 11
---

# Bài 2.9: Skills — Dạy Claude một lần, dùng mãi mãi

**Module 4 — Hệ sinh thái mở rộng** · ~30 phút · Trung cấp

## Ý chính

Nếu bạn nhắc Claude cùng một thứ nhiều lần (vd mỗi PR phải nhắc: Conventional Commits, footer `BREAKING CHANGE`, số ticket Linear `[PROJ-XXX]`, tag reviewer), đó là một **skill đang chờ được viết**. 50 PR/tháng → 0 lần nhắc.

**Agent Skill** = thư mục chứa `SKILL.md` (kèm script/tài nguyên tùy chọn); Claude tự phát hiện và tải khi yêu cầu của bạn khớp với **description**. Không phải lệnh bạn phải gõ, mà là kiến thức Claude tự biết khi nào dùng.

**Cơ chế:** Claude so prompt với danh sách description của các skill; không khớp thì chỉ tốn tên + mô tả (vài chục token); khớp thì mới tải toàn bộ nội dung → tiết kiệm context hơn CLAUDE.md (Bài 2.5).

## Cấu trúc SKILL.md

**YAML frontmatter** (`name` kebab-case ngắn; `description` quyết định khi nào kích hoạt) + **body** hướng dẫn chi tiết (tiêu đề, checklist, ví dụ code, định dạng output). Ví dụ `pr-review`: kiểm theo thứ tự (1) bảo mật (không hardcode credential, validate input, SQL tham số hóa, không lộ stack trace), (2) chất lượng code (không dead code, đặt tên rõ, hàm ≤ 50 dòng, một hàm một việc), (3) test (happy path, edge case rỗng/null/0/âm, case lỗi), (4) output `CRITICAL / MAJOR / MINOR / SUGGESTION`.

**Viết description tốt:** nêu skill làm gì + liệt kê 3-6 cụm trigger tự nhiên.

| Description | Hậu quả |
|---|---|
| Quá chung ("dùng khi review code") | Kích hoạt nhầm bất cứ khi nào hỏi về code |
| Quá cứng ("chỉ khi gõ đúng 'review PR #123'") | Bỏ sót mọi biến thể hợp lệ |
| Đúng | Mô tả rõ loại task + vài cụm trigger tự nhiên, không giới hạn cứng nhắc |

Body không giới hạn độ dài nhưng sẽ được tải toàn bộ khi khớp, nên giữ tập trung.

## Skill vs CLAUDE.md vs Slash command vs Hook

| | Skill | CLAUDE.md | Slash command | Hook |
|---|---|---|---|---|
| Trigger | Tự động (Claude nhận biết) | Luôn luôn (tải mọi phiên) | Thủ công (`/tên`) | Theo sự kiện |
| Chi phí context | Thấp (chỉ tên + mô tả đến khi khớp) | Cao (luôn có) | Theo yêu cầu | 0 với context của Claude |
| Ai quyết định | Claude | N/A | Bạn | Hệ thống |
| Dùng cho | Kiến thức theo loại task | Thông tin luôn liên quan | Gọi tường minh | Tự động hóa tất định |
| Ví dụ | Checklist review PR | "Dùng pnpm, lệnh build là X" | `/commit-pr` | Auto-format sau mỗi Edit |

**Quy tắc quyết định:** liên quan đến mọi task? → CLAUDE.md. Muốn tự gọi? → slash command. Phải xảy ra chắc chắn mỗi khi có sự kiện? → hook (Bài 2.11). Kiến thức/checklist/chuẩn cho một loại task? → **Skill**.

## Đặt skill ở đâu

| | Cá nhân `~/.claude/skills/` | Dự án `.claude/skills/` |
|---|---|---|
| Phạm vi | Theo bạn qua mọi dự án, không commit | Check-in git, ai clone repo cũng có |
| Dùng cho | Phong cách riêng, cách giải thích code, quy trình debug cá nhân | Chuẩn của team, brand guideline, convention tổ chức, checklist bảo mật PR |

Mỗi skill một thư mục (có thể kèm `scripts/` hoặc `assets/`).

## Ví dụ: tạo skill `code-explainer`

Tình huống: tech lead mỗi tháng có 2-3 dev mới và phải giải thích lại luồng xác thực, xử lý lỗi chuẩn, vì sao dùng repository pattern. Sau lần giải thích thứ 4 → làm skill.

1. `mkdir -p ~/.claude/skills/code-explainer && touch ~/.claude/skills/code-explainer/SKILL.md`
2. **Frontmatter:** description nêu phạm vi (giải thích codebase/quyết định kiến trúc cho dev mới) + trigger ("giải thích điều này", "X hoạt động như thế nào", "tại sao lại dùng", "hướng dẫn tôi từng bước").
3. **Body gồm:** nguyên tắc giải thích (Why → What → How → Ví dụ từ codebase); điều chỉnh theo đối tượng (người mới: ví dụ đời thực trước, không bỏ bước; người có kinh nghiệm: đi thẳng vào trade-off); quy trình giải thích auth (entry point → middleware chain → JWT → refresh → các case lỗi, kèm sơ đồ ASCII nếu có nhánh); format quyết định kiến trúc (Bối cảnh, Quyết định, Lý do, Đánh đổi, Phương án đã cân nhắc); kết thúc bằng "nếu chỉ nhớ một điều: X" và gợi ý file đọc tiếp.
4. **Test 3 biến thể** ("Giải thích quy trình xác thực cho người mới", "Tại sao code này dùng repository pattern?", "Hướng dẫn tôi qua middleware xử lý lỗi"); không kích hoạt thì thêm từ khóa vào description.
5. **Lặp lại:** giải thích quá dài → thêm mục độ dài (ngắn 150-300 từ, vừa 300-500, dài 500-800; hỏi "bản ngắn hay chi tiết?" nếu chưa rõ). Skill tốt thường cần 2-3 lần chỉnh.

## Case study theo vai trò

| Vai trò | Skill | Nội dung chính | Kết quả |
|---|---|---|---|
| Team kỹ thuật | `release-notes` | `git log` từ tag trước, khớp commit với ticket Linear, nhóm feature/fix/breaking, format Markdown của team | 1 giờ → 5 phút |
| Designer (Megan) | `design-handoff` | Màu, spacing scale, typography; checklist dark mode, hover/focus, WCAG 2.1 AA, breakpoint; output bảng Markdown (thuộc tính, mặc định, link Figma) | Không phải nhắc lại design system |
| DevOps | `postmortem` | Mẫu 5-whys, checklist thu thập dữ liệu (timeline, metric, log), action item có owner/deadline/status, văn hóa không đổ lỗi | Báo cáo nhất quán, không sót action |
| Mobile | `icon-export` | iOS (`name@1x/@2x/@3x.png`, `Assets.xcassets`); Android (mdpi → xxxhdpi, ưu tiên vector drawable); batch SVG bằng Inkscape CLI | Không phải nhớ convention hai nền tảng |
| Maintainer OSS | `triage-issue` | Checklist (bug/feature/question/duplicate), ánh xạ từ khóa → nhãn, mẫu comment đóng duplicate, đề xuất assignee theo chuyên môn | Phân loại 30-50 issue/tuần |
| Marketing | `blog-seo-check` | Cấu trúc H1/H2, meta 150-160 ký tự, ≥ 2-3 internal link, alt text, từ khóa trong tiêu đề + đoạn đầu + một H2, độ dễ đọc (Flesch) | Không quên SEO trước khi publish |

## Khi nào tạo skill

Nhắc lại cùng một thứ ≥ 3 lần → nếu phải chạy **chắc chắn mỗi khi có sự kiện** thì viết Hook; nếu muốn tự gọi thì Slash command; nếu liên quan mọi task thì CLAUDE.md; còn lại (mẫu lặp lại, theo loại task, tự kích hoạt) → Skill.

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Description quá chung ("giúp Claude làm việc tốt hơn với code") | Nêu loại task + cụm trigger (kiểm tra bảo mật, kiểm toán code, danh sách pentest) |
| Description quá cứng | Mô tả theo chủ đề, chấp nhận biến thể diễn đạt |
| Một skill khổng lồ > 500 dòng (review + commit + release notes + bảo mật) | Skill nhỏ, tập trung (`pr-review`, `commit-pr`, `release-notes`, `security-check`) |
| Hardcode đường dẫn/tên user (`/Users/alice/...`, `@alice-dev-2023`) | Dùng lệnh trong CLAUDE.md (`npm test`), tham chiếu `CODEOWNERS` |
| Dùng skill thay cho hook ("kích hoạt MỖI LẦN Claude sửa file" → dấu hiệu của hook) | Tự động đảm bảo theo sự kiện → Hook (Bài 2.11) |
| Skill cá nhân commit vào repo (hoặc chuẩn team để ở `~/.claude/skills/`) | Cá nhân → `~/.claude/skills/`; chuẩn team → `.claude/skills/` |
| Không cập nhật khi stack đổi (skill `deploy-staging` còn ghi Heroku sau khi chuyển AWS ECS) | Coi skill như code; ghi chú version trong tiêu đề (`<!-- Cập nhật lần cuối: 2026-01 ... -->`) |

## Mẹo nâng cao

- **Test description với 5 biến thể** trước khi dùng thật ("xem xét PR này trước khi merge", "kiểm tra code của nhánh feature/auth", "có vấn đề gì với cách implement này không?", "trước khi commit có gì cần sửa không?", "xem lại PR #456 đi"); bỏ sót 2/5 thì bổ sung từ khóa.
- **Skill có thể kết hợp:** skill này tham chiếu skill khác (vd `release-notes` gọi `git-log-summary` nếu có, nếu không thì theo format `[type] [scope]: [desc] [ticket]`).
- **Đọc skill thực tế** (thư mục `skills/`, `.claude/skills/`); xem khóa "Introduction to Agent Skills" của Anthropic.
- **Kho mẫu skill của tổ chức** (`team-claude-skills/`: `commit-conventional`, `pr-review-standard`, `code-explain-beginner`, `security-basics`...) để người mới copy vào `~/.claude/skills/`.
- **Ghi rõ trigger đa ngôn ngữ** trong description (VI: "review code", "xem code", "có lỗi gì không"; EN: "review", "any issues", "before merge").

## Bài tập

1. **Viết skill đầu tiên (20'):** tìm điều bạn nhắc Claude ≥ 3 lần trong 3 tháng qua; tạo thư mục skill (cá nhân hoặc dự án); viết `SKILL.md` (tên kebab-case, description có phạm vi + ≥ 3 trigger, body ≥ 3 phần cụ thể, không hardcode đường dẫn); thử 3 biến thể; dùng 1 tuần và ghi số lần kích hoạt đúng / sai (false positive) / bỏ sót.
2. **Khám phá skill có sẵn (15'):** `ls ~/.claude/skills/` và `ls .claude/skills/`; đọc một `SKILL.md`; thử 2-3 biến thể prompt; nếu chưa có, xem khóa Introduction to Agent Skills.

## Tóm tắt

- **Skill = dạy một lần, Claude tự áp dụng mãi.** Lặp ≥ 3 lần là tín hiệu cần viết.
- **Description là linh hồn:** quá chung kích hoạt nhầm, quá cứng bỏ sót; test nhiều biến thể.
- **Cá nhân theo bạn, dự án theo repo.**
- **Tiết kiệm context hơn CLAUDE.md** cho kiến thức theo loại task.
- **Skill không thay thế hook hay slash command:** mỗi cái có chỗ dùng riêng.
