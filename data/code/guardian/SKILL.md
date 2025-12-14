---
name: skill-code-guardian
description: Automated code quality enforcer. Prevents bad code through quality gates, linting rules, static analysis, and CI/CD integration. Sets up pre-commit hooks, automated security scanning, and coding standards enforcement. Language-agnostic tool configuration and quality gate design.
allowed-tools: Read
---

# Code Guardian - Prevention and Standards Enforcement

## Overview

Code Guardian prevents bad code from entering the codebase through automated quality gates, linting rules, static analysis, and CI/CD integration. It enforces standards proactively rather than finding issues after the fact.

**Philosophy**: "Prevention is better than cure" - catch issues before they reach code review.

Language-agnostic guidance for setting up automated quality enforcement across any technology stack.

## Requirements

**No external dependencies required**

Configures existing tools (linters, static analyzers, CI/CD platforms) based on technology stack.

## Core Capabilities

**Quality Gates**:

- Security gates (dependency scanning, secret detection, SAST)
- Code quality gates (linting, complexity, coverage minimums)
- Performance gates (bundle size, build time limits)
- Testing gates (all tests pass, no flaky tests)

**Linting and Static Analysis**:

- Configure language-specific linters (ESLint, Pylint, etc.)
- Set up formatters (Prettier, Black, gofmt)
- Enable type checking (TypeScript, MyPy, Flow)
- Multi-language tools (SonarQube, CodeQL, Semgrep)

**Pre-Commit Hooks**:

- Secret detection before commit
- Fast linting and formatting
- Type checking
- Quick unit tests (< 30 seconds)

**CI/CD Integration**:

- GitHub Actions, GitLab CI, Jenkins configuration
- Three-stage validation (pre-commit, PR, pre-deploy)
- Automated security scanning
- Test coverage enforcement

## When to Use

**Use guardian for**:

- Setting up quality gates for CI/CD pipelines
- Configuring pre-commit hooks for teams
- Establishing linting and formatting rules
- Implementing automated security scanning
- Preventing merges of problematic code
- Enforcing team coding standards

**Don't use guardian for**:

- Manual code reviews (use code-reviewer)
- Teaching developers best practices (use code-mentor)
- One-time codebase audits (use code-auditor)

## Integration with Other Skills

**tech-stack-advisor**: Technology-specific tool recommendations

- Python: Black, Flake8, MyPy, Bandit
- JavaScript: ESLint, Prettier, TypeScript
- Go: gofmt, golint, go vet
- Java: Checkstyle, PMD, SpotBugs

**simplicity**: Avoid over-engineering quality gates

- Start with essential checks (security, tests)
- Add complexity gates incrementally
- Don't block on minor style issues

**git**: Integrate with git workflows

- Pre-commit hooks for local validation
- PR checks before merge
- Branch protection rules

**code-security**: Security-specific gates

- SAST tools (Bandit, ESLint security, Semgrep)
- Dependency vulnerability scanning
- Secret detection

## Three-Stage Quality Enforcement

### Stage 1: Pre-Commit (Local, < 30s)

**Fast checks before commit**:

- Linting and formatting
- Secret detection
- Type checking
- Fast unit tests (changed files only)

**Example Configuration**:

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/psf/black
    rev: 23.0.0
    hooks:
      - id: black

  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets

  - repo: local
    hooks:
      - id: lint
        name: Run linter
        entry: npm run lint
        language: system
```

### Stage 2: Pull Request (CI, < 10 min)

**Thorough checks before merge**:

- Full test suite
- Code coverage (> 80%)
- Security scanning (SAST, dependency audit)
- Complexity analysis
- Integration tests

**GitHub Actions Example**:

```yaml
name: PR Quality Gates
on: [pull_request]
jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Lint
        run: npm run lint

      - name: Type Check
        run: npm run type-check

      - name: Test with Coverage
        run: npm test -- --coverage --coverageThreshold='{"global":{"lines":80}}'

      - name: Security Scan
        run: npm audit

      - name: Dependency Check
        uses: snyk/actions/node@master
```

### Stage 3: Pre-Deploy (Production Gates)

**Final validation before production**:

- All PR checks passed
- Integration tests against staging
- Performance benchmarks
- Security scan (production dependencies)
- Manual approval (if required)

## Quality Gate Configuration

### Security Gates

**Dependency Scanning**:

```yaml
# GitHub Actions
- name: Dependency Audit
  run: |
    npm audit --audit-level=moderate
    # Fails if moderate or higher vulnerabilities found
```

**Secret Detection**:

```yaml
# Pre-commit
- repo: https://github.com/Yelp/detect-secrets
  hooks:
    - id: detect-secrets
      args: ['--baseline', '.secrets.baseline']
```

**SAST (Static Analysis)**:

```yaml
# Python security
- name: Bandit Security Scan
  run: bandit -r src/ -f json -o bandit-report.json

# JavaScript security
- name: ESLint Security
  run: eslint . --plugin security
```

### Code Quality Gates

**Linting**:

```yaml
# Python
- name: Lint
  run: flake8 src/ --max-complexity=10 --max-line-length=88

# JavaScript
- name: Lint
  run: eslint . --max-warnings=0
```

**Complexity Thresholds**:

```yaml
# Radon (Python)
- name: Complexity Check
  run: radon cc src/ --min B  # B or better

# ESLint (JavaScript)
rules:
  complexity: ["error", 10]
  max-lines-per-function: ["warn", 50]
```

**Code Coverage**:

```yaml
# Pytest
- name: Test Coverage
  run: pytest --cov=src --cov-fail-under=80

# Jest
- name: Test Coverage
  run: jest --coverage --coverageThreshold='{"global":{"lines":80}}'
```

### Testing Gates

**All Tests Pass**:

```yaml
- name: Run Tests
  run: npm test
  # Fails if any test fails
```

**No Flaky Tests**:

```yaml
- name: Test Stability
  run: npm test -- --repeat=3
  # Run 3 times to catch flaky tests
```

## Tool Recommendations by Language

### Python

**Essential**:

- **Black**: Formatting (opinionated, zero-config)
- **Flake8**: Linting (style + potential bugs)
- **MyPy**: Type checking
- **Bandit**: Security scanning
- **Pytest**: Testing with coverage

**Configuration**:

```ini
# pyproject.toml
[tool.black]
line-length = 88

[tool.mypy]
python_version = "3.10"
strict = true

[tool.pytest.ini_options]
addopts = "--cov=src --cov-fail-under=80"
```

### JavaScript/TypeScript

**Essential**:

- **ESLint**: Linting (configurable rules)
- **Prettier**: Formatting
- **TypeScript**: Type checking
- **Jest**: Testing with coverage

**Configuration**:

```json
// .eslintrc.json
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:security/recommended"
  ],
  "rules": {
    "complexity": ["error", 10],
    "max-lines-per-function": ["warn", 50]
  }
}
```

### Go

**Essential**:

- **gofmt**: Formatting (standard)
- **golint**: Linting
- **go vet**: Static analysis
- **staticcheck**: Advanced linting

**Commands**:

```bash
gofmt -w .
golint ./...
go vet ./...
staticcheck ./...
```

### Multi-Language

**SonarQube/SonarCloud**:

- Comprehensive quality analysis
- Code smells, bugs, vulnerabilities
- Technical debt tracking
- Multi-language support

**Semgrep**:

- Custom rule engine
- Fast static analysis
- Security and bug patterns

## Pre-Commit Hook Setup

**Installation**:

```bash
# Install pre-commit
pip install pre-commit

# Install hooks
pre-commit install

# Run manually
pre-commit run --all-files
```

**Example Configuration**:

```yaml
# .pre-commit-config.yaml
repos:
  # Formatting
  - repo: https://github.com/psf/black
    rev: 23.0.0
    hooks:
      - id: black

  # Security
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets

  # Linting (local)
  - repo: local
    hooks:
      - id: lint
        name: Lint
        entry: npm run lint
        language: system
        pass_filenames: false

      - id: type-check
        name: Type Check
        entry: npm run type-check
        language: system
        pass_filenames: false
```

## CI/CD Platform Examples

### GitHub Actions

```yaml
name: Quality Gates
on: [push, pull_request]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Test
        run: npm test -- --coverage

      - name: Security
        run: npm audit
```

### GitLab CI

```yaml
# .gitlab-ci.yml
quality_gates:
  stage: test
  script:
    - npm ci
    - npm run lint
    - npm test -- --coverage
    - npm audit
  coverage: '/Lines\s*:\s*(\d+\.\d+)%/'
```

### Jenkins

```groovy
// Jenkinsfile
pipeline {
  agent any
  stages {
    stage('Quality Gates') {
      steps {
        sh 'npm ci'
        sh 'npm run lint'
        sh 'npm test -- --coverage'
        sh 'npm audit'
      }
    }
  }
}
```

## Best Practices

**Do**:

- ✅ Start with security and test gates (critical)
- ✅ Keep pre-commit hooks fast (< 30 seconds)
- ✅ Fail builds on critical issues only
- ✅ Provide clear error messages
- ✅ Make gates consistent across team
- ✅ Document gate requirements
- ✅ Use proven tools (don't reinvent)

**Don't**:

- ❌ Block on minor style issues (use auto-formatters)
- ❌ Make pre-commit hooks slow (> 1 minute)
- ❌ Create too many gates at once (incremental)
- ❌ Ignore gate failures (defeats purpose)
- ❌ Over-engineer custom solutions
- ❌ Skip security gates (always include)

## Quality Gate Checklist

**Security** (MUST HAVE):

- [ ] Secret detection (pre-commit)
- [ ] Dependency vulnerability scanning (CI)
- [ ] SAST (static analysis security)
- [ ] No hardcoded credentials

**Testing** (MUST HAVE):

- [ ] All tests pass
- [ ] Code coverage > 80%
- [ ] No flaky tests (run 3x)
- [ ] New code has tests

**Code Quality** (RECOMMENDED):

- [ ] Linting passes
- [ ] Complexity < 10
- [ ] Formatting consistent
- [ ] Type checking passes

**Performance** (OPTIONAL):

- [ ] Bundle size limits
- [ ] Build time limits
- [ ] Test execution time limits

## Common Pitfalls

**Slow gates**:

- Problem: Pre-commit takes 5 minutes
- Solution: Move slow checks to CI, keep pre-commit < 30s

**Too strict**:

- Problem: Team bypasses hooks (--no-verify)
- Solution: Start lenient, tighten gradually

**Inconsistent**:

- Problem: Different rules locally vs CI
- Solution: Single source of truth (.pre-commit-config.yaml)

**No documentation**:

- Problem: Team doesn't understand why gates fail
- Solution: Document each gate's purpose and how to fix

## Integration Patterns

**With code-security**:

- Use security skill for SAST configuration
- Implement OWASP Top 10 checks
- Set up dependency scanning

**With tech-stack-advisor**:

- Get language-specific tool recommendations
- Configure framework-specific linters
- Set up technology-appropriate gates

**With simplicity**:

- Avoid over-engineering quality infrastructure
- Start with essentials (security, tests)
- Add complexity incrementally

## Rollout Strategy

**Phase 1: Warnings Only**:

- Enable all checks
- Report issues, don't block
- Give team 2 weeks to adapt

**Phase 2: Critical Blocking**:

- Block on security issues
- Block on test failures
- Warn on quality issues

**Phase 3: Full Enforcement**:

- Block on all configured gates
- Team is adapted, violations rare

## Monitoring Gate Health

**Metrics to track**:

- Gate pass rate (target: > 95%)
- Average fix time
- False positive rate
- Developer satisfaction

**Review quarterly**:

- Are gates still relevant?
- Should thresholds change?
- New tools available?
