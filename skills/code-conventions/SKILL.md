---
name: code-conventions
description: "코드 컨벤션, 코딩 스타일, 코드 스타일, 네이밍, 컨벤션, 타입스크립트, 파이썬, 자바, 함수 크기, 파일 크기 - Always apply when writing code. Code style, naming rules, function/file size limits for TypeScript, Python, and Java."
---

# Code Conventions - Code Style Persona

## Role

Guide **how to write code**:
- Maintain consistent code style
- Write readable code
- Create maintainable structure

## General Principles

### Early Return Pattern

Prefer early returns over nested conditions for better readability.

### Size Limits

| Target | Limit | When Exceeded |
|--------|-------|---------------|
| Function/Method | 50 lines | Split |
| File/Class | 200 lines | Split into modules |
| Nesting depth | 3 levels | Apply early return |

---

## TypeScript Conventions

### Naming

| Type | Pattern | Example |
|------|---------|---------|
| Class/Interface/Type | PascalCase | `OrderService`, `UserEntity` |
| Function/Variable | camelCase | `calculateTotal`, `userName` |
| Constant | SCREAMING_SNAKE | `MAX_RETRY_COUNT` |
| File | kebab-case or camelCase | `order-service.ts` |
| Boolean | is/has/can prefix | `isActive`, `hasPermission` |

### Best Practices

- Use arrow functions when possible
- Prefer `const` over `let`, avoid `var`
- Always declare explicit types
- Use `interface` for object shapes, `type` for unions/intersections
- Prefer `unknown` over `any`

### Avoid

```
❌ utils.ts, helpers.ts, common.ts, shared.ts
```

---

## Python Conventions (PEP-8)

### Naming

| Type | Pattern | Example |
|------|---------|---------|
| Class | PascalCase | `OrderService`, `UserEntity` |
| Function/Variable | snake_case | `calculate_total`, `user_name` |
| Constant | SCREAMING_SNAKE | `MAX_RETRY_COUNT` |
| Module/File | snake_case | `order_service.py` |
| Private | leading underscore | `_internal_method` |

### Best Practices

- Maximum line length: 79 characters (code), 72 (docstrings)
- Use 4 spaces for indentation (no tabs)
- Two blank lines between top-level definitions
- One blank line between method definitions
- Use type hints (PEP 484)
- Use docstrings for public modules, functions, classes, methods

### Import Order (PEP-8)

```python
# 1. Standard library
import os
import sys

# 2. Third-party
import requests
import numpy as np

# 3. Local application
from myapp import utils
```

### Avoid

```
❌ utils.py, helpers.py, common.py, shared.py
```

---

## Java Conventions

### Naming

| Type | Pattern | Example |
|------|---------|---------|
| Class/Interface | PascalCase | `OrderService`, `UserEntity` |
| Method/Variable | camelCase | `calculateTotal`, `userName` |
| Constant | SCREAMING_SNAKE | `MAX_RETRY_COUNT` |
| Package | lowercase | `com.example.order` |
| File | Same as class name | `OrderService.java` |

### Best Practices

- One public class per file
- Use `final` for immutable variables
- Prefer composition over inheritance
- Use interfaces for abstraction
- Follow JavaDoc conventions for documentation
- Use Optional instead of null for return values

### Avoid

```
❌ Utils.java, Helpers.java, Common.java, Shared.java
```

---

## Checklist

### When Writing Code

- [ ] Is function/method under 50 lines?
- [ ] Is file/class under 200 lines?
- [ ] Is nesting under 3 levels?
- [ ] Is early return applied?
- [ ] Is naming following language conventions?
- [ ] Are types/type hints declared?

### Before PR

- [ ] Consistent code style maintained
- [ ] Unnecessary comments removed
- [ ] Error handling appropriate
- [ ] Linter/formatter passed

## Anti-Patterns

- **Magic Numbers**: Using meaningless numbers directly
- **God Function/Class**: Giant function/class that does everything
- **Callback Hell / Nested Conditionals**: Deep nesting
- **Copy-Paste Code**: Duplicated code
- **Generic Naming**: utils, helpers, common, shared, misc
