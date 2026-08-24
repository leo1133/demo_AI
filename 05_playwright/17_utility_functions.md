# Utility Functions

Utility Functions trong Playwright là các hàm trợ giúp (helper functions) được tái sử dụng để xử lý các tác vụ lặp đi lặp lại như: tạo dữ liệu giả, tương tác nâng cao với DOM, chờ API/Network, hoặc xử lý các luồng phức tạp (đăng nhập, upload file).

## Các mẫu Utility Functions phổ biến và hữu ích nhất

### 1. Helper cho Login/Authentication

**Mục đích**: Giúp sinh dữ liệu test tĩnh hoặc ngẫu nhiên để tránh việc sử dụng trùng lặp dữ liệu trên môi trường test.

```ts
// utils/dataGenerators.ts
export const generateRandomUser = () => {
  const timestamp = Date.now();
  return {
    username: `user_${timestamp}`,
    email: `test_${timestamp}@example.com`,
    password: "Password123!",
  };
};

export const getRandomNumber = (min: number, max: number): number => {
  return Math.floor(Math.random() * (max - min + 1)) + min;
};
```

### 2. Hàm tương tác và chờ giao diện (UI Helpers)

**Mục đích**: Xử lý các thao tác cuộn trang hoặc xử lý Toast message/thông báo hệ thống.

```ts
// utils/uiHelpers.ts
import { Page, Locator, expect } from "@playwright/test";

// Xử lý cuộn xuống cuối trang (infinite scroll)
export async function scrollToBottom(page: Page) {
  await page.evaluate(async () => {
    await new Promise((resolve) => {
      let totalHeight = 0;
      const distance = 100;
      const timer = setInterval(() => {
        const scrollHeight = document.body.scrollHeight;
        window.scrollBy(0, distance);
        totalHeight += distance;

        if (totalHeight >= scrollHeight) {
          clearInterval(timer);
          resolve(true);
        }
      }, 100);
    });
  });
}

// Bắt và kiểm tra thông báo Toast xuất hiện
export async function verifyToastMessage(page: Page, expectedText: string) {
  const toast = page.locator('.toast-message, [role="alert"]');
  await expect(toast).toBeVisible({ timeout: 5000 });
  await expect(toast).toContainText(expectedText);
}
```

### 3. Hàm tương tác Network & API (API Helpers)

**Mục đích**: Hỗ trợ gọi API trực tiếp, có thể viết helper để tạo dữ liệu nhanh qua API trước khi chạy UI test.

```ts
// utils/apiHelpers.ts
import { APIRequestContext } from "@playwright/test";

export async function createTestUserViaAPI(
  request: APIRequestContext,
  userData: object,
) {
  const response = await request.post("/api/v1/users", {
    data: userData,
  });

  if (!response.ok()) {
    throw new Error(`Failed to create user via API: ${response.statusText()}`);
  }

  return await response.json();
}
```

### 4. Báo cáo & Chụp ảnh màn hình (Screenshot & Debug Helpers)

**Mục đích**: Hỗ trợ chụp ảnh màn hình hoặc đính kèm log vào báo cáo khi có lỗi.

```ts
// utils/reportHelpers.ts
import { Page, TestInfo } from "@playwright/test";

export async function captureScreenshotOnFailure(
  page: Page,
  testInfo: TestInfo,
) {
  if (testInfo.status !== testInfo.expectedStatus) {
    const screenshotPath = testInfo.outputPath(
      `failure_${testInfo.title.replace(/\s+/g, "_")}.png`,
    );
    await page.screenshot({ path: screenshotPath, fullPage: true });
    testInfo.attachments.push({
      name: "screenshot-on-failure",
      path: screenshotPath,
      contentType: "image/png",
    });
  }
}
```

## Cách tối ưu Utility Functions bằng Custom Fixtures

Thay vì phải import hàm helper vào từng file test, cách chuẩn nhất trong Playwright là tích hợp utility vào Custom Fixtures

Ví dụ:

**Tạo Custom Fixtures**

```ts
// fixtures/myTest.ts
import { test as base } from "@playwright/test";
import { generateRandomUser } from "../utils/dataGenerators";

type MyUtilities = {
  randomUser: ReturnType<typeof generateRandomUser>;
};

export const test = base.extend<MyUtilities>({
  randomUser: async ({}, use) => {
    // Tự động tạo user mới cho mỗi test case
    const user = generateRandomUser();
    await use(user);
  },
});

export { expect } from "@playwright/test";
```

**Tích hợp trong Test**

```ts
import { test, expect } from "./fixtures/myTest";

test("Đăng ký tài khoản thành công", async ({ page, randomUser }) => {
  await page.goto("/register");
  await page.getByLabel("Email").fill(randomUser.email);
  await page.getByLabel("Password").fill(randomUser.password);
  await page.getByRole("button", { name: "Sign up" }).click();
});
```
