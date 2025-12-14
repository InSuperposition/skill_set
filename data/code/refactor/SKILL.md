---
name: skill-code-refactor
description: Safe code improvement and structural transformation. Guides incremental refactoring to improve code quality without changing behavior. Emphasizes test-driven approach, small steps with frequent commits, and proven refactoring patterns. Makes technical debt paydown safe and systematic.
allowed-tools: Read
---

# Code Refactor - Safe Code Improvement

## Overview

Code Refactor guides the process of improving code structure without changing its external behavior. It provides systematic approaches to make code more maintainable, understandable, and extensible while minimizing the risk of introducing bugs.

This skill emphasizes safety through comprehensive testing, incremental changes, and proven refactoring patterns that work across all programming languages.

**Philosophy**: "Make the change easy, then make the easy change" - prepare the code structure, then implement the feature safely.

## Requirements

No external dependencies required.

Applies language-agnostic refactoring principles with specific pattern recommendations based on code structure and quality issues.

## Core Capabilities

**Safe Refactoring Principles**:

- Tests first: Ensure comprehensive test coverage before refactoring
- Small steps: One change at a time with frequent commits
- Keep it green: All tests pass after every change
- No behavior changes: Same inputs produce same outputs
- Separate concerns: Never mix refactoring with new features

**Common Refactoring Patterns**:

- Extract method/function: Break down long procedures
- Extract variable: Name complex expressions
- Replace conditional with polymorphism: Remove complex if/else chains
- Replace magic numbers with constants: Improve clarity
- Move method/field: Put code where it belongs
- Inline method/variable: Remove unnecessary indirection

**Code Smell Detection**:

- Long methods (> 50 lines)
- Large classes (> 500 lines)
- Duplicated code
- Long parameter lists (> 4 parameters)
- Complex conditionals
- Feature envy (method uses another class more than its own)

**Integration Support**:

- Simplicity principles for reducing complexity
- First-principles thinking for understanding requirements
- Git workflows for safe incremental changes
- Test coverage verification before refactoring

## When to Use

**Use refactor for**:

- Preparing code before adding new features
- Improving code quality during code review
- Paying down technical debt systematically
- Fixing bugs (prevent similar issues)
- When tests provide a safety net
- Boy scout rule: leaving code better than you found it

**Don't use refactor for**:

- Code without tests (no safety net)
- While actively debugging (too many variables)
- On tight deadlines without time for testing
- Across entire codebase at once (too risky)
- When you don't understand the code

## Integration with Other Skills

**simplicity**: Refactoring target

- Refactor toward simplicity and clarity
- Remove unnecessary complexity
- Make code easier to understand
- Example: "Replace three abstraction layers with direct implementation"

**first-principles**: Understanding requirements

- Question why code is complex
- Understand root requirements
- Rebuild from fundamentals when needed
- Example: "Why does this need to be abstract? What's the actual requirement?"

**git**: Safe incremental changes

- Use feature branches for large refactors
- Commit frequently (after each working change)
- Review git diff to verify behavior unchanged
- Example: "git commit after each extract method refactoring"

**code-testability**: Safety net

- Ensure tests exist before refactoring
- Maintain test coverage during changes
- Use tests to verify behavior preservation
- Example: "Add missing tests, then refactor with confidence"

## Refactoring Workflow

### Step 1: Ensure Tests Exist

**Check Coverage**:

```bash
# Python
pytest --cov=module_to_refactor --cov-report=term-missing

# JavaScript
npm test -- --coverage

# Goal: > 80% coverage before refactoring
```

**If Coverage < 80%**:
Write tests FIRST, then refactor. Never refactor without a safety net.

### Step 2: Identify Code Smells

**Common Smells**:

- Long methods (> 50 lines)
- Large classes (> 500 lines)
- Duplicated code
- Long parameter lists (> 4 params)
- Complex conditionals (nested if/else)
- Feature envy (method uses another class more)

### Step 3: Plan Small Steps

**Bad Approach**: "Rewrite the entire module"

**Good Approach**:

1. Extract validation logic
2. Extract calculation logic
3. Simplify conditionals
4. Remove duplication
5. Each step: change, test, commit

### Step 4: Refactor Incrementally

**For Each Step**:

1. Make ONE change
2. Run tests (should pass)
3. Commit with descriptive message
4. Repeat

**Example Commit Sequence**:

```bash
git commit -m "Extract validate_order method"
git commit -m "Extract calculate_total method"
git commit -m "Extract save_and_notify method"
git commit -m "Simplify process_order using extracted methods"
```

### Step 5: Review and Document

**After Refactoring**:

- Code review the changes
- Update relevant documentation
- Celebrate the improvement

## Examples

### Example 1: Extract Method

**Before** (Long, complex method):

```python
def process_order(order):
    # Validation
    if not order.items:
        raise ValueError("Empty order")
    if order.total < 0:
        raise ValueError("Negative total")

    # Calculation
    subtotal = sum(item.price * item.quantity for item in order.items)
    tax = subtotal * 0.1
    total = subtotal + tax

    # Persistence
    db.save(order)
    send_confirmation(order.customer.email)

    return total
```

**After** (Extracted into focused methods):

```python
def process_order(order):
    validate_order(order)
    order.total = calculate_total(order)
    save_and_notify(order)
    return order.total

def validate_order(order):
    if not order.items:
        raise ValueError("Empty order")
    if order.total < 0:
        raise ValueError("Negative total")

def calculate_total(order):
    subtotal = sum(item.price * item.quantity for item in order.items)
    tax = subtotal * 0.1
    return subtotal + tax

def save_and_notify(order):
    db.save(order)
    send_confirmation(order.customer.email)
```

**Benefits**:

- Each function has single responsibility
- Easy to test each step independently
- Clear what each part does
- Can reuse validation or calculation elsewhere

### Example 2: Replace Conditional with Polymorphism

**Before** (Complex conditional):

```python
def calculate_shipping(order):
    if order.shipping_type == "standard":
        return order.weight * 0.5
    elif order.shipping_type == "express":
        return order.weight * 1.5 + 10
    elif order.shipping_type == "overnight":
        return order.weight * 3.0 + 25
    else:
        raise ValueError(f"Unknown type: {order.shipping_type}")
```

**After** (Polymorphic strategy):

```python
class StandardShipping:
    def calculate(self, order):
        return order.weight * 0.5

class ExpressShipping:
    def calculate(self, order):
        return order.weight * 1.5 + 10

class OvernightShipping:
    def calculate(self, order):
        return order.weight * 3.0 + 25

def calculate_shipping(order):
    return order.shipping_strategy.calculate(order)
```

**Benefits**:

- Adding new shipping types doesn't require modifying existing code
- Each strategy is independent and testable
- Follows Open/Closed Principle

### Example 3: Replace Magic Numbers with Constants

**Before** (Unclear magic numbers):

```python
def calculate_discount(user, price):
    if user.account_age > 30:  # What's 30?
        return price * 0.15    # What's 0.15?
    return 0
```

**After** (Named constants):

```python
LOYALTY_THRESHOLD_DAYS = 30
LOYALTY_DISCOUNT_RATE = 0.15

def calculate_discount(user, price):
    if user.account_age > LOYALTY_THRESHOLD_DAYS:
        return price * LOYALTY_DISCOUNT_RATE
    return 0
```

**Benefits**:

- Clear what the numbers mean
- Easy to change business rules (one place)
- Self-documenting code

## Common Refactoring Patterns

**Code-Level**:

- Rename variable/method: Improve clarity
- Extract method: Break down long functions
- Inline method: Remove unnecessary indirection
- Extract variable: Name complex expressions
- Inline variable: Remove trivial variables

**Class-Level**:

- Extract class: Split god objects
- Move method: Put method where it belongs
- Move field: Put data where it's used
- Inline class: Remove unnecessary classes

**Architecture-Level**:

- Extract module: Separate concerns
- Introduce layer: Add abstraction level
- Replace inheritance with delegation: Favor composition
- Collapse hierarchy: Remove unnecessary inheritance

## Tools and Automation

**IDE Refactoring**:

- Rename: Update all references safely
- Extract method/function: Automated extraction
- Move: Relocate code with dependency updates
- Inline: Remove indirection automatically

**Language-Specific**:

- Python: PyCharm, VS Code, rope library
- JavaScript: VS Code, WebStorm, TypeScript compiler
- Java: IntelliJ IDEA, Eclipse
- Go: Built-in refactoring tools

**Verification**:

- git diff: Review changes systematically
- Test suites: Ensure behavior preserved
- Coverage tools: Maintain coverage percentage

## Best Practices

**Do**:

- Write or verify tests first
- Make small, incremental changes
- Keep all tests passing (stay green)
- Commit frequently with clear messages
- Get code review for large refactorings
- Preserve existing behavior exactly
- Use IDE refactoring tools when available
- Focus on one smell at a time

**Don't**:

- Refactor without tests (no safety net)
- Mix refactoring with new features
- Make big changes all at once
- Refactor unfamiliar code without understanding it
- Skip code review
- Change behavior during refactoring
- Trust yourself without running tests
- Try to fix everything simultaneously

## Incremental Refactoring Strategy

**Boy Scout Rule**:
Leave code better than you found it. When touching code:

- Spend 10-20% of time on small improvements
- Fix obvious issues as you go
- Don't let perfect be enemy of good

**Strangler Fig Pattern**:
Gradually replace legacy code:

1. Route new features to new implementation
2. Migrate old features incrementally
3. Decommission old code when empty
4. Never big-bang rewrite

**Dedicated Refactoring Time**:
Allocate 10-20% of development time:

- Regular refactoring sessions
- Technical debt sprints
- Quality improvement weeks

## Integration Patterns

**With code-reviewer**:

- Identify refactoring opportunities in reviews
- Suggest specific refactoring patterns
- Track improvement over time
- Example: "This god object should be split - I can help with that"

**With code-mentor**:

- Teach refactoring patterns to team
- Explain when and why to refactor
- Show safe refactoring techniques
- Example: "Let me show you how to safely extract this method"

**With code-auditor**:

- Prioritize refactoring based on hotspots
- Focus on high-impact areas first
- Track quality improvement metrics
- Example: "Audit shows payment_processor needs refactoring - let's tackle that"

## Common Pitfalls and Solutions

**Pitfall**: Refactoring without tests
**Solution**: Add tests first, then refactor with confidence

**Pitfall**: Mixing refactoring with new features
**Solution**: Separate commits - refactor first, feature second

**Pitfall**: Making too many changes at once
**Solution**: Break into smaller steps, commit frequently

**Pitfall**: Breaking existing functionality
**Solution**: Run full test suite after every change

**Pitfall**: Refactoring on deadline
**Solution**: Quick fixes for now, plan proper refactoring for later

## See Also

Related code quality skills:

- code-reviewer: Identify refactoring opportunities
- code-mentor: Learn refactoring patterns
- code-auditor: Prioritize refactoring efforts
- code-maintainability: Target for refactoring
- code-architecture: Architectural refactoring patterns

Related foundational skills:

- first-principles: Understand root requirements
- simplicity: Refactor toward simplicity
- git: Safe incremental changes
- code-testability: Safety net for refactoring
