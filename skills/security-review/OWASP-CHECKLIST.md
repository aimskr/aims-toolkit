# OWASP Top 10 Checklist & Security Patterns

## OWASP Top 10 (2021) Checklist

```
□ A01 - Broken Access Control
  - Horizontal privilege escalation (accessing other users' data)
  - Vertical privilege escalation (accessing admin functions)
  - CORS configuration verification

□ A02 - Cryptographic Failures
  - Plaintext transmission (HTTP vs HTTPS)
  - Weak encryption algorithms
  - Hardcoded keys/secrets

□ A03 - Injection
  - SQL Injection
  - XSS (Stored, Reflected, DOM)
  - Command Injection
  - LDAP/NoSQL Injection

□ A04 - Insecure Design
  - Threat modeling performed
  - Security requirements defined

□ A05 - Security Misconfiguration
  - Default accounts/passwords
  - Unnecessary features enabled
  - Verbose error messages exposed

□ A06 - Vulnerable Components
  - Outdated dependencies
  - Known vulnerabilities (CVE)

□ A07 - Authentication Failures
  - Weak password policy
  - Brute force protection
  - Session management

□ A08 - Data Integrity Failures
  - Signature verification
  - Untrusted deserialization

□ A09 - Logging Failures
  - Security event logging
  - Sensitive data in logs

□ A10 - SSRF
  - Server-side URL request validation
```

---

## Vulnerable Code Pattern Detection

### SQL Injection
```python
# ❌ Vulnerable
query = f"SELECT * FROM users WHERE id = {user_id}"

# ✅ Safe
query = "SELECT * FROM users WHERE id = ?"
cursor.execute(query, (user_id,))
```

### XSS
```javascript
// ❌ Vulnerable
element.innerHTML = user_input

// ✅ Safe
element.textContent = user_input
```

### Command Injection
```python
# ❌ Vulnerable
os.system(f"ls {user_path}")

# ✅ Safe
subprocess.run(["ls", user_path])
```

### Hardcoded Secrets
```python
# ❌ Vulnerable
API_KEY = "sk-1234567890"

# ✅ Safe
API_KEY = os.environ.get("API_KEY")
```

---

## Search Query Examples

```bash
# SQL Injection potential
grep -rn "f\".*SELECT.*{" --include="*.py"
grep -rn "query.*\+" --include="*.js"

# Hardcoded secrets
grep -rn "password.*=" --include="*.py"
grep -rn "api_key.*=" --include="*.js"

# Dangerous functions
grep -rn "eval\|exec\|system" --include="*.py"
grep -rn "innerHTML\|document.write" --include="*.js"
```

---

## Severity Classification

| Level | Description | Response Time |
|-------|-------------|---------------|
| 🔴 Critical | Immediately exploitable, severe impact | Within 24 hours |
| 🟠 High | Exploitable, significant impact | Within 1 week |
| 🟡 Medium | Conditional exploit, limited impact | Within 1 month |
| 🟢 Low | Hard to exploit, minimal impact | Next release |

---

## Vulnerability Report Format

```markdown
### 🔴 [Vulnerability ID]: [Vulnerability Name]

**Location:** `src/path/file.py:45`

**Description:**
[Description of the vulnerability]

**Impact:**
- [Impact 1]
- [Impact 2]

**PoC (Proof of Concept):**
```
[Attack example]
```

**Remediation:**
```python
# Before (Vulnerable)
[Vulnerable code]

# After (Safe)
[Fixed code]
```

**Reference:** OWASP A0X:2021 - [Category Name]
```

---

## Security Design Guide

### Authentication Design
```yaml
Authentication:
  method: JWT
  access_token:
    expiry: 15m
    storage: memory
  refresh_token:
    expiry: 7d
    storage: httpOnly cookie
  password:
    algorithm: bcrypt
    cost: 12
    min_length: 12
```

### Authorization Design
```yaml
Authorization:
  model: RBAC
  roles:
    - admin: ["*"]
    - user: ["read:own", "write:own"]
    - guest: ["read:public"]
  enforcement: middleware
```

---

## Security Review Report Template

```markdown
# Security Review Report: [Target]

## Summary
- Review Date:
- Review Scope:
- Findings: 🔴 X | 🟠 X | 🟡 X | 🟢 X

## OWASP Top 10 Check
| Item | Status | Notes |
|------|--------|-------|
| A01 Access Control | ✅/⚠️/❌ | |
| A02 Cryptography | ✅/⚠️/❌ | |
| A03 Injection | ✅/⚠️/❌ | |
| A04 Insecure Design | ✅/⚠️/❌ | |
| A05 Misconfiguration | ✅/⚠️/❌ | |
| A06 Vulnerable Components | ✅/⚠️/❌ | |
| A07 Authentication | ✅/⚠️/❌ | |
| A08 Data Integrity | ✅/⚠️/❌ | |
| A09 Logging | ✅/⚠️/❌ | |
| A10 SSRF | ✅/⚠️/❌ | |

## Discovered Vulnerabilities
[Details]

## Recommendations
[Prioritized action items]
```
