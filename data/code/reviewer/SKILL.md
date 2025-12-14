---
name: skill-code-reviewer
description: Interactive code reviewer for pull requests and commits. Analyzes code across security, performance, correctness, maintainability, testability, and architecture dimensions. Provides severity-based findings with actionable fixes. Integrates with git skill for context and first-principles for logic verification.
allowed-tools: Read
---

# Code Reviewer - Interactive Code Quality Analysis

## Overview

Code Reviewer provides thorough, interactive code reviews for pull requests, commits, and manual code inspections. It analyzes code across six quality dimensions and delivers severity-based findings with specific, actionable recommendations.

This skill combines first-principles thinking for logic correctness, simplicity principles for complexity evaluation, git skill for code evolution analysis, and tech-stack-advisor for technology-specific best practices.

**Philosophy**: Focus on security and correctness first, then maintainability, then performance. Be constructive and educational.

## Requirements

**No external dependencies required**

Language-agnostic analysis applicable to any programming language or technology stack.

## Core Capabilities

**Multi-Dimensional Analysis**:

- Security: OWASP Top 10, CWE patterns, input validation, auth/authz
- Performance: Big O analysis, N+1 queries, memory leaks, network optimization
- Correctness: Logic errors, edge cases, type safety, error handling
- Maintainability: Code clarity, complexity, duplication, documentation
- Testability: Coverage gaps, test quality, design for testability
- Architecture: SOLID principles, design patterns, separation of concerns

**Severity-Based Findings**:

- 🔴 CRITICAL: Security vulnerabilities, data loss risks, production-breaking bugs
- 🟡 HIGH: Performance bottlenecks, significant logic errors, maintainability issues
- 🟠 MEDIUM: Minor issues, code duplication, missing tests
- 🟢 LOW: Style improvements, optimization opportunities

**Actionable Output**:

- Specific fixes with code examples
- Exploit scenarios for security issues
- Performance impact quantification
- Links to relevant documentation

## When to Use

**Use reviewer for**:

- Pull request reviews before merge
- Manual code spot-checks and validation
- Security vulnerability scans
- Performance bottleneck detection
- Pre-deployment code validation
- Legacy code assessment

**Don't use reviewer for**:

- Automated CI/CD enforcement (use code-guardian)
- Teaching best practices (use code-mentor)
- Comprehensive codebase audits (use code-auditor)

## Integration with Other Skills

**first-principles**: Logic verification from fundamental truths

- Question assumptions ("Why is this check necessary?")
- Edge case analysis ("What if input is null?")
- Root cause analysis for bugs

**simplicity**: Complexity evaluation

- Detect over-engineering and unnecessary abstractions
- Identify hidden complexity
- Assess coupling and cohesion

**git**: Code evolution analysis

- Identify hotspots (frequently changed files)
- Understand change context
- Check previous bug fixes in area

**tech-stack-advisor**: Technology-specific guidance

- Framework-specific patterns
- Library usage recommendations
- Language idioms and conventions

## Review Workflow

### 1. Context Gathering

**Understand the change**:

- What problem does this solve?
- What are the requirements?
- What is the scope?

**Gather context**:

- Read commit messages and PR description
- Check related issues/tickets
- Use git skill for file history

### 2. Initial Scan

**Quick pass**:

- Code structure and organization
- Obvious bugs or security issues
- Test coverage presence

**Red flags**:

- Hardcoded credentials/secrets
- SQL queries with string interpolation
- Missing error handling
- Very long functions (>100 lines)
- TODO/FIXME comments

### 3. Deep Analysis

**Security deep-dive**:

- Input validation (all user inputs)
- Output encoding (XSS prevention)
- Authentication and authorization checks
- Cryptographic operations
- Secret management

**Performance deep-dive**:

- Database queries (N+1, indexes, SELECT *)
- Algorithmic complexity (nested loops)
- Memory usage (leaks, large objects)
- Network calls (batching, caching)

**Logic deep-dive**:

- Edge cases (null, empty, zero, negative, large)
- Error paths (what if API fails?)
- State management (race conditions)
- Type safety

**Maintainability deep-dive**:

- Naming clarity
- Function complexity
- Code duplication
- Documentation completeness

### 4. Generate Findings

**Prioritize**:

1. Security (always critical)
2. Correctness (bugs)
3. Maintainability
4. Performance (unless severe)

**Make actionable**:

- Specific fix, not "improve this"
- Code examples
- Rationale for change

### 5. Constructive Delivery

**Positive framing**:

- Start with what's good
- Be specific about issues
- Suggest alternatives
- Focus on code, not person

**Educational value**:

- Explain "why" not just "what"
- Link to principles
- Reference best practices

## Review Output Format

```md
## Review Summary
- X critical issues
- Y high-priority issues
- Z medium issues
- N low-priority issues

## Critical Issues
1. [Issue Name] - [Location]
   - Issue: [Description]
   - Exploit/Impact: [What could go wrong]
   - Fix: [Recommended solution with code]

## High Priority Issues
[...]

## Medium/Low Issues
[...]

## Recommendations
- Priority action items
- Overall assessment
```

## Examples

### Example 1: Security Review

**Input**:

```python
def authenticate(username, password):
    user = db.execute(f"SELECT * FROM users WHERE name='{username}'")
    if user and user.password == password:
        return create_session(user)
    return None
```

**Output**:

```md
🔴 CRITICAL - Security Vulnerabilities (3):

1. SQL Injection (OWASP #3)
   Location: Line 2
   Issue: Direct string interpolation in SQL query
   Exploit: username="admin' OR '1'='1';--" bypasses auth
   Fix: Use parameterized queries

   db.execute("SELECT * FROM users WHERE name=?", (username,))

2. Plaintext Password Comparison (OWASP #2)
   Location: Line 3
   Issue: Passwords stored/compared in plaintext
   Fix: Use bcrypt/argon2 for password hashing

   if user and bcrypt.checkpw(password.encode(), user.password_hash):

3. No Rate Limiting (OWASP #7)
   Issue: Allows unlimited login attempts
   Fix: Implement rate limiting (5 attempts per 15 min)

🟡 HIGH - Logic Issues:
- No input validation (null/empty username/password)
- Missing logging for security events

Recommendation: DO NOT MERGE until Critical issues fixed.
```

### Example 2: Performance Review

**Input**:

```python
def get_dashboard_data(user_id):
    dashboard = Dashboard.query.get(user_id)
    widgets = []
    for widget_id in dashboard.widget_ids:
        widget = Widget.query.get(widget_id)  # N+1 query!
        widget.data = compute_data(widget)
        widgets.append(widget)
    return widgets
```

**Output**:

```md
🔴 CRITICAL - Performance Issues:

1. N+1 Query Problem
   Location: Lines 4-6
   Issue: Separate DB query for each widget (1 + N queries)
   Impact: For 20 widgets: 21 queries instead of 1-2
   Fix: Use eager loading

   dashboard = Dashboard.query.options(
       joinedload(Dashboard.widgets)
   ).get(user_id)

   Performance: 500ms → 50ms (10x faster)

🟠 MEDIUM - Optimization:
2. Synchronous computation in loop
   Fix: Use async/await for independent data

Recommendation: Fix N+1 before merge.
Current: ~700ms per request
After fix: ~70ms per request
```

### Example 3: Correctness Review

**Input**:

```python
def calculate_discount(price, discount_percent):
    discount = price * (discount_percent / 100)
    return price - discount
```

**Output**:

```md
🟡 HIGH - Logic Issues:

1. Missing Input Validation
   Edge cases not handled:
   - price < 0 (negative prices)
   - discount_percent > 100 (negative final price)
   - None values (TypeError)

   First-principles: What are valid ranges?

   Fix:
   def calculate_discount(price, discount_percent):
       if price is None or discount_percent is None:
           raise ValueError("Cannot be None")
       if price < 0:
           raise ValueError("Price cannot be negative")
       if not (0 <= discount_percent <= 100):
           raise ValueError("Discount must be 0-100")

       return price * (1 - discount_percent / 100)

🟠 MEDIUM:
- Missing docstring
- No type hints

Recommendation: Add validation before merge.
```

### Example 4: Maintainability Review

**Input**:

```python
class UserManagerServiceFactoryProvider:
    def __init__(self):
        self.cache = LRUCache()
        self.validator = ValidationService()
        self.repository = UserRepository()

    def get_user(self, id):
        # 50 lines of complex caching logic...

    def create_user(self, data):
        # 60 lines of validation and creation...

    # ... 20 more methods (800 lines total)
```

**Output**:

```md
🟡 HIGH - Maintainability Issues:

1. God Object Anti-Pattern
   Issue: Single class doing too much (22 methods, 800 lines)
   Violates: Single Responsibility Principle

   Fix: Split into focused classes:
   - UserCache (caching)
   - UserValidator (validation)
   - UserRepository (data access)
   - UserService (orchestration)

2. Over-Engineering
   Issue: "ManagerServiceFactoryProvider" - unnecessary layers
   Simplicity: "Simple is better than complex"

   Fix: Rename to "UserService" (clear, direct)

🟠 MEDIUM:
3. Long Methods
   - get_user(): 50 lines → extract smaller functions
   - create_user(): 60 lines → extract validation

Recommendation: Refactor before adding features.
```

## Integration Patterns

### With Git Skill

**Hotspot analysis**:

```md
User: "Review auth.py"
1. Check git history: "45 commits in 3 months"
2. Flag as hotspot requiring extra scrutiny
3. Recommend refactoring or additional tests
```

**Change context**:

```md
User: "Review this function"
1. Check git: "Changed 3 times this week"
2. Question: "Are we fixing same bug repeatedly?"
3. Suggest root cause analysis
```

### With First-Principles

**Logic verification**:

```md
Code: if user.role != 'admin': return False

First-principles questions:
- What are ALL possible role values?
- What if role is None?
- What if role is 'ADMIN' (uppercase)?
- Should empty role be allowed?

Recommendation: Use whitelist approach
```

### With Simplicity

**Complexity assessment**:

```md
User: "Review this caching implementation"

Simplicity analysis:
- 3 abstraction layers for simple key-value cache
- Cache logic mixed with business logic
- Implicit cache invalidation

Recommendation: Use Redis directly (proven, simple)
```

## Multi-Dimensional Review Checklist

**Security** (CRITICAL priority):

- [ ] No SQL injection (parameterized queries)
- [ ] No XSS (output encoding)
- [ ] Input validation on all user inputs
- [ ] Authentication and authorization checks
- [ ] No hardcoded secrets
- [ ] Crypto operations correct (bcrypt for passwords)
- [ ] Rate limiting on auth endpoints

**Performance** (HIGH for bottlenecks):

- [ ] No N+1 queries (use eager loading)
- [ ] Database indexes on query predicates
- [ ] O(n) or better algorithms (avoid O(n²))
- [ ] No memory leaks (resources released)
- [ ] Caching for expensive operations
- [ ] Batched network calls

**Correctness** (HIGH priority):

- [ ] Edge cases handled (null, empty, zero, negative)
- [ ] Error handling complete (try/catch, validation)
- [ ] No race conditions (proper locking)
- [ ] Type safety (type hints, validation)
- [ ] Off-by-one errors checked
- [ ] Test coverage > 80%

**Maintainability** (MEDIUM priority):

- [ ] Clear, descriptive names
- [ ] Functions < 50 lines
- [ ] Cyclomatic complexity < 10
- [ ] No code duplication (DRY)
- [ ] Documentation for complex logic
- [ ] Consistent code style

**Testability** (MEDIUM priority):

- [ ] Tests exist for new code
- [ ] Edge cases tested
- [ ] Error paths tested
- [ ] Dependency injection (not hardcoded)
- [ ] Pure functions where possible

**Architecture** (MEDIUM priority):

- [ ] SOLID principles followed
- [ ] Separation of concerns
- [ ] No god objects
- [ ] Appropriate design patterns
- [ ] No circular dependencies

## Best Practices

**Do**:

- ✅ Start with security and correctness
- ✅ Provide specific, actionable fixes
- ✅ Include code examples
- ✅ Explain rationale ("why", not just "what")
- ✅ Be constructive and kind
- ✅ Prioritize issues by severity
- ✅ Consider context (team expertise, timeline)

**Don't**:

- ❌ Focus only on style/formatting (use linters)
- ❌ Criticize without suggesting alternatives
- ❌ Block on minor issues
- ❌ Assume malicious intent
- ❌ Review without understanding context
- ❌ Miss security issues (always check)
- ❌ Over-engineer solutions

## Common Review Anti-Patterns

**Avoid**:

- Nitpicking style when linters exist
- Blocking PRs on personal preferences
- Long review cycles without feedback
- Reviewing entire codebase in one PR
- Missing the forest for the trees (focus on critical issues)
- Being negative without being helpful

**Instead**:

- Focus on critical issues first
- Provide timely, incremental feedback
- Suggest alternatives, not just problems
- Celebrate good code too
- Be educational, not punitive
