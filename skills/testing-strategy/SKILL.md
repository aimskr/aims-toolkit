---
name: testing-strategy
description: "테스트, 테스팅, QA, 테스트 전략, 품질 보증 - Use when designing test strategies, writing test plans, or ensuring quality assurance. Guides systematic testing approach from unit to E2E with proper coverage and scenario design."
---

# Testing Strategy Skill

Provides a systematic workflow for test strategy planning and quality assurance.

## When to Use

- Designing test strategy for new features
- Improving test coverage
- Creating QA checklists
- Designing test scenarios

## The Process

### Phase 1: Test Scope Analysis

**Current State Assessment:**
1. Analyze existing test code structure (`tests/`, `__tests__/`, `*.spec.*`)
2. Check current coverage
3. Review test stages in CI pipeline

**Scope Definition:**
- In Scope: What to test this time
- Out of Scope: What to exclude and why
- Dependencies: External dependencies requiring mocks/stubs

### Phase 2: Test Pyramid Design

```
Ratio Guide:

┌─────────────────────────────────────┐
│  E2E (5-10%)                        │
│  - Critical User Journeys only      │
│  - Execution: Slow                  │
│  - Maintenance: Difficult           │
├─────────────────────────────────────┤
│  Integration (15-25%)               │
│  - API boundaries, DB integration   │
│  - Module interactions              │
├─────────────────────────────────────┤
│  Unit (70-80%)                      │
│  - Pure functions, business logic   │
│  - Fast feedback                    │
│  - Isolated tests                   │
└─────────────────────────────────────┘
```

### Phase 3: Test Scenario Writing

**Scenario Categories:**

| Type | Description | Example |
|------|-------------|---------|
| Happy Path | Normal flow | Valid input → Success |
| Edge Cases | Boundary conditions | Empty array, max value, null |
| Error Cases | Error handling | Invalid input, network error |
| Security | Security scenarios | Unauthenticated access, permission overflow |

**Boundary Value Analysis (BVA):**
```
Input range: 1-100

Test values:
- 0 (lower -1) → Error
- 1 (lower bound) → Success
- 50 (middle) → Success
- 100 (upper bound) → Success
- 101 (upper +1) → Error
```

### Phase 4: Test Code Writing Guide

**AAA Pattern:**
```
def test_feature_condition_expectedResult():
    # Arrange - Setup
    user = User(name="test")

    # Act - Execute
    result = user.greet()

    # Assert - Verify
    assert result == "Hello, test"
```

**FIRST Principles:**
- **F**ast: Execute quickly
- **I**ndependent: No dependency on other tests
- **R**epeatable: Always same result
- **S**elf-validating: Clear success/failure
- **T**imely: Written alongside code

### Phase 5: Test Execution and Verification

**Coverage Targets:**
```
Target Setting Guide:

- Line Coverage: 80%+ (minimum baseline)
- Branch Coverage: 75%+ (conditional verification)
- Function Coverage: 90%+ (prevent function omission)

Note: Coverage is a means, not an end
High coverage ≠ Good tests
```

**Flaky Test Handling:**
1. Identify reproduction conditions
2. Timing issues → Explicit waits
3. Order dependency → Strengthen isolation
4. External dependency → Mock handling

## Output Templates

### Test Strategy Document
```markdown
# Test Strategy: [Feature Name]

## Test Scope
- **In Scope:**
- **Out of Scope:**

## Test Pyramid
| Type | Ratio | Target | Tools |
|------|-------|--------|-------|
| Unit | 70% | Business logic | Jest/Pytest/JUnit |
| Integration | 20% | API, DB | Supertest/pytest |
| E2E | 10% | Critical flows | Playwright/Cypress |

## Test Scenarios
### Normal Cases
| ID | Scenario | Input | Expected Result |
|----|----------|-------|-----------------|

### Exception Cases
| ID | Scenario | Input | Expected Result |
|----|----------|-------|-----------------|

## Coverage Targets
- Line: 80%
- Branch: 75%

## Test Environment
- Local: Docker Compose
- CI: GitHub Actions
```

## Key Principles

1. **Test First (TDD)**: Write tests before code when possible
2. **Isolation**: Each test must run independently
3. **Clear Failure**: Cause should be immediately apparent on failure
4. **Maintainability**: Manage test code like production code
5. **Appropriate Level**: Meaningful tests over 100% coverage
