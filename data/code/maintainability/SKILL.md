---
name: skill-code-maintainability
description: Code clarity and long-term sustainability assessment. Evaluates readability, simplicity, structure, and documentation. Applies simplicity principles to reduce complexity. Ensures code can be easily understood, modified, and extended by current and future developers.
allowed-tools: Read
---

# Code Maintainability - Clarity and Sustainability

## Overview

Code Maintainability evaluates how easily code can be understood, modified, and extended over time. It emphasizes readability, simplicity, clear structure, and appropriate documentation using simplicity principles.

Maintainable code reduces long-term costs, enables faster feature development, and makes debugging significantly easier.

**Philosophy**: "Code is read 10x more than it's written" - optimize for understanding, not just writing.

## Requirements

No external dependencies required.

Applies universal maintainability principles across all programming languages and paradigms.

## Core Capabilities

**Readability Assessment**:

- Clear, descriptive naming
- Appropriate function and class length
- Consistent code style and formatting
- Logical code organization

**Simplicity Evaluation** (from simplicity skill):

- Objective complexity measurement
- Over-engineering detection
- Hidden complexity identification
- Coupling and cohesion analysis

**Structural Quality**:

- Separation of concerns
- DRY (Don't Repeat Yourself) compliance
- Appropriate abstraction levels
- Clear module boundaries

**Documentation Assessment**:

- Code comments quality (why, not what)
- API documentation completeness
- README and setup instructions
- Architecture decision records (ADRs)

## When to Use

**Use maintainability for**:

- Evaluating code clarity and understandability
- Identifying over-engineered solutions
- Detecting code duplication
- Reviewing documentation quality
- Assessing long-term sustainability
- Simplification opportunities

**Don't use maintainability for**:

- Security vulnerabilities (use code-security)
- Performance optimization (use code-performance)
- Correctness verification (use code-correctness)

## Integration with Other Skills

**simplicity**: Core evaluation framework

- Objective vs subjective complexity
- Identify over-engineering
- Hidden complexity detection
- Example: "Three factory layers for simple object creation is over-engineered"

**first-principles**: Understanding requirements

- Question complexity necessity
- Verify abstraction value
- Challenge assumptions
- Example: "Why is this complex? Is the requirement complex or the implementation?"

**code-refactor**: Improvement path

- Identify refactoring opportunities
- Simplify complex code
- Reduce duplication
- Example: "Extract these duplicated validation rules into a shared function"

## Maintainability Factors

### Readability

**Clear Naming**:

```python
# Bad: Cryptic names
def f(x, y):
    return x * y * 0.1

# Good: Descriptive names
def calculate_sales_tax(price, quantity):
    TAX_RATE = 0.1
    return price * quantity * TAX_RATE
```

**Function Length**:

```python
# Bad: 200-line function
def process_order(order):
    # 200 lines of everything

# Good: Focused functions
def process_order(order):
    validate_order(order)
    calculate_totals(order)
    save_order(order)
    send_confirmation(order)
```

### Simplicity

**Complexity Metrics**:

- Cyclomatic complexity: < 10 per function
- Lines of code: Functions < 50, Classes < 500
- Nesting depth: < 4 levels
- Parameter count: < 4 parameters

**Example - Over-Engineering**:

```python
# Bad: Unnecessary abstraction
class AbstractUserFactoryProviderStrategy:
    ...

# Good: Simple and direct
def create_user(name, email):
    return User(name=name, email=email)
```

### Structure

**Separation of Concerns**:

```python
# Bad: Mixed concerns
class UserController:
    def create_user(self, data):
        # Validation, database, email, logging all mixed

# Good: Separated
class UserService:
    def create_user(self, data):
        user = self.validator.validate(data)
        user = self.repository.save(user)
        self.notifier.send_welcome(user)
        return user
```

**DRY (Don't Repeat Yourself)**:

```python
# Bad: Duplication
def process_user_order(user, order):
    if user.age < 18:
        raise ValueError("Too young")
    # process...

def process_admin_order(admin, order):
    if admin.age < 18:  # Duplicated
        raise ValueError("Too young")
    # process...

# Good: Extract common logic
def validate_age(person):
    if person.age < 18:
        raise ValueError("Too young")
```

### Documentation

**Good Comments** (explain "why", not "what"):

```python
# Bad: Obvious comment
i += 1  # Increment i

# Good: Explain reasoning
i += 1  # Skip header row

# Good: Explain complex logic
# Use binary search because list is sorted and large (10M items)
index = binary_search(items, target)
```

**Docstrings**:

```python
def calculate_compound_interest(principal, rate, years):
    """
    Calculate compound interest.

    Args:
        principal: Initial amount ($)
        rate: Annual interest rate (0.05 = 5%)
        years: Number of years

    Returns:
        Final amount after compound interest

    Example:
        >>> calculate_compound_interest(1000, 0.05, 10)
        1628.89
    """
    return principal * (1 + rate) ** years
```

## Examples

### Example 1: Improving Readability

**Before**:

```python
def calc(p, r, t):
    return p * (1 + r) ** t
```

**After**:

```python
def calculate_compound_interest(
    principal: float,
    annual_rate: float,
    years: int
) -> float:
    """Calculate final amount with compound interest."""
    return principal * (1 + annual_rate) ** years
```

### Example 2: Reducing Complexity

**Before** (Complexity: 15):

```python
def get_discount(user, order):
    if user.is_premium:
        if order.total > 100:
            if user.account_age > 365:
                return 0.25
            else:
                return 0.15
        else:
            return 0.10
    else:
        if order.total > 100:
            return 0.05
        else:
            return 0.0
```

**After** (Complexity: 5):

```python
def get_discount(user, order):
    if not user.is_premium:
        return 0.05 if order.total > 100 else 0.0

    base_discount = 0.10
    if order.total > 100:
        base_discount = 0.15
    if user.account_age > 365:
        base_discount = 0.25

    return base_discount
```

## Complexity Metrics

**Cyclomatic Complexity**: Decision points (if, while, for)

- Target: < 10 per function
- 1-10: Easy to understand
- 11-20: Moderate complexity
- 21+: Hard to maintain

**Lines of Code**:

- Functions: < 50 lines
- Classes: < 500 lines
- Files: < 1000 lines

**Nesting Depth**:

- Target: < 4 levels deep
- Deep nesting indicates complexity

## Maintainability Checklist

**Code Quality**:

- Clear, descriptive names
- Functions < 50 lines
- Cyclomatic complexity < 10
- Nesting < 4 levels
- No code duplication

**Structure**:

- Separation of concerns
- Single Responsibility Principle
- Appropriate abstractions (not over-engineered)
- Clear module boundaries

**Documentation**:

- Complex logic explained
- Public APIs documented
- README up to date
- No commented-out code

**Simplicity**:

- No unnecessary complexity
- No premature optimization
- No over-engineering
- Explicit over implicit

## Best Practices

**Do**:

- Write self-documenting code
- Keep functions small and focused
- Follow language conventions
- Use consistent formatting
- Explain complex logic with comments
- Simplify when possible
- Name things clearly
- Separate concerns

**Don't**:

- Write clever, obscure code
- Use cryptic abbreviations
- Create deep nesting
- Mix concerns in one function
- Duplicate code
- Over-engineer solutions
- Skip documentation for complex code
- Make readers guess intent

## See Also

Related code quality skills:

- code-refactor: Improve maintainability
- code-architecture: Design for maintainability
- code-mentor: Teaching maintainability
- code-reviewer: Maintainability review

Related foundational skills:

- simplicity: Complexity evaluation
- first-principles: Understanding requirements
