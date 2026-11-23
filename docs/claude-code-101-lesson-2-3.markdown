---
layout: page
title: Bài 2.3 - Prompt đầu tiên
permalink: /claude-code-101-lesson-2-3/
parent: Claude Code 101
grand_parent: Claude
nav_order: 5
---

# Bài 2.3: Prompt đầu tiên

**Module 1 — Nền tảng** · ~25 phút · Cơ bản → Trung cấp

## Ý chính

Prompt không phải là lệnh mà là **brief** cho một kỹ sư mới rất giỏi nhưng chưa biết codebase của bạn. Boris Cherny: *"Đừng dùng Claude Code để viết code trước tiên. Hãy dùng nó để đặt câu hỏi về codebase."* Prompt mơ hồ ("viết hàm đăng nhập") dễ ra code sai stack (express-session thay vì JWT, bcrypt thay vì argon2) và mất 20 phút gỡ.

## 3 permission modes (`Shift+Tab` để xoay vòng)

| Mode | Hành vi | Dùng khi | Điểm yếu |
|---|---|---|---|
| **Default** | Hỏi từng lần edit và từng lệnh | Branch production, repo khách hàng, lần đầu với dự án mới, chi phí sai cao | Phải duyệt liên tục |
| **Auto Accept** | Tự edit/tạo file; **vẫn hỏi** khi chạy shell | Sandbox, dev branch, đã hiểu rõ phạm vi | Hiểu sai yêu cầu có thể sửa nhiều file trước khi bạn kịp thấy (kiểm bằng `git diff`) |
| **Plan Mode** | Read-only: đọc, grep, phân tích → trả **kế hoạch**, hỏi làm rõ | Task lớn, tính năng mới trên codebase chưa rõ, review code an toàn | Chậm với việc nhỏ (typo, đổi màu nút) |

**Quy tắc nhanh:** task < 5 phút → Default hoặc Auto Accept; task > 30 phút hoặc nhiều file → **Plan mode trước**. Không dùng Auto Accept trên `main`/production: tạo feature branch trước.

## Khung 3 task của Boris

| | **Easy** (one-shot) | **Medium** (planned) | **Hard** (you drive) |
|---|---|---|---|
| Ví dụ | Fix typo, đổi màu, thêm comment, format file | JWT refresh, dark mode toggle, API endpoint mới, test suite | Refactor kiến trúc auth, migrate DB schema, debug race condition |
| Ai lái | Claude | Claude (sau khi có plan) | **Bạn** |
| Mode | Auto Accept hoặc GitHub `@claude` | Plan Mode → Auto Accept | Default (review từng bước) |
| Thời gian | < 5 phút | 15-60 phút | 1+ giờ |
| Verify | Nhìn kết quả | Review plan + chạy test | Review sâu từng bước |

- **Easy:** vd comment PR "thêm loading state cho nút" → `@Claude thêm loading state vào Submit trong SubmitButton.tsx; khi isLoading=true thì disable và hiện spinner`.
- **Medium:** `Shift+Tab` vào Plan mode, thống nhất kế hoạch, hài lòng thì chuyển Auto Accept.
- **Hard:** chia thành task con, làm từng cái ở Default, bạn quyết định hướng sau mỗi bước. Đừng nhét "refactor auth architecture" vào 1 prompt.

## Demo: Dark mode toggle (task Medium, từng bước)

Ứng dụng React 18 + Tailwind v3 + TypeScript; cần dark mode toàn app, nút toggle ở header, lưu qua reload, tương phản đẹp. Thủ công ~nửa ngày; với Plan mode ~25 phút.

1. **Vào Plan mode:** `claude` → `Shift+Tab` hai lần.
2. **Prompt mạnh:** nêu stack, cấu trúc thư mục (`src/components`, `src/pages`, `src/layouts/Header.tsx`), 5 yêu cầu (toàn app; toggle ở `Header.tsx` cạnh avatar; `localStorage`; theo `prefers-color-scheme` nếu chưa chọn; đọc `tailwind.config.ts` để chọn màu tương phản) và ràng buộc **không dùng thư viện ngoài** (chỉ Tailwind `dark:` + localStorage).
3. **Claude trả kế hoạch 7 bước (read-only):** (1) đọc `tailwind.config.ts` → đề xuất bảng màu tối, (2) `darkMode: 'class'`, (3) `DarkModeContext` + hook `useDarkMode` (localStorage + OS preference), (4) bọc `App.tsx` bằng provider, thêm class `dark` vào `<html>`, (5) component `DarkModeToggle` (aria-label, keyboard), (6) nhúng vào `Header.tsx`, (7) cập nhật class trong các component (`bg-white dark:bg-gray-900`, ...). Câu hỏi: cập nhật cả ~12 component hay chỉ `layouts/` và `pages/` trước?
4. **Nhận xét kế hoạch:** bỏ animation/transition ở V1; ở bước 7 cập nhật **tất cả** component.
5. **Approve** → chuyển Auto Accept (`Shift+Tab`), Claude thực thi từng bước, bạn vẫn thấy diff của từng file.
6. **Verify:** `npm run dev`, thử toggle: light đúng như cũ, dark đủ tương phản, reload giữ cài đặt, tự nhận OS dark mode. Thiếu sót (vd `ProductCard.tsx` còn nền trắng) → nhắc: "ProductCard chưa có dark mode, dùng `dark:bg-gray-800` cho container."

Kết quả: ~20-30 phút (gồm đọc plan, review, verify), không conflict vì đã duyệt plan trước.

## Cấu trúc một prompt tốt

| Thành phần | Nội dung | Ví dụ |
|---|---|---|
| **Context** | File, thư mục, khu vực codebase | "File `src/auth/login.ts`, hàm `verifyPassword()` dòng 42" |
| **Constraints** | Style, lib được/không được dùng, hiệu năng, deadline | "Không dùng thư viện ngoài, giữ bundle nhỏ" |
| **Success criteria** | Kết quả trông thế nào, test nào phải pass | "Toggle hiện ở header, unit test pass" |
| **"Think hard" trigger** (task khó) | Kích hoạt extended thinking | Thêm "think hard" cuối prompt |

Prompt mạnh luôn trả lời ba câu: cần đọc file nào? ràng buộc gì? thế nào là "xong"?

| Yếu | Mạnh |
|---|---|
| "Sửa lỗi xác thực" | "`verifyPassword()` trong `src/auth/login.ts` dòng 42-58 trả `true` khi mật khẩu rỗng vì `bcrypt.compare()` bị catch silent. Sửa: ném lỗi. Cập nhật `tests/auth/login.test.ts` cho case này." |
| "Tối ưu hiệu suất" | "`ProductList.tsx` re-render mỗi khi state cha đổi dù props không đổi. Dùng `React.memo` + `useMemo` cho `filteredProducts`; đo trước/sau bằng React DevTools Profiler." |
| "Viết test" | "Unit test cho `src/utils/dateFormatter.ts`: (1) ngày hợp lệ, (2) không hợp lệ, (3) biên múi giờ. Vitest + @testing-library, mock `Date.now()` khi cần." |
| "Sửa lỗi kia" | "Submit form 2 lần liên tiếp (double click) → API gọi 2 lần. File `src/pages/CheckoutPage.tsx`. Fix: debounce hoặc disable nút sau lần đầu. Test: thêm case double-submit trong `CheckoutPage.test.tsx`." |

## Mẹo nâng cao

- **"Think hard" / "think harder" / "ultrathink":** lần lượt cho lỗi khó, task đặc biệt phức tạp, quyết định kiến trúc không được sai. Claude "viết nháp" suy luận trước khi trả lời → chính xác hơn rõ rệt.
- **Tham chiếu bằng `@`:** `Đọc @src/auth/login.ts và @src/auth/middleware.ts rồi...` thay vì mô tả file.
- **3 dòng cụ thể là đủ** cho task trung bình (lỗi + file; yêu cầu; kiểm thử). Cụ thể > dài dòng.
- **Ngắt sớm bằng `ESC`** khi Claude đi sai hướng, rồi giải thích lý do và hướng mới. Tiết kiệm thời gian và context window (Bài 2.5).

## Mẫu sai cần tránh

| Sai | Đúng |
|---|---|
| Yêu cầu quá rộng ("xây app quản lý todo") | Một tính năng cụ thể: "Component `TodoList` React + TS + Tailwind với dữ liệu giả, chưa backend; hiển thị, thêm, đánh dấu xong; V1 chưa cần xóa" |
| Bỏ Plan mode cho task > 30 phút | Task ước tính > 30 phút → Plan mode trước, không ngoại lệ |
| Auto Accept trên `main`/production | Tạo feature branch trước; trên production dùng Default |
| Không định nghĩa "xong" ("tối ưu toàn bộ codebase") | Có tiêu chí: "Tối ưu query `src/api/products.ts`, mục tiêu < 200ms (hiện ~800ms), đo bằng test, đạt thì dừng" |
| "Sửa tất cả lỗi trong dự án" | Một lỗi một prompt; hoặc dùng Plan mode để Claude liệt kê lỗi, bạn chọn cái cần sửa |

## Bài tập

1. **Tái tạo demo dark mode (15')** trên dự án React + Tailwind bất kỳ: `claude` → Plan mode → prompt dark mode (chỉnh theo stack) → ghi: bao nhiêu bước? câu hỏi làm rõ nào? muốn điều chỉnh gì? → approve → verify (toggle hoạt động, lưu localStorage, tương phản đẹp).
2. **Prompt yếu vs mạnh (10')**: cùng một task (vd thêm loading state vào nút), thử "Thêm trạng thái tải vào nút." rồi bản chi tiết (component, prop `isLoading`, spinner `animate-spin`, cập nhật interface TS, 2 test case). So sánh số lần Claude hỏi lại, số token, số lần phải sửa, tổng thời gian. Thường prompt mạnh nhanh hơn và ít token hơn dù dài hơn.

## Tóm tắt

- **Prompt là brief**: context + constraints + success criteria.
- **3 mode:** Default (production, an toàn), Auto Accept (sandbox, sau plan), Plan (task phức tạp, chỉnh trước).
- **3 task của Boris:** Easy → Auto Accept; Medium → Plan rồi Auto Accept; Hard → bạn lái, Default.
- **Plan mode cho task > 30 phút**: read-only, không rủi ro; duyệt và chỉnh trước khi approve.
- **"think hard" / "think harder" / "ultrathink"** cho việc khó; ngắt bằng `ESC` khi đi sai hướng.
- Liên quan: Bài 2.4 (EPCC), 2.5 (context), 2.6 (subagent reviewer).
