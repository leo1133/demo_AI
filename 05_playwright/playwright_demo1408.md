# Demo 1408

Từ codegen -> lấy được

```ts
import { test, expect } from "@playwright/test";

test("test", async ({ page }) => {
  await page.goto("https://crm.anhtester.com/admin/authentication");
  await page.getByRole("textbox", { name: "Email Address" }).click();
  await page
    .getByRole("textbox", { name: "Email Address" })
    .fill("admin@example.com");
  await page.getByRole("textbox", { name: "Password" }).click();
  await page.getByRole("textbox", { name: "Password" }).fill("123456");
  await page.getByRole("checkbox", { name: "Remember me" }).check();
  await page.getByRole("button", { name: "Login" }).click();
});
```

Giải thích

| Nhóm          | Code                    | Ý nghĩa                    |
| ------------- | ----------------------- | -------------------------- |
| Locator       | getByRole               | Tìm element                |
| Action        | page.goto(...)          | Mở trang                   |
| Action        | .fill                   | Nhập dữ liệu               |
| Assertion     | expect(...).toHaveValue | Kiểm tra value             |
| Wait Strategy | await + auto-wait       | Chờ action/element phù hợp |
| Wait Strategy | toHaveValue             | Tự retry assertion         |
