# Playwright Advanced Features & Case Study Dự án Live2D

Tài liệu này được cấu trúc thành 2 phần chính:
* **Phần 1: Lý thuyết nền tảng (Playwright Advanced Theory)** — 5 tính năng nâng cao cốt lõi của Playwright.
* **Phần 2: Case Study Dự án Thực tế `live2d` (So sánh Before vs. After)** — Đánh giá chi tiết dự án, đối chiếu trực diện code **TRƯỚC** và **SAU** khi tối ưu, kèm kịch bản thuyết trình thực tế.

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

# PHẦN 2: CASE STUDY DỰ ÁN THỰC TẾ `LIVE2D` (SO SÁNH BEFORE vs. AFTER)

## 1. Dự án `live2d` ĐÃ CÓ những gì?

Dự án [live2d](file:///Users/ngaphuong/projects/live2d) là một framework kiểm thử đã được xây dựng bài bản với nhiều tính năng nâng cao sẵn có:

| STT | Thành phần đã có | Vị trí file & dòng code | Chi tiết cách hoạt động trong `live2d` |
| :---: | :--- | :--- | :--- |
| **1** | **API Token Caching & Auto-Refresh** | [src/fixtures/baseTest.js:L40-L80](file:///Users/ngaphuong/projects/live2d/src/fixtures/baseTest.js#L40-L80) | Tự động đọc payload JWT để kiểm tra thời hạn (`exp`). Nếu hết hạn hoặc gặp lỗi `401/403`, client tự động xóa cache và gọi API login lấy token mới lưu vào `tests/auth/user_<env>.json`. |
| **2** | **Custom Fixtures & Dependency Injection** | [src/fixtures/baseTest.js:L82-L112](file:///Users/ngaphuong/projects/live2d/src/fixtures/baseTest.js#L82-L112) | Mở rộng `test.extend` tự động cung cấp: `loginPage`, `userPage`, `gachaPage`, `authAPI`, `authenticatedRequest` (gắn sẵn Bearer token), `unauthenticatedRequest` và `db`. |
| **3** | **Multi-Project Architecture** | [playwright.config.js:L62-L105](file:///Users/ngaphuong/projects/live2d/playwright.config.js#L62-L105) | Cấu hình chia các projects: `setup`, `Admin UI Tests`, `API Tests` và `E2E Tests`. |
| **4** | **HTTP Basic Auth Auto-handling** | [playwright.config.js:L52-L58](file:///Users/ngaphuong/projects/live2d/playwright.config.js#L52-L58) | Cấu hình `httpCredentials` tự động điền username/password Basic Auth của server để vượt qua popup bảo vệ của môi trường Dev/Staging. |
| **5** | **PostgreSQL & CSV Integration** | [src/utils/db.helper.js](file:///Users/ngaphuong/projects/live2d/src/utils/db.helper.js)<br>[src/utils/csvHelper.js](file:///Users/ngaphuong/projects/live2d/src/utils/csvHelper.js) | Tích hợp thư viện `pg` để đối soát database thực tế và `csv-parse` phục vụ Data-Driven Testing. |

---

## 2. Bảng Tổng Hợp So Sánh Trước & Sau Tối Ưu (Slide Overview)

| Tiêu chí | Trước khi tối ưu (BEFORE) | Sau khi tối ưu (AFTER) | Tác động / Giá trị mang lại |
| :--- | :--- | :--- | :--- |
| **1. Xác thực UI** | Mọi file UI đều mở form và điền user/password; dùng `try...catch` thủ công để chống lag. | Đăng nhập 1 lần duy nhất trong `setup` và tái sử dụng session qua `storageState`. | Tiết kiệm ~70% thời gian đăng nhập; loại bỏ hoàn toàn mã `try...catch` rác. |
| **2. Tốc độ thực thi** | Chạy tuần tự: `workers: 1`, `fullyParallel: false`. | Chạy song song: `workers: 4`, `fullyParallel: true` + Data Isolation. | Thời gian chạy toàn bộ test suite giảm từ 3–4 lần. |
| **3. Lưu lượng mạng** | Tải toàn bộ ảnh tĩnh/avatar nặng; không test được các case lỗi Server 500. | Dùng `page.route()` để chặn tải ảnh và Mock lỗi 500/403. | Tốc độ load trang tăng mạnh; test được kịch bản biên không phụ thuộc backend. |
| **4. Báo cáo & Debug** | Chỉ có `trace: "on-first-retry"`, không có screenshot hay video khi fail. | Bộ ba `screenshot: "only-on-failure"`, `video: "retain-on-failure"`, `trace`. | Xem ngay ảnh/video lỗi trên HTML Report mà không bắt buộc mở Trace Viewer. |

---

## 3. Chi Tiết 4 Cặp So Sánh Trực Diện (Code Before vs. Code After)

### 📌 Điểm nghẽn 1: Đăng nhập UI rườm rà & lạm dụng `try...catch` thủ công

#### 🔴 Code TRƯỚC (BEFORE):
Trong các file kiểm thử UI (`tests/ui/02.user.ui.spec.js`, `tests/ui/03.gacha.ui.spec.js`), mỗi suite đều phải tự import `LoginPage`, khởi tạo page mới, điền form đăng nhập và bọc trong khối `try...catch` để tự thao tác lại khi UI bị chậm:

```javascript
// tests/ui/02.user.ui.spec.js (BEFORE - Code ban đầu)
test.beforeAll(async ({ browser }) => {
  const context = await browser.newContext({ baseURL: process.env.UI_BASE_URL });
  sharedPage = await context.newPage();
  
  const loginPage = new LoginPage(sharedPage);
  userPage = new UserPage(sharedPage);

  // ❌ Mỗi file test đều phải tự gõ email/password qua UI từ đầu
  await loginPage.goto();
  await loginPage.login(loginData.credentials.account, loginData.credentials.password);

  // ❌ Phải dùng try...catch thủ công để tự click login lại nếu bị timeout/chậm mạng
  try {
    await loginPage.verifyLoginSuccess(new RegExp(userData.titles.dashboard));
  } catch (err) {
    console.warn("Retrying login after transient failure...");
    await loginPage.goto();
    await loginPage.login(loginData.credentials.account, loginData.credentials.password);
    await loginPage.verifyLoginSuccess(new RegExp(userData.titles.dashboard));
  }
});
```

* **Vấn đề / Hậu quả cần nhấn mạnh với người nghe:**
  1. **Lặp code (Code Duplication):** File UI nào cũng phải viết lại 15–20 dòng logic đăng nhập.
  2. **Tạo ra "lỗi ẩn" (False Positive):** Dù bước login bị lag hoặc lỗi một lần, khối `catch` đã nuốt lỗi và chạy lại, khiến báo cáo cuối cùng vẫn hiện **màu Xanh** (Pass 100%), che giấu sự bất ổn định của hệ thống.
  3. **Lãng phí thời gian:** Trình duyệt liên tục phải render form đăng nhập từ đầu đến cuối trước mỗi test suite.

#### 🟢 Code SAU (AFTER):

**Bước 1: Tách việc đăng nhập vào một Setup độc lập chạy đúng 1 lần:**
```javascript
// tests/auth/admin.setup.js (AFTER)
import { test as setup, expect } from '@playwright/test';
import { LoginPage } from '../../src/pages/LoginPage.js';
import path from 'path';

const authFile = path.resolve(process.cwd(), 'tests/auth/ui_admin.json');

setup('Authenticate UI Admin', async ({ page }) => {
  const loginPage = new LoginPage(page);
  await loginPage.goto();
  await loginPage.login(process.env.ADMIN_EMAIL, process.env.ADMIN_PASSWORD);
  
  await expect(page).toHaveURL(/.*\/dashboard/);

  // ✅ Lưu toàn bộ Cookie, Local Storage ra file json duy nhất
  await page.context().storageState({ path: authFile });
});
```

**Bước 2: Các file UI test chỉ việc mở thẳng trang cần test:**
```javascript
// tests/ui/02.user.ui.spec.js (AFTER)
test.beforeAll(async ({ browser }) => {
  // ✅ Nạp sẵn storageState đã đăng nhập - KHÔNG cần mở form login
  const context = await browser.newContext({
    baseURL: process.env.UI_BASE_URL,
    storageState: 'tests/auth/ui_admin.json', 
  });
  sharedPage = await context.newPage();
  userPage = new UserPage(sharedPage);

  // ✅ Đi thẳng vào trang nghiệp vụ, xóa sạch 100% code try...catch thừa!
  await sharedPage.goto('/users');
});
```

---

### 📌 Điểm nghẽn 2: Chạy tuần tự (`workers: 1`) gây chậm thời gian kiểm thử

#### 🔴 Code TRƯỚC (BEFORE):
```javascript
// playwright.config.js (BEFORE - Code ban đầu)
export default defineConfig({
  testDir: "./tests",
  timeout: 30 * 1000,

  // ❌ Ép chạy tuần tự toàn bộ test
  fullyParallel: false,

  // ❌ Chỉ dùng 1 luồng duy nhất để tránh xung đột dữ liệu
  workers: 1,

  projects: [
    {
      name: "Admin UI Tests - Chrome",
      testDir: "./tests/ui",
      // ❌ Không có cơ chế dependencies, chạy chay từ đầu
      use: { ...devices["Desktop Chrome"] },
    },
    {
      name: "API Tests",
      testDir: "./tests/api",
    },
    {
      name: "E2E Tests",
      testDir: "./tests/e2e",
    },
  ],
});
```

* **Vấn đề / Hậu quả cần nhấn mạnh với người nghe:**
  * Toàn bộ 57 test cases phải xếp hàng chạy lần lượt từng case một.
  * Hiện tại có 57 case có thể mất 5–7 phút; nếu dự án mở rộng lên 200–300 case, CI/CD pipeline sẽ mất 30–45 phút, gây nghẽn toàn bộ quá trình release của team.

#### 🟢 Code SAU (AFTER):
```javascript
// playwright.config.js (AFTER)
export default defineConfig({
  testDir: "./tests",
  timeout: 30 * 1000,

  // ✅ Kích hoạt chạy song song tối đa
  fullyParallel: true,

  // ✅ Tự động tận dụng 4 luồng trên local hoặc 2 luồng trên CI
  workers: process.env.CI ? 2 : 4,

  projects: [
    // 1. Chạy setup xác thực đầu tiên
    {
      name: "setup",
      testMatch: /.*admin\.setup\.js/,
    },
    // 2. Chạy song song các test UI ngay sau khi setup hoàn tất
    {
      name: "Admin UI Tests",
      testDir: "./tests/ui",
      dependencies: ["setup"], // ✅ Phụ thuộc vào setup
      use: {
        ...devices["Desktop Chrome"],
        storageState: "tests/auth/ui_admin.json",
      },
    },
    {
      name: "API Tests",
      testDir: "./tests/api",
    },
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

> **Chiến lược Data Isolation khi chạy 4 Workers:**
> Để chạy song song mà không sợ các workers xung đột dữ liệu (ví dụ trùng email khi tạo user):
> 1. **Tạo dữ liệu động duy nhất:** Sử dụng timestamp:
>    ```javascript
>    const uniqueEmail = `test_admin_${Date.now()}_${Math.floor(Math.random() * 1000)}@test.com`;
>    ```
> 2. **Tự động dọn dẹp dữ liệu qua DB Fixture:**
>    ```javascript
>    test.afterEach(async ({ db }) => {
>      await db.query(`DELETE FROM users WHERE email LIKE 'test_admin_%'`);
>    });
>    ```

---

### 📌 Điểm nghẽn 3: Chưa sử dụng Network Intercept & Mocking

#### 🔴 Code TRƯỚC (BEFORE):
* Trong toàn bộ codebase không có bất kỳ dòng lệnh `page.route()` nào.
* Trình duyệt luôn phải tải xuống toàn bộ ảnh tĩnh (`.png`, `.jpg`), avatar và các model 3D preview nặng của Live2D mỗi khi chuyển trang.
* **Không thể test kịch bản Server lỗi 500:** Nếu muốn kiểm tra xem UI có hiển thị đúng thông báo lỗi (Toast Error) khi máy chủ backend gặp sự cố hay không, nhóm kiểm thử bắt buộc phải can thiệp thủ công vào server hoặc chấp nhận bỏ qua kịch bản này.

#### 🟢 Code SAU (AFTER):

**Tối ưu 1: Chặn ảnh nặng để tăng tốc độ tải trang:**
```javascript
// tests/ui/02.user.ui.spec.js (AFTER)
test.beforeEach(async ({ page }) => {
  // ✅ Huỷ bỏ tải các định dạng ảnh giúp render bảng người dùng tức thì
  await page.route(/\.(png|jpeg|jpg|svg|webp)$/, (route) => route.abort());
});
```

**Tối ưu 2: Mock API lỗi 500 để kiểm thử thông báo lỗi UI:**
```javascript
// tests/ui/02.user.ui.spec.js (AFTER)
test('Xác minh UI hiển thị thông báo lỗi khi API Server bị lỗi 500', async ({ page }) => {
  // ✅ Giả lập API backend bị sập trả về 500 mà không cần đụng vào backend thật
  await page.route('**/api/v1/users/**', async (route) => {
    await route.fulfill({
      status: 500,
      contentType: 'application/json',
      body: JSON.stringify({ message: 'Internal Server Error' }),
    });
  });

  await page.goto('/users');
  // ✅ Kiểm tra toast thông báo lỗi hiển thị đúng trên UI
  await expect(page.getByText('Internal Server Error')).toBeVisible();
});
```

---

### 📌 Điểm nghẽn 4: Thiếu Visual Artifacts khi Test Fail (Screenshot & Video)

#### 🔴 Code TRƯỚC (BEFORE):
```javascript
// playwright.config.js (BEFORE - Code ban đầu)
use: {
  /* ❌ Chỉ ghi trace khi test thất bại và chạy retry lần 2. Không chụp ảnh, không quay video */
  trace: "on-first-retry",
  httpCredentials: process.env.BASIC_AUTH_USER ? { ... } : undefined,
}
```
* **Vấn đề / Hậu quả cần nhấn mạnh với người nghe:**
  * Khi test case fail trên CI/CD, tester/dev chỉ có file trace nén (`trace.zip`).
  * Bắt buộc phải tải file về máy và mở lệnh `npx playwright show-trace` mới nhìn thấy được màn hình lỗi, rất bất tiện và tốn thời gian trao đổi bug.

#### 🟢 Code SAU (AFTER):
```javascript
// playwright.config.js (AFTER)
use: {
  // ✅ Bộ ba hoàn chỉnh: Chỉ lưu trữ tài nguyên khi test THỰC SỰ BỊ FAIL
  trace: "retain-on-failure",
  screenshot: "only-on-failure",
  video: "retain-on-failure",

  httpCredentials: process.env.BASIC_AUTH_USER
    ? {
        username: process.env.BASIC_AUTH_USER,
        password: process.env.BASIC_AUTH_PASS || "",
      }
    : undefined,
}
```
* **Lợi ích:** Khi mở HTML Report, tester/dev sẽ nhìn thấy ngay ảnh chụp màn hình lúc crash và một video clip 10 giây ghi lại toàn bộ thao tác dẫn tới lỗi.

---

## 4. File Cấu Hình `playwright.config.js` Hoàn Chỉnh Sau Tối Ưu

```javascript
// live2d/playwright.config.js (Bản tối ưu hoàn chỉnh)
import { defineConfig, devices } from "@playwright/test";
import dotenv from "dotenv";
import path from "path";

const ENV = process.env.ENV || "dev";
dotenv.config({ path: path.resolve(process.cwd(), `.env.${ENV}`) });

export default defineConfig({
  testDir: "./tests",
  timeout: 30 * 1000,
  expect: { timeout: 10 * 1000 },

  // 1. Kích hoạt chạy song song để tăng tốc toàn bộ test suite
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

    // Project 2: API Tests - Độc lập, chạy song song nhanh chóng
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

---

## 5. Kịch Bản Thuyết Trình & Talking Points Cho Buổi Báo Cáo

Khi trình bày trước team hoặc cấp trên, bạn có thể áp dụng luồng kịch bản 4 bước sau:

### Bước 1: Mở đầu & Tạo sự chú ý (The Hook)
> *"Dự án `live2d` của chúng ta đã xây dựng được 57 test cases bao phủ đầy đủ UI, API và E2E. Tuy nhiên, khi đào sâu vào hiệu năng và độ ổn định của mã nguồn kiểm thử, chúng ta đang đối mặt với 3 điểm nghẽn lớn: thời gian chạy lâu, lặp code đăng nhập UI và thiếu cơ chế bắt lỗi trực quan."*

### Bước 2: Chỉ ra "Code Smell" ở bản Before (The Pain Point)
> *"Hãy nhìn vào đoạn code `beforeAll` trước đây: mỗi file UI đều phải tự mở form và gõ tài khoản mật khẩu. Để chống mạng lag, chúng ta đã phải viết thêm một khối `try...catch` để tự click login lại. Điều này dẫn tới hai hậu quả nguy hiểm:
> 1. Code test bị rối và lặp lại.
> 2. Tạo ra 'lỗi ẩn' (False Positive) – test vẫn báo màu Xanh dù màn hình login vừa bị lag hoặc sập."*

### Bước 3: Đưa ra giải pháp Before vs. After (The Solution)
> *"Giải pháp của chúng ta là áp dụng các tính năng nâng cao của Playwright:
> * Thay thế `try...catch` bằng **Setup Project + `storageState`**: Đăng nhập 1 lần và nạp lại session cho tất cả các file sau.
> * Chuyển từ **`workers: 1` sang `workers: 4`** kết hợp chiến lược cô lập dữ liệu (Data Isolation) để rút ngắn 70% thời gian chạy suite.
> * Bổ sung **`page.route()`** để vừa chặn ảnh nặng tăng tốc độ trang, vừa chủ động mock lỗi 500 kiểm thử khả năng chịu lỗi của UI.
> * Bật tự động **Screenshot & Video** khi có lỗi để dev nhìn thấy ngay nguyên nhân mà không cần debug thủ công."*

### Bước 4: Khẳng định giá trị đạt được (The Impact)
> *"Kết quả sau khi tối ưu: Codebase gọn gàng chuẩn Clean Code, thời gian kiểm thử trên CI/CD được rút ngắn đáng kể và khả năng phát hiện lỗi chính xác, minh bạch hơn."*
