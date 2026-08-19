# Command line

## Command Line (CLI) là gì

Playwright cung cấp command-line interface để thực hiện các công việc chính:

- Chạy test `npx playwright test`
- Chạy một test cụ thể `npx playwright test tests/example.spec.ts`
- Chạy test theo browser `npx playwright test --browser=chrome`
- Debug test `npx playwright test --debug`
- Chạy UI Mode `npx playwright test --ui`
- Generate test bằng Codegen `npx playwright codegen`
- Xem HTML Report `npx playwright show-report`
- Xem Trace `npx playwright show-trace`
- Install browser `npx playwright install`
- Chạy lại test fail `npx playwright test --retries=2`
- Chạy test song song `npx playwright test --workers=2`
- Retry test `npx playwright test --retries=2`
- Sharding test `npx playwright test --workers=2`
- Update snapshot `npx playwright test --update-snapshots`

Cú pháp chung: `npx playwright <command> [options] <specFiles...>`
