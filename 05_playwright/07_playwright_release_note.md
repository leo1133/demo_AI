# Release Note Playwright

## Một version của Playwright sẽ có 3 nhóm:

- API mới → biết Playwright có khả năng gì mới.
- Breaking Changes / Deprecation → biết code cũ nào có thể bị lỗi.
- Behavior Changes → biết hành vi của Playwright thay đổi như thế nào.

## Playwright Release Notes dùng để làm gì?

Sau khi nâng version Playwright, Release Notes cho biết:

- Có API mới không?
- API cũ có bị deprecated không?
- Browser version có thay đổi không?
- Cách retry, timeout, locator, screenshot... có thay đổi không?
- Có breaking change nào ảnh hưởng code hiện tại không?

Nó giống như changelog của Playwright.

## Version 1.62 – những thay đổi đáng chú ý

### Component Testing model mới

Playwright thay đổi cách tiếp cận Component Testing sang mô hình:
Component
↓
Story
↓
Gallery
↓
mount()
↓
Test
Component Testing không còn đơn giản chỉ là mount component trực tiếp, mà sử dụng stories + gallery để định nghĩa các scenario của component.
Giả sử có: `button`

```
Button
├── Default
├── Disabled
├── Loading
├── With Icon
└── Long Text
```

=> Mỗi cái có thể trở thành một story/scenario.

### AbortSignal – Cancel một operation

Playwright đã thêm tham số `abortSignal` vào nhiều API, cho phép cancel operation thủ công.
Áp dụng cho:

```ts
locator.click({ abortSignal });
locator.fill({ abortSignal });
locator.waitFor({ abortSignal });
promise.race([
  page.waitForNavigation(),
  new Promise((resolve) => setTimeout(resolve, 3000, { timeout: true })),
]);
```

AbortSignal không thay thế timeout
Khi nào thì sử dụng AbortSignal:

- Nếu operation chạy quá lâu, bạn có thể chủ động cancel thay vì chờ toàn bộ timeout.
- Timeout vẫn chạy ngầm (không bị ảnh hưởng).

### WebP Screenshot

Playwright 1.62 hỗ trợ screenshot/snapshot bằng WebP.
Thay đổi trong `Playwright.config.ts`:

```ts
{
  "use": {
    "screenshot": "only-on-failure",
    "video": "retain-on-failure",
    "trace": "retain-on-failure",
    "screenshot": "webp"  // <-- thêm cái này
  }
}
```

### Isolated Retries

Trong Playwright 1.62, retry được thực hiện:

- Với strategy mặc định: retry sẽ chạy khi worker có thể chạy lại test.
- Với isolated: chỉ chạy các failed tests sau khi các test khác hoàn thành, từng retry một trong một worker.

```ts
export default defineConfig({
  retries: 2,
  retryStrategy: "isolated",
});
```

### scroll: "none"

Playwright thường tự động scroll element vào viewport trước khi action nhưng version 1.62 cho phép kiểm soát điều này

```ts
await locator.click({
  scroll: "none",
});
```

### scroll: "none"

Các action có option

- scroll: "auto"
- scroll: "none"
  Khi scroll = "none", Playwright sẽ không scroll vào viewport trước khi action.

```
Element ngoài viewport
        ↓
Playwright auto scroll
        ↓
Click
```

Cài đặt `scroll: "none"` -> Playwright không thực hiện automatic scroll-into-view.
Điều này hữu ích khi test những UI mà scroll behavior chính là thứ bạn muốn kiểm tra.

### apiResponse.timing()

Playwright 1.62 thêm `apiResponse.timing()` để lấy thông tin timing của API response.
Đây là một feature đáng chú ý nếu mục tiêu là: `Playwright + API Testing + Performance awareness.`

```ts
const resp = await page.goto("https://playwright.dev");
const timing = resp.timing(); // {} với keys như fetchStart, domainLookupStart, domainLookupEnd, connectStart, secureConnectionStart, requestStart, responseStart, responseEnd, domInteractive, domContentLoaded, loadEvent
```

### locator.waitForFunction()

Playwright 1.62 thêm `locator.waitForFunction()` mục đích là chờ cho một function chạy trên element và trả về truthy.
Chỉ dùng waitForFunction() khi condition là custom.

```ts
const loc = page.getByRole("button", { name: "Submit" });

// Wait cho đến khi locator.textContent() contain "Submitted"
await loc.waitForFunction(async (el) => {
  return el.textContent().includes("Submitted");
});

// Hoặc với argument
await loc.waitForFunction(async (el, value) => {
  return el.textContent().includes(value);
}, "Submitted");
```

### Playwright MCP và playwright-cli

Playwright hiện bundle:

```
Playwright MCP
playwright-cli
```

và có thể chạy `npx playwright mcp` hoặc `npx playwright cli`
