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
