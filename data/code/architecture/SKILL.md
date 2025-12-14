---
name: skill-code-architecture
description: Design patterns and structural quality assessment. Evaluates SOLID principles adherence, design pattern appropriateness, architectural style consistency, and anti-pattern detection. Ensures code structure supports long-term maintainability, extensibility, and scalability.
allowed-tools: Read
---

# Code Architecture - Design Patterns and Structure

## Overview

Code Architecture evaluates code structure, design patterns, and long-term maintainability. It assesses adherence to SOLID principles, appropriate use of design patterns, architectural style consistency, and identifies structural anti-patterns.

Good architecture makes systems easy to understand, develop, maintain, and deploy. It enables teams to move quickly while keeping code quality high.

**Philosophy**: "Good architecture makes the system easy to change" - optimize for flexibility and clarity, not just current requirements.

## Requirements

No external dependencies required.

Applies universal architectural principles across all programming languages and paradigms.

## Core Capabilities

**SOLID Principles Assessment**:

- Single Responsibility Principle
- Open/Closed Principle
- Liskov Substitution Principle
- Interface Segregation Principle
- Dependency Inversion Principle

**Design Pattern Evaluation**:

- Creational patterns (Factory, Singleton, Builder)
- Structural patterns (Adapter, Decorator, Facade)
- Behavioral patterns (Strategy, Observer, Command)
- Pattern appropriateness and anti-pattern detection

**Architectural Style Analysis**:

- Layered architecture
- Hexagonal architecture (Ports & Adapters)
- Microservices architecture
- Event-driven architecture
- Consistency and clarity of style

**Anti-Pattern Detection**:

- God objects
- Big ball of mud
- Leaky abstractions
- Circular dependencies
- Inappropriate coupling

**Structural Quality**:

- Low coupling, high cohesion
- Clear module boundaries
- Appropriate abstraction levels
- Dependency direction

## When to Use

**Use architecture for**:

- Design review of new features
- Evaluating system structure
- Identifying architectural debt
- Reviewing design patterns usage
- Assessing scalability readiness
- Architecture decision records (ADRs)
- Long-term maintainability evaluation

**Don't use architecture for**:

- Line-by-line code review (use code-reviewer)
- Quick bug fixes
- Minor refactoring
- Style issues

## Integration with Other Skills

**first-principles**: Design validation

- Question why patterns are used
- Verify pattern solves actual problem
- Avoid cargo-cult architecture
- Example: "Why use Factory here? Is polymorphism actually needed?"

**simplicity**: Avoiding over-engineering

- Choose boring, proven patterns
- Start simple, evolve as needed
- Avoid unnecessary abstraction
- Example: "Strategy pattern for two algorithms is over-engineered"

**git**: Architectural hotspots

- Identify frequently changed files
- Track coupling through co-change analysis
- Visualize module dependencies over time
- Example: "These files always change together - hidden coupling"

**tech-stack-advisor**: Framework patterns

- Framework-specific architectural patterns
- Language idioms and conventions
- Technology best practices
- Example: "Django encourages fat models, thin views"

## SOLID Principles

### Single Responsibility Principle (SRP)

**Each class has one reason to change**:

```python
# Bad: Multiple responsibilities
class UserManager:
    def create_user(self, data):
        self.validate_email(data['email'])  # Validation
        self.db.insert(data)                # Persistence
        self.send_welcome_email(data)       # Notification
        self.track_signup(data)             # Analytics

# Good: Single responsibility
class UserService:
    def __init__(self, validator, repository, notifier, analytics):
        self.validator = validator
        self.repository = repository
        self.notifier = notifier
        self.analytics = analytics

    def create_user(self, data):
        user = self.validator.validate(data)
        user = self.repository.save(user)
        self.notifier.send_welcome(user)
        self.analytics.track_signup(user)
        return user
```

### Open/Closed Principle (OCP)

**Open for extension, closed for modification**:

```python
# Bad: Must modify for new types
class PaymentProcessor:
    def process(self, payment_type, amount):
        if payment_type == "credit_card":
            # Process credit card
        elif payment_type == "paypal":
            # Process PayPal
        # Adding new type requires modification

# Good: Extend without modification
class PaymentProcessor:
    def __init__(self):
        self.processors = {}

    def register(self, payment_type, processor):
        self.processors[payment_type] = processor

    def process(self, payment_type, amount):
        return self.processors[payment_type].process(amount)

# New types extend without modifying existing code
class CreditCardProcessor:
    def process(self, amount):
        ...

class PayPalProcessor:
    def process(self, amount):
        ...
```

### Liskov Substitution Principle (LSP)

**Subtypes must be substitutable for base types**:

```python
# Bad: Square violates Rectangle contract
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

class Square(Rectangle):
    def set_width(self, width):
        self.width = width
        self.height = width  # Violates LSP

# Good: Separate hierarchies
class Shape:
    def area(self):
        raise NotImplementedError

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

class Square(Shape):
    def __init__(self, size):
        self.size = size

    def area(self):
        return self.size * self.size
```

### Interface Segregation Principle (ISP)

**No client should depend on methods it doesn't use**:

```python
# Bad: Fat interface
class Worker:
    def work(self):
        pass
    def eat(self):
        pass
    def sleep(self):
        pass

class Robot(Worker):
    def eat(self):
        raise NotImplementedError  # Robots don't eat

# Good: Segregated interfaces
class Workable:
    def work(self):
        pass

class Eatable:
    def eat(self):
        pass

class Human(Workable, Eatable):
    def work(self):
        return "Working"
    def eat(self):
        return "Eating"

class Robot(Workable):
    def work(self):
        return "Working"
```

### Dependency Inversion Principle (DIP)

**Depend on abstractions, not concretions**:

```python
# Bad: Depends on concrete implementation
class UserService:
    def __init__(self):
        self.email = EmailService()  # Concrete class

# Good: Depends on abstraction
from abc import ABC, abstractmethod

class Notifier(ABC):
    @abstractmethod
    def send(self, to, message):
        pass

class EmailNotifier(Notifier):
    def send(self, to, message):
        # Email implementation
        pass

class SMSNotifier(Notifier):
    def send(self, to, message):
        # SMS implementation
        pass

class UserService:
    def __init__(self, notifier: Notifier):
        self.notifier = notifier  # Abstraction
```

## Design Patterns

### Creational Patterns

**Factory Pattern**:

```python
class ShippingFactory:
    @staticmethod
    def create(shipping_type):
        if shipping_type == "standard":
            return StandardShipping()
        elif shipping_type == "express":
            return ExpressShipping()
        raise ValueError(f"Unknown type: {shipping_type}")
```

**Singleton Pattern**:

```python
class DatabaseConnection:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance.connect()
        return cls._instance
```

**Builder Pattern**:

```python
class QueryBuilder:
    def __init__(self):
        self._select = []
        self._from = None
        self._where = []

    def select(self, *columns):
        self._select.extend(columns)
        return self

    def from_table(self, table):
        self._from = table
        return self

    def where(self, condition):
        self._where.append(condition)
        return self

    def build(self):
        query = f"SELECT {', '.join(self._select)} FROM {self._from}"
        if self._where:
            query += f" WHERE {' AND '.join(self._where)}"
        return query
```

### Structural Patterns

**Adapter Pattern**:

```python
class LegacyPaymentGateway:
    def make_payment(self, amount_in_cents):
        # Legacy API uses cents
        pass

class PaymentAdapter:
    def __init__(self, legacy_gateway):
        self.legacy = legacy_gateway

    def process(self, amount_in_dollars):
        amount_in_cents = int(amount_in_dollars * 100)
        return self.legacy.make_payment(amount_in_cents)
```

**Decorator Pattern**:

```python
class Coffee:
    def cost(self):
        return 2.0

class MilkDecorator:
    def __init__(self, coffee):
        self._coffee = coffee

    def cost(self):
        return self._coffee.cost() + 0.5
```

### Behavioral Patterns

**Strategy Pattern**:

```python
class SortStrategy(ABC):
    @abstractmethod
    def sort(self, data):
        pass

class QuickSort(SortStrategy):
    def sort(self, data):
        # QuickSort implementation
        pass

class Sorter:
    def __init__(self, strategy: SortStrategy):
        self.strategy = strategy

    def sort(self, data):
        return self.strategy.sort(data)
```

**Observer Pattern**:

```python
class EventManager:
    def __init__(self):
        self._subscribers = []

    def subscribe(self, subscriber):
        self._subscribers.append(subscriber)

    def notify(self, event):
        for subscriber in self._subscribers:
            subscriber.update(event)
```

**Command Pattern**:

```python
class Command(ABC):
    @abstractmethod
    def execute(self):
        pass

    @abstractmethod
    def undo(self):
        pass

class AddUserCommand(Command):
    def __init__(self, repository, user_data):
        self.repository = repository
        self.user_data = user_data

    def execute(self):
        self.user_id = self.repository.add(self.user_data)

    def undo(self):
        self.repository.delete(self.user_id)
```

## Architectural Styles

### Layered Architecture

```md
┌─────────────────────┐
│  Presentation       │  Controllers, UI
├─────────────────────┤
│  Business Logic     │  Services, Domain
├─────────────────────┤
│  Data Access        │  Repositories
├─────────────────────┤
│  Database           │  PostgreSQL, etc.
└─────────────────────┘
```

**Example**:

```python
# Presentation Layer
class UserController:
    def __init__(self, user_service):
        self.user_service = user_service

    def create_user(self, request):
        user = self.user_service.register(request.data)
        return {"id": user.id, "name": user.name}

# Business Logic Layer
class UserService:
    def __init__(self, repository, email_service):
        self.repository = repository
        self.email = email_service

    def register(self, data):
        user = User(**data)
        user = self.repository.save(user)
        self.email.send_welcome(user)
        return user

# Data Access Layer
class UserRepository:
    def save(self, user):
        return self.db.insert("users", user)
```

### Hexagonal Architecture (Ports & Adapters)

```md
┌───────────────────────────┐
│    Application Core       │
│  (Business Logic)         │
│                           │
│  ┌─────┐     ┌─────┐      │
│  │Port │     │Port │      │
│  └─────┘     └─────┘      │
└─────┬─────────┬───────────┘
      │         │
 ┌────▼────┐ ┌─▼──────┐
 │Adapter  │ │Adapter │
 │ (HTTP)  │ │ (DB)   │
 └─────────┘ └────────┘
```

**Example**:

```python
# Core Domain
class OrderService:
    def __init__(self, order_repository, payment_gateway):
        self.repository = order_repository  # Port
        self.payment = payment_gateway       # Port

    def place_order(self, order):
        order = self.repository.save(order)
        self.payment.charge(order.total)
        return order

# Adapters
class PostgresOrderRepository:
    def save(self, order):
        # PostgreSQL implementation
        pass

class StripePaymentGateway:
    def charge(self, amount):
        # Stripe API implementation
        pass
```

### Microservices Architecture

```md
┌──────────┐    ┌──────────┐    ┌──────────┐
│  User    │◄──►│  Order   │◄──►│ Payment  │
│ Service  │    │ Service  │    │ Service  │
└────┬─────┘    └────┬─────┘    └────┬─────┘
     │               │               │
     ▼               ▼               ▼
  [User DB]      [Order DB]     [Payment DB]
```

**Key Principles**:

- Each service owns its data
- Communicate via APIs (REST, gRPC, events)
- Independent deployment
- Decentralized governance

## Anti-Patterns

### God Object

**Problem**: One class does everything

**Solution**: Split by responsibility

```python
# Bad: God object
class Application:
    # 50 dependencies, 30 methods, 2000 lines

# Good: Focused services
class UserService:
    # User-related operations only

class OrderService:
    # Order-related operations only
```

### Big Ball of Mud

**Problem**: No clear structure, tangled dependencies

**Solution**: Introduce layered or hexagonal architecture incrementally

### Leaky Abstraction

**Problem**: Implementation details leak through

```python
# Bad: Leaky abstraction
class UserRepository:
    def find_by_id(self, user_id):
        return session.query(User).filter_by(id=user_id).first()
        # Returns SQLAlchemy model (implementation detail)

# Good: Proper abstraction
class UserRepository:
    def find_by_id(self, user_id):
        user_row = session.query(User).filter_by(id=user_id).first()
        return User(id=user_row.id, name=user_row.name)
        # Returns domain model
```

### Circular Dependencies

**Problem**: A depends on B, B depends on A

**Solution**: Introduce interface or shared module

## Architecture Review Checklist

**SOLID Principles**:

- Single Responsibility: One reason to change
- Open/Closed: Extend without modifying
- Liskov Substitution: Subtypes substitutable
- Interface Segregation: No fat interfaces
- Dependency Inversion: Depend on abstractions

**Design Patterns**:

- Patterns used appropriately
- Pattern intent matches use case
- No pattern abuse

**Structure**:

- Clear module boundaries
- Low coupling, high cohesion
- No circular dependencies
- Consistent abstraction levels

**Scalability**:

- Stateless where possible
- Database scalable
- Caching strategy
- Async processing for slow operations

## Best Practices

**Do**:

- Apply SOLID principles
- Use patterns when they fit
- Keep abstractions clean
- Design for change
- Document architectural decisions (ADRs)
- Review architecture regularly
- Start simple, evolve as needed
- Focus on clarity and maintainability

**Don't**:

- Over-engineer (YAGNI)
- Use patterns without understanding
- Create god objects
- Allow circular dependencies
- Leak implementation details
- Copy architectures without context
- Optimize prematurely
- Add complexity without clear benefit

## See Also

Related code quality skills:

- code-maintainability: Architecture affects maintainability
- code-refactor: Refactor toward better architecture
- code-auditor: Architectural debt assessment
- code-testability: Architecture affects testability

Related foundational skills:

- first-principles: Design validation
- simplicity: Avoiding over-engineering
- git: Identifying architectural hotspots
- tech-stack-advisor: Framework-specific patterns
