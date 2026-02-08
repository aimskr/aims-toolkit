---
name: e2e-runner
description: "E2E 테스트, Playwright, 엔드투엔드, 통합 테스트, UI 테스트 - End-to-end testing workflow using Playwright. Generates, runs, and maintains E2E tests for web applications."
tools: Read, Write, Edit, Bash, Grep, Glob
model: opus
---

# E2E Runner

Playwright를 사용한 엔드투엔드 테스트 워크플로우입니다.

## 핵심 기능

1. **테스트 생성**: 사용자 시나리오 기반 테스트 자동 생성
2. **테스트 실행**: 다양한 브라우저에서 테스트 실행
3. **결과 분석**: 실패 원인 분석 및 수정 제안
4. **유지보수**: 깨진 테스트 자동 수정

## 테스트 시나리오 템플릿

### 기본 구조
```typescript
import { test, expect } from '@playwright/test';

test.describe('Feature: [기능명]', () => {
  test.beforeEach(async ({ page }) => {
    // 공통 설정
    await page.goto('/');
  });

  test('should [expected behavior]', async ({ page }) => {
    // Arrange
    // Act
    // Assert
  });
});
```

### 사용자 시나리오 예시
```typescript
test.describe('User Authentication', () => {
  test('should login successfully with valid credentials', async ({ page }) => {
    // Navigate to login page
    await page.goto('/login');
    
    // Fill in credentials
    await page.fill('[data-testid="email"]', 'user@example.com');
    await page.fill('[data-testid="password"]', 'password123');
    
    // Submit form
    await page.click('[data-testid="login-button"]');
    
    // Verify successful login
    await expect(page).toHaveURL('/dashboard');
    await expect(page.locator('[data-testid="welcome-message"]')).toBeVisible();
  });

  test('should show error for invalid credentials', async ({ page }) => {
    await page.goto('/login');
    await page.fill('[data-testid="email"]', 'invalid@example.com');
    await page.fill('[data-testid="password"]', 'wrongpassword');
    await page.click('[data-testid="login-button"]');
    
    await expect(page.locator('[data-testid="error-message"]')).toContainText('Invalid credentials');
  });
});
```

## 테스트 생성 프로세스

### Phase 1: 시나리오 정의
```
사용자 스토리 → 테스트 시나리오 변환

예:
"사용자가 로그인할 수 있다"
→ 
1. 유효한 자격 증명으로 로그인 성공
2. 잘못된 비밀번호로 로그인 실패
3. 존재하지 않는 이메일로 로그인 실패
4. 빈 필드로 로그인 시도
```

### Phase 2: 선택자 식별
```
우선순위:
1. data-testid (권장)
2. role + name
3. text content
4. CSS selector (최후 수단)
```

### Phase 3: 테스트 작성
```
AAA 패턴:
- Arrange: 테스트 환경 설정
- Act: 사용자 액션 수행
- Assert: 결과 검증
```

## Playwright 설정

### playwright.config.ts
```typescript
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './e2e',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: [
    ['html'],
    ['json', { outputFile: 'test-results.json' }]
  ],
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
    {
      name: 'Mobile Chrome',
      use: { ...devices['Pixel 5'] },
    },
  ],
  webServer: {
    command: 'npm run dev',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
  },
});
```

## 테스트 실행 명령어

### 기본 실행
```bash
# 모든 테스트 실행
npx playwright test

# 특정 파일 실행
npx playwright test e2e/login.spec.ts

# 특정 브라우저
npx playwright test --project=chromium

# UI 모드
npx playwright test --ui

# 헤드리스 모드 비활성화
npx playwright test --headed
```

### 디버깅
```bash
# 디버그 모드
npx playwright test --debug

# 트레이스 뷰어
npx playwright show-trace trace.zip
```

### 리포트
```bash
# HTML 리포트 열기
npx playwright show-report
```

## 자주 사용하는 패턴

### 페이지 네비게이션
```typescript
await page.goto('/dashboard');
await page.waitForURL('**/dashboard');
```

### 요소 대기
```typescript
await page.waitForSelector('[data-testid="loading"]', { state: 'hidden' });
await expect(page.locator('[data-testid="content"]')).toBeVisible();
```

### 폼 입력
```typescript
await page.fill('input[name="email"]', 'test@example.com');
await page.selectOption('select[name="country"]', 'KR');
await page.check('input[type="checkbox"]');
```

### API 모킹
```typescript
await page.route('**/api/users', async route => {
  await route.fulfill({
    status: 200,
    body: JSON.stringify([{ id: 1, name: 'Test User' }]),
  });
});
```

### 스크린샷
```typescript
await page.screenshot({ path: 'screenshot.png', fullPage: true });
```

## 테스트 유지보수

### 깨진 테스트 수정
```
1. 실패 원인 분석 (스크린샷, 트레이스)
2. 선택자 변경 확인
3. 타이밍 이슈 확인
4. API 응답 변경 확인
5. 테스트 수정 또는 기능 버그 보고
```

### Flaky 테스트 처리
```typescript
// 재시도 추가
test('flaky test', async ({ page }) => {
  test.retry(2);
  // ...
});

// 명시적 대기 추가
await page.waitForLoadState('networkidle');
```

## 사용 방법

### 테스트 생성 요청
```
/e2e

또는

로그인 기능에 대한 E2E 테스트를 생성해주세요.
```

### 테스트 실행 및 분석
```
E2E 테스트를 실행하고 실패한 테스트를 분석해주세요.
```

### 깨진 테스트 수정
```
이 E2E 테스트가 실패합니다. 수정해주세요.
[테스트 코드]
[에러 메시지]
```

## 테스트 구조 예시

```
e2e/
├── fixtures/
│   ├── test-data.json
│   └── auth.setup.ts
├── pages/
│   ├── login.page.ts
│   └── dashboard.page.ts
├── tests/
│   ├── auth/
│   │   ├── login.spec.ts
│   │   └── logout.spec.ts
│   ├── dashboard/
│   │   └── overview.spec.ts
│   └── settings/
│       └── profile.spec.ts
└── playwright.config.ts
```

## Page Object 패턴

```typescript
// pages/login.page.ts
import { Page, Locator } from '@playwright/test';

export class LoginPage {
  readonly page: Page;
  readonly emailInput: Locator;
  readonly passwordInput: Locator;
  readonly loginButton: Locator;
  readonly errorMessage: Locator;

  constructor(page: Page) {
    this.page = page;
    this.emailInput = page.locator('[data-testid="email"]');
    this.passwordInput = page.locator('[data-testid="password"]');
    this.loginButton = page.locator('[data-testid="login-button"]');
    this.errorMessage = page.locator('[data-testid="error-message"]');
  }

  async goto() {
    await this.page.goto('/login');
  }

  async login(email: string, password: string) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.loginButton.click();
  }
}
```
