# Test Structure

Các thành phần chính trong file test

- Import module: Lấy `test` và `expect` từ thư viện `@playwright/test`.
- Nhóm test (`test.describe`): Gom các test case có cùng chức năng hoặc giao diện lại với nhau.
- Test case (`test`): Viết tên kịch bản và đoạn mã thực thi bên trong hàm `async`.
- Hành động (`page.\*`): Điều hướng, bấm chuột, nhập dữ liệu (ví dụ: `page.goto()`, `page.click()`).
- Khẳng định (`expect`): Kiểm tra kết quả hiển thị có đúng như mong đợi hay không.

Ví dụ:

```ts
import { test, expect } from "@playwright/test";

test.describe("Nhóm chức năng đăng nhập", () => {
  test("Đăng nhập thành công", async ({ page }) => {
    // 1. Mở trang web
    await page.goto("https://example.com");

    // 2. Nhập thông tin
    await page.fill("#username", "user_test");
    await page.fill("#password", "123456");

    // 3. Bấm nút đăng nhập
    await page.click('button[type="submit"]');

    // 4. Kiểm tra kết quả
    await expect(page).toHaveURL("https://example.com");
  });
});
```

## test()

Là đơn vị kiểm thử nhỏ nhất và là nơi thực thi kịch bản test thực tế.

- Nhiệm vụ: Chứa các bước tương tác (Click, Fill,...) và kiểm tra kết quả (expect).
- Cú pháp: Nhận vào một chuỗi mô tả tên test và một hàm bất đồng bộ (async).
- Fixture: Tự động cung cấp các công cụ như { page }, { context } vào trong hàm để sử dụng

## test.describe()

Là công cụ dùng để gom nhóm các bài test có liên quan lại với nhau.
Nhiệm vụ: Tạo cấu trúc phân cấp (ví dụ: nhóm Toàn bộ tính năng Giỏ hàng, nhóm Tính năng Đăng nhập).
Đặc điểm:

- Giúp báo cáo (Report) hiển thị rõ ràng theo từng phân hệ.
- Có thể lồng nhiều `test.describe` vào nhau.
- Chia sẻ chung cấu hình hoặc các Hooks bên trong nhóm đó.

## Hooks

Là các hàm đặc biệt dùng để thiết lập môi trường trước khi test chạy và dọn dẹp sau khi test xong.

- `test.beforeAll`: Chạy 1 lần duy nhất trước khi tất cả các test trong file (hoặc trong nhóm) bắt đầu. Thường dùng để thiết lập dữ liệu lớn hoặc khởi tạo kết nối Database.
- `test.beforeEach`: Chạy trước mỗi test case (`test`). Thường dùng để đăng nhập, reset dữ liệu.
- `test.afterEach`: Chạy sau mỗi test case. Thường dùng để chụp ảnh màn hình khi test thất bại (take screenshot).
- `test.afterAll`: Chạy 1 lần duy nhất sau khi tất cả các test trong file (hoặc trong nhóm) kết thúc. Thường dùng để tắt kết nối hoặc dọn dẹp tài nguyên.

Sơ đồ thứ tự chạy (Execution Order)

```
[beforeAll] -> Chạy 1 lần duy nhất ban đầu
   │
   ├── [beforeEach] -> Chạy trước Test 1
   ├── [Test 1]      -> Thực thi Test 1
   ├── [afterEach]  -> Chạy sau Test 1
   │
   ├── [beforeEach] -> Chạy trước Test 2
   ├── [Test 2]      -> Thực thi Test 2
   ├── [afterEach]  -> Chạy sau Test 2
   │
[afterAll]  -> Chạy 1 lần duy nhất cuối cùng
```

Ví dụ:

```ts
import { test, expect } from "@playwright/test";

test.describe("Chức năng Giỏ hàng", () => {
  // Chạy 1 lần trước khi vào các bài test giỏ hàng
  test.beforeAll(async () => {
    console.log("Khởi tạo: Chuẩn bị tài khoản test có sẵn tiền.");
  });

  // Trước mỗi test case, đều phải vào trang sản phẩm
  test.beforeEach(async ({ page }) => {
    await page.goto("https://example.com");
  });

  test("Thêm sản phẩm vào giỏ", async ({ page }) => {
    await page.click(".add-to-cart-btn");
    await expect(page.locator(".cart-count")).hasText("1");
  });

  test("Xóa sản phẩm khỏi giỏ", async ({ page }) => {
    // Giả sử có thêm bước phụ để test xóa
    await page.click(".add-to-cart-btn");
    await page.click(".remove-btn");
    await expect(page.locator(".cart-count")).hasText("0");
  });

  // Sau mỗi test case thì dọn dẹp giỏ hàng
  test.afterEach(async ({ page }) => {
    await page.evaluate(() => localStorage.clear());
  });

  // Chạy sau khi tất cả test case trong nhóm này đã xong
  test.afterAll(async () => {
    console.log("Dọn dẹp: Đóng phiên kiểm thử giỏ hàng.");
  });
});
```
