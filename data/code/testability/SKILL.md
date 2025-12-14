---
name: skill-code-testability
description: Test coverage and quality assessment. Evaluates how easily code can be tested, measures test coverage completeness, assesses test quality, and identifies design issues preventing testability. Promotes dependency injection, pure functions, and clear separation of concerns for testable code.
allowed-tools: Read
---

# Code Testability - Test Coverage and Quality

## Overview

Code Testability measures how easily code can be tested and evaluates the quality of existing tests. It identifies design issues that prevent testing, promotes testable code patterns, and ensures adequate test coverage across all quality dimensions.

Testable code is maintainable code - if code is hard to test, it's usually poorly designed.

**Philosophy**: "Untestable code is unmaintainable code" - design for testability from the start.

## Requirements

No external dependencies required.

Recommends testing frameworks and coverage tools based on technology stack (pytest, Jest, JUnit, etc.).

## Core Capabilities

**Coverage Analysis**:

- Line coverage percentage
- Branch coverage percentage
- Function coverage percentage
- Critical path coverage identification
- Coverage gap detection

**Test Quality Assessment**:

- Fast execution (< 10 seconds for unit tests)
- Test isolation (no dependencies between tests)
- Repeatability (deterministic results)
- Clear test names and structure
- Flaky test identification

**Design for Testability**:

- Dependency injection patterns
- Pure function identification
- Seam creation for testing
- Mock/stub opportunities
- Testability anti-pattern detection

**Test Maintainability**:

- Test organization and structure
- Fixture reusability
- Parameterized test usage
- Test duplication detection

**Testing Strategies**:

- Unit testing (isolated components)
- Integration testing (component interaction)
- Test-Driven Development (TDD)
- Property-based testing

## When to Use

**Use testability for**:

- Evaluating test coverage adequacy
- Identifying untestable code patterns
- Improving test quality
- Finding coverage gaps
- Detecting flaky tests
- Reviewing test maintainability
- Design review for testability

**Don't use testability for**:

- Writing actual tests (implementation task)
- Performance testing (use code-performance)
- Security testing (use code-security)

## Integration with Other Skills

**first-principles**: Understanding test needs

- Question what behavior to test
- Break down complex tests to fundamentals
- Identify true dependencies vs coupling
- Example: "What behavior am I actually testing here?"

**simplicity**: Test simplicity

- Simple code is easier to test
- Avoid over-mocking
- Prefer integration tests over complex mocks
- Example: "Ten mocks for one test indicates poor design"

**git**: Coverage tracking

- Identify untested code with git blame
- Find gaps in frequently changed code
- Track coverage trends over time
- Example: "This hotspot has 0% coverage - critical risk"

**code-refactor**: Improving testability

- Refactor for testability before adding tests
- Extract dependencies for injection
- Create seams for testing
- Example: "Extract database dependency to make this testable"

## Test Coverage Metrics

### Coverage Types

**Line Coverage**: % of code lines executed
**Branch Coverage**: % of decision branches taken
**Function Coverage**: % of functions called
**Condition Coverage**: % of boolean sub-expressions tested

**Example**:

```python
def calculate_discount(price, user_type):
    if user_type == "premium":
        return price * 0.2
    elif user_type == "regular":
        return price * 0.1
    else:
        return 0

# 33% coverage - only one branch tested
def test_premium_discount():
    assert calculate_discount(100, "premium") == 20

# 100% coverage - all branches tested
def test_all_discount_types():
    assert calculate_discount(100, "premium") == 20
    assert calculate_discount(100, "regular") == 10
    assert calculate_discount(100, "guest") == 0
```

### Coverage Targets

- Critical paths (auth, payments, security): 100%
- Business logic: 90%+
- General code: 80%+
- UI/glue code: 60%+

### Coverage Tools

```bash
# Python
pytest --cov=myapp --cov-report=html

# JavaScript
npm test -- --coverage

# Go
go test -cover ./...

# Rust
cargo tarpaulin
```

## Test Quality

### Good Test Characteristics

**FIRST Principles**:

- **Fast**: Run in milliseconds
- **Isolated**: No dependencies between tests
- **Repeatable**: Same result every time
- **Self-checking**: Pass/fail automatically
- **Timely**: Written with or before code

### Test Smells

**Flaky Tests** (non-deterministic):

```python
# Bad: Depends on current time
def test_is_business_hours():
    assert is_business_hours()  # Fails at night

# Good: Inject time dependency
def test_is_business_hours():
    morning = datetime(2024, 1, 1, 9, 0)
    assert is_business_hours(morning) == True
```

**Slow Tests**:

```python
# Bad: Actual sleep
def test_retry_logic():
    time.sleep(5)
    assert retry_successful()

# Good: Mock time
def test_retry_logic(mock_time):
    mock_time.fast_forward(5)
    assert retry_successful()
```

**Unclear Tests**:

```python
# Bad: Unclear intent
def test_case_1():
    result = do_something(5, True, "abc")
    assert result == 42

# Good: AAA structure, clear name
def test_calculate_tax_returns_10_percent_for_standard_items():
    # Arrange
    item_price = 100
    item_type = "standard"

    # Act
    tax = calculate_tax(item_price, item_type)

    # Assert
    assert tax == 10
```

## Design for Testability

### Dependency Injection

**Testable Design**:

```python
# Bad: Hardcoded dependency
class UserService:
    def __init__(self):
        self.db = PostgresDB()  # Cannot mock

# Good: Injected dependency
class UserService:
    def __init__(self, db):
        self.db = db

# Test with mock
def test_get_user():
    mock_db = Mock()
    mock_db.query.return_value = {"id": 1, "name": "Alice"}

    service = UserService(mock_db)
    user = service.get_user(1)

    assert user["name"] == "Alice"
```

### Pure Functions

**Easiest to Test**:

```python
# Bad: Depends on global state
current_discount = 0.1

def calculate_price(base_price):
    return base_price * (1 - current_discount)

# Good: Pure function
def calculate_price(base_price, discount):
    return base_price * (1 - discount)

# Easy to test
def test_calculate_price():
    assert calculate_price(100, 0.1) == 90
    assert calculate_price(100, 0.2) == 80
```

### Seams for Testing

**Creating Test Points**:

```python
# Bad: No seams
def send_notification(user):
    smtp = SMTPClient("smtp.example.com")  # Hardcoded
    smtp.send(user.email, "Welcome!")

# Good: Seam for testing
def send_notification(user, email_client=None):
    if email_client is None:
        email_client = SMTPClient("smtp.example.com")
    email_client.send(user.email, "Welcome!")

# Test with mock
def test_send_notification():
    mock_email = Mock()
    user = User(email="test@example.com")

    send_notification(user, mock_email)

    mock_email.send.assert_called_once_with(
        "test@example.com",
        "Welcome!"
    )
```

## Testing Strategies

### Unit Testing

**Scope**: Single function/method in isolation

**Example**:

```python
def calculate_shipping(weight, distance):
    base_rate = 5.0
    weight_rate = 0.5
    distance_rate = 0.1
    return base_rate + (weight * weight_rate) + (distance * distance_rate)

def test_shipping_light_package_short_distance():
    assert calculate_shipping(1, 10) == 6.5

def test_shipping_heavy_package_long_distance():
    assert calculate_shipping(10, 100) == 20.0

def test_shipping_zero_weight():
    assert calculate_shipping(0, 10) == 6.0
```

### Integration Testing

**Scope**: Multiple components together

**Example**:

```python
def test_user_registration_flow():
    # Setup
    db = TestDatabase()
    email_service = MockEmailService()
    user_service = UserService(db, email_service)

    # Execute
    user = user_service.register({
        "name": "Alice",
        "email": "alice@example.com",
        "password": "secure123"
    })

    # Verify database
    saved_user = db.get_user(user.id)
    assert saved_user.name == "Alice"

    # Verify email sent
    assert email_service.sent_count == 1
```

### Test-Driven Development (TDD)

**Red-Green-Refactor**:

1. **Red**: Write failing test

```python
def test_fibonacci_of_0_returns_0():
    assert fibonacci(0) == 0  # Fails
```

2. **Green**: Minimal code to pass

```python
def fibonacci(n):
    return 0  # Passes test
```

3. **Refactor**: Improve implementation

```python
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

4. **Repeat**: Add more tests

### Property-Based Testing

**Test Properties, Not Examples**:

```python
from hypothesis import given, strategies as st

# Property: Reversing twice returns original
@given(st.lists(st.integers()))
def test_reverse_twice_is_identity(items):
    assert list(reversed(list(reversed(items)))) == items

# Property: Sorting is idempotent
@given(st.lists(st.integers()))
def test_sort_idempotent(items):
    sorted_once = sorted(items)
    sorted_twice = sorted(sorted_once)
    assert sorted_once == sorted_twice
```

## Test Organization

### Directory Structure

```sh
tests/
  unit/
    test_user_service.py
    test_order_service.py
  integration/
    test_api.py
    test_database.py
  e2e/
    test_user_journey.py
  fixtures/
    sample_data.py
```

### Test Fixtures

**Reusable Test Data**:

```python
import pytest

@pytest.fixture
def sample_user():
    return User(
        id=1,
        name="Alice",
        email="alice@example.com",
        role="user"
    )

@pytest.fixture
def admin_user():
    return User(
        id=2,
        name="Bob",
        email="bob@example.com",
        role="admin"
    )

def test_user_permissions(sample_user):
    assert sample_user.can_view_dashboard() == True
    assert sample_user.can_access_admin() == False
```

### Parameterized Tests

**Avoid Duplication**:

```python
# Bad: Duplicated test logic
def test_valid_email_lowercase():
    assert is_valid_email("test@example.com") == True

def test_valid_email_uppercase():
    assert is_valid_email("TEST@EXAMPLE.COM") == True

# Good: Parameterized
@pytest.mark.parametrize("email", [
    "test@example.com",
    "TEST@EXAMPLE.COM",
    "test@mail.example.com",
    "user+tag@example.com"
])
def test_valid_email_formats(email):
    assert is_valid_email(email) == True
```

## Common Testability Issues

### God Objects

**Problem**: Too many responsibilities, hard to test

**Solution**:

```python
# Bad: God object
class Application:
    def __init__(self):
        self.db = Database()
        self.cache = Redis()
        # 10 more dependencies

# Good: Focused components
class OrderProcessor:
    def __init__(self, repository, notifier):
        self.repository = repository
        self.notifier = notifier
```

### Hidden Dependencies

**Problem**: Global state, environment dependencies

**Solution**:

```python
# Bad: Hidden dependency
def get_api_key():
    return os.environ["API_KEY"]

# Good: Explicit dependency
def get_api_key(config):
    return config.api_key

# Test
def test_get_api_key():
    config = Config(api_key="test-key")
    assert get_api_key(config) == "test-key"
```

### Non-Deterministic Code

**Problem**: Different results each run

**Solution**:

```python
# Bad: Non-deterministic
def generate_token():
    return random.randint(100000, 999999)

# Good: Deterministic with seed
def generate_token(seed=None):
    if seed is not None:
        random.seed(seed)
    return random.randint(100000, 999999)

# Test
def test_generate_token():
    assert generate_token(seed=42) == generate_token(seed=42)
```

## Testing Checklist

**Coverage**:

- Critical paths: 100%
- Business logic: 90%+
- Edge cases tested
- Error paths tested

**Quality**:

- Tests fast (< 10s for unit tests)
- Tests isolated
- Tests deterministic
- Clear test names
- AAA structure (Arrange-Act-Assert)

**Design**:

- Dependencies injected
- Pure functions where possible
- Seams for testing
- No global state
- No hidden dependencies

**Maintainability**:

- Organized by type
- Fixtures reused
- Parameterized tests
- No test duplication

## Best Practices

**Do**:

- Write tests for all new code
- Test edge cases thoroughly
- Test error paths
- Use descriptive test names
- Follow AAA pattern
- Keep tests independent
- Make tests fast
- Refactor for testability

**Don't**:

- Test implementation details
- Keep flaky tests
- Write slow tests
- Use unclear names
- Create test dependencies
- Over-mock (test behavior)
- Skip edge cases
- Ignore test failures

## See Also

Related code quality skills:

- code-reviewer: Test quality review
- code-maintainability: Testable code is maintainable
- code-refactor: Refactor with test safety net
- code-correctness: Tests verify correctness

Related foundational skills:

- first-principles: Understanding test requirements
- simplicity: Simple tests for simple code
- git: Coverage tracking over time
