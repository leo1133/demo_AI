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
+ requirements/existing-testcases/ => Nơi người dùng đưa test case hiện tại của project vào

#### B. Folder knowledge-base/ - AI lưu kiến thức đã phân tích:
- Đây là output/working knowledge của AI. Output sẽ update qua từng step.
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

### C. ai/prompts/ — Prompt của AI
- Đây là nơi chứa prompt để điều khiển AI.
+ ai/prompts/system/qa-agent.md => Prompt của QA Agent
+ ai/prompts/requirement/analyze.md => Prompt của Requirement Agent
+ ai/prompts/requirement/review.md => Prompt của Requirement Agent
+ ai/prompts/qa/generate-questions.md => Prompt của QA Agent
+ ai/prompts/qa/generate-scenario.md => Prompt của QA Agent
+ ai/prompts/qa/generate-testcase.md => Prompt của QA Agent
+ ai/prompts/automation/analyze-testcase.md => Prompt của Automation Agent
+ ai/prompts/automation/generate-script.md => Prompt của Automation Agent
+ ai/prompts/automation/review-script.md => Prompt của Automation Agent

### D. ai/agents/ — Code của AI Agent
- Đây là nơi chứa code của AI Agent. 
+ ai/agents/requirement-agent.ts => Requirement Agent
+ ai/agents/qa-agent.ts => QA Agent
+ ai/agents/testcase-agent.ts => Testcase Agent
+ ai/agents/automation-agent.ts => Automation Agent
+ ai/agents/review-agent.ts => Review Agent

### E. ai/schemas/ — Format chuẩn của AI output
- Đây là nơi chứa schema của AI output. 
- Mục đích là đảm bảo AI luôn trả output cùng một format chuẩn
+ ai/schemas/requirement.schema.json => Format của Requirement Agent
+ ai/schemas/scenario.schema.json => Format của QA Agent
+ ai/schemas/testcase.schema.json => Format của Testcase Agent
+ ai/schemas/automation.schema.json => Format của Automation Agent

### F. qa/analysis/ - Output của AI
- Đây là nơi AI lưu kết quả.
+ qa/analysis/requirement-analysis/ => Output của Requirement Agent
+ qa/analysis/qna/ => Output của QA Agent
+ qa/analysis/scenarios/ => Output của QA Agent
+ qa/analysis/testcases/ => Output của Testcase Agent

### G. qa/traceability/ - Cầu nối giữa requirement và testcase
- Đây là nơi lưu kết quả.
- qa/traceability/requirement-testcase.json => Traceability giữa requirement và testcase

### H. automation/ — Playwright
- automation/tests/ => Test script
- automation/pages/ => Page Object Model
- automation/components/ => Page component
- automation/fixtures/ => Playwright fixture
automation/api/ => API test
- automation/utils/ => Utility functions
- automation/test-data/ => Test data

### I. reports/ 
- reports/playwright/ => Playwright report
- reports/ai-analysis/ => AI analysis report
- reports/coverage/ => Coverage report

### J. scripts/ 
- scripts/analyze-requirement.ts => Analyze requirement
- scripts/generate-testcase.ts => Generate testcase
- scripts/generate-automation.ts => Generate automation
- scripts/run-ai-qa.ts => Run ai qa

