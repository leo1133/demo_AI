# Annotation

## Annotation trong Playwright

Annotations dùng để gắn metadata/đánh dấu trạng thái hoặc thông tin bổ sung cho test, và các thông tin này có thể xuất hiện trong test report.
Annotation có 2 thành phần chính: `type` , `description`
Ví dụ

```ts
test(
  "Login with valid account",
  {
    annotation: {
      type: "issue",
      description: "https://github.com/example/issues/123",
    },
  },
  async ({ page }) => {
    // test
  },
);
```

Playwright HTML Reporter có thể hiển thị các annotation này, đồng thời chúng cũng có thể được truy cập thông qua Reporter API.

## Phân biệt Tag vs Annotation

|                             | Tag                     | Annotation                      |
| --------------------------- | ----------------------- | ------------------------------- |
| Mục đích                    | Phân loại/filter test   | Cung cấp thông tin chi tiết     |
| Syntax                      | `@smoke`                | `{ type, description }`         |
| Có thể filter bằng `--grep` | ✅                      | ❌ trực tiếp                    |
| Có description              | ❌                      | ✅                              |
| Hiển thị report             | ✅                      | ✅                              |
| Ví dụ                       | `@smoke`, `@regression` | `issue`, `owner`, `performance` |

## Built-in Annotations

Playwright có 4 annotation/built-in modifier rất quan trọng:

```
test.skip()
test.fail()
test.fixme()
test.slow()
```

### test.skip()

Dùng khi không muốn chạy test.
Cú pháp:

```ts
test.skip("Login with Facebook", async ({ page }) => {
  // test này không chạy
});
```

Playwright đánh dấu test là skipped và không thực thi test đó.
Ví dụ: Ví dụ hệ thống có login bằng email, facebook nhưng hiện tại chưa hỗ trợ login bằng facebook thì có thể dùng test.skip() để skip test này.

```ts
test.skip("Login with Facebook", async ({ page }) => {
  // Facebook login chưa được hỗ trợ
});
```

### test.fail()

Dùng khi muốn test luôn fail.
Cú pháp

```ts
test.fail("Test login with Facebook", async ({ page }) => {
  // test này sẽ luôn fail
});
```

Test được đánh dấu là `Failed`, nhưng thực tế test không chạy.
Trong HTML reporter, tab Failed sẽ hiển thị test này.

Ví dụ: Tìm kiếm bằng số điện thoại chưa được hỗ trợ

```ts
test.fail("Search by phone number", async ({ page }) => {
  // Tìm kiếm bằng số điện thoại chưa được hỗ trợ
});
```

### test.fail() khác test.skip() như thế nào?

skip -> không chạy test
fail -> vẫn chạy test nhưng expect = fail

### test.fixme()

Dùng khi test đang bị lỗi và chưa có thời gian sửa (Fix me).
Cú pháp

```ts
test.fixme("Test login with Facebook", async ({ page }) => {
  // test này sẽ luôn fail
});
```

Test được đánh dấu là `Failed`, nhưng thực tế test không chạy.
Trong HTML reporter, tab Failed sẽ hiển thị test này.

### test.slow()

Dùng khi muốn test chạy lâu hơn bình thường (slow).
Cú pháp

```ts
test.slow("Test login with Facebook", async ({ page }) => {
  // test này sẽ chạy lâu hơn bình thường
});
```

Test được đánh dấu là `Slow`, nhưng thực tế test không chạy.
Trong HTML reporter, tab Slow sẽ hiển thị test này.

## Custom Annotation

Ngoài các built-in annotation, bạn có thể tự định nghĩa annotation theo nhu cầu.

```ts
const { test, expect } = require("@playwright/test");

test("has title", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await expect(page).toHaveTitle(/Playwright/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
test("get started link should navigate to docs page", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(page).toHaveURL(/.*intro/);
});
```

## Sử dụng Annotation

```ts
test.skip(
  "Login with Facebook",
  {
    annotation: {
      type: "issue",
      description: "https://github.com/example/issues/123",
    },
  },
  async ({ page }) => {
    // test
  },
);
```

### test.only()

Dùng để focus vào một test.
Cú pháp

```ts
test.only("Login successfully", async ({ page }) => {
  // chỉ test này chạy
});
```

Ví dụ: Nếu có các test

```ts
test('Test 1', ...)
test.only('Test 2', ...)
test('Test 3', ...)
```

-> chỉ chạt 'Test 2'
Playwright chỉ chạy test được focus khi có test.only().
