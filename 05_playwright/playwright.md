### Cách tạo mới folder bằng terminal

mkdir {tên_folder} // {tên_folder} là tên của folder bạn muốn tạo

### Cách di chuyển vào folder

cd {tên_folder} // {tên_folder} là tên của folder bạn muốn di chuyển vào

### Cài đặt Playwright

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

### Generate tests with the Playwright Inspector

- Câu lệnh:

  ```
  npx playwright codegen [URL]
  ```

-
