# Playwright Inspector

- Playwright Inspector là công cụ GUI của Playwright dùng để debug test, kiểm tra locator và theo dõi từng action. Nó đặc biệt hữu ích khi bạn đang viết automation test bằng Playwright.

- Inspector dùng để làm gì?
  - Step-by-step từng action trong test.
  - Pause test tại một bước.
  - Pick Locator trực tiếp trên UI.
  - Chỉnh sửa locator và xem element nào match.
  - Xem Actionability Logs để biết Playwright đang chờ điều kiện gì.
  - Debug các lỗi như locator not found, element not visible, element not enabled...
  - Kiểm tra xem locator có match 1 hay nhiều element.

- Mở Playwright Inspector:
  `npx playwright test --debug`

- Debug một test cụ thể:
  `npx playwright test demo-todo-app.spec.ts --debug`

- Debug từ một dòng cụ thể:
  `npx playwright test demo-todo-app.spec.ts:11 --debug`

- `page.pause()` - dừng test tại một điểm cụ thể:

  Ví dụ:

  ```ts
  test("should add a todo", async ({ page }) => {
    await page.goto("https://playwright.dev/");
    await page.pause(); // Dừng test tại đây
  });
  ```

- Pick Locator trong Inspector:
  - Khi test đang chạy trong Inspector có thể hover chuột vào bất kỳ element nào trên trang để xem locator của nó.
  - Playwright sẽ highlight element đó và hiển thị locator tương ứng trong phần Playwright Inspector -> không quá phụ thuộc vào CSS/XPath, mà ưu tiên dùng text, aria-label, role...

- Inspector + Codegen
  - Khi cần viết test dài, có nhiều action, có thể dùng `npx playwright codegen` để record hành động của mình -> Playwright sẽ tự động sinh ra đoạn code tương ứng -> Sau đó dùng Inspector để review lại code, chỉnh sửa locator, thêm pause, thêm assertions...
  - Hãy kết hợp both để tạo ra các test automation nhanh, hiệu quả.
  - Câu lệnh: `npx playwright codegen [URL]`
    - [URL] là trang web bạn muốn test. Có thể bỏ qua [URL] nếu bạn muốn mở trình duyệt và nhập URL thủ công.
  - Tuy nhiên không nên phụ thuộc hoàn toàn vào codegen, nên review lại locator, test data, assertion và structure của test.
