# Cách tạo mới folder bằng terminal

`mkdir {tên_folder}` // {tên_folder} là tên của folder bạn muốn tạo

# Cách di chuyển vào folder

`cd {tên_folder}` // {tên_folder} là tên của folder bạn muốn di chuyển vào

# Cài đặt Playwright

Lệnh npm: `npm init playwright@latest` // Tạo file cấu hình cho Npm

Lệnh yarn: `yarn create playwright` // Tạo file cấu hình cho Yarn

Lệnh pnpm: `pnpm create playwright` // Tạo file cấu hình cho Pnpm

# Một số lệnh khác trong playwright

`npx playwright install chromium` // Cài đặt trình duyệt Chromium cho Playwright

`npx playwright install` // Cài đặt tất cả các trình duyệt (Chromium, Firefox, WebKit)

`npx playwright codegen [URL]` // Mở trình duyệt và ghi lại hành động của người dùng, sau đó chuyển đổi hành động thành mã test

`npx playwright test [options]` // Chạy tất cả các test trong dự án

`npx playwright test test/login.spec.ts --ui` // Chạy test với giao diện người dùng

`npx playwright show-report` // Hiển thị báo cáo kết quả test dưới dạng HTML

`npx playwright codegen --debug [URL]` // Mở trình duyệt với chế độ gỡ lỗi, cho phép kiểm tra từng bước thực hiện test

# Cấu trúc và Vai trò File Cấu hình Playwright

File playwright.config (thường là .ts hoặc .js) là thành phần trung tâm trong cấu trúc dự án (scaffold) được tạo ra tự động khi thực hiện cài đặt Playwright

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

# Playwright Locators

- Trong Playwright, Locator là cơ chế chính để tìm và tương tác với element trên UI.
- Locator không chỉ đơn giản là "selector"; nó còn kết hợp với cơ chế auto-waiting và retry-ability, giúp test ổn định hơn khi UI thay đổi hoặc render bất đồng bộ.
- Locator được đánh giá tại thời điểm action thực hiện, nên nếu DOM được re-render thì Playwright có thể tìm lại element phù hợp thay vì giữ một element cũ.

## Các loại locator quan trọng

| Locator              | Mục đích                  | Ví dụ                             |
| -------------------- | ------------------------- | --------------------------------- |
| `getByRole()`        | Button, link, checkbox... | `getByRole('button')`             |
| `getByLabel()`       | Input có label            | `getByLabel('Email')`             |
| `getByText()`        | Element theo text         | `getByText('Welcome')`            |
| `getByPlaceholder()` | Input theo placeholder    | `getByPlaceholder('Enter email')` |
| `getByTestId()`      | Element có test ID        | `getByTestId('login-btn')`        |
| `getByAltText()`     | Image theo alt            | `getByAltText('Logo')`            |
| `getByTitle()`       | Element theo title        | `getByTitle('Close')`             |
| `locator()`          | CSS/XPath hoặc selector   | `locator('#email')`               |

### 1. getByRole() — Locator nên ưu tiên

- `getByRole()` dựa trên ARIA/accessibility role và accessible name, nên thường phản ánh tốt cách user hoặc assistive technology nhìn thấy UI.
- Ưu điểm của getByRole():
  - bền vững hơn khi UI thay đổi
  - phản ánh tốt cách user sử dụng UI
  - tương tác tốt với assistive technology
- Hãy dùng getByRole() bất cứ khi nào có thể, vì nó tương tác tốt nhất với assistive technology và giúp test của bạn bền vững hơn.

- Ví dụ:

```ts
// Tìm button có text "Submit"
const submitButton = page.getByRole("button", { name: "Submit" });
await submitButton.click();

// Tìm input có label "Email"
const emailInput = page.getByRole("textbox", { name: "Email" });
await emailInput.fill("[EMAIL_ADDRESS]");
```

- Các role thường gặp:
  - button
  - link
  - textbox
  - checkbox
  - radio
  - heading
  - list
  - listitem
  - table
  - row
  - cell
  - dialog
  - combobox

### 2. getByText() — Locator theo text

- getByText() tìm element dựa trên nội dung text hoặc Regexp.

- Cú pháp:
  `page.getByText(text, options?)`
  - `text`: là một chuỗi hoặc Regexp chỉ định nội dung text.
  - `options` (optional): một object chứa các thuộc tính sau:
    - `exact`: boolean, mặc định là `false`. Nếu là `true`, Playwright chỉ tìm element có text khớp chính xác với giá trị truyền vào.
    - `ignoreCase`: boolean, mặc định là `false`. Nếu là `true`, Playwright sẽ tìm element có text không phân biệt chữ hoa chữ thường.
    - `excludeHidden`: boolean, mặc định là `true`. Nếu là `true`, Playwright sẽ loại trừ element bị ẩn bởi CSS `display: none` hoặc `visibility: hidden`.

- Ví dụ:

```ts
// Tìm element có text "Welcome"
const welcomeElement = page.getByText("Welcome");
await welcomeElement.click();

// Tìm element có text "Submit" chính xác (không match "Submit 2")
const submitButton = page.getByText("Submit", { exact: true });
await submitButton.click();

// Tìm element có text "Login" không phân biệt chữ hoa chữ thường
const loginElement = page.getByText("login", { ignoreCase: true });
await loginElement.click();
```

- Các locator thường gặp:
  - Login
  - Register
  - Forgot Password
  - Profile
  - Checkout
  - Address
  - Payment

### 3. getByPlaceholder() - Locator theo placeholder

- getByPlaceholder() tìm element dựa trên nội dung placeholder của element.
- Hãy dùng getByPlaceholder() khi element không có label, aria-label, hoặc test ID rõ ràng, nhưng có placeholder dễ nhận biết. Tránh dùng getByPlaceholder() nếu placeholder dễ thay đổi hoặc có thể trùng nhau giữa các element khác nhau.

- Cú pháp:
  `page.getByPlaceholder(placeholder, options?)`
  - `placeholder`: là một chuỗi hoặc Regexp chỉ định nội dung placeholder.
  - `options` (optional): một object chứa các thuộc tính sau:
    - `exact`: boolean, mặc định là `false`. Nếu là `true`, Playwright chỉ tìm element có placeholder khớp chính xác với giá trị truyền vào.
    - `ignoreCase`: boolean, mặc định là `false`. Nếu là `true`, Playwright sẽ tìm element có placeholder không phân biệt chữ hoa chữ thường.
    - `excludeHidden`: boolean, mặc định là `true`. Nếu là `true`, Playwright sẽ loại trừ element bị ẩn bởi CSS `display: none` hoặc `visibility: hidden`.

- Ví dụ:

```ts
// Tìm input có placeholder "Email"
const emailInput = page.getByPlaceholder("Email");
await emailInput.fill("[EMAIL_ADDRESS]");

// Tìm input có placeholder "Username" chính xác
const usernameInput = page.getByPlaceholder("Username", { exact: true });
await usernameInput.fill("admin");

// Tìm input có placeholder "Email" không phân biệt chữ hoa chữ thường
const emailInputIgnoreCase = page.getByPlaceholder("email", {
  ignoreCase: true,
});
await emailInputIgnoreCase.fill("[EMAIL_ADDRESS]");
```

### 4. getByText() - Locator dựa trên text

- getByText() tìm element dựa trên nội dung text của element.
- Hãy dùng getByText() khi element không có label, aria-label, hoặc test ID rõ ràng, nhưng có text dễ nhận biết. Tránh dùng getByText() nếu text dễ thay đổi hoặc có thể trùng nhau giữa các element khác nhau.

- Cú pháp:
  `page.getByText(text, options?)`
  - `text`: là một chuỗi hoặc Regexp chỉ định nội dung text.
  - `options` (optional): một object chứa các thuộc tính sau:
    - `exact`: boolean, mặc định là `false`. Nếu là `true`, Playwright chỉ tìm element có text khớp chính xác với giá trị truyền vào.
    - `ignoreCase`: boolean, mặc định là `false`. Nếu là `true`, Playwright sẽ tìm element có text không phân biệt chữ hoa chữ thường.
    - `excludeHidden`: boolean, mặc định là `true`. Nếu là `true`, Playwright sẽ loại trừ element bị ẩn bởi CSS `display: none` hoặc `visibility: hidden`.

- Ví dụ:

```ts
// Tìm element có text "Welcome"
const welcomeElement = page.getByText("Welcome");
await welcomeElement.click();

// Tìm element có text "Submit" chính xác (không match "Submit 2")
const submitButton = page.getByText("Submit", { exact: true });
await submitButton.click();

// Tìm element có text "Login" không phân biệt chữ hoa chữ thường
const loginElement = page.getByText("login", { ignoreCase: true });
await loginElement.click();
```

- Khi nào nên dùng:
  - Tốt cho các element không mang tính interactive
- Các element thường dùng:
  - div
  - span
  - p
  - h1
  - h2
  - message
  - error message
  - notification

### 5. getByAltText() - Locator dựa trên alt text

- getByAltText() tìm element dựa trên nội dung alt attribute của element.

- Cú pháp:
  `page.getByAltText(altText, options?)`
  - `altText`: là một chuỗi hoặc Regexp chỉ định nội dung alt attribute.
  - `options` (optional): một object chứa các thuộc tính sau:
    - `exact`: boolean, mặc định là `false`. Nếu là `true`, Playwright chỉ tìm element có alt attribute khớp chính xác với giá trị truyền vào.
    - `ignoreCase`: boolean, mặc định là `false`. Nếu là `true`, Playwright sẽ tìm element có alt attribute không phân biệt chữ hoa chữ thường.
    - `excludeHidden`: boolean, mặc định là `true`. Nếu là `true`, Playwright sẽ loại trừ element bị ẩn bởi CSS `display: none` hoặc `visibility: hidden`.

- Các element thường dùng:
  - image
  - input
  - button
  - a
  - span
  - div
  - svg
  - iframe

- Ví dụ:

  ```ts
  // Tìm image có alt text "Logo"
  const logoImage = page.getByAltText("Logo");
  await logoImage.click();

  // Tìm image có alt text "Profile" chính xác
  const profileImage = page.getByAltText("Profile", { exact: true });
  await profileImage.click();

  // Tìm image có alt text "Login" không phân biệt chữ hoa chữ thường
  const loginImage = page.getByAltText("login", { ignoreCase: true });
  await loginImage.click();
  ```

### 6. getByTestId() - Locator dựa trên test ID

- getByTestId() tìm element dựa trên nội dung data-testid attribute của element.
- Hãy dùng getByTestId() khi element không có label, aria-label, hoặc test ID rõ ràng, nhưng có test ID dễ nhận biết. Tránh dùng getByTestId() nếu test ID dễ thay đổi hoặc có thể trùng nhau giữa các element khác nhau.
- Ưu điểm:
  - Text thay đổi → Test vẫn có thể chạy
  - CSS thay đổi → Test vẫn có thể chạy
  - DOM thay đổi → Test ít bị ảnh hưởng
- Nhược điểm:
  - Cần dev phải thêm data-testid vào HTML
- Cú pháp:
  `page.getByTestId(testId, options?)`
  - `testId`: là một chuỗi hoặc Regexp chỉ định nội dung test ID.
  - `options` (optional): một object chứa các thuộc tính sau:
    - `exact`: boolean, mặc định là `false`. Nếu là `true`, Playwright chỉ tìm element có test ID khớp chính xác với giá trị truyền vào.
    - `ignoreCase`: boolean, mặc định là `false`. Nếu là `true`, Playwright sẽ tìm element có test ID không phân biệt chữ hoa chữ thường.
    - `excludeHidden`: boolean, mặc định là `true`. Nếu là `true`, Playwright sẽ loại trừ element bị ẩn bởi CSS `display: none` hoặc `visibility: hidden`.

- Các element thường dùng:
  - div
  - span
  - p
  - h1
  - h2
  - button
  - a
  - input
  - select
  - textarea
  - svg
  - iframe

- Ví dụ:

  ```ts
  // Tìm element có data-testid="Login"
  const loginElement = page.getByTestId("Login");
  await loginElement.click();

  // Tìm element có data-testid="Login" chính xác
  const loginElementExact = page.getByTestId("Login", { exact: true });
  await loginElementExact.click();

  // Tìm element có data-testid="Login" không phân biệt chữ hoa chữ thường
  const loginElementIgnoreCase = page.getByTestId("login", {
    ignoreCase: true,
  });
  await loginElementIgnoreCase.click();
  ```

### 7. getByTitle() - Locator dựa trên title

- getByTitle() tìm element dựa trên nội dung title attribute của element.
- Hãy dùng getByTitle() khi element có title rõ ràng và duy nhất. Tránh dùng getByTitle() nếu title dễ thay đổi hoặc có thể trùng nhau giữa các element khác nhau.
- Cú pháp:
  `page.getByTitle(title, options?)`
  - `title`: là một chuỗi hoặc Regexp chỉ định nội dung title.
  - `options` (optional): một object chứa các thuộc tính sau:
    - `exact`: boolean, mặc định là `false`. Nếu là `true`, Playwright chỉ tìm element có title khớp chính xác với giá trị truyền vào.
    - `ignoreCase`: boolean, mặc định là `false`. Nếu là `true`, Playwright sẽ tìm element có title không phân biệt chữ hoa chữ thường.
    - `excludeHidden`: boolean, mặc định là `true`. Nếu là `true`, Playwright sẽ loại trừ element bị ẩn bởi CSS `display: none` hoặc `visibility: hidden`.

- Các element thường dùng:
  - Phù hợp với element có thuộc tính title
  - Element có attribute title rõ ràng
  - Element có tooltip
- Ưu điểm:
  - Tìm element dựa trên title
  - Title là một attribute được hỗ trợ bởi HTML
  - Có thể tìm element dựa trên title không phân biệt chữ hoa chữ thường
  - Có thể tìm element dựa trên title chính xác
- Nhược điểm:
  - Title dễ thay đổi
  - Title có thể trùng nhau giữa các element khác nhau
  - Title không phải lúc nào cũng có sẵn
- Ví dụ:

  ```ts
  // Tìm element có title="Login"
  const loginElement = page.getByTitle("Login");
  await loginElement.click();

  // Tìm element có title="Login" chính xác
  const loginElementExact = page.getByTitle("Login", { exact: true });
  await loginElementExact.click();

  // Tìm element có title="Login" không phân biệt chữ hoa chữ thường
  const loginElementIgnoreCase = page.getByTitle("login", {
    ignoreCase: true,
  });
  await loginElementIgnoreCase.click();
  ```

### 8. locator() — CSS / XPath

- locator() là phương thức mạnh mẽ nhất trong Playwright để tìm element. Nó cho phép bạn tìm element dựa trên CSS selector hoặc XPath selector.
- Cú pháp:
  `page.locator(selector, options?)`
  - `selector`: là một chuỗi hoặc Regexp chỉ định CSS selector hoặc XPath selector.
  - `options` (optional): một object chứa các thuộc tính sau:
    - `hasText`: chỉ tìm element có text khớp với giá trị truyền vào.
    - `has`: chỉ tìm element có element con khớp với giá trị truyền vào.
    - `nth`: chỉ tìm element thứ n.
    - `locale`: locale của element.
    - `timeout`: thời gian chờ trước khi timeout.

- Ví dụ:

```ts
// Tìm element dựa trên CSS selector
const loginElement = page.locator(".login");
await loginElement.click();

// Tìm element dựa trên XPath selector
const loginElement = page.locator("//div[@class='login']");
await loginElement.click();

// Tìm element có text "Login"
const loginElement = page.locator(".login", { hasText: "Login" });
await loginElement.click();

// Tìm element có element con có text "Login"
const loginElement = page.locator(".login", { has: page.getByText("Login") });
await loginElement.click();

// Tìm element thứ n
const loginElement = page.locator(".login", { nth: 0 });
await loginElement.click();
```

## Khi nào nên dùng locator() instead of getBy\*()

- Sử dụng `getBy*()` khi có thể vì nó cho kết quả rõ ràng và dễ hiểu.
- Sử dụng `locator()` khi `getBy*()` không thể sử dụng hoặc khi cần tìm element dựa trên CSS hoặc XPath selector.
- Ưu điểm của `getBy*()`: Dễ đọc, dễ hiểu, dễ maintain
- Nhược điểm của `getBy*()`: Chỉ hỗ trợ một số selector nhất định
- Ưu điểm của `locator()`: Hỗ trợ nhiều selector, linh hoạt
- Nhược điểm của `locator()`: Khó đọc, dễ bị ảnh hưởng bởi thay đổi của HTML

### 9. Chain locators (lồng nhau)

- Locator có thể được chain để tìm element.
- Playwright hỗ trợ filter() với hasText, has, hasNot, hasNotText để thu hẹp locator.
- Cú pháp:
  `page.locator(selector1).locator(selector2)`

- Ví dụ: Cho đoạn HTML sau. Với đoạn HTML này, có thể tìm các phần tử con bằng cách chain locator.

  ```ts
  <div class="product">
    <h3>iPhone 17</h3>
    <button>Add to cart</button>
  </div>

  <div class="product">
    <h3>MacBook</h3>
    <button>Add to cart</button>
  </div>
  ```

  ```ts
  const product = page.getByRole("listitem").filter({ hasText: "MacBook" });
  await product.getByRole("button", { name: "Add to cart" }).click();
  ```

### 10. first(), last(), nth()

- first(): chọn element đầu tiên trong số các element được tìm thấy.
- last(): chọn element cuối cùng trong số các element được tìm thấy.
- nth(): chọn element thứ n trong số các element được tìm thấy.

- Cú pháp:
  - `locator.first(options?)`
  - `locator.last(options?)`
  - `locator.nth(index, options?)`

- Ví dụ:

  ```ts
  // Chọn element đầu tiên
  const firstElement = page.getByText("Product").first();
  await firstElement.click();

  // Chọn element cuối cùng
  const lastElement = page.getByText("Product").last();
  await lastElement.click();

  // Chọn element thứ n
  const nthElement = page.getByText("Product").nth(2);
  await nthElement.click();
  ```

- Tuy nhiên không nên lạm dụng vì có thể hôm nay button thứ 4 là Delete, nhưng ngày mai frontend thêm một button mới → test click nhầm. Playwright cũng khuyến nghị tạo locator unique và có ý nghĩa thay vì phụ thuộc vào nth().

### 11. Strictness

- Locator của Playwright có tính strict. Điều này có nghĩa là Playwright sẽ tìm element dựa trên selector và trả về element đó. Nếu không tìm thấy element, Playwright sẽ throw một error.
- Một locator được coi là "strict" nếu nó tìm thấy duy nhất 1 element trên DOM. Khi sử dụng locator, bạn có thể kiểm soát tính strict của locator bằng cách sử dụng các phương thức sau:
  - `strict():` Locator được coi là strict. Nếu tìm thấy nhiều hơn 1 element, Playwright sẽ throw một error.
  - `nonStrict():` Locator được coi là non-strict. Nếu tìm thấy nhiều hơn 1 element, Playwright sẽ trả về element đầu tiên.
  - `nth(index):` Locator được coi là strict. Nếu tìm thấy nhiều hơn 1 element, Playwright sẽ trả về element thứ n.

- Ví dụ:

  ```ts
  // Locator strict
  const strictLocator = page.getByText("Product").strict();
  await strictLocator.click();

  // Locator non-strict
  const nonStrictLocator = page.getByText("Product").nonStrict();
  await nonStrictLocator.click();

  // Locator thứ n
  const nthLocator = page.getByText("Product").nth(2);
  await nthLocator.click();
  ```

### 12. filter()

- filter() là một phương thức của locator cho phép bạn lọc các element dựa trên các điều kiện nhất định.
- Cú pháp:
  `locator.filter(options)`
  - `options`: một object chứa các thuộc tính sau:
    - `hasText`: chỉ tìm element có text khớp với giá trị truyền vào.
    - `has`: chỉ tìm element có element con khớp với giá trị truyền vào.
    - `hasNotText`: chỉ tìm element có text không khớp với giá trị truyền vào.
    - `hasNot`: chỉ tìm element có element con không khớp với giá trị truyền vào.

- Ví dụ: Bạn muốn click Add to cart của Product B.
  Product A
  [Add to cart]

Product B
[Add to cart]

Product C
[Add to cart]

```ts
const product = page.getByRole("listitem").filter({ hasText: "Product B" });
await product.getByRole("button", { name: "Add to cart" }).click();
```

### 13. locator.and()

- and() là một phương thức của locator cho phép bạn kết hợp nhiều locator để tìm element. Cú pháp: `locator1.and(locator2)`
- Ví dụ: Tìm tất cả input có thuộc tính required

```ts
const requiredInputs = page
  .locator("input")
  .and(page.locator(":has([required])"));
await requiredInputs.count();
```

### 14. locator.or()

- or() là một phương thức của locator cho phép bạn kết hợp nhiều locator để tìm element. Cú pháp: `locator1.or(locator2)`
- Ví dụ: Tìm tất cả input có thuộc tính required hoặc email

```ts
const requiredInputs = page.locator("input").or(page.locator("[type='email']"));
await requiredInputs.count();
```

### Thứ tự ưu tiên Locator nên dùng

1. getByRole()
2. getByLabel()
3. getByTestId()
4. getByText()
5. getByPlaceholder()
6. getByAltText()
7. getByTitle()
8. locator(CSS)
9. locator(XPath)

- Tuy nhiên không phải lúc nào cũng cứng nhắc. Mục tiêu là tạo locator ổn định, dễ đọc và unique. Playwright chính thức cũng khuyến nghị ưu tiên locator theo user-facing attributes và explicit contracts.

## Ví dụ thực tế:

Cho màn hình Login.

```
Login

Email
[________________]

Password
[________________]

☐ Remember me

[ Login ]

Forgot password
```

```html
<div class="login-container">
  <h2>Login</h2>

  <label for="email">Email</label>
  <input id="email" type="email" required />

  <label for="password">Password</label>
  <input id="password" type="password" required />

  <label> <input type="checkbox" /> Remember me </label>

  <button>Login</button>

  <a href="#">Forgot password?</a>
</div>
```

```ts
test("Login successfully", async ({ page }) => {
  await page.getByLabel("Email").fill("user@gmail.com");

  await page.getByLabel("Password").fill("123456");

  await page.getByRole("checkbox", { name: "Remember me" }).check();

  await page.getByRole("button", { name: "Login" }).click();

  await expect(page.getByText("Dashboard")).toBeVisible();
});
```

# Actions

- Trong Playwright, Actions là các thao tác mà người dùng thực hiện trên UI, dùng để tương tác với element như click, nhập text, chọn option, upload file...
- Các action thường được thực hiện thông qua Locator và Playwright sẽ tự động kiểm tra element có thể tương tác trước khi thực hiện action.

## Các Action phổ biến

| Action            | Mục đích             | Ví dụ                               |
| ----------------- | -------------------- | ----------------------------------- |
| `click()`         | Click element        | `page.getByRole('button').click()`  |
| `dblclick()`      | Double click         | `locator.dblclick()`                |
| `fill()`          | Nhập text vào input  | `locator.fill('hello')`             |
| `press()`         | Nhấn phím            | `locator.press('Enter')`            |
| `check()`         | Check checkbox/radio | `locator.check()`                   |
| `uncheck()`       | Bỏ check checkbox    | `locator.uncheck()`                 |
| `selectOption()`  | Chọn option          | `locator.selectOption('VN')`        |
| `hover()`         | Hover element        | `locator.hover()`                   |
| `focus()`         | Focus element        | `locator.focus()`                   |
| `clear()`         | Xóa nội dung input   | `locator.clear()`                   |
| `setInputFiles()` | Upload file          | `locator.setInputFiles('test.pdf')` |

### 1. click()

- Dùng để click button, link, checkbox, menu...
- Playwright không chỉ đơn giản gửi event click. Nó thực hiện các kiểm tra như element có visible, enabled và có thể nhận interaction hay không trước khi click.
- Cú pháp: `locator.click(options?)`
- options:
  - `button`: 'left', 'right', 'middle'
  - `clickCount`: số lần click
  - `delay`: Thời gian chờ giữa các lần click
  - `modifiers`: Các phím modifier cần giữ (shift, ctrl, alt, meta)
  - `position`: Vị trí click (x, y)
  - `strict`: Strict mode

- Ví dụ:

  ```ts
  // Click button
  await page.getByRole("button", { name: "Login" }).click();

  // Double click
  await page.getByRole("button", { name: "Login" }).dblclick();

  // Click với delay
  await page.getByRole("button", { name: "Login" }).click({ delay: 100 });

  // Click với modifiers
  await page
    .getByRole("button", { name: "Login" })
    .click({ modifiers: ["shift"] });

  // Click với position
  await page
    .getByRole("button", { name: "Login" })
    .click({ position: { x: 10, y: 10 } });
  ```

- Note: Mặc định, click sẽ đợi element hiện và có thể tương tác. Nếu muốn click mà không đợi, có thể dùng `locator.click({ force: true })`, tuy nhiên cần cân nhắc khi sử dụng vì có thể click vào element không mong muốn.

### 2. fill()

- Dùng để nhập dữ liệu vào: `<input>`, `<textarea>`, `<div contenteditable>`
- fill() sẽ tự động:
  1. focus vào element
  2. chọn hết nội dung hiện tại
  3. nhập nội dung mới
- Cú pháp: `locator.fill(value, options?)`
- options:
  - `strict`: Strict mode

- Ví dụ:

  ```ts
  // Fill email
  await page.getByLabel("Email").fill([EMAIL_ADDRESS]");

  // Fill password
  await page.getByLabel("Password").fill("123456");

  // Fill với strict mode
  await page.getByLabel("Email").fill([EMAIL_ADDRESS]", { strict: true });
  ```

### 3. type()

- Là một action dùng để nhập text vào một element bằng cách mô phỏng việc gõ từng ký tự.
- Cú pháp: `locator.type(text, options?)`
- Khi dùng type, playwright sẽ mô phỏng quá trình nhập dữ liệu bằng cách gửi event keydown, keypress và keyup cho từng ký tự `t → e → s → t → @ → e → x → a → m → p → l → e → . → c → o → m`
- type() khác fill() như thế nào?

|                         | `fill()`              | `type()`        |
| ----------------------- | --------------------- | --------------- |
| Cách nhập               | Đặt giá trị trực tiếp | Gõ từng ký tự   |
| Mô phỏng typing         | ❌                    | ✅              |
| Có delay giữa ký tự     | Không                 | Có thể cấu hình |
| Code mới                | ✅ Khuyến nghị        | ❌ Deprecated   |
| Test keyboard behavior  | Hạn chế               | Phù hợp hơn     |
| Test input thông thường | ✅                    | Không cần       |

- Khi nào nên dùng `fill()`?
  - Khi bạn chỉ cần nhập dữ liệu vào input và không quan tâm đến việc mô phỏng typing.
  - Khi bạn muốn nhập dữ liệu nhanh chóng.
- Khi nào nên dùng `type()`?
  - Khi bạn cần mô phỏng việc gõ từng ký tự.
  - Khi bạn cần test keyboard behavior.
  - Khi bạn cần nhập dữ liệu với delay.

### 4. press()

- Là một action dùng để nhấn phím.
- Cú pháp: `locator.press(key, options?)`
- key có thể là string hoặc array các string.
- options:
  - `delay`: Thời gian chờ giữa các lần nhấn phím.
  - `modifiers`: Các phím modifier cần giữ (shift, ctrl, alt, meta)

- Các phím có thể sử dụng:
  - Enter
  - Escape
  - Tab
  - ArrowDown
  - Backspace

- Ví dụ:

  ```ts
  // Press Enter
  await page.getByRole("button", { name: "Login" }).press("Enter");
  ```

- Có thể kết hợp modifier
  ```ts
  await page.getByLabel("Email").press("Control+A");
  await page.getByLabel("Email").press("Control+C");
  ```

### 5. check() / uncheck()

- Dùng cho checkbox / radio button
- Cú pháp: `locator.check(options?)` / `locator.uncheck(options?)`
- Ví dụ:

  ```ts
  // Check checkbox
  await page.getByLabel("Remember me").check();

  // Uncheck checkbox
  await page.getByLabel("Remember me").uncheck();
  ```

- Note:
  - check() sẽ tự động check nếu checkbox chưa được check.
  - uncheck() sẽ tự động uncheck nếu checkbox đã được check.
  - Không nên dùng click() để thay thế check() khi đang test trạng thái checkbox.

### 6. selectOption()

- Là một action dùng để chọn option từ `<select>` element hoặc input có role `combobox`.
- Cú pháp: `locator.selectOption(value|labels|values|indexes, options?)`
- options:
  - `strict`: Strict mode

- Ví dụ:

  ```ts
  // Select by value
  await page.getByRole("combobox").selectOption("VN");

  // Select by label
  await page.getByRole("combobox").selectOption({ label: "Vietnam" });

  // Select by index
  await page.getByRole("combobox").selectOption({ index: 1 });
  ```

- Note: selectOption() sẽ tự động clear option cũ trước khi chọn option mới.

### 7. hover()

- Là một action dùng để hover vào element.
- Cú pháp: `locator.hover(options?)`
- options:
  - `position`: Vị trí hover
  - `strict`: Strict mode

- Ví dụ:

  ```ts
  // Hover vào element
  await page.getByRole("button", { name: "Login" }).hover();

  // Hover với position
  await page
    .getByRole("button", { name: "Login" })
    .hover({ position: { x: 10, y: 10 } });
  ```

### 8. focus()

- Là một action dùng để focus vào element.
- Cú pháp: `locator.focus(options?)`
- Ví dụ:
  ```ts
  // Focus vào element
  await page.getByRole("button", { name: "Login" }).focus();
  ```

### 9. clear()

- Là một action dùng để clear nội dung input.
- Cú pháp: `locator.clear(options?)`
- Ví dụ:
  ```ts
  // Clear nội dung input
  await page.getByLabel("Email").clear();
  ```

### 10. setInputFiles()

- Là một action dùng để upload file.
- Cú pháp: `locator.setInputFiles(files, options?)`
- files có thể là:
  - string (path đến file)
  - array các string
- Ví dụ:

  ```ts
  // Upload file
  await page.getByLabel("Upload").setInputFiles("test.pdf");

  // Upload multiple files
  await page.getByLabel("Upload").setInputFiles(["test1.pdf", "test2.pdf"]);
  ```

- Note:
  - setInputFiles() sẽ tự động clear option cũ trước khi chọn option mới.
  - Nếu muốn upload file sau khi đã có file, dùng appendInputFiles()
    - Cú pháp: `locator.appendInputFiles(files, options?)`
    - Ví dụ:
      ```ts
      // Append file
      await page.getByLabel("Upload").appendInputFiles("test.pdf");
      ```

### 11. dragTo()

- Là một action dùng để kéo và thả element.
- Cú pháp: `locator.dragTo(target, options?)`
- target: Element đích để kéo đến
- Ví dụ:
  ```ts
  // Kéo element đến đích
  await page
    .getByRole("button", { name: "Drag me" })
    .dragTo(page.getByRole("button", { name: "Drop me" }));
  ```

### 12. Actions + Locator

- Trong Playwright, Locator và Action thường đi cùng nhau:
  - Locator → xác định element
  - Action → thao tác lên element
- Một Locator có thể thực hiện nhiều Action

  ```ts
  const email = page.getByLabel("Email");

  await email.fill("test@example.com");
  await email.clear();
  await email.fill("admin@example.com");
  await email.press("Enter");
  ```

# Assertion

- Trong Playwright, Assertion là cơ chế dùng để xác nhận kết quả thực tế có đúng với kết quả mong đợi hay không.
- Nếu `Action` là thực hiện hành động, thì `Assertion` là kiểm tra kết quả sau hành động.
- Cấu trúc cơ bản: `await expect(actual).matcher(expected);`
  - `actual`: giá trị thực tế cần kiểm tra, thường là **Locator**, **Value**, ...
  - `expect()`: function nhận một `actual` (giá trị thực tế) và trả về một `Expect` object.
  - `matcher`: method của `Expect` object, nó nhận một `expected` (giá trị mong đợi).
- Một số `Matcher` phổ biến:
  - expect(value).toBe(expected);
  - expect(value).not.toBe(expected);
  - expect(value).toEqual(expected);
  - expect(value).toContain(expected);
  - expect(value).toBeTruthy();
  - expect(value).toBeFalsy();
  - expect(value).toBeGreaterThan(10);
  - expect(value).toBeLessThan(10);
  - expect(value).toBeGreaterThanOrEqual(10);
  - expect(value).toBeLessThanOrEqual(10);

## Vì sao Playwright Assertion đặc biệt?

- **Auto-waiting**: Playwright không kiểm tra đúng một lần. Thay vào đó, nó sẽ thực hiện Action và sau đó **chờ đợi** để kết quả trả về đúng với mong đợi. Nếu chưa đúng, nó sẽ lặp lại Action theo một khoảng thời gian nhất định cho đến khi timeout. Nếu hết timeout mà vẫn không xuất hiện -> FAIL.

- Ví dụ:
  ```ts
  // Wait for locator to be visible, then get its text content and assert it
  await expect(page.locator(".loader")).toBeVisible();
  await expect(page.locator(".loader").textContent()).toBe("Loaded");
  ```

## Các Assertion thường dùng

- Kiểm tra element visible: `expect(locator).toBeVisible()`
- Kiểm tra element hidden: `expect(locator).toBeHidden()`
- Kiểm tra element disabled: `expect(locator).toBeDisabled()`
- Kiểm tra element enabled: `expect(locator).toBeEnabled()`
- Kiểm tra element checked: `expect(locator).toBeChecked()`
- Kiểm tra element unchecked: `expect(locator).toBeUnchecked()`
- Kiểm tra element selected: `expect(locator).toBeSelected()`
- Kiểm tra element not selected: `expect(locator).toBeNotSelected()`
- Kiểm tra element text content: `expect(locator).textContent()`
- Kiểm tra element inner text: `expect(locator).innerText()`
- Kiểm tra element value: `expect(locator).value()`
- Kiểm tra element attribute: `expect(locator).toHaveAttribute(name, value)`
- Kiểm tra element text: `expect(locator).toHaveText(text)`
- Kiểm tra element text (contains): `expect(locator).toContainText(text)`
- Kiểm tra element title: `expect(page).toHaveTitle(title)`
- Kiểm tra element tồn tại trong DOM: `expect(locator).toBeAttached()`
- Kiểm tra element không tồn tại trong DOM: `expect(locator).toBeDetached()`
- Kiểm tra URL: `expect(page).toHaveURL(url)`
- Kiểm tra số lượng element: `expect(locator).toHaveCount(count)`

## toBe vs toEqual

- `toBe()`: Kiểm tra strict equality (===). Chỉ dùng cho primitive values (string, number, boolean).
- `toEqual()`: Kiểm tra deep equality (so sánh giá trị nội dung). Dùng cho object và array.

## .not

- `.not` là một property dùng để đảo ngược kết quả của một matcher.
- Cú pháp: `expect(actual).not.matcher(expected);`
- Ví dụ:
  ```ts
  // Kiểm tra element không visible
  await expect(page.locator(".loader")).not.toBeVisible();
  ```

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

#
