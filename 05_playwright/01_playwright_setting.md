# Cách tạo mới folder bằng terminal

`mkdir {tên_folder}` // {tên_folder} là tên của folder bạn muốn tạo

# Cách di chuyển vào folder

`cd {tên_folder}` // {tên_folder} là tên của folder bạn muốn di chuyển vào

# Cài đặt Playwright

Lệnh npm: `npm init playwright@latest` // Tạo file cấu hình cho Npm

Lệnh yarn: `yarn create playwright` // Tạo file cấu hình cho Yarn

Lệnh pnpm: `pnpm create playwright` // Tạo file cấu hình cho Pnpm

# Một số lệnh khác trong playwright

`npx playwright install chromium` // Cài đặt trình duyệt Chromium cho Playwright

`npx playwright install` // Cài đặt tất cả các trình duyệt (Chromium, Firefox, WebKit)

`npx playwright codegen [URL]` // Mở trình duyệt và ghi lại hành động của người dùng, sau đó chuyển đổi hành động thành mã test

`npx playwright test [options]` // Chạy tất cả các test trong dự án

`npx playwright test test/login.spec.ts --ui` // Chạy test với giao diện người dùng

`npx playwright show-report` // Hiển thị báo cáo kết quả test dưới dạng HTML

`npx playwright codegen --debug [URL]` // Mở trình duyệt với chế độ gỡ lỗi, cho phép kiểm tra từng bước thực hiện test
