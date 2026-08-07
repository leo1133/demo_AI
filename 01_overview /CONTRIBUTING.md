# Tài liệu hướng dẫn (bản Markdown)

## 1. Kết nối Figma với Antigravity IDE thông qua Figma MCP (Model Context Protocol)

Mục tiêu: Đảm bảo Antigravity nhận biết cấu trúc component, style, biến màu (variables) từ Figma để AI có thể sinh code/test chính xác theo design system.  
Cách cài đặt 
- Mở Antigravity IDE.
- Trong cửa sổ Agent, nhấn dấu ....
- Chọn MCP Servers.
- Tìm Figma và nhấn Install.
- Đăng nhập tài khoản Figma khi được yêu cầu.
- Cho phép Antigravity truy cập Figma.

## 2. Flow để ứng dụng AI phân tích yêu cầu => đặt Q&A => gen testcase manual => sử dụng testcase manual để sinh code playwright

### 2.1. Flow của AI QA Agent

- Flow tổng thể 
Requirement
    ↓
Analysis
    ↓
Knowledge Base
    ↓
Q&A
    ↓
Test Scenario
    ↓
Test Case
    ↓
Automation
    ↓
Playwright
    ↓
Test Result
    ↓
AI Review

- Điểm quan trọng: AI phải phát hiện thông tin thiếu/mâu thuẫn và đặt câu hỏi làm rõ thay vì tự suy diễn. Khi thiếu thông tin -> Hỏi người dùng -> Wait response -> Tiếp tục phân tích.

### 2.2. Cấu trúc project

- Cấu trúc 
AI-QA-Agent/
│
├── README.md
├── package.json
├── playwright.config.ts
├── .env
│
├── requirements/
│   ├── brd/
│   ├── srs/
│   ├── api/
│   ├── design/
│   ├── business-rules/
│   ├── release-notes/
│   └── existing-testcases/
│
├── knowledge-base/
│   ├── project-overview.md
│   ├── actors.md
│   ├── modules.md
│   ├── business-rules.md
│   ├── workflows.md
│   ├── permissions.md
│   ├── api-contracts.md
│   ├── database.md
│   ├── risks.md
│   └── open-questions.md
│
├── ai/
│   ├── prompts/
│   │   ├── system/
│   │   │   └── qa-agent.md
│   │   │
│   │   ├── requirement/
│   │   │   ├── analyze.md
│   │   │   └── review.md
│   │   │
│   │   ├── qa/
│   │   │   ├── generate-questions.md
│   │   │   ├── generate-scenario.md
│   │   │   └── generate-testcase.md
│   │   │
│   │   └── automation/
│   │       ├── analyze-testcase.md
│   │       ├── generate-script.md
│   │       └── review-script.md
│   │
│   ├── agents/
│   │   ├── requirement-agent.ts
│   │   ├── qa-agent.ts
│   │   ├── testcase-agent.ts
│   │   ├── automation-agent.ts
│   │   └── review-agent.ts
│   │
│   └── schemas/
│       ├── requirement.schema.json
│       ├── scenario.schema.json
│       ├── testcase.schema.json
│       └── automation.schema.json
│
├── qa/
│   ├── analysis/
│   │   ├── requirement-analysis/
│   │   ├── qna/
│   │   ├── scenarios/
│   │   └── testcases/
│   │
│   └── traceability/
│       └── requirement-testcase.json
│
├── automation/
│   ├── tests/
│   │   ├── login/
│   │   ├── product/
│   │   ├── cart/
│   │   └── checkout/
│   │
│   ├── pages/
│   │   ├── LoginPage.ts
│   │   ├── HomePage.ts
│   │   ├── ProductPage.ts
│   │   └── CartPage.ts
│   │
│   ├── components/
│   ├── fixtures/
│   ├── api/
│   ├── utils/
│   └── test-data/
│
├── reports/
│   ├── playwright/
│   ├── ai-analysis/
│   └── coverage/
│
└── scripts/
    ├── analyze-requirement.ts
    ├── generate-testcase.ts
    ├── generate-automation.ts
    └── run-ai-qa.ts

### 2.3. Chức năng của từng file/folder 
#### A. Folder requirements/ - INPUT cho AI:
- Đây là nơi bạn đưa tài liệu dự án vào.
+ requirements/brd/ => AI lấy thông tin về: Business goal, Scope, Feature, Actor, Business flow
+ requirements/srs/ => AI lấy thông tin về: Detailed behavior, Validation, Exception, Expected behavior, Business logic
+ requirements/api/ => AI dùng để xác định: endpoint, method, request body, response, status code, validation, error code...
+ requirements/design/ => AI lấy thông tin về: UI element, component, design, variable..., layout, flow...
+ requirements/business-rules/ => AI lấy thông tin về: Business rule
+ requirements/release-notes/ => AI lấy thông tin về: Version, Release date, Feature, Bug fix, Known issue...
+ requirements/existing-testcases/ => AI lấy thông tin về: Existing test case...

#### B. Folder knowledge-base/ - KNOWLEDGE BASE cho AI: 
- AI sẽ đọc toàn bộ tài liệu trong folder này để hiểu về hệ thống.
+ knowledge-base/project-overview.md => Tổng quan dự án
+ knowledge-base/actors.md => Actor của hệ thống
+ knowledge-base/modules.md => Module của hệ thống
+ knowledge-base/business-rules.md => Business rule
+ knowledge-base/workflows.md => Workflow của hệ thống
+ knowledge-base/permissions.md => Permission của hệ thống
+ knowledge-base/api-contracts.md => API contract
+ knowledge-base/database.md => Database schema
+ knowledge-base/risks.md => Risk của hệ thống
+ knowledge-base/open-questions.md => Open question
