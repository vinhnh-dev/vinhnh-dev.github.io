---
layout: page
title: Module 4 - Claude Code nâng cao
permalink: /claude-cowork-module-4/
parent: Claude Cowork
grand_parent: Claude
nav_order: 4
---

# Module 4

Gồm 9 bài: **Steering Long Sessions**, **A CLAUDE.md That Follows**, **Verification Skills**, **Permission Modes**, **Hooks**, **Routines and Headless**, **GitHub Actions and Code Review**, **Trust It: Verifying Unsupervised Runs**, **Plugins**. *Nội dung xoay quanh Claude Code.*

## 01. Steering Long Sessions (điều hướng phiên làm việc dài)

## Ý chính

Việc nhanh thì dễ: hỏi, Claude làm, kiểm tra. Việc dài (refactor hàng chục file, dựng feature mới, kéo dài hàng giờ) thì khác: càng phải điều hướng nhiều, càng kéo dài. Chỉ cần hai thói quen: **chốt phạm vi trước khi Claude bắt đầu** và **điều hướng khi nó chạy**.

## 1. Chốt phạm vi trước với plan mode

Trước khi Claude viết dòng code đầu tiên, bắt nó lập kế hoạch. Trong **plan mode** Claude nghiên cứu ở chế độ read-only: đọc code, tìm ra thứ cần đổi, đưa plan để bạn duyệt. **Đọc kỹ plan, đừng lướt:** plan càng kỹ thì càng ít bất ngờ khi thực thi. Thiếu/sai gì thì bảo Claude bổ sung đúng chỗ; chỉnh plan nhanh hơn nhiều so với để Claude chạy rồi dọn hậu quả.

## 2. Điều hướng khi Claude đang chạy

### Compact (nén)

`/compact` tóm tắt hội thoại, lấy bản tóm tắt làm context mới, xóa tin nhắn cũ → giải phóng context để Claude đi tiếp. Rủi ro: bản tóm tắt bỏ sót điều quan trọng và Claude lệch hướng. Vì vậy **đừng chạy `/compact` trơn**; thêm chỉ dẫn sau lệnh để định hướng phần giữ lại, ví dụ đã debug xong và giờ chỉ quan tâm thay đổi API:

```
/compact Focus on the --version flag implementation
```

Những gì viết sau lệnh quyết định bản tóm tắt giữ gì: đó là "vô lăng" của context.

### Rewind (tua lại)

Khi Claude đi sai đường, không cần prompt để lôi nó ra. Mỗi prompt của user tạo một **checkpoint**. Mở menu bằng cách **nhấn Esc hai lần** trên prompt trống:

| Lựa chọn | Tác dụng |
|---|---|
| **Restore code and conversation** | Quay lại cả code lẫn hội thoại |
| **Restore conversation** | Chỉ quay lại hội thoại |
| **Restore code** | Chỉ quay lại file |
| **Summarize from here** | Tóm tắt mọi thứ *sau* checkpoint (hợp khi có cuộc trao đổi phụ, muốn giải phóng chỗ) |
| **Summarize up to here** | Tóm tắt mọi thứ *trước* checkpoint (hợp khi giai đoạn setup dài muốn nén nhưng giữ nguyên phần implementation) |

## 3. Cho Claude tự chủ hơn

Phần trên giả định bạn trực tiếp theo dõi và chỉnh. Muốn tự chủ hơn có **goal** và **loop**.

- **Goal:** đặt điều kiện hoàn thành. Bạn mô tả "xong" trông thế nào; Claude làm tiếp qua nhiều lượt cho đến khi một evaluator nhanh xác nhận điều kiện đạt, không dừng ngay lần đầu nó *nghĩ* là xong. Ví dụ: `/goal all tests in src/billing pass, and the type checker reports zero errors`. Hủy bằng `/goal clear`. **Ràng buộc:** evaluator chỉ đọc transcript, nên điều kiện phải kiểm chứng được từ đầu ra Claude thực sự tạo (như kết quả chạy test).
- **Loop:** chạy một prompt theo chu kỳ giữa các lượt (cố định hoặc tự điều nhịp). Dùng để theo dõi thứ bên ngoài như CI run hay deploy rồi hành động khi trạng thái đổi. Dừng loop bằng `Esc`.

## 4. Chạy song song với worktree

Một vô lăng cho một xe: chạy nhiều agent trên cùng codebase mà dùng chung file là không an toàn (hai phiên Claude tranh nhau file → xung đột). **Worktree** cho mỗi phiên một **cây file độc lập**, nên không đè thay đổi của nhau. Khi phiên thoát, worktree sạch tự động bị xóa.

File hữu ích: **`.worktreeinclude`** ở root repo liệt kê các file bị git-ignore cần copy vào mỗi worktree (vd file biến môi trường, cấu hình local cần có ở mọi worktree nhưng không commit).

## Tổng hợp thói quen

1. Chốt phạm vi trước, rồi mới điều hướng.
2. **Định hướng compaction** để bản tóm tắt giữ đúng thứ quan trọng.
3. Dùng **menu rewind** để chỉnh hướng khi Claude lệch.
4. Đặt **goal** khi bạn mô tả "xong" tốt hơn mô tả các bước.
5. Chạy việc song song bằng **worktree**.

Làm vậy, bạn có thể tin tưởng một phiên chạy dài mà không phải canh từng bước.

---

## 02. A CLAUDE.md That Follows (CLAUDE.md để Claude làm theo)

### Ý chính

Cái bẫy ai cũng dính: CLAUDE.md phình dần (gặp vấn đề thì thêm luật), rồi Claude bắt đầu bỏ qua một phần. Đó không phải lỗi của Claude mà là cách file hoạt động: **CLAUDE.md không phải cấu hình bị cưỡng chế, nó là hướng dẫn.** Mỗi dòng cạnh tranh sự chú ý với mọi dòng khác; file càng dài càng "tự cạnh tranh" và Claude càng ít tuân thủ từng luật. Mục tiêu không phải ghi hết mà là **giữ file gọn**: file càng gọn thì Claude làm theo càng nhiều.

### Trước hết: CLAUDE.md có phải công cụ đúng không?

Có luật là *hướng dẫn*, có luật là *ranh giới cứng không được vượt*; hai việc khác nhau. Luật như "never push to main" đặt trong CLAUDE.md thì bạn chỉ *hy vọng* Claude đọc và tuân thủ (đa phần là có, nhưng "đa phần" không đủ cho việc nguy hiểm). Luật cứng nên nằm trong **pre-tool-use hook**: là code chạy trước hành động của Claude và **chặn được** hành động. Dù Claude cố push lên main, hook vẫn chặn: cưỡng chế thật, không phải lời nhờ lịch sự. Chuyển luật cứng sang hook, để CLAUDE.md lo các quy ước mềm.

### 4 vị trí của CLAUDE.md

Claude nạp **cả bốn** khi khởi động và chúng **cộng dồn**, không cái nào bị bỏ:

| Vị trí | Dùng để |
|---|---|
| **Managed policy** | File cấp tổ chức do platform team kiểm soát; không loại trừ được, nên chính sách tổ chức luôn có hiệu lực |
| **User** | Sở thích cá nhân đi theo bạn qua mọi dự án trên máy |
| **Project** | File chia sẻ với team, commit vào repo |
| **Local** | Bị git bỏ qua; ghi chú cá nhân cho riêng repo này |

*Local* dễ bị bỏ quên nhưng rất tiện: vd bạn refactor trên nhánh riêng và muốn Claude nhớ vài quyết định kiến trúc, thì điều đó không thuộc file project dùng chung (ảnh hưởng cả team) mà thuộc local.

### Tách file lớn bằng import

Khi file project dài, tách bằng cú pháp import đường dẫn:

```
@.claude/conventions/code-style.md
@.claude/conventions/testing.md
@.claude/conventions/workflow.md
```

Hữu ích để **tổ chức**, nhưng cần hiểu đúng: khi khởi động Claude **mở rộng các file import ngay tại chỗ**, nên mọi thứ vẫn được nạp từ đầu. **Import không giảm lượng context Claude phải đọc.** Dùng import để sắp xếp, không phải để thu nhỏ tải.

### Cách viết để luật "dính"

Khi đã quyết định luật thuộc CLAUDE.md, việc Claude có tuân thủ hay không do **cách diễn đạt**. Phần lớn luật hỏng vì mơ hồ.

| Nguyên tắc | Mơ hồ / hở | Cụ thể / khép kín |
|---|---|---|
| **Cụ thể, kiểm chứng được** (nếu bạn không kiểm tra được thì Claude cũng không) | "Follow best practices for API routes." | "Put new API routes in `src/api/handlers`, one per file." |
| **Nêu cái thay thế, đừng chỉ cấm** | "Don't use default exports." | "Use named exports, not default exports." |
| **Nhấn mạnh là ngân sách** | Mọi luật đều "IMPORTANT/YOU MUST" → không còn gì nổi bật | Dùng nhấn mạnh cho 2-3 luật mà vi phạm gây hại thật; phần còn lại giọng bình thường |

### Giữ file luôn được chỉnh sửa

CLAUDE.md không bao giờ "xong"; hãy coi như code sống. Khi Claude làm sai, đừng thở dài rồi tự sửa tay: coi đó là **bug report cho CLAUDE.md**; có thể nói thẳng "add that to the CLAUDE.md file" để Claude viết luật cho bạn. Mỗi lần có sự cố, file tốt hơn.

### Tổng hợp

Coi CLAUDE.md như production code: **không biện minh được dòng nào thì xóa.**
1. Luật cứng → **hook** (được cưỡng chế thật).
2. File dài → tổ chức bằng import (nhớ import không giảm context).
3. Mỗi luật **cụ thể, kiểm chứng được, nêu cái thay thế**.
4. Chi tiêu **ngân sách nhấn mạnh** cho vài luật quan trọng nhất.
5. Liên tục chỉnh sửa mỗi khi Claude sai.

Ý cốt lõi: **file càng gọn, Claude càng làm theo.**

---

## 03. Verification Skills (skill kiểm chứng)

### Ý chính

Khi dự án lớn dần, cùng một việc lặp đi lặp lại. Skill là cách tự động hóa; bài này nói về một việc skill làm rất tốt: **kiểm chứng công việc của chính Claude**. Nếu chỉ xây một skill đầu tiên, hãy xây cái này.

### Vì sao nên xây skill kiểm chứng đầu tiên

Bình thường bạn nhờ Claude refactor, nó xong, rồi bạn phải *nhớ* kiểm tra lại (chạy test, đọc diff). Việc kiểm tra phụ thuộc vào việc bạn nhớ yêu cầu; bỏ qua một lần là code xấu lọt qua. **Verification skill gỡ sự phụ thuộc đó.** Hình dạng: nhờ Claude refactor → khi xong, thay đổi khớp với **description** của skill nên skill **tự kích hoạt** → nó:
1. Chạy test suite.
2. Đọc diff.
3. Kiểm tra **không có test nào bị nới lỏng** chỉ để cho qua.
4. Báo pass/fail **kèm bằng chứng**.

Toàn bộ chạy không cần bạn yêu cầu; description là thứ kích hoạt, và khi chạy thì đi đúng các bước mỗi lần.

Bước cuối quan trọng: chạy test thấy xanh chưa đủ, vì test có thể bị nới lỏng để luôn qua. Skill đọc diff để xác nhận test không bị làm yếu. **"Xong" không phải là "code trông đúng khi đọc diff", mà là các cổng kiểm đã được chạy và quan sát, với kết quả nêu rõ.**

Cùng khuôn này áp dụng cho mọi quy trình team lặp lại: checklist release, công thức migration, kiểm tra trước PR. **Quy tắc ngón tay cái: nếu bạn đã gõ cùng một chỉ dẫn nhiều bước hai lần thì đó là skill.**

### Thư mục skill chứa được nhiều hơn instructions

Skill không chỉ là một file `skill.md`; thư mục quanh nó có thể chứa thêm, và đó là chỗ skill mạnh cho việc kiểm chứng:
- **`reference.md`** đặt cạnh skill cho tài liệu chi tiết, link từ `skill.md`; Claude chỉ đọc khi thật sự cần độ sâu đó, file chính vẫn ngắn.
- **Scripts** trong thư mục: Claude **thực thi** chúng thay vì nạp nội dung vào context, nên skill mang theo công cụ riêng (vd `check.sh` chạy mọi cổng kiểm).

Kết luận: giữ `skill.md` gọn; đẩy phần nặng (giải thích dài, script chạy được) sang file phụ. File gọn mô tả *làm gì*; file phụ giữ *độ sâu và công cụ*.

### Bề mặt chỉ dẫn nào sở hữu luật nào

| Loại | Đặt ở đâu |
|---|---|
| **Quy ước áp dụng mọi lúc** (quy tắc đặt tên, file để đâu) | `CLAUDE.md` |
| **Quy trình và tài liệu tham khảo gắn với một loại task** | **Skill** |
| **Luật Claude không được phép bỏ qua** | **Hook** (CLAUDE.md và skill đều là chỉ dẫn Claude *làm theo*, còn hook là code *thực sự chạy*; nếu bỏ qua là không chấp nhận được thì đừng để phụ thuộc vào việc làm theo chỉ dẫn) |

### Tóm tắt

Skill là một thư mục chứa `skill.md`: tên, description kích hoạt, và chính quy trình. Chỉ **description** được nạp vào context cho đến khi skill thực sự cần, nên gói mọi quy trình lặp lại không tốn chi phí gì. **Bắt đầu từ verification:** xây skill, commit vào `.claude/skills` của dự án, và cả team thừa hưởng cùng một nước đi: công việc của mọi người được kiểm theo cùng một cách, tự động, không ai phải nhớ yêu cầu.

---

## 04. Permission Modes (chế độ quyền)

### Ý chính

Permission mode cho bạn **quyết định một lần** Claude được chạy gì mà không dừng hỏi; thay vì duyệt từng hành động, chọn mode hợp công việc và để Claude làm ở mức tin tưởng bạn thoải mái. `Shift+Tab` xoay vòng các mode hằng ngày: **manual, accept edits, plan** (và **auto**). Thanh trạng thái dưới cùng luôn hiện mode hiện tại.

### 6 permission mode

| Mode | Hành vi |
|---|---|
| **Manual** | Chỉ đọc không hỏi; mọi thứ khác hỏi trước |
| **Accept edits** | Đọc, sửa file và lệnh bash hệ thống file thông dụng chạy không hỏi; dùng khi lặp trên code rồi review sau |
| **Plan** | Chỉ đọc; nghiên cứu và đề xuất thay đổi, không sửa gì |
| **Auto** | Chấp nhận mọi thứ, nhưng một **classifier model** riêng review từng hành động trước khi chạy |
| **Don't ask** | Chỉ cho phép tool đã duyệt trước; mọi thứ khác **tự động bị từ chối, không hỏi** |
| **Bypass permissions** | Bỏ qua mọi kiểm tra (tương đương flag `dangerously-skip-permissions`); **chỉ chạy trong container hoặc VM cô lập** |

### Auto mode hoạt động thế nào

Auto là chế độ "không cầm tay": Claude tự chạy, nhưng trước mỗi hành động, classifier riêng review. Classifier canh **ý định**: các bước leo thang vượt quá điều bạn thực sự yêu cầu.
- **Được thiết kế để chặn:** deploy/migration lên production; force push hoặc pipe code tải về thẳng vào shell; gửi dữ liệu nhạy cảm tới endpoint ngoài; hủy file tồn tại cho phiên.
- **Cho qua:** sửa local trong dự án, cài dependency theo lockfile, request chỉ đọc, push lên branch của chính bạn.

**Classifier không làm được gì:** nó kiểm **ý định**, không kiểm **tính đúng đắn**. Nó không bắt được code chạy có đúng không: nhờ refactor authentication mà Claude viết auth hỏng thì classifier vẫn cho qua vì "hỏng" không "nguy hiểm". Vì vậy **kết hợp auto với một stop hook chạy test**: auto canh *Claude định làm gì* khi chạy; stop hook xác nhận *code thực sự chạy* khi Claude xong. Một cái bảo vệ ý định trước mỗi hành động, cái kia bảo vệ tính đúng đắn sau đó. Guardrail của auto còn đang phát triển: xem docs để biết danh sách chặn/cho phép hiện hành.

### Don't ask cho chạy không người

Hợp khi không có ai duyệt prompt: **CI pipeline, scheduled job, batch qua đêm.** Chỉ tool đã duyệt trước được phép, thứ gì ngoài danh sách bị từ chối tự động: pipeline tiếp tục chạy thay vì treo chờ một sự đồng ý không ai ở đó để cho.

### Chọn mode theo công việc

- **Auto:** không cầm tay; classifier kiểm ý định trước từng hành động, stop hook kiểm tính đúng đắn sau.
- **Don't ask:** pipeline không người trông.
- **Bypass permissions:** chỉ trong container/VM cô lập.
- Chọn mode khớp việc đang làm và để Claude chạy ở mức đó.

---

## 05. Hooks

### Ý chính

Bảo Claude làm gì đó trong CLAUDE.md là **lời yêu cầu, không phải bảo đảm**: "luôn format sau khi sửa" thì Claude *thường* nghe; nhưng trong phiên dài bạn không theo dõi, "thường" là không đủ. **Hook** là code **tất định** chạy tại một điểm cố định trong vòng lặp, nên *bảo đảm* hành vi thay vì hy vọng: biến luật từ "Claude thường nghe" thành "Claude không thể bỏ qua".

### Các hook event

Claude Code bắn khoảng **30 hook event** trong một phiên; bạn chỉ cần một nhúm hay dùng, khớp với các điểm trong agentic loop (session bắt đầu → prompt vào → tool được gọi → turn kết thúc).

| Event | Khi nào / dùng làm gì |
|---|---|
| **PreToolUse** | Trước khi gọi tool. **Đơn vị cưỡng chế**: chặn được hành động trước khi xảy ra |
| **PostToolUse** | Sau khi tool gọi thành công; thường là chỗ auto-format, auto-lint |
| **Stop** | Khi Claude muốn kết thúc turn; có thể từ chối ("chưa xong") nếu điều kiện chưa đạt. Có `SubagentStop` tương ứng khi subagent xong |
| **PreCompact / PostCompact** | Trước/sau compaction |
| **InstructionsLoaded** | Khi CLAUDE.md hoặc rule file được nạp; tiện để audit thứ thực sự vào context |
| **SessionStart** | Lúc bắt đầu, chuẩn bị môi trường; dùng source `startup` nếu chỉ muốn khi khởi động mới |

**Dễ nhầm:** muốn tiêm lại context sau compaction thì **không dùng PostCompact**; dùng **SessionStart với matcher `compact`**: đó mới là cái đưa output trở lại hội thoại.

### PreToolUse: trả quyết định bằng JSON

PreToolUse chặn được tool call trước khi chạy. Cách "nói chuyện lại" với Claude: **in JSON và thoát với mã 0**. Trường chính `permissionDecision` có 3 giá trị:
- `allow`: cho qua
- `deny`: chặn
- `ask`: trả cho người dùng quyết định

(Về kỹ thuật còn `defer`, chỉ áp dụng cho chạy không tương tác `-p` nơi tiến trình gọi tạm dừng tool rồi tiếp tục sau; hiếm dùng.)

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "deny",
    "permissionDecisionReason": "...",
    "updatedInput": { "command": "..." }
  }
}
```

**`updatedInput`:** thay vì chặn, bạn có thể **viết lại** lời gọi (vd xóa secret khỏi lệnh bash rồi vẫn cho chạy). Lưu ý: `updatedInput` **thay cả object input**, nên phải echo lại các trường không đổi, nếu không sẽ mất chúng.

### Exit code (hook không trả JSON)

| Exit code | Ý nghĩa |
|---|---|
| **0** | Thành công. Nếu stdout là JSON, Claude parse; text thường bị bỏ qua ở hầu hết event, riêng `SessionStart`, `UserPromptSubmit`, `UserPromptExpansion` thì text thường được **thêm vào context** (cơ chế của hook giữ trạng thái) |
| **2** | **Lỗi chặn**: stderr được đưa lại cho Claude làm context; đây là mã chặn ở hầu hết nơi |
| Khác | Không chặn: stderr được log, Claude chạy tiếp |

**Bẫy:** exit **1** nghe như lỗi nhưng **không chặn**, Claude vẫn chạy lệnh. Muốn dừng thì **exit 2**, không phải 1. Thêm vài chi tiết: exit 2 chặn được cả `Stop` (cách nói "chưa xong"); `PostToolUse` chạy *sau khi* tool đã chạy nên chặn ở đó là quá muộn (vẫn có thể đưa text lại cho Claude); một số event bỏ qua việc chặn hoàn toàn (như `Notification`, `SessionStart`: chỉ hiện stderr rồi chạy tiếp).

### Guardrail thực tế: redact thay vì chặn

Với PreToolUse trên tool Bash: **matcher** chọn tool cần canh, mệnh đề `if` tùy chọn thu hẹp theo lệnh cụ thể. Cách hiển nhiên là `deny` để chặn lệnh nguy hiểm. Cách ít người biết và thú vị hơn là trả **`updatedInput`** để viết lại lời gọi: bỏ secret ra khỏi lệnh mà vẫn cho chạy. Ví dụ: Claude chạy lệnh chứa chuỗi giống secret thật; hook chặn lại, thấy mẫu `sk_live_`, thay bằng placeholder trước khi lệnh được thực thi. Lệnh vẫn chạy, việc vẫn xong, nhưng secret không lọt qua: khác biệt giữa **chặn** và **redact**, và là thứ hook cưỡng chế được mỗi lần.

### Giữ trạng thái qua compact

Khi Claude compact hội thoại dài, nó bỏ nhiều chi tiết. Hook **`SessionStart` với matcher `compact`** chạy ngay sau compaction: cho nó in ra một bản tóm tắt ngắn về các file đang làm; bản tóm tắt đó quay lại context, Claude tiếp tục đúng chỗ thay vì bắt đầu nguội.

### Tổng kết

Hook biến luật Claude *thường* theo thành luật *luôn* theo. Đừng chỉ dừng ở auto-format: canh tool bằng **PreToolUse**, chặn kết thúc turn bằng **Stop**, giữ trạng thái qua compact bằng **SessionStart**. Thiết lập tốn chút công đầu, nhưng đền lại ngay lần đầu nó bắt được lỗi trong một lần chạy bạn không theo dõi.

---

## 06. Routines and Headless (tự động hóa việc lặp lại)

### Ý chính

Khi đã tin Claude làm một task, bước tiếp theo là **ngừng làm tay**: cùng một prompt trên một trigger lặp lại thì không cần ngồi kích hoạt mỗi lần. Hai cách giao việc, như một dải phổ: **Routines** (bạn không phải xây gì, chạy trên hạ tầng do Anthropic quản lý) ↔ **Headless mode + Agent SDK** (chạy Claude Code từ code của bạn, kiểm soát hoàn toàn). Bắt đầu từ đầu "ít phải xây nhất".

### Routines: prompt được lưu, chạy trên cloud

**Routine** là cách tự động hóa trực tiếp nhất: không script, không server. Nó gói **3 thứ**: **prompt**, **repository** nó làm việc trên, và **connector** cần dùng; rồi chạy gói đó trên cloud mỗi khi được kích hoạt. Hạ tầng là của Anthropic: không máy của bạn phải bật qua đêm, không file workflow phải bảo trì.

**Trigger:** lịch **cron** (vd mỗi sáng 9h); **HTTP POST** tới API endpoint của routine (code của bạn tự kích hoạt); **GitHub event** (vd có PR mới). Hợp với mọi việc "cùng prompt trên trigger lặp lại": audit dependency buổi sáng; triage PR khi có PR mới; quét hằng ngày ticket Sentry để xác định cái nào khẩn nhất.

**2 cách tạo:** trên web `claude.ai/code/routines` (đặt tên, viết instructions cho mỗi session, chọn repo, chọn trigger), hoặc trong Claude Code bằng lệnh `/schedule` mô tả bằng ngôn ngữ thường, ví dụ `/schedule daily dependency audit at 9am`.

**3 giới hạn cần biết trước khi dựa vào routine:**
1. Routines là **research preview**: hành vi và giới hạn sẽ còn thay đổi.
2. Lịch lặp lại chạy **tối đa mỗi giờ một lần**; cần dày hơn thì routine không phải công cụ.
3. Mỗi lần chạy bắt đầu từ **bản clone mới của default branch** và **chỉ push được lên các branch tiền tố `claude/`** (trừ khi bạn nới lỏng theo từng repo): đây là rào chắn ngăn một lần chạy tự chủ ghi đè `main`.

### Headless mode: khi cần môi trường của bạn

Khi việc cần môi trường của bạn hoặc logic bao quanh lần chạy thì dùng **headless mode**. Cốt lõi là flag **`-p`** (`--print`): chạy Claude Code như lệnh một lần, không UI tương tác, đọc stdin ghi stdout nên pipe như mọi shell tool:

```
claude -p "summarize the changes in this diff"
```

`-p` **bỏ qua tự động khám phá** hook, skill, plugin, MCP server và `CLAUDE.md`: bạn chỉ có Claude + tool bạn cho phép tường minh, không có gì môi trường local tự nạp (bù lại khởi động nhanh hơn nhiều).

**Output có cấu trúc:** ghép **JSON schema** với output format JSON, Claude ràng buộc đầu ra theo schema; object khớp schema nằm ở trường `structured_output` của JSON response, lấy bằng `jq` rồi đưa vào database hoặc script khác:

```
claude -p "Extract the exported function names from src/core/style.js" \
  --output-format json \
  --json-schema '{"type":"object","properties":{"functions":{"type":"array","items":{"type":"string"}}},"required":["functions"]}' \
  | jq '.structured_output.functions'
```

**Tự động hóa nhiều bước bằng session:** lấy **session ID** từ output JSON rồi resume sau: `claude --resume "$(jq -r .session_id /tmp/plan.json)"`. Một script khởi động việc, script khác tiếp tục với đầy đủ context (hợp khi lượt đầu ra kế hoạch, lượt hai thực hiện).

**Chạy tất định cho CI:** flag **`--bare`** cho chế độ tất định: chọn khi chạy Claude Code trong pipeline và cần output lặp lại được, dự đoán được, không đổi theo từng lần chạy.

### Agent SDK: Claude Code trong app của bạn

Bước cuối của dải phổ: thư viện nhúng Claude Code vào ứng dụng **TypeScript hoặc Python** của bạn. Cả hai ngôn ngữ có hàm **`query`** và cùng primitive với CLI: truyền prompt + options (`allowedTools` kiểm soát việc Claude được làm, system prompt, permission mode), rồi lặp qua các message Claude stream về và xử lý theo nhu cầu app. Cùng engine với CLI, chỉ khác là gọi được từ bên trong sản phẩm của bạn.

### Chọn cái nào?

| Dùng | Khi |
|---|---|
| **Routines** | Mặc định cho việc lặp lại; chạy trên hạ tầng Anthropic, không phải host gì |
| **Headless `-p`** | Việc cần pipeline của bạn, muốn pipe dữ liệu qua script |
| **`--bare`** | CI cần cùng kết quả mỗi lần chạy |
| **Agent SDK** | Việc thuộc về bên trong sản phẩm của bạn |

**Bắt đầu với routines**; chỉ đi xuống dải phổ khi việc thực sự cần thêm quyền kiểm soát.

---

## 07. GitHub Actions and Code Review

### Ý chính

Nơi tốt nhất để giao việc lặp lại là **pull request** (chỗ review, chỗ thay đổi đổ vào, nhiều việc vặt). Hai cách đặt Claude vào đây, giải quyết hai vấn đề khác nhau: **dịch vụ managed** bạn bật lên, và **GitHub Action** bạn tự nối.

### Đường managed: Code Review

Dịch vụ do Anthropic host review PR của bạn qua **Claude GitHub app**; không phải xây hay host gì. Bật lên là nó đăng finding dưới dạng **inline comment** ngay trên dòng liên quan.

- **Bật:** admin tổ chức vào cài đặt admin của Claude Code, mục **Code review** → nút **Configure**; cài Claude GitHub app, chọn repo theo dõi, chọn thời điểm chạy: **một lần khi PR mở**, **mỗi lần push vào PR**, hoặc **chỉ khi ai đó comment `@claude review`**.
- **Cách chạy:** trên hạ tầng Anthropic; một bộ **review agent** phân tích diff **so với toàn bộ codebase** (không chỉ các dòng thay đổi tách biệt), rồi đăng finding thành inline comment, gắn nhãn **mức độ nghiêm trọng**, kèm bảng tóm tắt trong check run. Finding gồm giải thích rõ và gợi ý sửa.
- **Khử trùng lặp và xếp hạng:** thay vì một bức tường nitpick, bạn đọc một nhúm vấn đề thật.

**Làm gì và không làm gì:**
- **Không bao giờ approve hay chặn PR:** quyết định vẫn ở người; Claude gắn cờ, bạn quyết.
- **Không có autofix managed:** dịch vụ chỉ đăng finding.
- **Research preview**, dành cho gói **Team và Enterprise**; hành vi sẽ còn đổi.
- Muốn áp dụng finding thì làm **local**: từ terminal, lệnh `/code-review` review một diff và flag `--fix` áp dụng finding vào working tree. Luồng: Claude tìm ra trong PR → bạn kéo về và sửa local.

### Đường tự làm: GitHub Action

Khi việc vượt ra ngoài review: CI tùy chỉnh (hiện thực thay đổi từ comment, báo cáo định kỳ, mọi thứ bạn thường viết workflow cho). Chạy agent trên PR comment, job theo lịch, và mọi GitHub event.

- **Setup:** trong Claude Code chạy `/install-github-app` (cần quyền **admin repo**); lệnh dẫn bạn cài GitHub app và đặt secret API key của Anthropic cho repo.
- **Action:** `anthropics/claude-code-action@v1`. Input hay dùng:

| Input | Ý nghĩa |
|---|---|
| `anthropic_api_key` | Tùy chọn |
| `github_token` | Mặc định `secrets.GITHUB_TOKEN` |
| `trigger_phrase` | Cụm action nghe trong comment; mặc định `@claude` |
| `use_bedrock` / `use_vertex` | Chuyển sang provider đó nếu dùng Bedrock/Vertex |
| `prompt` | Chỉ dẫn cho lần chạy |
| `claude_args` | Chuỗi tham số CLI truyền thẳng vào Claude Code |

**Workflow phản hồi `@claude`:** đặt `.github/workflows/claude.yaml`, lắng nghe `@claude` trên PR comment và issue comment. Bước cốt lõi:

```yaml
- uses: anthropics/claude-code-action@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    github_token: ${{ secrets.GITHUB_TOKEN }}
    trigger_phrase: "@claude"
    prompt: "Your instructions here"
    claude_args: "--max-turns 5 --model claude-sonnet-5"
```

Ai đó viết `@claude implement the spec in the linked Linear issue` trên PR, action nhận, Claude push commit và đăng comment mô tả việc đã làm.

**Workflow theo lịch:** cùng action cho báo cáo hằng ngày: trigger cron (vd 9:00 UTC), action chạy, Claude đăng kết quả. Thêm trigger `workflow_dispatch` để kích hoạt tay từ tab Actions. Bạn xem từng bước chạy trong tab Actions như mọi workflow GitHub.

**Tinh chỉnh bằng `claude_args`:**
- `--max-turns 5`: trần cứng cho agent loop, không chạy mãi.
- **Permission mode:** job không người trông thì đặt để *không dừng hỏi* (không ai trả lời).
- **Allowed tools:** cho đúng thứ job cần, không hơn; báo cáo thì chỉ đọc.

### Chọn cái nào?

- **Review PR → đường managed:** bật Code Review, để GitHub app đăng finding inline, áp dụng fix local bằng `/code-review --fix`.
- **Việc vượt review → Action:** `/install-github-app` để setup, một workflow cho mention `@claude`, một cho cron, mọi tinh chỉnh nằm trong `claude_args`.
- **Bắt đầu với dịch vụ managed**; chuyển sang action ngay khi cần Claude *làm* gì đó trong CI, không chỉ comment.

---

## 08. Trust It: Verifying Unsupervised Runs (kiểm chứng các lần chạy không giám sát)

### Ý chính

Bạn giao task cho Claude và để nó chạy không theo dõi; giờ nó báo xong. Trước khi ship, cần cách kiểm tra thứ bạn không hề giám sát: chính việc đó làm cho Claude Code "không cầm tay" an toàn để dựa vào. Nguyên tắc: **kiểm chứng tương xứng với mức "dây" bạn đã nới cho lần chạy**: phiên ngắn bạn xem tin nhắn cuộn qua thì liếc qua là đủ; lần chạy không người, hay job chạy trong CI mà không ai ở đó, cần kiểm tra thật vì không ai thấy chuyện gì xảy ra nên phải *dựng lại sau đó*. **Càng ít xem, càng phải kiểm nhiều.**

### Giữ lần chạy không giám sát ở auto mode

Khi chạy không người ở công việc, hãy giữ ở **auto mode** thay vì bypass permissions: trong auto, classifier vẫn review từng hành động để tìm nguy hiểm; đó là lưới an toàn đáng giữ. Nhưng hiểu rõ lưới đó làm gì: classifier **không bao giờ đánh giá code có đúng không**, chỉ gắn cờ hành động nguy hiểm. Nên ngưỡng kiểm chứng của bạn giữ nguyên; đặt theo mức độ không giám sát của lần chạy.

### Bắt đầu từ diff, không phải bản tóm tắt

Đừng bắt đầu từ lời tóm tắt của Claude; bắt đầu từ **diff**. Chạy `/code-review` để đi qua các thay đổi và gắn cờ vấn đề, rồi tự nhìn `git diff`. Bẫy: bản tóm tắt gọn gàng đọc rất ổn trong khi diff thực tế chạm vào một file bạn không ngờ; chỉ diff mới cho biết. Đọc những file thuộc kế hoạch trước, rồi tìm mọi thứ nằm ngoài kế hoạch. **Bản viết sạch không phải bằng chứng code sạch.**

### Biến test thành cổng, không phải lời hứa

Cổng thật của một lần chạy không giám sát là test có qua không, và Claude **thực sự chạy** hay chỉ *nói* là đã chạy. Đừng để tùy lòng tin; nối thành **hook** để Claude không bỏ qua được:
- **Stop hook** chạy test và **từ chối kết thúc turn** nếu fail.
- **PostToolUse hook** lint và type-check sau mỗi lần sửa.

Chi tiết then chốt là **exit code**: hook thoát bằng **exit 2** đưa thẳng lỗi lại cho Claude, nó đọc và tự sửa mà bạn không phải nhờ. Quan trọng nhất: check chạy ở mọi lần, dù bạn có nhớ yêu cầu hay không.

### Xin ý kiến thứ hai "lạnh"

Subagent code review bạn chạy trước PR dùng được cả ở đây: mở **phiên mới hoặc subagent** và cho nó review code đã đổi mà **không nhớ** code được dựng thế nào. Vì không có lợi ích gắn với cách tiếp cận, nó bắt được thứ lần chạy gốc đã tự thuyết phục mình bỏ qua. Người review thứ hai với con mắt mới thấy điều tác giả đã biện minh cho qua.

### Tổng hợp

Làm kiểm tra nghiêm túc tương xứng với mức không giám sát:
1. **Tự đọc diff.**
2. **Biến test thành hook** chặn turn.
3. Với headless run, **kiểm bằng kết quả JSON và exit code**.
4. Lấy **ý kiến thứ hai lạnh** cho bất cứ thứ gì quan trọng.

Làm vậy thì "Claude làm trong lúc tôi không nhìn" không còn cần đến niềm tin.

---

## 09. Plugins

### Ý chính

Một setup bạn tin cậy đáng giá hơn nhiều khi cả team dùng, nhưng vấn đề là *chuyển nó đi*. Bạn dựng thư mục `.claude` tốt (skill, subagent, hook) rồi sao? Mọi người copy-paste file giữa các máy và hy vọng đồng bộ. **Plugin** là cách Claude Code đóng gói một setup và chuyển từ người này sang người khác. Hai phía: **dùng plugin người khác publish**, và **đóng gói plugin của riêng bạn**.

### Plugin là gì

Một **đơn vị cài đặt**, gom mọi thứ bạn thường chia sẻ bằng tay: skill, subagent, hook, cấu hình MCP server, cộng với phần đuôi dài (language server protocol server, background monitor, theme, một phần `settings.json`). Một version, một lần cài. Nơi plugin nằm quyết định cách cài. Trong phiên có thể cài thẳng theo tên: `/plugin install org-name@plugin-name`; Claude Code cài và nhắc chạy `/reload-plugins` để áp dụng.

**Thêm marketplace cho team:** với team, tốt hơn là thêm **private marketplace** một lần, nguồn dùng chung mà plugin giải quyết qua đó: `/plugin marketplace add your-org/claude-plugins`. Từ đó mọi lần cài đều đi qua nó: khám phá tập trung, theo dõi version, cập nhật ở một chỗ thay vì rải trên laptop từng người. Duyệt các plugin có sẵn ở tab **Discover**.

### Đọc trước khi cài

Phần quan trọng nhất: **plugin chạy code trên máy bạn, với quyền của bạn**; hook của nó chạy trên mọi tool call khớp. Cài plugin vì skill của nó thì bạn cũng nhận luôn hook `PreToolUse` và `Stop` của nó, dù có đọc hay không. Ví dụ plugin cộng đồng có thể gửi `Stop` hook gọi ra endpoint mạng mỗi lần mà không có gì trong cấu hình cảnh báo bạn. Đó không phải lý do né plugin mà là lý do **nhìn trước**: trước khi cài, xem chi tiết plugin (Claude Code cho biết sẽ cài gì và ước lượng chi phí context, kèm cảnh báo rõ rằng Anthropic không kiểm soát nội dung plugin bên thứ ba).

Về nguồn: form gửi trong app đăng lên marketplace cộng đồng sau khi qua review tự động của Anthropic; marketplace chính thức được tuyển chọn trên một luồng riêng. Nhưng **được review không đồng nghĩa đáng tin**: review tự động bắt một số thứ, không phải tất cả. Quy tắc: **chỉ cài plugin và thêm marketplace từ nguồn bạn thật sự tin**, và kiểm tra plugin thực sự làm gì trước khi bật.

### Thành phần chạy cạnh thành phần của bạn

Plugin không ghi đè cấu hình của bạn; thành phần của nó chạy **song song**:
- **Hook xếp chồng:** `PreToolUse` của plugin và của bạn đều chạy trên mọi tool call, cái nào cũng không thay thế cái kia (nên cần đọc chi tiết trước).
- **Skill, agent, command được đặt namespace** theo tên plugin nên không đụng của bạn. Plugin có thể mang `settings.json` nhưng chỉ rất hẹp: Claude Code chỉ chấp nhận **hai key** từ đó: key `agent` và key status line của subagent. Key `agent` đáng lưu ý: đặt nó **đẩy một subagent của plugin lên main thread**, kèm system prompt, giới hạn tool và model của nó; tức bật plugin có thể **đổi hành vi mặc định** của Claude Code: một trong những lý do chính để nhìn trước khi bật.

Plugin đã cài thì xem mọi thứ nó thêm, quản lý và gỡ trong panel plugin.

### Đóng gói plugin của riêng bạn

Khi `.claude` của bạn đã chạy tốt, đừng bắt team copy-paste giữa máy; đóng gói. **Không cần cấu trúc lại gì**: plugin dùng đúng hình dạng `.claude` bạn đã quen:
- Mỗi skill một thư mục.
- Mỗi subagent một file markdown dưới `agents`.
- `hooks/hooks.json` và `.mcp.json` ở root plugin.

Cấu trúc thư mục làm phần lớn việc: Claude Code khám phá thành phần **theo quy ước**.

**Manifest** (tùy chọn) nằm ở `.claude-plugin/plugin.json`, chứa name, version, description, author:

```json
{
  "name": "svg-splitter-review",
  "version": "0.1.0",
  "description": "Reviews the SVG Splitter repo",
  "author": { "name": "Lewis Menelaws" }
}
```

Bỏ manifest thì Claude Code vẫn khám phá thành phần theo quy ước thư mục. **`name` là trường bắt buộc duy nhất**, đóng vai namespace cho skill (`company-name:skill-name`) để không đụng skill của người khác. **Đặt version như mọi dependency**: giúp cập nhật và theo dõi version trong team.

### Điều cần nhớ

1. **Dùng plugin: đọc trước khi cài.** Plugin chạy code với quyền của bạn; xem hook, agent, MCP server của nó trước.
2. **Xây plugin: đóng gói `.claude` ngay khi nó chạy tốt.** Một manifest, một lần cài.

Một đơn vị cài đặt, và setup bạn tin cậy đến được cả team.
