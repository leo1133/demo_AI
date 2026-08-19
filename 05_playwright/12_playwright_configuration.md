# Configuration

Configuration nói về cách cấu hình toàn bộ cách Playwright thu thập, chạy, retry, report và quản lý test.

## Cấu trúc và Vai trò File Cấu hình Playwright

File playwright.config (thường là .ts hoặc .js) là thành phần trung tâm trong cấu trúc dự án (scaffold) được tạo ra tự động khi thực hiện cài đặt Playwright

## playwright.config.ts

Là nơi cấu hình test runner.

Ví dụ

```ts
import { defineConfig, devices } from "@playwright/test";

export default defineConfig({
  testDir: "./tests",

  fullyParallel: true,

  retries: 2,

  reporter: "html",

  use: {
    baseURL: "http://localhost:3000",
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

Tuy nhiên, không phải mọi configuration đều nằm trong use.
Playwright chia configuration thành hai nhóm chính:

```
playwright.config.ts
│
├── Test Runner Configuration
│   ├── testDir
│   ├── timeout
│   ├── retries
│   ├── workers
│   ├── reporter
│   ├── fullyParallel
│   └── projects
│
└── use
    ├── baseURL
    ├── browser
    ├── viewport
    ├── storageState
    ├── screenshot
    ├── video
    └── trace
```

## Basic Configuration

### 1. testDir

Xác định thư mục chứa testcase.

```ts
export default defineConfig({
  testDir: "./tests",
});
```

Ví dụ:

```
project/
├── tests/
│   ├── login.spec.ts
│   ├── register.spec.ts
│   └── checkout.spec.ts
│
└── playwright.config.ts

-> Playwright sẽ tìm testcase trong tests.
```

### 2. fullyParallel

```ts
fullyParallel: true;
```

-> Cho phép chạy tất cả các test trong mọi project song song. Điều này giúp giảm thời gian chạy regression lớn.
Playwright mặc định đã chạy các test file song song; `fullyParallel` cho phép mức song song rộng hơn, bao gồm các test trong các file.

### 3. forbidOnly

```ts
forbidOnly: !!process.env.CI;
```

Mục đích chính là tránh việc vô tình commit:

```ts
test.only("login", () => { ... });
```

Mà không xoá sau đó.

Khi `forbidOnly` là `true`, Playwright sẽ chặn tất cả các test có `test.only()` hoặc `test.skip()` (trừ khi test.skip được override bởi `fullyParallel` hoặc configuration khác).

### 4. retries

```ts
retries: 2;
```

Playwright định nghĩa retries là số lần retry tối đa cho mỗi test.

Ví dụ:

```ts
// test1.spec.ts

test("test1", () => {
  throw new Error("test1 failed");
});

// playwright.config.ts
export default defineConfig({
  retries: 2,
});
```

Khi `retries: 2`, nếu test1 fail, nó sẽ retry 2 lần nữa trước khi báo fail.

### 5. workers

```ts
// worker configuration
workers: 4;
```

- workers: Số lượng workers mặc định (mỗi worker có 1 browser)
- workers: -1: Sử dụng tất cả worker
- workers: 0: Sử dụng tất cả CPU cores

### 6. Timeout

Timeout trong Playwright có 4 cấp độ:

1. Global timeout: Thời gian tối đa cho mỗi test
2. Test timeout: Thời gian tối đa cho mỗi test
3. Each timeout: Thời gian tối đa cho mỗi action
4. Navigation timeout: Thời gian tối đa cho mỗi navigation

Ví dụ:

```ts
// Global timeout (1 minute)
timeout: 60000;

// Test timeout (30 seconds)
testTimeout: 30000;

// Action timeout (10 seconds)
eachTimeout: 10000;

// Navigation timeout (5 seconds)
navigationTimeout: 5000;
```

### 7. Reporter

```ts
reporter: [["dot"], ["list"], ["html", { open: "never" }]];
```

Mục đích: Playwright hỗ trợ nhiều định dạng output khi chạy test.

- "dot": Output đơn giản
- "list": Output dạng danh sách
- "html": Output dạng HTML (mặc định)

### 8. project

`projects` cho phép bạn chạy cùng bộ testcase với nhiều configuration khác nhau.

Ví dụ:

```ts
projects: [
  {
    name: "chromium",
    use: { ...devices["Desktop Chrome"] },
  },
  {
    name: "firefox",
    use: { ...devices["Desktop Firefox"] },
  },
  {
    name: "webkit",
    use: { ...devices["Desktop Safari"] },
  },
];
```

-> Chạy 1 file testcase trên 3 browser cùng 1 lúc. (1 worker = 1 browser)

## use

`use` dùng để cấu hình môi trường mà testcase sẽ chạy, đặc biệt là Browser, BrowserContext và các hành vi mặc định của page.
`use` chính là nơi bạn thiết lập default options cho test

Ví dụ:

```ts
import { defineConfig } from "@playwright/test";

export default defineConfig({
  use: {
    baseURL: "https://example.com",
    headless: true,
    screenshot: "only-on-failure",
    video: "on-first-retry",
    trace: "on-first-retry",
  },
});
```

### 1. baseURL

Cấu hình URL cơ sở cho toàn bộ test suite

```ts
use: {
  baseURL: 'https://example.com',
}
```

Khi có nhiều environment -> chỉ cần thay baseURL.

### 2. browser

Có 3 option: chromium, firefox, webkit.

### 3. viewport

Dùng để cấu hình kích thước màn hình.

```ts
use: {
  viewport: {
    width: 1280,
    height: 720,
  },
}
```

-> Browser sẽ chạy với màn hình có kích thước 1280 x 720.
Một testcase có thể chạy trên nhiều viewport (nếu được define trong projects).

```ts
projects: [
  {
    name: "desktop",
    use: {
      viewport: {
        width: 1440,
        height: 900,
      },
    },
  },

  {
    name: "mobile",
    use: {
      viewport: {
        width: 390,
        height: 844,
      },
    },
  },
];
```

### 4. storageState

```ts
use: {
  storageState: 'playwright/.auth/user.json',
}
```

File này có thể lưu: `cookies`, `localStorage`, `sessionStorage` -> testcase không cần login lại mỗi lần.

### 5. screenshot

```ts
use: {
  screenshot: "only-on-failure",
}
```

Các mode phổ biến

- `off`: Không chụp screenshot.
- `on`: Mỗi test đều có screenshot.
- `only-on-failure`: Chỉ chụp khi testcase fail.

### 6. video

Dùng để record video khi test chạy.

```ts
use: {
  video: 'on-first-retry',
}
```

Các mode phổ biến

- `off`: Tắt video.
- `on`: Luôn quay video.
- `only-on-failure`: Chỉ quay video khi testcase fail.
- `on-first-retry`: Chỉ quay video lần đầu tiên khi testcase fail.

### 7. trace

```ts
use: {
  trace: 'on-first-retry',
}
```

Trace Viewer có thể giúp bạn xem:

- action
- locator
- screenshot
- DOM snapshot
- network
- console
- timing
