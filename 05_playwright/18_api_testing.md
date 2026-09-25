# API Testing

## API Testing trong Playwright

Playwright không chỉ dùng để test UI.
Nó có thể gửi trực tiếp các HTTP request đến backend thông qua `APIRequestContext`.
Mục đích chính của API testing:

- Test trực tiếp server API
- Chuẩn bị state phía server trước UI test
- Kiểm tra post-condition sau khi thao tác trên UI

Ví dụ:

```ts
const response = await request.post("/api/login", {
  data: {
    email: "test@example.com",
    password: "123456",
  },
});
```

**Ưu điểm**

- Không cần start web browser
- Nhanh hơn
- Test được ở mọi nơi

## Thành phần quan trọng nhất: APIRequestContext

`APIRequestContext` chịu trách nhiệm gửi HTTP request. `APIRequestContext` có thể gửi các HTTP(S) request trực tiếp tới server.

Bạn có thể khởi tạo context thông qua `request.newContext()` hoặc `browser.newContext().request`.

### Ví dụ cách tạo APIRequestContext

```ts
import { test, expect } from "@playwright/test";

test("test API", async ({ request }) => {
  const response = await request.get(
    "https://jsonplaceholder.typicode.com/posts/1",
  );
});
```

### Ví dụ cách tạo APIRequestContext với Base URL

Ví dụ này sẽ call endpoint `https://jsonplaceholder.typicode.com/posts/1`.

```ts
import { test, expect } from "@playwright/test";

test("test API with base URL", async ({ request }) => {
  const response = await request.get("/posts/1");
});
```

### Ví dụ cách tạo APIRequestContext với config

```ts
import { test, expect } from "@playwright/test";

test("test API with config", async ({ request }) => {
  const apiRequestContext = await request.newContext({
    baseURL: "https://jsonplaceholder.typicode.com",
  });
  const response = await apiRequestContext.get("/posts/1");
});
```

## request fixture – cách bạn sẽ dùng nhiều nhất

Playwright Test cung cấp sẵn fixture: `request`, chính là API client mà Playwright cung cấp cho test. Fixture này trong các test.

```ts
import { test, expect } from "@playwright/test";

test("Create user", async ({ request }) => {
  const response = await request.post("/users", {
    data: {
      name: "Nga",
      email: "nga@example.com",
    },
  });

  expect(response.ok()).toBeTruthy();
});
```

## Các HTTP Method

| Postman | Playwright         |
| ------- | ------------------ |
| GET     | `request.get()`    |
| POST    | `request.post()`   |
| PUT     | `request.put()`    |
| PATCH   | `request.patch()`  |
| DELETE  | `request.delete()` |
| Body    | `data`             |
| Headers | `headers`          |
| Tests   | `expect()`         |

## baseURL

Trong Playwright Test, bạn có thể cấu hình baseURL thông qua `test.use` hoặc file `playwright.config.ts`.

Ví dụ trong file playwright.config.ts:

```ts
import { defineConfig } from "@playwright/test";

export default defineConfig({
  use: {
    baseURL: "https://jsonplaceholder.typicode.com",
  },
});
```

## Headers

Bạn có thể cấu hình headers thông qua `test.use` hoặc file `playwright.config.ts`.

Ví dụ trong playwright.config.ts cho tất cả các request:

```ts
export default defineConfig({
  use: {
    baseURL: "https://api.example.com",

    extraHTTPHeaders: {
      Accept: "application/json",
      "Content-Type": "application/json",
    },
  },
});
```

Ví dụ config cho 1 request bất kì

```ts
const response = await request.get("/users", {
  headers: {
    Accept: "application/json",
  },
});
```

## Authentication

Nếu api require token, bạn cần config cho request. Sau đó dùng token để request đến các endpoint require token.

Ví dụ cách login để lấy token và lưu token vào env

```ts
test("Login", async ({ request }) => {
  const response = await request.post("/login", {
    data: {
      email: process.env.EMAIL,
      password: process.env.PASSWORD,
    },
  });

  expect(response.ok()).toBeTruthy();

  const token = await response.json();
  process.env.TOKEN = token;
});
```

Trong các file test khác, bạn có thể dùng token để request đến các endpoint require token:

```ts
import { test, expect } from "@playwright/test";

test("test API with token", async ({ request }) => {
  const apiRequestContext = await request.newContext({
    baseURL: "https://jsonplaceholder.typicode.com",
  });
  const response = await apiRequestContext.get("/posts/1", {
    headers: {
      Authorization: `Bearer ${process.env.TOKEN}`,
    },
  });

  expect(response.ok()).toBeTruthy();
});
```

## Request Body

Với các testcase bạn từng xây dựng cho Login API, bạn có thể chuyển khá trực tiếp từ Postman sang Playwright. Body của request trong Postman có dạng:

```js
{
    "title": "Playwright Testing",
    "body": "API Testing with Playwright",
    "userId": 1
}
```

Bạn có thể chuyển sang Playwright bằng cách sử dụng `data` property của `request` method:

```ts
import { test, expect } from "@playwright/test";

test("Create post", async ({ request }) => {
  const response = await request.post("/posts", {
    data: {
      title: "Playwright Testing",
      body: "API Testing with Playwright",
      userId: 1,
    },
  });

  expect(response.ok()).toBeTruthy();
});
```

## Response

Sau khi gọi 1 request sẽ nhận được status code và response body. Sử dụng `expect(response.status()).toBe(200);` để kiểm tra status code. Sử dụng `expect(response.ok()).toBeTruthy();` để kiểm tra response thành công.

`ok()` kiểm tra response có thuộc nhóm HTTP thành công hay không. Trong ví dụ chính thức, Playwright sử dụng `expect(newIssue.ok()).toBeTruthy()` để xác nhận request thành công.

Ví dụ:

```ts
test("Create issue", async ({ request }) => {
  const issueData = {
    title: "issue title",
    body: "issue body",
  };

  const response = await request.post(
    `${process.env.API_URL}/repos/octocat/Spoon-Knife/issues`,
    {
      data: issueData,
      headers: {
        Accept: "application/vnd.github+json",
        "X-GitHub-Api-Version": "2022-11-28",
      },
    },
  );

  expect(response.ok()).toBeTruthy();

  const issue = await response.json();
  expect(issue.title).toBe(issueData.title);
  expect(issue.body).toBe(issueData.body);
});
```

### Assert status code

```ts
test("test status code", async ({ request }) => {
  const response = await request.get("/posts/1");
  expect(response.status()).toBe(200);
});
```

### Assert response body

Ví dụ cách lấy response body và assertion trên response body:

```ts
import { test, expect } from "@playwright/test";

test("test response body", async ({ request }) => {
  const response = await request.get("/posts/1");
  const json = await response.json();
  expect(json.title).toBe(
    "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  );
});
```

# Liên hệ giữa POSTMAN và PLAYWRIGHT

| Postman            | Playwright                      |
| ------------------ | ------------------------------- |
| Collection         | Test files / test suite         |
| Request            | `request.get/post/...`          |
| Environment        | `.env` / `process.env`          |
| Pre-request Script | JS / fixture / hook             |
| Tests              | `expect()`                      |
| Variables          | JS variables / env variables    |
| Runner             | `npx playwright test`           |
| CSV Data           | Parameterized tests / test data |
| Collection auth    | `extraHTTPHeaders` / fixture    |
| Response JSON      | `await response.json()`         |
| Status code        | `response.status()`             |

Điểm khác biệt lớn là Playwright đưa API testing vào cùng một test framework với UI testing, nên bạn có thể xây dựng end-to-end flow mà không cần chuyển qua lại giữa Postman và UI automation.

# Khi làm dự án automation bằng playwright có nhất thiết phải làm cả API testing không?

Không bắt buộc. Việc có nên thêm API Testing vào framework Playwright hay không phụ thuộc vào rất nhiều yếu tố của dự án.
Điều quan trọng là hiểu mục tiêu của project automation.

## Nếu mục tiêu là UI Automation thì chỉ cần UI

Project của bạn chỉ muốn tự động hóa các chức năng trên giao diện:

- Login, logout
- CRUD products
- CRUD users
- ...
  -> Chỉ cần UI Testing theo POM

## Nếu project là E2E thì cần cả UI và API

Dự án của bạn muốn kiểm tra flow người dùng từ đầu đến cuối, bao gồm cả UI và API:

- Login
- CRUD products
- CRUD users
- ...
  -> Cần cả UI Testing và API Testing.
  - UI Testing theo POM
  - API Testing theo POJO hoặc Service POJO
  - Test data dùng chung

## Khi nào cần dùng API Testing

API Testing nên được thêm vào khi nó mang lại lợi ích rõ ràng.

- Chuẩn bị test data cho UI Testing
- Cleanup data
- Verify backend sau thao tác UI

## Khi nào không cần API?

- Chỉ xây dựng POM (thay vì POJO)
- Test các chức năng UI cơ bản
- Không có API hoặc không được access API
- Mục tiêu chỉ là UI Regression
