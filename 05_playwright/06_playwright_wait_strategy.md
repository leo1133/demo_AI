# Wait Strategy

- Trong Playwright, `Wait Strategy` là cách Playwright chờ ứng dụng đạt đến trạng thái mong muốn trước khi thực hiện action hoặc assertion
- Playwright chia thành 2 loại wait: `Auto Wait` và `Explicit Wait`
- Auto Wait là cơ chế Playwright tự động chờ element đạt trạng thái phù hợp trước khi thực hiện action.
- Explicit Wait là khi bạn chủ động yêu cầu Playwright chờ một điều kiện hoặc sự kiện cụ thể.

- Playwright ưu tiên: `Wait for condition`, không `wait for time`.

## Playwright ưu tiên:

| Strategy               | Dùng khi                         |
| ---------------------- | -------------------------------- |
| **Auto-waiting**       | Action trên element              |
| **Assertion waiting**  | Kiểm tra UI state                |
| **Locator waiting**    | Chờ element xuất hiện/trạng thái |
| **Navigation waiting** | Chờ page/navigation              |
| **Explicit waiting**   | Những trường hợp đặc biệt        |

### 1. Auto-waiting — Strategy quan trọng nhất

- Khi bạn thực hiện một `Action` trên `Locator`, Playwright không click ngay lập tức.
- Thay vào đó, nó sẽ tự kiểm tra element có:
- tồn tại
- visible
- stable
- enabled
- có thể nhận event
  hay chưa.
- Ví dụ:
  ```ts
  await page.getByLabel("Email").fill("test@gmail.com");
  await page.getByLabel("Password").fill("123456");
  await page.getByRole("button", { name: "Login" }).click();
  ```
  không cần
  ```ts
  await page.waitForTimeout(1000);
  ```
  => Đây là preferred strategy trong Playwright.

### 2. Assertion Waiting

- Assertion trong Playwright cũng có cơ chế auto-retry.
- Playwright sẽ retry assertion cho đến khi: `Element visible` → `Assertion pass` hoặc `Timeout` → `Assertion fail`
- Một số assertion thường dùng
  - `await expect(locator).toBeVisible();`
  - `await expect(locator).toBeHidden();`
  - `await expect(locator).toBeEnabled();`
  - `await expect(locator).toBeDisabled();`
  - `await expect(locator).toHaveText('Success');`
  - `await expect(locator).toHaveValue('test@gmail.com');`
  - `await expect(locator).toHaveCount(5);`
  - `await expect(page).toHaveURL('/dashboard');`

- Ví dụ:
  ```ts
  await page.getByRole("button", { name: "Submit" }).click();
  await expect(page.getByText("Order created")).toBeVisible();
  ```
  không cần
  ```ts
  await page.waitForTimeout(1000);
  ```

### 3. Locator Waiting

- Có thể sử dụng locator để chờ một trạng thái cụ thể.
- Các state phổ biến: `visible`, `hidden`, `enabled`, `disabled`, `checked`, `unchecked`, `selected`, `not_selected`.
- Cú pháp: `await locator.waitFor({ state: 'visible' });`
- Ví dụ:
- Chờ element xuất hiện:
  ```ts
  await page.locator(".toast").waitFor({
    state: "visible",
  });
  ```
- Chờ loading biến mất:
  ```ts
  await page.locator(".loader").waitFor({
    state: "hidden",
  });
  ```
- Chờ element được remove khỏi DOM
  ```ts
  await page.locator(".modal").waitFor({
    state: "detached",
  });
  ```

### 4. Navigation Waiting

- Khi thao tác dẫn đến navigation, playwright sẽ chờ URL đạt expected state.
- Cú pháp: `await page.goto(url, { waitUntil: 'load' });`
- Ví dụ:
  ```ts
  await page.getByRole("button", { name: "Login" }).click();
  await expect(page).toHaveURL(/dashboard/);
  ```

### 5. Wait for API Response

- `waitForResponse()` dùng khi muốn đợi một API request hoàn thành rồi mới tiếp tục test.
- Cú pháp: `const response = await page.waitForResponse('**/api/users');`
- **Lưu ý**: Phải tạo `waitForResponse()` trước action trigger API do API có thể đã được gửi/hoàn thành trước khi Playwright bắt đầu chờ.

### 6. Explicit Waiting

- Explicit waiting là chờ một điều kiện xảy ra trước khi tiếp tục.
- Dùng khi không muốn dùng assertion hoặc locator waiting.
- Cú pháp: `await page.waitForTimeout(1000);`
- Ví dụ: `await expect(locator).toBeVisible()`
- Chỉ nên dùng khi thật sự cần chờ đợi một điều kiện nhất định.

### 7. Wait for Load State

- Playwright có 3 load state: `load`, `domcontentloaded`, `networkidle`.
- Cú pháp: `await page.waitForLoadState('load');`
- Ví dụ:
  ```ts
  await page.goto("https://example.com");
  await page.waitForLoadState("load");
  ```

### 8. waitForTimeout()

- Là set 1 khoảng thời gian nhất định để chờ. Đây là hard wait.
- Khi hard wait, nó không quan tâm UI đã sẵn sàng chưa. Nên hạn chế dùng.
- Cú pháp: `await page.waitForTimeout(1000);`
- Ví dụ:
  ```ts
  await page.goto("https://example.com");
  await page.waitForTimeout(1000);
  ```
- Chỉ dùng khi không có cách nào khác để chờ.

## Wait Strategy theo tình huống

| Tình huống                       | Wait Strategy                       |
| -------------------------------- | ----------------------------------- |
| Thực hiện action trên element    | **Auto-waiting**                    |
| Kiểm tra UI state                | **Assertion Waiting**               |
| Chờ element xuất hiện/trạng thái | **Locator Waiting**                 |
| Chờ navigation                   | **Navigation Waiting**              |
| Chờ API response                 | **waitForResponse()**               |
| Chờ 1 khoảng thời gian nhất định | **waitForTimeout()** (hạn chế dùng) |
