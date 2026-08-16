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
