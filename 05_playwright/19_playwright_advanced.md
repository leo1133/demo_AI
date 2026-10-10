# Playwright Advanced Features & Case Study Dự án Live2D

Tài liệu này được cấu trúc thành 2 phần chính:
* **Phần 1: Lý thuyết nền tảng (Playwright Advanced Theory)** — 5 tính năng nâng cao cốt lõi của Playwright.
* **Phần 2: Phân tích & Tối ưu dự án thực tế `live2d`** — Đánh giá chi tiết dự án đang có những gì, các điểm nghẽn (vấn đề) ở đâu và giải pháp nâng cấp bằng Advanced Features.

---

# PHẦN 1: LÝ THUYẾT NỀN TẢNG (PLAYWRIGHT ADVANCED)

## 1. Authentication & Storage State
### 1.1. Vấn đề của phương pháp truyền thống
Nếu mỗi test case đều phải mở trang login, điền form email/password rồi bấm đăng nhập:
* Thời gian thực thi test suite kéo dài gấp 3 – 5 lần.
* Dễ phát sinh lỗi giả (Flaky Test) do mạng chập chờn ở màn hình đăng nhập.

### 1.2. Cơ chế `storageState`
Playwright cho phép trích xuất toàn bộ trạng thái phiên làm việc (Cookies, Local Storage, Session Storage) lưu ra một file JSON (`.auth/user.json`). Các test case sau chỉ cần nạp lại file này là tự động ở trạng thái đã đăng nhập mà không cần qua UI login.

### 1.3. Mô hình chuẩn: Setup Project Dependencies
* Tạo file setup độc lập (`auth.setup.ts`) để thực hiện đăng nhập và lưu session.
* Khai báo `dependencies: ['setup']` trong `playwright.config.ts`.
* Các project test chính chỉ định thuộc tính `storageState: '.auth/user.json'`.

---

## 2. Network & Intercept Request
Playwright cung cấp phương thức `page.route()` giúp kiểm soát trực tiếp lưu lượng mạng của trình duyệt:

* **Mock API Response:** Giả lập dữ liệu trả về (mock data) hoặc status code (200, 400, 403, 500) mà không cần backend thật:
  ```ts
  await page.route('**/api/v1/products', async (route) => {
    await route.fulfill({
      status: 200,
      contentType: 'application/json',
      body: JSON.stringify([{ id: 1, name: 'Sản phẩm mẫu' }]),
    });
  });
  ```
* **Chặn tài nguyên không cần thiết (Abort Requests):** Chặn tải ảnh nặng (`.png`, `.jpg`), fonts, video hoặc các dịch vụ analytics (Google Analytics, Sentry) giúp tăng tốc độ tải trang:
  ```ts
  await page.route(/\.(png|jpeg|jpg|svg|webp)$|google-analytics\.com/, (route) => route.abort());
  ```
* **Sửa đổi Request/Response (Modify):** Bổ sung headers xác thực, custom flags hoặc thay đổi payload gửi lên server.
* **Lắng nghe sự kiện mạng:** Sử dụng `page.waitForResponse()` hoặc `page.waitForRequest()` để đồng bộ hóa chính xác thời điểm dữ liệu được tải về.

---

## 3. Parallel Execution & Workers
### 3.1. Khái niệm Worker Process
* Mỗi **Worker** là một tiến trình OS độc lập sở hữu một phiên bản trình duyệt (Browser Instance) và môi trường thực thi riêng biệt (không dùng chung RAM hay storage).
* Số lượng worker xác định số lượng test có thể chạy đồng thời tại một thời điểm.

### 3.2. Cấu hình song song
* `fullyParallel: true`: Chạy song song tất cả các test case, kể cả các test nằm trong cùng một file.
* `workers`: Cấu hình số luồng chạy:
  * **Local:** Mặc định sử dụng 50% số nhân CPU.
  * **CI/CD:** Thường giới hạn `workers: 2` hoặc `4` để tránh quá tải tài nguyên phần cứng của Runner.
* `test.describe.configure({ mode: 'parallel' | 'serial' })`: Tùy chỉnh chế độ chạy song song hoặc tuần tự cho từng nhóm test cụ thể.

---

## 4. Retry & Report
### 4.1. Phân biệt 2 cơ chế Retry
| Tiêu chí | 1. Built-in Test Runner Retry (`retries: 2`) | 2. Custom `try...catch` trong Test Code |
| :--- | :--- | :--- |
| **Cấp độ** | Framework Level (Playwright quản lý) | Code/Test Script Level (Tự viết tay) |
| **Cách xử lý** | Đóng context cũ, tạo mới context sạch và chạy lại toàn bộ test case từ đầu. | Bắt lỗi cục bộ tại đúng 1 bước và thử chạy lại bước đó ngay trong context hiện tại. |
| **Báo cáo** | Đánh dấu rõ **Flaky Test** kèm video/trace của từng lần chạy để theo dõi và sửa. | Báo cáo màu Xanh (Pass 100%), che giấu việc test đã từng bị lỗi mạng hoặc chập chờn. |
| **Khuyến nghị** | ⭐ **Best Practice** (Nên dùng trên CI/CD). | ⚠️ Hạn chế dùng (Nếu cần retry cục bộ, nên dùng `expect.toPass()`). |

### 4.2. Quản lý Báo cáo (Reporters)
Playwright hỗ trợ đa dạng định dạng báo cáo cùng lúc trong `playwright.config.ts`:
* `html`: Báo cáo giao diện web tương tác đầy đủ hình ảnh, video, trace viewer.
* `list` / `dot`: Hiển thị tiến độ ngắn gọn trên terminal console.
* `json` / `junit`: Xuất kết quả tích hợp CI/CD (Jenkins, GitLab, GitHub Actions).

---

## 5. Screenshot, Video & Debugging
Playwright cung cấp bộ công cụ gỡ lỗi mạnh mẽ với chiến lược lưu trữ thông minh:

* **Chụp ảnh & Quay video có điều kiện:**
  ```ts
  use: {
    screenshot: 'only-on-failure', // Chỉ chụp khi test bị fail
    video: 'retain-on-failure',    // Chỉ lưu video của các lần chạy fail
    trace: 'retain-on-failure',    // Ghi lại toàn bộ hành động (DOM, Network, Action) khi fail
  }
  ```
* **Playwright Trace Viewer:** Công cụ gỡ lỗi tối thượng, cho phép tua lại từng mili-giây hành động của trình duyệt, kiểm tra snapshot DOM trước/sau mỗi click và xem toàn bộ network log:
  ```bash
  npx playwright show-trace test-results/trace.zip
  ```

---
---

# PHẦN 2: CASE STUDY DỰ ÁN THỰC TẾ `LIVE2D`

## 1. Dự án `live2d` ĐÃ CÓ những gì?

Dự án [live2d](file:///Users/ngaphuong/projects/live2d) là một framework kiểm thử đã được xây dựng rất bài bản với nhiều tính năng nâng cao sẵn có:

| STT | Thành phần đã có | Vị trí file & dòng code | Chi tiết cách hoạt động trong `live2d` |
| :---: | :--- | :--- | :--- |
| **1** | **API Token Caching & Auto-Refresh** | [src/fixtures/baseTest.js:L40-L80](file:///Users/ngaphuong/projects/live2d/src/fixtures/baseTest.js#L40-L80) | Tự động đọc payload JWT để kiểm tra thời hạn (`exp`). Nếu hết hạn hoặc gặp lỗi `401/403`, client tự động xóa cache và gọi API login lấy token mới lưu vào `tests/auth/user_<env>.json`. |
| **2** | **Custom Fixtures & Dependency Injection** | [src/fixtures/baseTest.js:L82-L112](file:///Users/ngaphuong/projects/live2d/src/fixtures/baseTest.js#L82-L112) | Mở rộng `test.extend` tự động cung cấp: `loginPage`, `userPage`, `gachaPage`, `authAPI`, `authenticatedRequest` (gắn sẵn Bearer token), `unauthenticatedRequest` và `db`. |
| **3** | **Multi-Project Architecture** | [playwright.config.js:L57-L89](file:///Users/ngaphuong/projects/live2d/playwright.config.js#L57-L89) | Cấu hình chia làm 3 projects độc lập: `Admin UI Tests` (Case 1-18), `API Tests` (Case 19-49) và `E2E Tests` (Case 50-57). |
| **4** | **HTTP Basic Auth Auto-handling** | [playwright.config.js:L48-L53](file:///Users/ngaphuong/projects/live2d/playwright.config.js#L48-L53) | Cấu hình `httpCredentials` tự động điền username/password Basic Auth của server để vượt qua popup bảo vệ của môi trường Dev/Staging. |
| **5** | **PostgreSQL & CSV Integration** | [src/utils/db.helper.js](file:///Users/ngaphuong/projects/live2d/src/utils/db.helper.js)<br>[src/utils/csvHelper.js](file:///Users/ngaphuong/projects/live2d/src/utils/csvHelper.js) | Tích hợp thư viện `pg` để đối soát database thực tế và `csv-parse` phục vụ Data-Driven Testing. |
| **6** | **Runner Retry & Trace Viewer** | [playwright.config.js:L35-L46](file:///Users/ngaphuong/projects/live2d/playwright.config.js#L35-L46) | Cấu hình `retries: process.env.CI ? 2 : 0` (tự retry 2 lần trên CI) và `trace: "on-first-retry"`. |
| **7** | **In-test Retry khi gặp lag mạng** | [tests/ui/02.user.ui.spec.js:L33-L43](file:///Users/ngaphuong/projects/live2d/tests/ui/02.user.ui.spec.js#L33-L43)<br>[tests/e2e/01.login.e2e.spec.js:L31-L46](file:///Users/ngaphuong/projects/live2d/tests/e2e/01.login.e2e.spec.js#L31-L46) | Viết khối `try...catch` in ra `Retrying login after transient failure...` để tự thao tác lại bước đăng nhập khi UI bị delay mạng. |

---

## 2. Các VẤN ĐỀ đang tồn tại trong `live2d` (Pain Points)

Mặc dù dự án đã hoạt động tốt, vẫn còn **4 điểm nghẽn chính** cần được nâng cấp:

### ⚠️ Vấn đề 1: Đăng nhập UI chưa tối ưu & lạm dụng `try...catch` thủ công
* **Hiện trạng:** API đã có cache token, nhưng trên giao diện UI, các test case vẫn phải thao tác điền form login.
* **Hậu quả:** Để chống lag mạng ở bước login này, dự án đang phải chèn nhiều đoạn code `try...catch` thủ công tại `02.user.ui.spec.js`, `03.gacha.ui.spec.js`, `01.login.e2e.spec.js`. Cách làm này làm rối mã nguồn test và tạo ra "lỗi ẩn" (báo cáo test vẫn màu xanh dù thực tế đã bị fail 1 lần).

### ⚠️ Vấn đề 2: Chạy tuần tự (`workers: 1`) gây chậm thời gian kiểm thử
* **Hiện trạng:** Trong `playwright.config.js` đang đặt `fullyParallel: false` và `workers: 1` để chạy tuần tự lần lượt từ Case 1 đến Case 57.
* **Hậu quả:** Toàn bộ test suite mất nhiều thời gian để hoàn thành. Khi số lượng test case tăng lên hàng trăm case, pipeline CI/CD sẽ bị nghẽn nghiêm trọng.

### ⚠️ Vấn đề 3: Chưa sử dụng Network Mocking & Intercept
* **Hiện trạng:** Chưa có test file nào sử dụng `page.route()`.
* **Hậu quả:**
  * Không kiểm thử được các tình huống biên: Server trả về lỗi `500 Internal Server Error`, mạng chậm 10s, hoặc lỗi phân quyền `403`.
  * Trình duyệt vẫn phải tải đầy đủ hình ảnh, avatar nặng và các request bên ngoài trong môi trường test, làm giảm tốc độ thực thi.

### ⚠️ Vấn đề 4: Thiếu Visual Artifacts khi Test Fail (Screenshot & Video)
* **Hiện trạng:** Trong `use` của `playwright.config.js` mới chỉ có `trace: "on-first-retry"`, chưa kích hoạt `screenshot` và `video`.
* **Hậu quả:** Khi một test case bị fail trên CI, tester/dev chỉ có file trace mà thiếu ảnh chụp màn hình nhanh hoặc video ngắn để xem ngay lỗi mà không cần mở Trace Viewer.

---

## 3. Hướng dẫn & Lộ trình CẢI THIỆN bằng Playwright Advanced

Dưới đây là 4 bước nâng cấp toàn diện cho dự án `live2d`:

### 🛠 Cải thiện 1: Thay thế `try...catch` login bằng `storageState` Project Setup

* **Mục tiêu:** Đăng nhập 1 lần duy nhất trên UI, lưu cookie/session vào file `tests/auth/ui_admin.json`. Tất cả các file UI test sau đó sẽ mở thẳng trang quản trị mà không cần đăng nhập lại, **xóa bỏ hoàn toàn nhu cầu viết `try...catch` chống lag mạng ở màn login**.

* **Tạo file mới:** `tests/auth/admin.setup.js`
```js
import { test as setup, expect } from '@playwright/test';
import { LoginPage } from '../../src/pages/LoginPage.js';
import path from 'path';

const authDir = path.resolve(process.cwd(), 'tests/auth');
const authFile = path.join(authDir, 'ui_admin.json');

setup('Authenticate UI Admin', async ({ page }) => {
  const loginPage = new LoginPage(page);
  await loginPage.navigate();
  await loginPage.login(process.env.ADMIN_EMAIL, process.env.ADMIN_PASSWORD);
  
  // Xác nhận đã chuyển hướng vào dashboard/users thành công
  await expect(page).toHaveURL(/.*\/users|.*\/dashboard/);

  // Lưu toàn bộ session state của trình duyệt ra file
  await page.context().storageState({ path: authFile });
});
```

---

### 🛠 Cải thiện 2: Bổ sung Network Intercept để chặn static assets & mock lỗi 500

* **Chặn ảnh tĩnh nặng để tăng tốc độ tải trang:**
```js
// tests/ui/02.user.ui.spec.js
test.beforeEach(async ({ page }) => {
  // Huỷ bỏ tải các định dạng ảnh để UI load tức thì
  await page.route(/\.(png|jpeg|jpg|svg|webp)$/, (route) => route.abort());
});
```

* **Mock API lỗi 500 để kiểm thử thông báo lỗi UI:**
```js
test('Kiểm tra hiển thị Toast thông báo khi API danh sách User bị lỗi 500', async ({ page }) => {
  await page.route('**/api/v1/users/**', async (route) => {
    await route.fulfill({
      status: 500,
      contentType: 'application/json',
      body: JSON.stringify({ message: 'Internal Server Error' }),
    });
  });

  await page.goto('/users');
  await expect(page.getByText('Internal Server Error')).toBeVisible();
});
```

---

### 🛠 Cải thiện 3: Thiết lập chiến lược Data Isolation để chạy Parallel (`workers: 4`)

Để chuyển sang chạy song song mà không sợ các workers xung đột dữ liệu:
1. **Dynamic Data Generation:** Tận dụng [src/utils/helpers.js](file:///Users/ngaphuong/projects/live2d/src/utils/helpers.js) tạo dữ liệu ngẫu nhiên duy nhất cho mỗi worker:
   ```js
   const uniqueEmail = `admin_test_${Date.now()}_${Math.floor(Math.random() * 1000)}@surrealdolls.com`;
   ```
2. **Tự động dọn dẹp dữ liệu qua DB Fixture:**
   ```js
   test.afterEach(async ({ db }) => {
     await db.query(`DELETE FROM users WHERE email LIKE 'admin_test_%'`);
   });
   ```

---

### 🛠 Cải thiện 4: Cập nhật file `playwright.config.js` hoàn chỉnh cho `live2d`

File cấu hình nâng cấp toàn diện, tích hợp đầy đủ: **Dependencies Setup**, **Multi-Workers (4)**, **Screenshot**, **Video**, và **StorageState**:

```js
// live2d/playwright.config.js (Bản nâng cấp tối ưu)
import { defineConfig, devices } from "@playwright/test";
import dotenv from "dotenv";
import path from "path";

const ENV = process.env.ENV || "dev";
dotenv.config({ path: path.resolve(process.cwd(), `.env.${ENV}`) });

export default defineConfig({
  testDir: "./tests",
  timeout: 30 * 1000,
  expect: { timeout: 10 * 1000 },

  // 1. Kích hoạt chạy song song để tăng tốc độ toàn bộ 57 test cases
  fullyParallel: true,
  workers: process.env.CI ? 2 : 4,

  // 2. Quản lý Retry minh bạch qua Test Runner
  retries: process.env.CI ? 2 : 0,

  reporter: [
    ["html", { open: "never", outputFolder: "playwright-report" }],
    ["list"],
  ],

  use: {
    baseURL: process.env.UI_BASE_URL,
    
    // 3. Đầy đủ bộ ba Debugging Artifacts khi có lỗi
    trace: "retain-on-failure",
    screenshot: "only-on-failure",
    video: "retain-on-failure",

    httpCredentials: process.env.BASIC_AUTH_USER
      ? {
          username: process.env.BASIC_AUTH_USER,
          password: process.env.BASIC_AUTH_PASS || "",
        }
      : undefined,
  },

  projects: [
    // Project 0: Setup UI Authentication (Chạy trước 1 lần duy nhất)
    {
      name: "setup",
      testMatch: /.*admin\.setup\.js/,
    },

    // Project 1: Admin UI Tests - Sử dụng storageState từ Setup
    {
      name: "Admin UI Tests",
      testDir: "./tests/ui",
      dependencies: ["setup"],
      use: {
        ...devices["Desktop Chrome"],
        storageState: "tests/auth/ui_admin.json",
      },
    },

    // Project 2: API Tests - Sử dụng Custom Fixture Token độc lập
    {
      name: "API Tests",
      testDir: "./tests/api",
      use: {
        baseURL: process.env.API_BASE_URL,
        extraHTTPHeaders: { Accept: "application/json" },
      },
    },

    // Project 3: E2E Tests - Tích hợp UI + API + Database
    {
      name: "E2E Tests",
      testDir: "./tests/e2e",
      dependencies: ["setup"],
      use: {
        ...devices["Desktop Chrome"],
        storageState: "tests/auth/ui_admin.json",
      },
    },
  ],
});
```
