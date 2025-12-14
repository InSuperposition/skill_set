---
name: skill-code-auditor
description: Comprehensive codebase assessment and technical debt analysis. Performs systematic quality evaluation, identifies hotspots using git analysis, measures quality metrics, and creates prioritized remediation roadmaps. Data-driven approach to understanding and improving codebase health.
allowed-tools: Read
---

# Code Auditor - Comprehensive Codebase Assessment

## Overview

Code Auditor performs comprehensive codebase assessments, identifies technical debt, analyzes quality metrics, and creates prioritized remediation roadmaps. It combines git analysis for hotspot detection with systematic quality evaluation across all dimensions.

Unlike point-in-time code reviews, auditor provides a holistic view of codebase health, tracking trends over time and helping teams make informed decisions about where to invest quality improvement efforts.

**Philosophy**: "Measure to improve" - data-driven quality assessment and strategic improvement planning.

## Requirements

No external dependencies required.

Configures and interprets output from standard code quality tools (SonarQube, code coverage, complexity analyzers) based on technology stack.

## Core Capabilities

**Comprehensive Assessment**:

- Discovery phase to understand codebase structure
- Multi-dimensional quality analysis (security, performance, maintainability)
- Technical debt classification and quantification
- Hotspot identification using git history analysis
- Quality metric trends over time

**Git-Based Hotspot Analysis**:

- High churn files (frequently changed code)
- Code ownership and knowledge distribution
- Change coupling detection (files that change together)
- Defect density by file and component
- Bus factor analysis

**Technical Debt Quantification**:

- Debt ratio calculation (remediation cost vs development cost)
- SQALE rating (A through E quality grades)
- Code smells and anti-pattern detection
- Test coverage gaps and quality issues
- Infrastructure and documentation debt

**Prioritization Framework**:

- Impact vs effort matrix
- Risk-based prioritization (likelihood × severity)
- Quick wins identification
- Phased remediation roadmaps

**Actionable Reporting**:

- Executive summaries with key metrics
- Detailed findings by category and severity
- Prioritized improvement roadmaps
- ROI analysis for remediation efforts

## When to Use

**Use auditor for**:

- Comprehensive codebase health assessments
- Technical debt evaluation and prioritization
- Legacy code evaluation before modernization
- Pre-acquisition due diligence
- Quality baseline establishment
- Architecture and design reviews
- Quarterly or annual quality checkpoints
- Identifying highest-impact improvement opportunities

**Don't use auditor for**:

- Pull request reviews (use code-reviewer)
- Real-time quality enforcement (use code-guardian)
- Teaching best practices (use code-mentor)
- Immediate bug fixes or hotfixes

## Integration with Other Skills

**git**: Hotspot and ownership analysis

- Identify high-churn files requiring attention
- Analyze code ownership and knowledge distribution
- Detect temporal coupling between files
- Track defect density by component
- Example: "Payment processor has 150 commits in 6 months with complexity 45 - critical hotspot"

**first-principles**: Root cause analysis

- Question why technical debt exists
- Challenge assumptions about necessity
- Identify fundamental issues vs symptoms
- Build remediation from core requirements
- Example: "Why is this complex? Is the business logic inherently complex or poorly structured?"

**simplicity**: Complexity evaluation

- Evaluate architectural complexity objectively
- Identify over-engineering patterns
- Suggest simplification strategies
- Assess cognitive load on developers
- Example: "Three abstraction layers for simple CRUD operations adds unnecessary complexity"

**tech-stack-advisor**: Technology assessment

- Dependency security and obsolescence analysis
- Framework best practices evaluation
- Technology migration recommendations
- Tool selection for quality improvement
- Example: "12 dependencies are 2+ major versions behind with known CVEs"

## Audit Methodology

### Phase 1: Discovery

**Understand the Codebase**:

- Repository structure and organization
- Technology stack and dependencies
- Team size and contribution patterns
- Development practices and workflows
- Documentation availability

**Gather Metrics**:

```bash
# Repository age and size
git rev-list --count HEAD  # Total commits
git log --reverse --format="%ai" | head -1  # Creation date

# Language breakdown
cloc .  # Count lines of code by language

# Dependency count
# Python: pip list | wc -l
# JavaScript: cat package.json | jq '.dependencies | length'
```

### Phase 2: Analysis

**Code Quality Metrics**:

- Cyclomatic complexity per function/method
- Code duplication percentage
- Maintainability index
- Test coverage percentage
- Technical debt ratio

**Hotspot Detection** (Git Analysis):

```bash
# High-churn files (changed frequently)
git log --format=format: --name-only | \
  grep -v '^$' | sort | uniq -c | sort -rn | head -20

# Recent activity hotspots
git log --since="3 months ago" --name-only --format=format: | \
  grep -v '^$' | sort | uniq -c | sort -rn
```

**Security Assessment**:

- SAST tool results (Bandit, Semgrep, etc.)
- Dependency vulnerability scanning
- Authentication and authorization review
- Secret detection in code and history

**Performance Analysis**:

- N+1 query detection
- Missing database indexes
- Algorithmic complexity issues
- Resource leak detection

### Phase 3: Assessment

**Severity Classification**:

- Critical: Security vulnerabilities, data loss risks
- High: Performance bottlenecks, frequent bug sources
- Medium: Code duplication, missing tests
- Low: Style issues, minor optimizations

**Impact Analysis**:

- Business impact (revenue, user experience)
- Security risk (data exposure, compliance)
- Development velocity (time to add features)
- Maintenance cost (time spent debugging)

**Effort Estimation**:

- Simple: Hours (add index, fix query)
- Medium: Days (refactor module, add tests)
- Complex: Weeks (architectural changes)
- Strategic: Months (major refactoring, migration)

### Phase 4: Roadmap

**Prioritization**:

```md
Priority Score = (Impact × Urgency) / Effort

Quick Wins: High impact, low effort
Critical: High impact, high effort (do soon)
Nice to Have: Low impact, low effort (when time permits)
Defer: Low impact, high effort (unlikely to do)
```

**Phased Approach**:

- Phase 1: Critical issues and quick wins (weeks 1-2)
- Phase 2: High priority items (weeks 3-6)
- Phase 3: Medium priority items (weeks 7-12)
- Phase 4: Ongoing improvements and monitoring

## Examples

### Example 1: Comprehensive Audit Report

**Codebase Quality Audit Report**

**Executive Summary**:

- Overall Health: MODERATE RISK (SQALE Rating: C)
- Debt Ratio: 15% (Warning level)
- Critical Issues: 12
- Recommendation: Address critical security issues and hotspots immediately

**Key Findings**:

**1. Critical Hotspots** (Git Analysis)

payment_processor.py - CRITICAL

- Commits (6 months): 150
- Cyclomatic complexity: 45
- Authors: 8 different developers
- Bug fixes: 15 in last quarter
- Risk: Changes frequently break, extremely difficult to maintain
- Action: Immediate refactoring required

user_service.py - HIGH

- Commits (6 months): 120
- Lines of code: 2000 (god object)
- Methods: 45
- Action: Split into focused modules

**2. Security Debt**

HIGH PRIORITY:

- 8 dependencies with known CVEs (CRITICAL)
- 3 files with hardcoded secrets
- No rate limiting on authentication endpoints
- Verbose error messages expose stack traces

MEDIUM PRIORITY:

- Missing CSRF protection on forms
- Weak password requirements (< 8 characters)
- No security headers (CSP, X-Frame-Options)

**3. Test Debt**

Coverage: 45% (Target: 80%)

Critical gaps:

- Payment processing: 0% coverage (CRITICAL)
- Authentication: 30% coverage (HIGH)
- Order creation: 65% coverage (MEDIUM)

Flaky tests: 12 tests fail intermittently

**4. Code Quality**

- Complexity violations: 45 functions > 10 complexity
- Duplication: 23% (Target: < 5%)
- God objects: 3 classes > 1500 lines
- Missing documentation: 60% of functions lack docstrings

**5. Infrastructure Debt**

- 23 outdated dependencies (12 major versions behind)
- No CI/CD automation
- Manual deployment process
- No automated backups

**Prioritized Remediation Roadmap**:

**Phase 1: Critical** (Weeks 1-2, 80 hours)

- Fix 8 CVEs in dependencies (HIGH ROI)
- Add tests for payment processing (0% → 80%)
- Remove hardcoded secrets
- Add rate limiting to authentication
→ Risk Reduction: HIGH | Effort: 2 weeks

**Phase 2: High Priority** (Weeks 3-5, 120 hours)

- Refactor payment_processor.py hotspot
- Increase auth test coverage (30% → 80%)
- Set up CI/CD pipeline
- Fix 12 flaky tests
→ Risk Reduction: MEDIUM | Productivity Gain: HIGH

**Phase 3: Medium Priority** (Weeks 6-10, 200 hours)

- Refactor user_service.py god object
- Reduce code duplication (23% → <10%)
- Update API documentation
- Add security headers
→ Maintenance Cost Reduction: MEDIUM

**Phase 4: Ongoing**

- Increase overall coverage (45% → 80%)
- Update dependencies incrementally
- Automate dependency updates (Dependabot)
- Documentation improvements

**ROI Analysis**:

| Phase | Effort | Risk Reduction | Productivity | ROI    |
|-------|--------|----------------|--------------|--------|
| 1     | 2wk    | High           | Medium       | High   |
| 2     | 3wk    | Medium         | High         | High   |
| 3     | 5wk    | Low            | Medium       | Medium |
| 4     | Ongoing| Low            | Medium       | Low    |

**Recommended Focus**: Phases 1-2 (5 weeks total)

- Eliminates critical security risks
- Improves most problematic hotspots
- Sets up automation for future quality

### Example 2: Legacy Code Assessment

**Legacy Codebase Assessment**

**Discovery Phase**:

Codebase Statistics:

- Age: 5 years
- Languages: Python 2.7, JavaScript ES5
- Total LOC: 150,000
- Dependencies: 87 (32 severely outdated)
- Last major refactor: 2 years ago

**Git Analysis**:

- Active contributors: 3 (down from 12)
- Bus factor: 1 (CRITICAL RISK)
- Knowledge concentration: 80% in 1 developer
- Abandoned modules: 15 files with no commits in 18 months

**Critical Findings**:

**1. Knowledge Risk - CRITICAL**

- Only 1 developer understands payment system
- No documentation for business logic
- Complex domain rules not written down
- Recommendation: Immediate knowledge capture sessions

**2. Dependency Risk - HIGH**

- Python 2.7 (end of life, unsupported)
- Django 1.11 (no security updates)
- 12 dependencies with critical CVEs
- Recommendation: Migration plan required

**3. Test Risk - HIGH**

- Coverage: 12% overall
- No integration tests
- Manual testing only
- Recommendation: Add tests before any changes

**Recommended Approach**:

**Phase 0: Stabilize** (Week 1, don't touch code)

1. Add monitoring and logging
2. Document critical workflows
3. Knowledge transfer sessions with key developer
4. Create runbooks for deployment

**Phase 1: Safety Net** (Weeks 2-4)

1. Add tests for critical paths (payment, auth)
2. Set up CI/CD pipeline
3. Automated deployment to staging
4. Reach 40% coverage minimum

**Phase 2: Update Dependencies** (Weeks 5-8)

1. Python 2 → 3 migration
2. Django upgrade path (incremental)
3. Fix security vulnerabilities
4. Update incrementally, test extensively

**Phase 3: Refactor Hotspots** (Weeks 9-16)

1. payment_processor.py (highest risk)
2. user_service.py (god object)
3. Reduce complexity systematically

**Warning**: Do NOT attempt full rewrite. Incremental improvement is safer and more likely to succeed.

## Technical Debt Classification

**Code Debt**:

- Poor code quality (high complexity, duplication)
- Anti-patterns (god objects, spaghetti code)
- Missing error handling
- Hard-coded configuration values

**Design Debt**:

- Architectural issues (tight coupling, low cohesion)
- Violated SOLID principles
- Inappropriate design patterns
- Missing necessary abstractions

**Documentation Debt**:

- Missing or outdated documentation
- No architecture decision records (ADRs)
- Poor API documentation
- Missing setup instructions

**Test Debt**:

- Low test coverage (< 80%)
- Missing tests for critical paths
- Flaky or unreliable tests
- No integration or E2E tests

**Infrastructure Debt**:

- Outdated dependencies with CVEs
- Missing CI/CD automation
- Manual deployment processes
- No monitoring or alerting

**Knowledge Debt**:

- Complex code with no experts (bus factor 1)
- High knowledge concentration
- No knowledge transfer process
- Undocumented tribal knowledge

## Metrics and Measurement

**Debt Ratio**:

```md
Debt Ratio = (Remediation Cost) / (Development Cost)

< 5%: Excellent
5-10%: Good
10-20%: Warning
> 20%: Critical
```

**SQALE Rating**:

- A: < 5% debt ratio (Excellent)
- B: 5-10% (Good)
- C: 10-20% (Warning)
- D: 20-50% (Poor)
- E: > 50% (Critical)

**Hotspot Metrics**:

- Change frequency: Commits in last 6 months
- Change volume: Lines added/deleted
- Complexity: Cyclomatic/cognitive complexity
- Author count: Number of different developers

**Example Hotspot Report**:

```md
File                    Commits  Complexity  Authors  Risk
payment_processor.py    150      45          8        CRITICAL
user_service.py         120      38          5        HIGH
auth.py                 95       32          3        MEDIUM
```

## Best Practices

**Do**:

- Use automated tools for objective metrics
- Integrate git analysis for hotspot detection
- Prioritize by business impact and risk
- Create actionable, phased roadmaps
- Involve stakeholders in prioritization
- Track progress and trends over time
- Re-audit quarterly to measure improvement
- Focus on high-impact, low-effort quick wins
- Celebrate progress publicly

**Don't**:

- Audit without creating actionable next steps
- Ignore business context and priorities
- Try to fix everything at once
- Skip quick wins for perfect solutions
- Forget to measure and track progress
- Audit just to audit (needs purpose)
- Overlook infrastructure and knowledge debt
- Make recommendations without ROI analysis

## Integration Patterns

**With code-reviewer**:

- Use audit findings to focus reviews
- Track review findings over time
- Measure improvement in common issues
- Validate remediation effectiveness

**With code-guardian**:

- Set up automated gates based on audit findings
- Prevent new debt in identified categories
- Track gate effectiveness over time
- Adjust thresholds based on progress

**With code-mentor**:

- Share audit findings as learning material
- Explain technical debt concepts to team
- Teach prioritization frameworks
- Build awareness of quality trends

## See Also

Related code quality skills:

- code-reviewer: Point-in-time code reviews
- code-guardian: Automated quality enforcement
- code-mentor: Teaching and education
- code-refactor: Debt paydown techniques
- code-security: Security-specific auditing

Related foundational skills:

- first-principles: Root cause analysis
- simplicity: Complexity evaluation
- git: History and hotspot analysis
- tech-stack-advisor: Technology assessment
