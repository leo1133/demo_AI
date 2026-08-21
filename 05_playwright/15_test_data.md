# JSON trong Playwright

Được sử dụng cho 2 mục đích chính:

- Quản lý dữ liệu kiểm thử (Test Data)
- Lưu trạng thái đăng nhập (Storage State)

Khi nào nên sử dụng JSON

- Khi cần quản lý dữ liệu tĩnh. Ví dụ cần tạo data cho từng môi trường -> tách riêng thành file JSON giúp thay đổi dữ liệu mà không cần sửa code
- Khi cần dữ liệu trạng thái tách biệt khỏi mã nguồn chính. Ví dụ: lưu trạng thái đăng nhập trong file json -> dùng lại nhiều test case -> tiết kiệm thời gian chạy.

## 1. Quản lý dữ liệu kiểm thử (Test Data)

JSON giúp tách biệt dữ liệu ra khỏi code, giúp dễ dàng quản lý và thay đổi dữ liệu mà không cần sửa code.

Ví dụ:

File: `data/user.json`

```js
// File test/data/loginData.ts
export const loginData = {
  users: {
    valid: {
      username: "user_test",
      password: "123456",
    },
    invalid: {
      username: "user_test",
      password: "12345",
    },
  },
};
```

File: `tests/login.spec.ts`

```typescript
import { test, expect } from "@playwright/test";
import { loginData } from "./data/user.json";

test("Đăng nhập thành công với dữ liệu JSON", async ({ page }) => {
  await page.goto("https://example.com/login");
  await page.fill("#username", loginData.users.valid.username);
  await page.fill("#password", loginData.users.valid.password);
  await page.click("button[type='submit']");
  await expect(page).toHaveURL("https://example.com/");
});
```

## 2. Lưu trạng thái đăng nhập (Storage State)

Giúp bỏ qua bước đăng nhập lại ở mỗi bài test, tiết kiệm thời gian chạy.

File: `tests/login.spec.ts`

```typescript
import { test, expect } from "@playwright/test";

test("Đăng nhập và lưu trạng thái", async ({ page }) => {
  await page.goto("https://example.com/login");
  await page.fill("#username", "user_test");
  await page.fill("#password", "123456");
  await page.click("button[type='submit']");
  await expect(page).toHaveURL("https://example.com/");

  // Lưu trạng thái đăng nhập vào file storageState.json
  await page.context().storageState({ path: "storageState.json" });
});
```

# Fixture trong Playwright

Fixture là cơ chế chuẩn bị môi trường trước khi chạy test và dọn dẹp sau khi test xong.

Playwright đi kèm sẵn các built-in fixtures như: `page`, `context`, `browser`, `request`. Tuy nhiên, sức mạnh lớn nhất của Fixture là cho phép bạn tạo Fixture tùy chỉnh (Custom Fixture) để triển khai Page Object Model (POM) hoặc khởi tạo dữ liệu tự động.

Khi nào nên dùng Fixture:

- Cần chuẩn bị dữ liệu cho test trước khi chạy -> test có thể tự động được setup môi trường
- Cần khởi tạo dữ liệu cho nhiều test case
- Cần khởi tạo dữ liệu từ nhiều nguồn khác nhau
- Cần dọn dẹp sau khi test xong

## Cách tạo Custom Fixture (Ví dụ về Page Object Model)

File: `fixtures/my-fixture.ts`

```ts
import { test as base } from "@playwright/test";
import { LoginPage } from "../pages/LoginPage";

// Định nghĩa kiểu dữ liệu cho Fixture
type MyFixtures = {
  loginPage: LoginPage;
};

// Mở rộng 'test' gốc bằng fixture mới
export const test = base.extend<MyFixtures>({
  loginPage: async ({ page }, use) => {
    // 1. Setup: Khởi tạo Page Object
    const loginPage = new LoginPage(page);

    // 2. Cung cấp fixture cho bài test sử dụng
    await use(loginPage);

    // 3. Teardown (Nếu cần dọn dẹp dữ liệu/log out sau khi test xong)
  },
});

export { expect } from "@playwright/test";
```

File: `tests/login.spec.ts`

```ts
import { test, expect } from "../fixtures/my-fixture";

// Nhận trực tiếp 'loginPage' mà không cần 'new LoginPage(page)'
test("Test với custom fixture", async ({ loginPage }) => {
  await loginPage.goto();
  await loginPage.login("user", "pass");
});
```
