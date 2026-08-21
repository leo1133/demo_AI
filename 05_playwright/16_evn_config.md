# Environment Config

Environment Config trong Playwright giúp quản lý linh hoạt các tham số như Domain URL, API Keys, Timeout, Browser settings hay Account test giữa các môi trường khác nhau (Dev, Staging, Production).

Mục đích chính: Khi thay đổi môi trường -> chỉ cần thay đổi file config mà không cần sửa code.

## 1. Các phương pháp cấu hình môi trường phổ biến

Sử dụng biến môi trường (.env & dotenv): Cách chuẩn nhất để truyền URL và các dữ liệu nhạy cảm (API Keys, Passwords).

Phân chia môi trường bằng projects trong playwright.config.ts: Thích hợp khi muốn chạy song song hoặc phân biệt rõ cấu hình chạy trên từng môi trường.

Tách riêng các file config (playwright.dev.config.ts, playwright.staging.config.ts): Áp dụng khi cấu hình giữa các môi trường khác biệt quá nhiều

## 2. Cài đặt dotenv

Bước 1: Cài đặt thư viện dotenv bằng lệnh `npm install dotenv --save-dev`

Bước 2: Tạo file .env trong thư mục gốc (same level with package.json)
Ví dụ:

dev

```ts
BASE_URL=https://dev.example.com
API_KEY=dev_secret_key_123
```

STG

```ts
BASE_URL=https://staging.example.com
API_KEY=staging_secret_key_456
```

Bước 3: Cấu hình file `playwright.config.ts`

```ts
import { defineConfig, devices } from "@playwright/test";
import dotenv from "dotenv";
import path from "path";

// Đọc môi trường từ dòng lệnh (mặc định là 'dev' nếu không truyền ENV)
const ENV = process.env.ENV || "dev";

// Load file .env tương ứng
dotenv.config({ path: path.resolve(__dirname, `.env.${ENV}`) });

export default defineConfig({
  testDir: "./tests",
  use: {
    // Tự động gán baseURL từ file .env
    baseURL: process.env.BASE_URL,
    trace: "on-first-retry",
  },
  projects: [
    {
      name: "chromium",
      use: { ...devices["Desktop Chrome"] },
    },
  ],
});
```

Bước 4: Sử dụng trong file Test

```ts
import { test, expect } from "@playwright/test";

test("Kiểm tra trang đăng nhập", async ({ page }) => {
  // Đi tới đường dẫn tương đối (tự nối với baseURL trong config)
  await page.goto("/login");

  // Lấy biến môi trường khác
  console.log("API Key đang dùng:", process.env.API_KEY);
});
```

Bước 5: Lệnh thực thi Test theo từng môi trường

```bash
# Chạy trên môi trường Dev (hoặc mặc định)
npx playwright test

# Chạy trên môi trường Staging (Linux/macOS)
ENV=staging npx playwright test

# Chạy trên môi trường Staging (Windows PowerShell)
$env:ENV="staging"; npx playwright test
```

## 3. Các tùy chọn cấu hình quan trọng trong use

| Tùy chọn         | Mô tả                                                                            |
| ---------------- | :------------------------------------------------------------------------------- |
| baseURL          | Địa chỉ URL gốc của ứng dụng (giúp dùng path tương đối như page.goto('/login')). |
| headless         | true (chạy ẩn) hoặc false (mở giao diện trình duyệt).                            |
| screenshot       | 'off', 'on', 'only-on-failure'                                                   |
| video            | 'off', 'on', 'only-on-failure'                                                   |
| extraHTTPHeaders | Thêm các Header cố định (như Authorization) cho mọi request network.             |
