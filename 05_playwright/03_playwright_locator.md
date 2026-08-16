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
  const requiredInputs = page
    .locator("input")
    .or(page.locator("[type='email']"));
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
