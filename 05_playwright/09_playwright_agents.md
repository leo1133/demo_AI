# Agents

## Playwright Test Agents là gì?

Playwright Test Agents là một tính năng của Playwright cho phép tự động hóa các tác vụ kiểm thử bằng cách sử dụng trí tuệ nhân tạo.
Playwright hiện cung cấp 3 AI Test Agents:

- AI Test Planner
- AI Test Generator
- AI Test Healer

Mỗi Agent đảm nhận một công việc khác nhau:
| Agent | Vai trò | Input | Output |
| ---------------- | -------------------------------------- | ---------------------------------------- | ---------------- |
| 🎭 **Planner** | Khám phá application và tạo test plan | App + request + seed test + PRD tùy chọn | `.md` test plan |
| 🎭 **Generator** | Chuyển test plan thành Playwright test | `.md` test plan | `.spec.ts` |
| 🎭 **Healer** | Chạy test và sửa test bị fail | Failing test | Test đã được sửa |

Playwright cho phép chạy 3 Agent độc lập, tuần tự, hoặc nối thành một agentic loop.

## Quy trình

Quy trình manual

```
Requirement / SRS
↓
Phân tích requirement
↓
Viết Test Scenario
↓
Viết Test Case
↓
Viết Automation Script
↓
Execute
↓
Test fail
↓
Debug
↓
Sửa automation
```

Quy trình với AI Test Agents

```
Application
↓
Planner
↓
Test Plan (.md)
↓
Generator
↓
Playwright Test (.spec.ts)
↓
Execute
↓
Fail
↓
Healer
↓
Repair Test
↓
Re-run
```

## 1. Planner Agent

Planner Agent có nhiệm vụ khám phá application và tạo ra Test Plan.

Cấu hình:

- `--url`: URL của application
- `--output`: Đường dẫn lưu output
- `--model`: Chọn model AI
- `--strategy`: Chọn strategy (Browser based hoặc URL based)

Ví dụ:

```bash
# Khám phá trang web và tạo test plan
$ npx playwright test agents plan-test scenarios --url https://playwright.dev

# Tạo test plan cho nhiều URL
$ npx playwright test agents plan-test scenarios --url https://playwright.dev https://github.com
```

Planner không chỉ "viết test case" mà nó có khả năng explore application. Nó có thể đi từng page, từng element và sinh ra đầy đủ đồ án cho bạn. Tức là bạn không chỉ nhận được testcase mà nhận được cả báo cáo chi tiết về page bạn đã duyệt, element bạn đã tương tác.
Agent có thể tương tác với application để tìm hiểu:

```
Login page
 ├── Email
 ├── Password
 ├── Login button
 ├── Forgot password
 └── Validation
```

Sau đó xây dựng scenario:

```
Scenario 1: Successful login with valid credentials

Steps:

1. Go to the login page.
2. Enter a valid email address in the email field.
3. Enter a valid password in the password field.
4. Click the login button.
5. Verify that the user is successfully logged in.
```

## 2. Generator Agent

Generator Agent có nhiệm vụ dựa trên Test Plan đã tạo bởi Planner Agent để viết ra Playwright Test.

Cấu hình:

```bash
# Sinh test từ test plan đã có sẵn
$ npx playwright test agents generate-tests ./test-plan.md

# Sinh test từ nhiều test plan
$ npx playwright test agents generate-tests ./test-plans/
```

Output của Agent Generator sẽ là file test case dưới dạng TypeScript:

```typescript
// playwright-runner/test-auth.spec.ts
import { test, expect } from '@playwright/test';

test('Authentication flow test', async ({ page }) => {
  await test.step('Go to login page', async () => {
    await page.goto('https://example.com/login');
  });

  await test.step('Enter email and password', async () => {
    await page.getByRole('textbox', { name: 'Email' }).fill([EMAIL_ADDRESS]');
    await page.getByRole('textbox', { name: 'Password' }).fill('your-password');
  });

  await test.step('Click login button', async () => {
    await page.getByRole('button', { name: 'Login' }).click();
  });

  await test.step('Verify successful login', async () => {
    await expect(page.getByRole('heading', { name: 'Welcome' })).toBeVisible();
  });
});
```

## 3. Healer Agent

Healer Agent có nhiệm vụ chạy test và sửa lỗi khi test fail.

Cấu hình:

```bash
# Chạy test và tự động sửa lỗi khi test fail
$ npx playwright test agents heal-tests --url https://playwright.dev
```

Healer Agent có khả năng tự động sửa lỗi khi test fail. Khi gặp lỗi, agent sẽ tự động chụp màn hình, ghi lại video, phân tích nguyên nhân và sửa lỗi. Sau đó, agent sẽ chạy lại test để kiểm tra kết quả.

## 4. Playwright Code Agent

Playwright Code Agent là một Agent có khả năng tự động hóa các tác vụ kiểm thử bằng cách sử dụng trí tuệ nhân tạo. Nó được giới thiệu vào ngày 19 tháng 5 năm 2026.

### 4.1. Demo Code Agent

```bash
$ npx playwright test agents code --base-url https://playwright.dev --code-pattern="**/test-*.spec.ts"
```

## 5. Prompt và System Messages cho Agent

### Prompt cho Agent

Playwright Test Agents được điều khiển bởi các System Message, chứa đựng các chỉ dẫn và thông tin để Agent hiểu được vai trò, mục tiêu cũng như quy tắc hành xử. Dưới đây là phần trích xuất và phân tích chi tiết system message từ các agent khác nhau.

**System prompt cho Planner Agent**

```typescript
    const plannerSystemPrompt = `
    {
      "name": "test-plan-scenario-generator",
      "description": "Generates a test plan with scenario and testcase markdown files for a given URL.",
      "steps": [
        {
          "name": "browser-based-explore",
          "description": "Explores the web page in a browser to discover its structure and features",
          "type": "tool",
          "toolName": "playwright-ui-explorer"
        },
        {
          "name": "url-based-explore",
          "description": "Explores web pages by visiting URLs and extracting content",
          "type": "tool",
          "toolName": "playwright-url-explorer"
        },
        {
          "name": "generate-test-plan",
          "description": "Generates a test plan with scenario and testcase markdown files",
          "type": "tool",
          "toolName": "playwright-test-plan-generator"
        }
      ],
      "options": {
        "browser-based-explore": {
          "url": "The starting URL to explore",
          "output": "The directory to save the exploration results",
          "headless": {
            "type": "boolean",
            "default": false,
            "description": "Whether to run the browser in headless mode"
          }
        },
        "url-based-explore": {
          "urls": "An array of URLs to explore",
          "output": "The directory to save the exploration results"
        },
        "generate-test-plan": {
          "testPlan": "The test plan to use for generating test cases",
          "output": "The directory to save the test plan",
          "seed": "Optional seed test cases"
        }
      }
    }
```

**System prompt cho Generator Agent**

```typescript
{
  "name": "playwright-code-generator",
  "description": "Generates Playwright test code from a test plan",
  "steps": [
    {
      "name": "load-test-plan",
      "description": "Loads the test plan from a markdown file",
      "type": "tool",
      "toolName": "playwright-load-test-plan"
    },
    {
      "name": "generate-test-code",
      "description": "Generates Playwright test code from the test plan",
      "type": "tool",
      "toolName": "playwright-test-code-generator"
    },
    {
      "name": "save-test-code",
      "description": "Saves the generated test code to a file",
      "type": "tool",
      "toolName": "playwright-save-test-code"
    }
  ],
  "options": {
    "load-test-plan": {
      "testPlan": "The path to the test plan file"
    },
    "generate-test-code": {
      "testPlan": "The test plan to use for generating test code",
      "output": "The directory to save the generated test code",
      "language": "The programming language to use (typescript or javascript)"
    },
    "save-test-code": {
      "testCode": "The generated test code",
      "output": "The path to save the test code"
    }
  }
}
```

**System prompt cho Healer Agent**

```typescript
{
  "name": "playwright-agent-healer",
  "description": "Automatically heals failing Playwright tests",
  "steps": [
    {
      "name": "collect-test-results",
      "description": "Collects test results from Playwright test runner",
      "type": "tool",
      "toolName": "playwright-collect-test-results"
    },
    {
      "name": "analyze-failures",
      "description": "Analyzes test failures to determine root causes",
      "type": "tool",
      "toolName": "playwright-analyze-failures"
    },
    {
      "name": "suggest-fixes",
      "description": "Suggests fixes for test failures",
      "type": "tool",
      "toolName": "playwright-suggest-fixes"
    },
    {
      "name": "apply-fixes",
      "description": "Applies fixes to test files",
      "type": "tool",
      "toolName": "playwright-apply-fixes"
    },
    {
      "name": "run-tests-again",
      "description": "Reruns tests after applying fixes",
      "type": "tool",
      "toolName": "playwright-run-tests"
    }
  ],
  "options": {
    "collect-test-results": {
      "testRun": "The test run to analyze"
    },
    "analyze-failures": {
      "testResults": "The test results to analyze"
    },
    "suggest-fixes": {
      "analysisResults": "The analysis results from analyzing failures"
    },
    "apply-fixes": {
      "fixes": "The fixes to apply",
      "testFiles": "The test files to apply fixes to"
    },
    "run-tests-again": {
      "testFiles": "The test files to run"
    }
  }
}
```

**System prompt cho Code Agent**

```typescript
{
  "name": "playwright-code-agent",
  "description": "Playwright Code Agent for generating and running Playwright tests",
  "steps": [
    {
      "name": "playwright-explore-url",
      "description": "Explore web pages by visiting URLs and extracting content",
      "type": "tool",
      "toolName": "playwright-explore-url"
    },
    {
      "name": "playwright-locate-elements",
      "description": "Locate elements in a web page",
      "type": "tool",
      "toolName": "playwright-locate-elements"
    },
    {
      "name": "playwright-generate-test",
      "description": "Generate Playwright test code",
      "type": "tool",
      "toolName": "playwright-generate-test"
    },
    {
      "name": "playwright-run-test",
      "description": "Run Playwright test code",
      "type": "tool",
      "toolName": "playwright-run-test"
    },
    {
      "name": "playwright-save-test",
      "description": "Save test code to a file",
      "type": "tool",
      "toolName": "playwright-save-test"
    }
  ],
  "options": {
    "playwright-explore-url": {
      "url": "The URL to explore",
      "output": "The directory to save exploration results"
    },
    "playwright-locate-elements": {
      "selector": "The CSS selector to locate elements"
    },
    "playwright-generate-test": {
      "testCases": "The test cases to generate code for",
      "output": "The directory to save the generated test code"
    },
    "playwright-run-test": {
      "testFile": "The test file to run"
    },
    "playwright-save-test": {
      "testCode": "The test code to save",
      "output": "The path to save the test code"
    }
  }
}
```

### System prompt của Test Plan Generator

```typescript
    {
  "name": "test-plan-scenario-generator",
  "description": "Generates a test plan with scenario and testcase markdown files for a given URL.",
  "steps": [
    {
      "name": "browser-based-explore",
      "description": "Explores the web page in a browser to discover its structure and features",
      "type": "tool",
      "toolName": "playwright-ui-explorer"
    },
    {
      "name": "url-based-explore",
      "description": "Explores web pages by visiting URLs and extracting content",
      "type": "tool",
      "toolName": "playwright-url-explorer"
    },
    {
      "name": "generate-test-plan",
      "description": "Generates a test plan with scenario and testcase markdown files",
      "type": "tool",
      "toolName": "playwright-test-plan-generator"
    }
  ],
  "options": {
    "browser-based-explore": {
      "url": "The starting URL to explore",
      "output": "The directory to save the exploration results",
      "headless": {
        "type": "boolean",
        "default": false,
        "description": "Whether to run the browser in headless mode"
      }
    },
    "url-based-explore": {
      "urls": "An array of URLs to explore",
      "output": "The directory to save the exploration results"
    },
    "generate-test-plan": {
      "testPlan": "The test plan to use for generating test cases",
      "output": "The directory to save the test plan",
      "seed": "Optional seed test cases"
    }
  }
}
```

## 6. Các Tools được cung cấp cho Agent

### UI Explorer Agent Tools

```typescript
    {
      "name": "playwright-ui-explorer",
      "description": "Explores a web page using Playwright to discover its structure and features.",
      "parameters": {
        "type": "object",
        "properties": {
          "url": {
            "type": "string",
            "description": "The starting URL to explore"
          },
          "output": {
            "type": "string",
            "description": "The directory to save exploration results (e.g., HTML, screenshots)"
          },
          "headless": {
            "type": "boolean",
            "default": false,
            "description": "Whether to run the browser in headless mode"
          }
        },
        "required": ["url"]
      }
    }
```

### URL Explorer Agent Tools

```typescript
{
  "name": "playwright-url-explorer",
  "description": "Explores a list of URLs using Playwright to discover their structure and features.",
  "parameters": {
    "type": "object",
    "properties": {
      "urls": {
        "type": "array",
        "items": { "type": "string" },
        "description": "An array of URLs to explore"
      },
      "output": {
        "type": "string",
        "description": "The directory to save exploration results"
      },
      "headless": {
        "type": "boolean",
        "default": false,
        "description": "Whether to run the browser in headless mode"
      }
    },
    "required": ["urls"]
  }
}
```

### Test Plan Generator Agent Tools

```typescript
{
  "name": "playwright-test-plan-generator",
  "description": "Generates a test plan with scenarios and test cases from exploration results.",
  "parameters": {
    "type": "object",
    "properties": {
      "explorationResults": {
        "type": "string",
        "description": "The path to the exploration results"
      },
      "output": {
        "type": "string",
        "description": "The directory to save the test plan"
      },
      "seed": {
        "type": "string",
        "description": "Optional seed test cases (markdown format)"
      },
      "personas": {
        "type": "array",
        "items": { "type": "string" },
        "description": "User personas to generate test cases for"
      }
    },
    "required": ["explorationResults", "output"]
  }
}
```

### Test Code Generator Agent Tools

```typescript
{
  "name": "playwright-code-generator",
  "description": "Generates Playwright test code from a test plan.",
  "parameters": {
    "type": "object",
    "properties": {
      "testPlan": {
        "type": "string",
        "description": "The path to the test plan markdown file"
      },
      "output": {
        "type": "string",
        "description": "The directory to save generated test code"
      },
      "language": {
        "type": "string",
        "enum": ["typescript", "javascript"],
        "default": "typescript",
        "description": "The programming language for the test code"
      },
      "browser": {
        "type": "string",
        "default": "chromium",
        "description": "The browser to generate tests for"
      }
    },
    "required": ["testPlan", "output"]
  }
}
```

### Test Execution Agent Tools

```typescript
{
  "name": "playwright-test-runner",
  "description": "Executes Playwright tests and generates reports.",
  "parameters": {
    "type": "object",
    "properties": {
      "testFiles": {
        "type": "array",
        "items": { "type": "string" },
        "description": "Array of test files to run"
      },
      "output": {
        "type": "string",
        "description": "Directory to save test results and reports"
      },
      "browser": {
        "type": "string",
        "enum": ["chromium", "firefox", "webkit", "all"],
        "default": "chromium",
        "description": "Browser to run tests on"
      },
      "headless": {
        "type": "boolean",
        "default": true,
        "description": "Whether to run in headless mode"
      }
    },
    "required": ["testFiles", "output"]
  }
}
```

### Test Analysis Agent Tools

```typescript
{
  "name": "playwright-test-analyzer",
  "description": "Analyzes test results and generates insights.",
  "parameters": {
    "type": "object",
    "properties": {
      "testResults": {
        "type": "string",
        "description": "Path to test results (JSON or HTML reports)"
      },
      "output": {
        "type": "string",
        "description": "Directory to save analysis reports"
      },
      "failRateThreshold": {
        "type": "number",
        "default": 20,
        "description": "Threshold for highlighting high failure rates (%)"
      }
    },
    "required": ["testResults", "output"]
  }
}
```

### Test Maintenance Agent Tools

```typescript
{
  "name": "playwright-test-maintenance",
  "description": "Maintains and updates test assets.",
  "parameters": {
    "type": "object",
    "properties": {
      "assetType": {
        "type": "string",
        "enum": ["test-plan", "test-code", "test-data"],
        "description": "Type of asset to update"
      },
      "assetPath": {
        "type": "string",
        "description": "Path to the asset to update"
      },
      "updateType": {
        "type": "string",
        "enum": ["audit", "refactor", "extend", "migrate"],
        "description": "Type of update to perform"
      },
      "reason": {
        "type": "string",
        "description": "Reason for updating"
      },
      "output": {
        "type": "string",
        "description": "Directory to save updated assets"
      }
    },
    "required": ["assetType", "assetPath", "updateType", "reason", "output"]
  }
}
```

### Full Test Generation Workflow (System Prompt)

Để triển khai bộ workflow, bạn có thể sử dụng system prompt kết hợp với các tools trên như sau:

```typescript
{
  "name": "playwright-test-generation-workflow",
  "description": "Generates and executes Playwright tests from a web URL using AI agents",
  "steps": [
    {
      "name": "explore-url",
      "description": "Explore the web page to discover its structure",
      "type": "tool",
      "toolName": "playwright-ui-explorer"
    },
    {
      "name": "generate-test-plan",
      "description": "Generate test plan with scenarios and test cases",
      "type": "tool",
      "toolName": "playwright-test-plan-generator"
    },
    {
      "name": "generate-test-code",
      "description": "Generate Playwright test code from test plan",
      "type": "tool",
      "toolName": "playwright-code-generator"
    },
    {
      "name": "run-tests",
      "description": "Execute the generated tests",
      "type": "tool",
      "toolName": "playwright-test-runner"
    },
    {
      "name": "analyze-results",
      "description": "Analyze test results and generate insights",
      "type": "tool",
      "toolName": "playwright-test-analyzer"
    }
  ],
  "options": {
    "explore-url": {
      "url": "The starting URL to explore",
      "output": "Directory to save exploration results"
    },
    "generate-test-plan": {
      "explorationResults": "Path to exploration results",
      "output": "Directory to save test plan"
    },
    "generate-test-code": {
      "testPlan": "Path to test plan",
      "output": "Directory to save test code"
    },
    "run-tests": {
      "testFiles": "Generated test files",
      "output": "Directory to save test results"
    },
    "analyze-results": {
      "testResults": "Path to test results",
      "output": "Directory to save analysis reports"
    }
  }
}
```

## 7. Triển khai End-to-End với Autogen

Ví dụ cụ thể triển khai full workflow từ khám phá URL đến generate test cases sử dụng Autogen:

```ts
import { Autogen, UserProxyAgent } from "autogen-ai";

// Khởi tạo Autogen
const autogen = new Autogen({
  apiKey: "YOUR_API_KEY",
});

// Tạo các agents
const urlExplorerAgent = new autogen.Agent({
  name: "playwright-url-explorer",
  description: "Explores a web page and discovers its structure",
  tools: [
    {
      name: "explore-url",
      description: "Explore a given URL",
      parameters: {
        type: "object",
        properties: {
          url: { type: "string" },
          output: { type: "string" },
        },
        required: ["url", "output"],
      },
      implementation: async ({ url, output }) => {
        // Implement URL exploration logic here
        return `Explored ${url}`;
      },
    },
  ],
});

const testPlanGeneratorAgent = new autogen.Agent({
  name: "playwright-test-plan-generator",
  description: "Generates a test plan from exploration results",
  tools: [
    {
      name: "generate-test-plan",
      description: "Generate a test plan",
      parameters: {
        type: "object",
        properties: {
          explorationResults: { type: "string" },
          output: { type: "string" },
        },
        required: ["explorationResults", "output"],
      },
      implementation: async ({ explorationResults, output }) => {
        // Implement test plan generation logic here
        return `Generated test plan for ${explorationResults}`;
      },
    },
  ],
});

const testCodeGeneratorAgent = new autogen.Agent({
  name: "playwright-code-generator",
  description: "Generates test code from a test plan",
  tools: [
    {
      name: "generate-test-code",
      description: "Generate test code",
      parameters: {
        type: "object",
        properties: {
          testPlan: { type: "string" },
          output: { type: "string" },
        },
        required: ["testPlan", "output"],
      },
      implementation: async ({ testPlan, output }) => {
        // Implement test code generation logic here
        return `Generated test code for ${testPlan}`;
      },
    },
  ],
});

// Tạo user proxy agent
const userProxyAgent = new UserProxyAgent({
  name: "user-proxy",
  description: "User proxy agent",
});

// Tạo user proxy agent
const groupChatManager = new autogen.GroupChatManager({
  name: "test-generation-manager",
  description: "Group chat manager for test generation",
  agents: [
    userProxyAgent,
    urlExplorerAgent,
    testPlanGeneratorAgent,
    testCodeGeneratorAgent,
  ],
});

// Bắt đầu workflow
async function startTestGenerationWorkflow() {
  const result = await groupChatManager.sendMessage(
    "Start test generation workflow",
  );
  console.log("Test generation workflow completed:", result);
}

startTestGenerationWorkflow();
```

## 8. Các thành phần quan trọng

### URL Explorer Agent

```typescript
import { BrowserContext, Page } from "playwright";

// Cấu hình
export interface URLExplorerConfig {
  url: string;
  outputDir: string;
  browser: BrowserType;
  headless: boolean;
  crawlDepth: number;
}

// Kết quả khám phá
export interface URLExplorationResult {
  url: string;
  pageTitle: string;
  pageDescription: string;
  headings: string[];
  buttons: string[];
  links: string[];
  inputs: string[];
  forms: any[];
  statusCode: number;
  screenshots: string[];
  htmlContent: string;
}
```

```

```

```

```
