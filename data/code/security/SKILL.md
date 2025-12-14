---
name: skill-code-security
description: Security vulnerability assessment and prevention. Identifies OWASP Top 10 vulnerabilities, CWE patterns, and defensive coding issues. Provides exploit scenarios, prevention techniques, and security best practices. Ensures input validation, authentication, authorization, and cryptographic operations are implemented correctly.
allowed-tools: Read
---

# Code Security - Vulnerability Assessment and Prevention

## Overview

Code Security identifies security vulnerabilities, prevents exploits, and ensures defensive coding practices. It covers OWASP Top 10 vulnerabilities, common weakness enumeration (CWE) patterns, and security best practices applicable to all programming languages.

Security issues are always CRITICAL priority - a single vulnerability can compromise entire systems, expose sensitive data, or enable complete system takeover.

**Philosophy**: "Security is not optional" - build it in from the start, never bolt it on later.

## Requirements

No external dependencies required.

Recommends and configures security scanning tools (Bandit, Semgrep, SAST tools) based on technology stack.

## Core Capabilities

**OWASP Top 10 Coverage**:

- Broken access control (authorization failures)
- Cryptographic failures (weak crypto, plaintext storage)
- Injection (SQL, XSS, command injection)
- Insecure design (missing security controls)
- Security misconfiguration (defaults, verbose errors)
- Vulnerable components (outdated dependencies)
- Authentication failures (weak passwords, no MFA)
- Data integrity failures (insecure deserialization)
- Logging failures (insufficient monitoring)
- Server-side request forgery (SSRF, URL validation)

**Common Vulnerability Detection**:

- Input validation gaps (all user input)
- Output encoding issues (XSS prevention)
- Authentication and authorization flaws
- Cryptographic operation mistakes
- Secret management problems
- Rate limiting absence

**Exploit Scenario Analysis**:

- How vulnerability can be exploited
- What attacker can achieve
- Business and data impact
- Compliance implications

**Prevention Techniques**:

- Parameterized queries (SQL injection)
- Output encoding (XSS prevention)
- Proper authentication (bcrypt, rate limiting)
- Access control verification
- Security headers configuration

## When to Use

**Use security for**:

- Security code reviews (always check)
- Vulnerability scanning and assessment
- Authentication and authorization review
- Cryptographic operation verification
- Input validation auditing
- Pre-deployment security checks
- Compliance requirement verification

**Don't use security for**:

- General code quality issues (use code-reviewer)
- Performance optimization (use code-performance)
- Teaching security concepts (use code-mentor)

## Integration with Other Skills

**first-principles**: Threat modeling

- Question: "What could an attacker do with this?"
- Think like an attacker systematically
- Verify security from fundamentals
- Example: "If user controls this input, what's the worst case?"

**tech-stack-advisor**: Framework security

- Use framework security features correctly
- Keep dependencies updated
- Choose secure defaults
- Example: "Django has built-in CSRF protection - ensure it's enabled"

**code-reviewer**: Security reviews

- Security review on every PR
- Critical issues block merge
- Second pair of eyes for security
- Example: "All code changes checked for OWASP Top 10"

**code-guardian**: Automated scanning

- SAST tools in CI/CD
- Dependency vulnerability scanning
- Secret detection in commits
- Example: "Bandit scans Python code for security issues automatically"

## OWASP Top 10 Vulnerabilities

### 1. Broken Access Control

**Issue**: Users can access resources they shouldn't.

**Examples**:

```python
# Bad: No authorization check
@app.route('/user/<user_id>/data')
def get_user_data(user_id):
    return User.query.get(user_id).data

# Good: Verify ownership
@app.route('/user/<user_id>/data')
@login_required
def get_user_data(user_id):
    if current_user.id != user_id and not current_user.is_admin:
        abort(403)
    return User.query.get(user_id).data
```

**Prevention**:

- Check permissions on every request
- Principle of least privilege
- No elevation without verification

### 2. Cryptographic Failures

**Issue**: Sensitive data exposed due to weak cryptography.

**Examples**:

```python
# Bad: Plaintext password
user.password = request.form['password']

# Good: Hashed with bcrypt
import bcrypt
password_hash = bcrypt.hashpw(
    request.form['password'].encode(),
    bcrypt.gensalt(rounds=12)
)
user.password_hash = password_hash
```

**Prevention**:

- Hash passwords with bcrypt or argon2
- Use HTTPS everywhere
- No hardcoded encryption keys
- Never use MD5/SHA1 for passwords

### 3. Injection (SQL, XSS, Command)

**SQL Injection**:

```python
# Bad: String interpolation
query = f"SELECT * FROM users WHERE name='{username}'"
# Exploit: username = "'; DROP TABLE users; --"

# Good: Parameterized query
query = "SELECT * FROM users WHERE name=?"
cursor.execute(query, (username,))
```

**XSS Prevention**:

```python
# Bad: Unescaped user input
return f"<h1>Welcome {request.args.get('name')}</h1>"
# Exploit: name = "<script>steal_cookies()</script>"

# Good: Template with auto-escaping
return render_template('welcome.html', name=request.args.get('name'))
```

**Prevention**:

- Always use parameterized queries
- Template engines with auto-escaping
- Validate and sanitize all input

### 4. Insecure Design

**Issue**: Missing security controls in design.

**Prevention**:

- Threat modeling during design
- Rate limiting on authentication
- Security requirements from start
- Defense in depth

**Example**:

```python
# Add rate limiting
from flask_limiter import Limiter

limiter = Limiter(app, key_func=lambda: request.remote_addr)

@limiter.limit("5 per 15 minutes")
@app.route('/login', methods=['POST'])
def login():
    # Prevents brute force attacks
    ...
```

### 5. Security Misconfiguration

**Issue**: Insecure default settings, verbose errors.

**Examples**:

```python
# Good: Security headers
@app.after_request
def add_security_headers(response):
    response.headers['X-Content-Type-Options'] = 'nosniff'
    response.headers['X-Frame-Options'] = 'DENY'
    response.headers['X-XSS-Protection'] = '1; mode=block'
    response.headers['Strict-Transport-Security'] = 'max-age=31536000'
    return response

# Good: Non-verbose errors in production
app.config['DEBUG'] = False
```

**Prevention**:

- Disable debug mode in production
- Set security headers
- Remove default credentials
- Disable unnecessary features

### 6. Vulnerable Components

**Issue**: Using libraries with known vulnerabilities.

**Prevention**:

```bash
# Check for vulnerabilities
npm audit
pip-audit
snyk test

# Auto-update dependencies
# Use Dependabot or Renovate
```

### 7. Authentication Failures

**Issue**: Weak authentication mechanisms.

**Examples**:

```python
# Password requirements
MIN_PASSWORD_LENGTH = 12
REQUIRE_COMPLEXITY = True

# Secure session configuration
app.config['SESSION_COOKIE_SECURE'] = True      # HTTPS only
app.config['SESSION_COOKIE_HTTPONLY'] = True    # No JS access
app.config['SESSION_COOKIE_SAMESITE'] = 'Lax'  # CSRF protection
```

**Prevention**:

- Strong password requirements (min 12 chars)
- Rate limiting on login
- MFA for sensitive accounts
- Secure session management

### 8. Data Integrity Failures

**Issue**: Insecure deserialization, no integrity checks.

**Examples**:

```python
# Bad: Unsafe deserialization
import pickle
data = pickle.loads(user_input)  # Code execution risk

# Good: Safe deserialization
import json
data = json.loads(user_input)  # Data only, no code
```

### 9. Logging and Monitoring Failures

**Issue**: Insufficient logging of security events.

**Examples**:

```python
# Log security events
import logging
security_logger = logging.getLogger('security')

@app.route('/login', methods=['POST'])
def login():
    if not authenticate(username, password):
        security_logger.warning(
            f"Failed login: {username} from {request.remote_addr}"
        )
        return "Invalid credentials", 401
```

**Prevention**:

- Log all security events
- Monitor for suspicious patterns
- Alert on security failures
- Never log sensitive data (passwords, tokens)

### 10. Server-Side Request Forgery (SSRF)

**Issue**: Application fetches URLs without validation.

**Examples**:

```python
# Bad: Unvalidated URL
url = request.args.get('url')
response = requests.get(url)  # Can access internal network

# Good: Whitelist domains
ALLOWED_DOMAINS = ['api.example.com', 'cdn.example.com']

def is_safe_url(url):
    parsed = urlparse(url)
    return parsed.netloc in ALLOWED_DOMAINS

if is_safe_url(url):
    response = requests.get(url)
```

## Security Checklist

**Authentication**:

- Strong password requirements (min 12 chars)
- Passwords hashed with bcrypt/argon2
- Rate limiting on login (5 attempts / 15 min)
- Session tokens cryptographically random
- HTTPS-only, HttpOnly, SameSite cookies
- MFA available for sensitive accounts

**Authorization**:

- Check permissions on every request
- Principle of least privilege
- No elevation without verification
- Admin features require admin role

**Data Protection**:

- HTTPS everywhere (no HTTP)
- Sensitive data encrypted at rest
- No secrets in code or version control
- PII handling compliant (GDPR, CCPA)

**Input Validation**:

- All user input validated (type, length, format)
- SQL queries parameterized (no string interpolation)
- Output escaped (XSS prevention)
- File upload restrictions (type, size)

**Security Headers**:

- Content-Security-Policy configured
- X-Content-Type-Options: nosniff
- X-Frame-Options: DENY
- Strict-Transport-Security enabled

**Logging and Monitoring**:

- Security events logged
- Sensitive data not logged (passwords, tokens)
- Logs monitored and alerted
- Log retention policy defined

## Common Vulnerability Patterns

**Secret Detection**:

```bash
# Check for hardcoded secrets
detect-secrets scan
git-secrets --scan

# What to find:
# - API keys in code
# - Database credentials
# - Private keys
# - OAuth tokens
```

**Input Validation**:

```python
from email_validator import validate_email

def register_user(email, age, role):
    # Validate email format
    validate_email(email)

    # Validate age range
    if not isinstance(age, int) or not (18 <= age <= 150):
        raise ValueError("Invalid age")

    # Validate role (whitelist)
    if role not in ['user', 'moderator']:
        raise ValueError("Invalid role")
```

**CSRF Protection**:

```python
# Use CSRF tokens
from flask_wtf.csrf import CSRFProtect
csrf = CSRFProtect(app)

# Or same-site cookies
app.config['SESSION_COOKIE_SAMESITE'] = 'Strict'
```

## Security Testing Tools

**Static Analysis (SAST)**:

- Bandit (Python security linter)
- ESLint security plugins (JavaScript)
- Semgrep (multi-language patterns)
- SonarQube (comprehensive analysis)

**Dependency Scanning**:

- npm audit (JavaScript)
- pip-audit (Python)
- Snyk (multi-language)
- Dependabot (automated updates)

**Dynamic Analysis (DAST)**:

- OWASP ZAP (web app scanner)
- Burp Suite (penetration testing)
- Manual penetration testing

## Examples

### Example 1: SQL Injection Fix

**Vulnerable Code**:

```python
def get_user(username):
    query = f"SELECT * FROM users WHERE name='{username}'"
    return db.execute(query)
```

**Exploit**: `username = "admin' OR '1'='1';--"` bypasses authentication

**Fixed Code**:

```python
def get_user(username):
    query = "SELECT * FROM users WHERE name=?"
    return db.execute(query, (username,))
```

### Example 2: Authentication Security

**Vulnerable Code**:

```python
def login(username, password):
    user = get_user(username)
    if user and user.password == password:
        return create_session(user)
```

**Issues**: Plaintext passwords, no rate limiting, timing attacks

**Fixed Code**:

```python
import bcrypt
from flask_limiter import Limiter

limiter = Limiter(app, key_func=lambda: request.remote_addr)

@limiter.limit("5 per 15 minutes")
def login(username, password):
    user = get_user(username)
    if not user:
        # Constant-time comparison
        bcrypt.checkpw(b"dummy", bcrypt.gensalt())
        return None

    if bcrypt.checkpw(password.encode(), user.password_hash):
        return create_session(user)
    return None
```

## Best Practices

**Do**:

- Validate all user input
- Use parameterized queries
- Hash passwords with bcrypt/argon2
- Implement rate limiting
- Log security events
- Keep dependencies updated
- Use HTTPS everywhere
- Apply principle of least privilege
- Test for OWASP Top 10
- Use security headers

**Don't**:

- Trust user input ever
- Roll your own crypto
- Store passwords in plaintext
- Use MD5/SHA1 for passwords
- Expose sensitive data in errors
- Ignore dependency vulnerabilities
- Give everyone admin access
- Skip input validation
- Disable security features for convenience

## See Also

Related code quality skills:

- code-reviewer: Security code reviews
- code-guardian: Automated security scanning
- code-auditor: Security audits
- code-mentor: Teaching security concepts

Related foundational skills:

- first-principles: Threat modeling
- tech-stack-advisor: Framework security features
