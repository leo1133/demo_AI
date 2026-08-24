# Demo Framework Design

Mục tiêu: xây dựng nền tảng vững chắc, tối ưu và có khả năng mở rộng cho dự án kiểm thử tự động (Automation Testing Framework). Cụ thể, kết quả cần đạt được sau khi hoàn thành bước này bao gồm:

- Test Structure: Nắm rõ và thiết lập được cấu trúc kịch bản kiểm thử chuẩn hóa bằng cách sử dụng các khối Describe, Test và Hooks (như beforeEach, afterEach).
- Page Object Model (POM): Xây dựng các Page Class riêng biệt để quản lý UI element và các thao tác trên màn hình, giúp mã nguồn dễ bảo trì và tái sử dụng.
- Test Data: Tổ chức và tách biệt dữ liệu kiểm thử ra khỏi mã nguồn bằng cách sử dụng các file JSON hoặc cơ chế Fixture.
- Environment Config: Thiết lập cấu hình linh hoạt cho nhiều môi trường khác nhau (Dev, Staging, Prod) để dễ dàng chuyển đổi khi chạy test.
- Utility Functions: Đóng gói các hàm dùng chung (Common Functions) phục vụ cho toàn bộ dự án (ví dụ: xử lý chuỗi, đọc file, định dạng dữ liệu).

## 1. Khởi tạo Project

Bước 1: Tạo mới project

Bước 2: Cài đặt playwright `npm init playwright@latest`

Bước 3: Cài đặt thư viện dotenv để quản lý môi trường `npm install dotenv`

## 2. Xây dựng Cấu trúc Thư mục Project

Tạo cây thư mục theo cấu trúc chuẩn sau:

```
Playwright_POM_Framework/
├── .env.dev                  # Biến môi trường Dev
├── .env.staging              # Biến môi trường Staging
├── playwright.config.js      # Cấu hình Playwright
├── package.json              # Quản lý scripts và dependencies
│
├── pages/                    # [POM] Xây dựng các Page Class
│   ├── basePage.js
│   └── loginPage.js
│
├── tests/                    # [Test Structure] Chứa các file test spec
│   └── login.spec.js
│
├── fixtures/                 # [Test Data / Fixtures] Custom Fixture
│   └── baseTest.js
│
├── test-data/                # [Test Data] File dữ liệu tĩnh (JSON)
│   └── users.json
│
└── utils/                    # [Utility Functions] Các hàm dùng chung
    ├── dataGenerator.js
    └── elementUtils.js
```

## 3. Cấu hình Environment Config

Bước 1: Tạo các file cấu hình môi trường `.env.dev` và `.env.staging` tại thư mục gốc

Bước 2: Cấu hình `playwright.config.js` để đọc file `.env` động

## 4. Viết Utility Functions (utils/)

### utils/dataGenerator.js (Hàm tạo dữ liệu ngẫu nhiên)

```ts
export class DataGenerator {
  static getRandomEmail() {
    return `test_user_${Date.now()}@example.com`;
  }

  static getRandomPassword(length = 10) {
    return Math.random().toString(36).slice(-length) + "A1!";
  }
}
```

### utils/elementUtils.js (Hàm hỗ trợ tương tác Element)

```ts
export class ElementUtils {
  static async scrollToBottom(page) {
    await page.evaluate(() => window.scrollTo(0, document.body.scrollHeight));
  }

  static async getToastMessage(page) {
    const toast = page.locator('.toast-message, [role="alert"]');
    await toast.waitFor({ state: "visible", timeout: 5000 });
    return await toast.textContent();
  }
}
```

## 5. Xây dựng Page Object Model (pages/)

### pages/basePage.js (Lớp cơ sở chứa các action dùng chung)

```ts
export class BasePage {
  constructor(page) {
    this.page = page;
  }

  async navigateTo(path = "") {
    await this.page.goto(path);
  }

  async getTitle() {
    return await this.page.title();
  }
}
```

### pages/loginPage.js (Xử lý riêng cho trang Login)

```ts
import { BasePage } from "./basePage";

export class LoginPage extends BasePage {
  constructor(page) {
    super(page);
    // Khai báo Locator
    this.emailInput = page.locator("#email");
    this.passwordInput = page.locator("#password");
    this.loginBtn = page.locator('button[type="submit"]');
    this.errorMessage = page.locator(".error-msg");
  }

  // Khai báo Action
  async login(email, password) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.loginBtn.click();
  }
}
```

## 6. Quản lý Test Data & Fixtures

### test-data/users.json (Dữ liệu tĩnh)

```ts
{
  "invalidUser": {
    "email": "wrong_user@example.com",
    "password": "WrongPassword123"
  }
}
```

### fixtures/baseTest.js (Tích hợp Pages và Data vào Custom Fixture)

```ts
import { test as base } from "@playwright/test";
import { LoginPage } from "../pages/loginPage";
import { DataGenerator } from "../utils/dataGenerator";

export const test = base.extend({
  // Tự động khởi tạo loginPage
  loginPage: async ({ page }, use) => {
    await use(new LoginPage(page));
  },

  // Tự động sinh ngẫu nhiên email cho mỗi test
  randomEmail: async ({}, use) => {
    await use(DataGenerator.getRandomEmail());
  },
});

export { expect } from "@playwright/test";
```

## 7. Viết Test Structure (tests/)

Thực thi kịch bản kiểm thử trong `tests/login.spec.js` kết hợp `describe`, `test`, `hooks` và `fixtures`

## 8. Cập nhật Scripts chạy Test (package.json)

Thêm các script để thực thi dễ dàng theo môi trường:

```json
"scripts": {
  "test:dev": "cross-env ENV=dev npx playwright test",
  "test:staging": "cross-env ENV=staging npx playwright test",
  "test:ui": "npx playwright test --ui"
}
```
