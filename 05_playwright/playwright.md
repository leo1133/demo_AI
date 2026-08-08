### Cách cài đặt playwright

npm: npm init playwright@latest // Tạo file cấu hình cho Npm
yarn: yarn create playwright // Tạo file cấu hình cho Yarn
pnpm: pnpm create playwright // Tạo file cấu hình cho Pnpm

npx playwright install chromium // Cài đặt trình duyệt Chromium cho Playwright
npx playwright install // Cài đặt tất cả các trình duyệt (Chromium, Firefox, WebKit)

npx playwright codegen [URL] // Mở trình duyệt và ghi lại hành động của người dùng, sau đó chuyển đổi hành động thành mã test

npx playwright test [options] // Chạy tất cả các test trong dự án

npx playwright test test/login.spec.ts --ui // Chạy test với giao diện người dùng

npx playwright show-report // Hiển thị báo cáo kết quả test dưới dạng HTML

npx playwright codegen --debug [URL] // Mở trình duyệt với chế độ gỡ lỗi, cho phép kiểm tra từng bước thực hiện test

### Cấu trúc và Vai trò File Cấu hình Playwright

File playwright.config (thường là .ts hoặc .js) là thành phần trung tâm trong cấu trúc dự án (scaffold) được tạo ra tự động khi thực hiện cài đặt Playwright

### Cấu trúc và Quản lý Thư mục Tests trong Playwright

Cấu trúc thư mục tests trong Playwright thường được tổ chức khoa học để quản lý và phân loại các kịch bản kiểm thử (test cases). Dưới đây là cấu trúc và vai trò điển hình của các thư mục chính:

1. **tests/**
   Thư mục gốc chứa toàn bộ các tệp và thư mục liên quan đến kiểm thử. Playwright tự động quét và nhận diện các tệp test trong thư mục này theo cấu hình mặc định.

2. **tests/example/**
   Thư mục ví dụ chứa các bài test mẫu được Playwright tạo ra tự động sau khi cài đặt. Các test mẫu này giúp người dùng làm quen nhanh với cú pháp và cấu trúc của Playwright.

### Tạo Test Cases trong Playwright

1. **Cú pháp cơ bản**
   Trong Playwright, mỗi test được định nghĩa trong một hàm được bao bọc bởi `test()` hoặc `it()` (là alias của `test`). Cấu trúc cơ bản bao gồm mô tả test (title), các options (như `{}` ), và một callback function chứa các bước thực hiện.

2. **Các thông số của test()**
   - Title (string): Mô tả ngắn gọn về test case, xuất hiện trong báo cáo kết quả.
   - Options (object, optional): Các cấu hình bổ sung như `solo` (chỉ chạy test này), `skip` (bỏ qua test), `timeout` (thời gian chờ tối đa).
   - Callback function (function): Chứa logic thực hiện test, nhận tham số `page` (đại diện cho trình duyệt) và các options đã cấu hình.

3. **Ví dụ**

   import { test, expect } from '@playwright/test';

   test('basic test', async ({ page }) => {
   await page.goto('https://playwright.dev/');
   await expect(page).toHaveTitle(/Playwright/);
   });

// Ghi lại các hành động và test
// npx playwright codegen --debug https://automationpractice.org/
// Khi chạy xong nó sẽ tạo ra 1 file trong thư mục test

### Generate tests with the Playwright Inspector

- Câu lệnh:

  ```
  npx playwright codegen [URL]
  ```

- Playwright Inspector sẽ mở trình duyệt và thực hiện các hành động của bạn. Bạn có thể tương tác với trang web như bình thường.
- Khi hoàn tất, Playwright Inspector sẽ hiển thị mã code tương ứng với hành động của bạn trong giao diện. Bạn có thể copy mã này để sử dụng trong các bài test của mình.

### View Playwright Test Results

- Câu lệnh:
  ```
  npx playwright show-report
  ```
