---
name: skill-code-correctness
description: Logic validation and bug detection through first-principles analysis. Ensures code works correctly for all inputs including edge cases. Validates type safety, error handling, boundary conditions, and algorithmic correctness. Emphasizes correctness before performance optimization.
allowed-tools: Read
---

# Code Correctness - Logic Validation and Bug Detection

## Overview

Code Correctness ensures code works as intended, handles all edge cases properly, and produces correct results for all valid inputs. It applies first-principles thinking to verify logic, identify boundary conditions, and prevent bugs before they reach production.

Correctness is the foundation of all quality - fast, maintainable code that produces wrong results is worthless.

**Philosophy**: "Correctness first, then performance" - working code is always better than fast broken code.

## Requirements

No external dependencies required.

Applies universal logic verification principles across all programming languages.

## Core Capabilities

**Logic Verification**:

- First-principles analysis of inputs, outputs, and transformations
- Edge case identification (null, empty, zero, negative, boundary values)
- Algorithmic correctness validation
- State management verification

**Type Safety**:

- Type validation (static and runtime)
- Type hints and annotations
- Null/undefined handling
- Type conversion verification

**Error Handling**:

- Explicit error path handling
- Appropriate exception types
- Error logging and propagation
- Resource cleanup (try/finally)

**Boundary Conditions**:

- Off-by-one error detection
- Range validation
- Integer overflow/underflow
- Array bounds checking

## When to Use

**Use correctness for**:

- Verifying business logic implementation
- Identifying edge case handling gaps
- Validating error handling completeness
- Reviewing type safety
- Preventing null pointer exceptions
- Checking boundary conditions
- First-principles logic verification

**Don't use correctness for**:

- Performance optimization (use code-performance)
- Code style issues (use linters)
- Security vulnerabilities (use code-security)

## Integration with Other Skills

**first-principles**: Logic verification foundation

- Question assumptions about inputs
- Verify requirements from fundamentals
- Identify all possible edge cases
- Example: "What are ALL valid input ranges? What happens at boundaries?"

**code-testability**: Verification through testing

- Test normal cases
- Test edge cases
- Test error paths
- Example: "Tests verify correctness claims"

**code-security**: Validation overlaps

- Input validation for security and correctness
- Error handling prevents information leakage
- Type safety prevents exploits
- Example: "Validating age range prevents both errors and attacks"

## Logic Verification Framework

### First-Principles Questions

For every function, ask:

- What are the inputs? (types, ranges, constraints)
- What are the outputs? (expected results, types)
- What are the transformations? (algorithm correctness)
- What are the edge cases? (boundary conditions, special values)

**Example**:

```python
def calculate_discount(price, discount_percent):
    discount = price * (discount_percent / 100)
    return price - discount

# First-principles questions:
# - What if price is negative?
# - What if discount_percent > 100?
# - What if inputs are None?
# - What if inputs are strings "100"?
```

### Edge Cases

**Common Edge Cases**:

- Empty/Null: None, [], "", {}
- Zero: 0, 0.0
- Negative: -1, -100
- Boundary: MAX_INT, MIN_INT
- Large: Very large numbers, arrays
- Special: NaN, Infinity, -0

**Example**:

```python
def get_first_element(items):
    # Bad: Assumes non-empty
    return items[0]  # IndexError if empty

    # Good: Handle empty case
    return items[0] if items else None
```

## Examples

### Example 1: Input Validation

**Before** (No validation):

```python
def calculate_discount(price, discount_percent):
    return price * (1 - discount_percent / 100)
```

**Issues**:

- Negative prices allowed
- discount_percent > 100 gives negative result
- None values cause TypeError
- No type checking

**After** (Complete validation):

```python
def calculate_discount(price: float, discount_percent: float) -> float:
    """
    Calculate discounted price.

    Args:
        price: Original price (must be >= 0)
        discount_percent: Discount percentage (0-100)

    Returns:
        Discounted price

    Raises:
        ValueError: If inputs are invalid
    """
    if price is None or discount_percent is None:
        raise ValueError("Inputs cannot be None")
    if price < 0:
        raise ValueError("Price cannot be negative")
    if not (0 <= discount_percent <= 100):
        raise ValueError("Discount must be 0-100")

    return price * (1 - discount_percent / 100)
```

### Example 2: Error Handling

**Before** (Swallowed exceptions):

```python
try:
    result = risky_operation()
except:
    pass  # What went wrong?
```

**After** (Explicit error handling):

```python
try:
    result = risky_operation()
except ValueError as e:
    logger.error(f"Invalid input: {e}")
    raise
except ConnectionError as e:
    logger.error(f"Connection failed: {e}")
    return fallback_value()
```

### Example 3: Boundary Conditions

**Before** (Off-by-one error):

```python
def get_items_page(page_number, page_size):
    offset = page_number * page_size  # Wrong for page 1
    return items[offset:offset + page_size]
```

**After** (Correct boundaries):

```python
def get_items_page(page_number: int, page_size: int):
    """
    Get paginated items.

    Args:
        page_number: Page number (1-based)
        page_size: Items per page (1-100)
    """
    if page_number < 1:
        raise ValueError("Page number must be >= 1")
    if not (1 <= page_size <= 100):
        raise ValueError("Page size must be 1-100")

    offset = (page_number - 1) * page_size  # Correct
    return items[offset:offset + page_size]
```

## Common Bug Patterns

**Race Conditions**:

```python
# Bad: Race condition
if balance >= amount:
    balance -= amount  # Another thread can modify balance

# Good: Atomic operation
with lock:
    if balance >= amount:
        balance -= amount
```

**Null Dereference**:

```python
# Bad: Assumes user exists
user_name = get_user(user_id).name  # NoneType error

# Good: Check first
user = get_user(user_id)
user_name = user.name if user else "Unknown"
```

**Type Confusion**:

```python
# Bad: No type checking
def add(a, b):
    return a + b  # "5" + "3" = "53"

# Good: Type hints and validation
def add(a: int, b: int) -> int:
    return a + b
```

## Testing for Correctness

**Unit Tests**:

```python
def test_calculate_discount():
    # Normal cases
    assert calculate_discount(100, 10) == 90
    assert calculate_discount(50, 20) == 40

    # Edge cases
    assert calculate_discount(100, 0) == 100   # 0% discount
    assert calculate_discount(0, 10) == 0      # $0 price

    # Error cases
    with pytest.raises(ValueError):
        calculate_discount(-100, 10)  # Negative price
    with pytest.raises(ValueError):
        calculate_discount(100, 150)  # > 100%
```

**Property-Based Testing**:

```python
from hypothesis import given
import hypothesis.strategies as st

@given(st.integers(), st.integers())
def test_addition_commutative(a, b):
    assert a + b == b + a  # Always true

@given(st.lists(st.integers()))
def test_sort_idempotent(items):
    sorted_once = sorted(items)
    sorted_twice = sorted(sorted_once)
    assert sorted_once == sorted_twice
```

## Correctness Checklist

**Input Validation**:

- All inputs validated (type, range, format)
- Null/empty cases handled explicitly
- Negative numbers handled appropriately
- Boundary conditions verified

**Logic**:

- Algorithm correct for all valid inputs
- Edge cases explicitly handled
- No off-by-one errors
- Race conditions prevented (proper locking)

**Error Handling**:

- All error paths handled
- Specific exceptions caught (not bare except)
- Errors logged appropriately
- Resources cleaned up (try/finally)

**Testing**:

- Unit tests for normal cases
- Unit tests for edge cases
- Unit tests for error cases
- Integration tests for system behavior

## Best Practices

**Do**:

- Validate all inputs explicitly
- Handle edge cases explicitly
- Use type hints/checking
- Write comprehensive tests
- Think through all error paths
- Use first-principles verification
- Question assumptions
- Test boundary conditions

**Don't**:

- Assume inputs are valid
- Ignore edge cases ("won't happen")
- Swallow exceptions
- Skip error handling
- Forget boundary conditions
- Trust without verification
- Use bare except clauses
- Skip null checks

## See Also

Related code quality skills:

- code-reviewer: Logic review in PRs
- code-testability: Testing strategies
- code-security: Input validation
- code-mentor: Teaching correctness

Related foundational skills:

- first-principles: Logic verification
- simplicity: Clear, correct code
