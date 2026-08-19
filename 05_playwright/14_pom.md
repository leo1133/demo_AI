# Page Object Model

## Khái niệm

Là một design pattern (mẫu thiết kế) nhằm tối ưu hóa việc quản lý code trong Automation Testing, đặc biệt hữu ích khi giao diện website phức tạp hoặc có nhiều trang.

## Nguyên lý

Nguyên lý: Tách biệt logic kiểm thử (Test Logic) ra khỏi logic giao diện (UI Elements). Mỗi trang web (hoặc một khu vực lớn của trang) sẽ được đóng gói thành một "Object" (Class).

## Cấu trúc

Cấu trúc điển hình: Trong một dự án dùng POM, bạn sẽ thường thấy 2 loại file:

- Page Object Files (.ts): Chứa các định nghĩa về phần tử (Selectors) và các hàm thao tác (Methods) trên trang đó. Ví dụ: LoginPage.ts, HomePage.ts, CartPage.ts.
- Test Files (.spec.ts): Chỉ chứa logic kiểm thử. Nó import các Object từ các file Page bên trên để sử dụng.

## Ưu điểm

- Dễ bảo trì (Maintainable): Nếu giao diện thay đổi (ví dụ đổi ID nút bấm), bạn chỉ cần sửa ở 1 chỗ (trong file Page Object) là tất cả các test case dùng chung phần tử đó sẽ tự động cập nhật.
- Dễ đọc hiểu (Readable): File test trông gọn gàng, dễ theo dõi kịch bản hơn.
- Tái sử dụng (Reusable): Cùng một hàm đăng nhập được định nghĩa trong LoginPage có thể được sử dụng bởi nhiều file test khác nhau.

## Ví dụ

Giả sử website có 2 trang: Trang Đăng nhập và Trang Chủ.

- Bước 1: Tạo thư mục cấu trúc dự án
  Cấu trúc thư mục gợi ý

```
my-project/
├── test-results/
├── tests/
│ ├── pages/ <-- Nơi chứa các Page Object
│ │ ├── LoginPage.ts
│ │ └── HomePage.ts
│ └── e2e/ <-- Nơi chứa kịch bản test
│ └── login.spec.ts
├── playwright-report/
└── playwright.config.ts
```

- Bước 2: Viết Page Object cho Trang Đăng nhập (LoginPage.ts)

```ts
// tests/pages/LoginPage.ts
import { Page, expect } from "@playwright/test";

export class LoginPage {
// 1. Khai báo các phần tử trên trang
readonly page: Page;
readonly fieldUsername: string = "#username";
readonly fieldPassword: string = "#password";
readonly btnLogin: string = "button[type='submit']";
readonly labelError: string = ".error-message";

constructor(page: Page) {
this.page = page;
}

// 2. Định nghĩa các hành động (Methods)
async goto() {
await this.page.goto("https://example.com/login");
}

async login(username: string, password: string) {
await this.page.fill(this.fieldUsername, username);
await this.page.fill(this.fieldPassword, password);
await this.page.click(this.btnLogin);
}

async getErrorMessage() {
return this.page.textContent(this.labelError);
}
}
Bước 3: Viết Page Object cho Trang Chủ (HomePage.ts)

// tests/pages/HomePage.ts
import { Page } from "@playwright/test";

export class HomePage {
readonly page: Page;
readonly linkLogout: string = "#logout-link";
readonly btnAddToCart: string = ".add-to-cart";

constructor(page: Page) {
this.page = page;
}

async clickLogout() {
await this.page.click(this.linkLogout);
}

async addToCart() {
await this.page.click(this.btnAddToCart);
}
}
Bước 4: Viết Test Case sử dụng các Page Object (login.spec.ts)

// tests/e2e/login.spec.ts
import { test, expect } from "@playwright/test";
import { LoginPage } from "../pages/LoginPage";
import { HomePage } from "../pages/HomePage";

test.describe("Kiểm thử Đăng nhập", () => {
// Khởi tạo các biến để chứa Page Objects
let loginPage: LoginPage;
let homePage: HomePage;

test.beforeEach(async ({ page }) => {
// Import instance page vào các Object trước khi dùng
loginPage = new LoginPage(page);
homePage = new HomePage(page);

    await loginPage.goto();

});

test("Đăng nhập thành công và thoát ra", async () => {
// Sử dụng các hành động đã định nghĩa
await loginPage.login("user_test", "123456");

    // Kiểm tra đang ở trang chủ
    await expect(homePage.page).toHaveURL("https://example.com/");

    // Thực hiện hành động ở trang chủ
    await homePage.clickLogout();
    await expect(loginPage.page).toHaveURL("https://example.com/login");

});

test("Đăng nhập thất bại với mật khẩu sai", async () => {
await loginPage.login("user_test", "wrong_password");

    // Kiểm tra hiển thị lỗi
    const errorMsg = await loginPage.getErrorMessage();
    expect(errorMsg).toContain("Sai mật khẩu");

});
});
```

## Khi nào nên áp dụng POM?

- Dự án có nhiều hơn 10 test cases.

- Có sự lặp lại code giữa các test (ví dụ: 5 test case nào cũng phải đăng nhập trước).

- Giao diện website nhiều thành phần và có khả năng thay đổi.

## Khi nào không cần POM?

- Dự án quá nhỏ (chỉ 1-2 test cases đơn giản).

- Bạn viết script chỉ dùng 1 lần rồi bỏ.
