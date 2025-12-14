# JavaScript Patterns - Design Patterns & Architectural Solutions

## Overview

JavaScript Patterns mode provides expert guidance on design patterns, architectural patterns, and proven solutions for JavaScript/TypeScript applications. It helps you recognize pattern opportunities, implement Gang of Four (GoF) patterns, organize code with module patterns, design event-driven systems, manage state effectively, and apply best practices for scalable, maintainable applications.

**Philosophy**: Patterns solve problems - apply design patterns when they reduce complexity, not for their own sake. Recognize when a pattern fits naturally.

## Requirements

**No external dependencies required**

Works with any JavaScript/TypeScript codebase. Patterns are implemented in vanilla JS/TS without framework dependencies.

## Core Capabilities

**Creational Patterns** (object creation):

- Singleton (single global instance)
- Factory (create objects without specifying exact class)
- Abstract Factory (family of related objects)
- Builder (construct complex objects step by step)
- Prototype (clone objects)

**Structural Patterns** (object composition):

- Adapter (interface conversion)
- Decorator (add behavior dynamically)
- Facade (simplified interface)
- Proxy (placeholder/wrapper)
- Composite (tree structures)
- Flyweight (share common state for efficiency)

**Behavioral Patterns** (object interaction):

- Observer (event subscription/notification)
- Strategy (interchangeable algorithms)
- Command (encapsulate requests as objects)
- Iterator (sequential access)
- State (behavior changes with state)
- Template Method (algorithm skeleton)
- Mediator (centralized communication)
- Chain of Responsibility (pass requests along chain)

**Module Patterns**:

- Module Pattern (IIFE-based encapsulation)
- Revealing Module Pattern (explicit public API)
- ES6 Modules (import/export)
- Namespace Pattern (avoid global pollution)
- Dependency Injection (inversion of control)

**Async Patterns**:

- Promise chaining
- Async/await patterns
- Parallel execution (Promise.all)
- Sequential execution
- Retry with backoff
- Circuit breaker
- Event aggregation

**Architectural Patterns**:

- MVC (Model-View-Controller)
- MVP (Model-View-Presenter)
- MVVM (Model-View-ViewModel)
- Flux/Redux (unidirectional data flow)
- Event-driven architecture
- Layered architecture
- Plugin architecture

**State Management Patterns**:

- Centralized store (Redux-style)
- State machines (finite state machine)
- Command pattern for state updates
- Observer pattern for subscriptions
- Immutable state updates

**Error Handling Patterns**:

- Try-catch boundaries
- Error middleware/interceptors
- Result type pattern (Either/Result)
- Error event emitters
- Global error handlers
- Graceful degradation

## When to Use

**Use Patterns Mode for**:

- Architectural decision making
- Recognizing opportunities to apply known patterns
- Refactoring to proven solutions
- Code organization and structure challenges
- Implementing reusable abstractions
- State management system design
- Event-driven system design
- Error handling architecture
- Plugin/extension systems
- Large-scale application structure

**Don't use Patterns Mode for**:

- Simple scripts (patterns add unnecessary complexity)
- Framework-specific patterns (React hooks, Vue composition API, etc.)
- Over-engineering small problems
- Language feature questions (use Language Mode)
- Performance optimization (use Performance Mode)

## Integration with Other Modes

**Language Mode**:

- Patterns use language features (modules, classes, closures, Proxies)
- Modern JavaScript enables cleaner pattern implementations
- Module systems affect module pattern choice

**Performance Mode**:

- Some patterns optimize performance (Flyweight, Object Pool, Lazy Loading)
- Pattern choice affects memory and CPU characteristics
- Caching patterns improve performance

**TypeScript Mode**:

- Types make pattern contracts explicit
- Generics enable type-safe pattern implementations
- Discriminated unions support State pattern

**Functional Mode**:

- Many patterns have functional equivalents (Strategy → HOF, Command → functions)
- Some patterns are inherently functional (Functor, Monad)
- Functional reactive programming patterns

## Standard Workflow

### 1. Identify Problem Domain

**Understand the challenge**:

- What problem needs solving?
- Is there a recurring pattern in the code?
- What are the requirements and constraints?
- What needs to be flexible or configurable?

### 2. Match Problem to Pattern

**Recognize pattern opportunities**:

- Creational: How are objects created?
- Structural: How are objects composed?
- Behavioral: How do objects interact?
- Is there an existing pattern that fits?

### 3. Implement Pattern

**Apply the pattern**:

- Follow pattern structure and intent
- Adapt to JavaScript/TypeScript idioms
- Keep it simple and understandable
- Document pattern usage and intent

### 4. Validate Design

**Ensure pattern solves problem**:

- Does it reduce complexity?
- Is code more maintainable?
- Are responsibilities clear?
- Is it easier to extend/modify?

### 5. Document and Communicate

**Make pattern usage clear**:

- Document which pattern is used and why
- Explain tradeoffs and alternatives
- Provide examples for team members

## Output Format

### Context

- What code/system was analyzed
- Architectural goals
- Current organization and structure

### Pattern Analysis

Categorized findings:

- **CRITICAL**: Missing critical patterns causing major issues (no error handling, global state chaos)
- **HIGH**: Significant pattern opportunities, code organization problems, unclear responsibilities
- **MEDIUM**: Moderate improvements, pattern refinement, better abstractions
- **LOW**: Minor pattern applications, naming improvements

### Pattern Recommendations

Specific patterns with implementation guidance:

```javascript
// Current: Tightly coupled code with scattered logic
function processData(data, formatType) {
  if (formatType === 'json') {
    return JSON.stringify(data);
  } else if (formatType === 'xml') {
    return convertToXML(data);
  } else if (formatType === 'csv') {
    return convertToCSV(data);
  }
}

// Pattern: Strategy Pattern (interchangeable algorithms)
class JSONFormatter {
  format(data) {
    return JSON.stringify(data);
  }
}

class XMLFormatter {
  format(data) {
    return convertToXML(data);
  }
}

class CSVFormatter {
  format(data) {
    return convertToCSV(data);
  }
}

class DataProcessor {
  constructor(formatter) {
    this.formatter = formatter;
  }

  process(data) {
    return this.formatter.format(data);
  }
}

// Usage
const processor = new DataProcessor(new JSONFormatter());
processor.process(data);
```

### Trade-offs

Explain benefits and costs:

- **Benefits**: Extensible (add new formatters without modifying processor), testable, follows Open/Closed principle
- **Costs**: More classes, slight verbosity
- **When to use**: Multiple algorithms, need to swap at runtime, want extensibility

### Verification

How to confirm pattern improves design:

- Code is easier to extend (add features without modifying existing code)
- Responsibilities are clearer
- Testing is simpler (test strategies in isolation)
- Team understands the pattern

## Examples

### Example 1: Singleton Pattern

**Ensure single instance**:

```javascript
// Using class and closure
const Database = (function() {
  let instance;

  class Database {
    constructor() {
      if (instance) {
        return instance;
      }
      this.connection = this.createConnection();
      instance = this;
    }

    createConnection() {
      return { /* connection object */ };
    }

    query(sql) {
      console.log(`Querying: ${sql}`);
      return this.connection;
    }
  }

  return Database;
})();

// Usage
const db1 = new Database();
const db2 = new Database();
console.log(db1 === db2); // true (same instance)

// Modern ES6+ approach using module
// database.js
let instance;

class Database {
  constructor() {
    if (instance) {
      throw new Error('Use Database.getInstance()');
    }
    this.connection = this.createConnection();
    instance = this;
  }

  static getInstance() {
    if (!instance) {
      instance = new Database();
    }
    return instance;
  }

  query(sql) {
    console.log(`Querying: ${sql}`);
  }
}

export default Database;
```

### Example 2: Factory Pattern

**Create objects without specifying exact class**:

```javascript
// Product interface (different user types)
class User {
  constructor(name) {
    this.name = name;
  }
}

class Admin extends User {
  constructor(name) {
    super(name);
    this.role = 'admin';
    this.permissions = ['read', 'write', 'delete'];
  }
}

class Guest extends User {
  constructor(name) {
    super(name);
    this.role = 'guest';
    this.permissions = ['read'];
  }
}

class Moderator extends User {
  constructor(name) {
    super(name);
    this.role = 'moderator';
    this.permissions = ['read', 'write'];
  }
}

// Factory
class UserFactory {
  createUser(type, name) {
    switch (type) {
      case 'admin':
        return new Admin(name);
      case 'moderator':
        return new Moderator(name);
      case 'guest':
        return new Guest(name);
      default:
        throw new Error(`Unknown user type: ${type}`);
    }
  }
}

// Usage
const factory = new UserFactory();
const admin = factory.createUser('admin', 'Alice');
const guest = factory.createUser('guest', 'Bob');

console.log(admin.permissions); // ['read', 'write', 'delete']
console.log(guest.permissions); // ['read']
```

### Example 3: Observer Pattern (Pub/Sub)

**Event subscription and notification**:

```javascript
class EventEmitter {
  constructor() {
    this.events = {};
  }

  on(event, callback) {
    if (!this.events[event]) {
      this.events[event] = [];
    }
    this.events[event].push(callback);
  }

  off(event, callback) {
    if (!this.events[event]) return;

    this.events[event] = this.events[event].filter(cb => cb !== callback);
  }

  emit(event, ...args) {
    if (!this.events[event]) return;

    this.events[event].forEach(callback => callback(...args));
  }

  once(event, callback) {
    const onceCallback = (...args) => {
      callback(...args);
      this.off(event, onceCallback);
    };
    this.on(event, onceCallback);
  }
}

// Usage
const emitter = new EventEmitter();

function handleUserLogin(user) {
  console.log(`User logged in: ${user.name}`);
}

emitter.on('user:login', handleUserLogin);
emitter.on('user:login', (user) => {
  console.log(`Welcome ${user.name}!`);
});

emitter.emit('user:login', { name: 'Alice' });
// User logged in: Alice
// Welcome Alice!

emitter.off('user:login', handleUserLogin);
emitter.emit('user:login', { name: 'Bob' });
// Welcome Bob! (only second listener fires)
```

### Example 4: Decorator Pattern

**Add behavior dynamically**:

```javascript
// Base component
class Coffee {
  cost() {
    return 5;
  }

  description() {
    return 'Coffee';
  }
}

// Decorators
class MilkDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }

  cost() {
    return this.coffee.cost() + 1;
  }

  description() {
    return this.coffee.description() + ', Milk';
  }
}

class SugarDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }

  cost() {
    return this.coffee.cost() + 0.5;
  }

  description() {
    return this.coffee.description() + ', Sugar';
  }
}

class WhipDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }

  cost() {
    return this.coffee.cost() + 2;
  }

  description() {
    return this.coffee.description() + ', Whipped Cream';
  }
}

// Usage
let coffee = new Coffee();
console.log(coffee.description(), '- $' + coffee.cost()); // Coffee - $5

coffee = new MilkDecorator(coffee);
console.log(coffee.description(), '- $' + coffee.cost()); // Coffee, Milk - $6

coffee = new SugarDecorator(coffee);
console.log(coffee.description(), '- $' + coffee.cost()); // Coffee, Milk, Sugar - $6.5

coffee = new WhipDecorator(coffee);
console.log(coffee.description(), '- $' + coffee.cost()); // Coffee, Milk, Sugar, Whipped Cream - $8.5
```

### Example 5: Command Pattern

**Encapsulate requests as objects**:

```javascript
// Receiver
class TextEditor {
  constructor() {
    this.text = '';
  }

  append(text) {
    this.text += text;
  }

  delete(length) {
    this.text = this.text.slice(0, -length);
  }

  getText() {
    return this.text;
  }
}

// Commands
class AppendCommand {
  constructor(editor, text) {
    this.editor = editor;
    this.text = text;
  }

  execute() {
    this.editor.append(this.text);
  }

  undo() {
    this.editor.delete(this.text.length);
  }
}

class DeleteCommand {
  constructor(editor, length) {
    this.editor = editor;
    this.length = length;
    this.deletedText = '';
  }

  execute() {
    this.deletedText = this.editor.getText().slice(-this.length);
    this.editor.delete(this.length);
  }

  undo() {
    this.editor.append(this.deletedText);
  }
}

// Invoker (with undo/redo)
class CommandHistory {
  constructor() {
    this.history = [];
    this.current = -1;
  }

  execute(command) {
    // Remove any commands after current position
    this.history = this.history.slice(0, this.current + 1);

    command.execute();
    this.history.push(command);
    this.current++;
  }

  undo() {
    if (this.current < 0) return;

    this.history[this.current].undo();
    this.current--;
  }

  redo() {
    if (this.current >= this.history.length - 1) return;

    this.current++;
    this.history[this.current].execute();
  }
}

// Usage
const editor = new TextEditor();
const history = new CommandHistory();

history.execute(new AppendCommand(editor, 'Hello '));
history.execute(new AppendCommand(editor, 'World'));
console.log(editor.getText()); // "Hello World"

history.undo();
console.log(editor.getText()); // "Hello "

history.undo();
console.log(editor.getText()); // ""

history.redo();
console.log(editor.getText()); // "Hello "

history.redo();
console.log(editor.getText()); // "Hello World"
```

### Example 6: State Pattern

**Behavior changes with state**:

```javascript
// States
class DraftState {
  publish(post) {
    post.state = new PublishedState();
    console.log('Post published');
  }

  delete(post) {
    console.log('Draft deleted');
  }
}

class PublishedState {
  publish(post) {
    console.log('Already published');
  }

  delete(post) {
    console.log('Cannot delete published post');
  }

  archive(post) {
    post.state = new ArchivedState();
    console.log('Post archived');
  }
}

class ArchivedState {
  publish(post) {
    post.state = new PublishedState();
    console.log('Post restored and published');
  }

  delete(post) {
    console.log('Archived post deleted');
  }
}

// Context
class BlogPost {
  constructor(title, content) {
    this.title = title;
    this.content = content;
    this.state = new DraftState(); // Initial state
  }

  publish() {
    this.state.publish(this);
  }

  delete() {
    this.state.delete(this);
  }

  archive() {
    this.state.archive(this);
  }
}

// Usage
const post = new BlogPost('My Post', 'Content here');

post.publish();  // Post published
post.archive();  // Post archived
post.delete();   // Archived post deleted
```

### Example 7: Module Pattern (Revealing Module)

**Encapsulation with public API**:

```javascript
const UserModule = (function() {
  // Private variables
  let users = [];
  let nextId = 1;

  // Private functions
  function validateUser(user) {
    if (!user.name || !user.email) {
      throw new Error('Invalid user data');
    }
  }

  function findUserIndex(id) {
    return users.findIndex(user => user.id === id);
  }

  // Public API
  return {
    addUser(name, email) {
      const user = { id: nextId++, name, email };
      validateUser(user);
      users.push(user);
      return user;
    },

    getUser(id) {
      return users.find(user => user.id === id);
    },

    getAllUsers() {
      return [...users]; // Return copy
    },

    updateUser(id, updates) {
      const index = findUserIndex(id);
      if (index === -1) {
        throw new Error('User not found');
      }
      users[index] = { ...users[index], ...updates };
      validateUser(users[index]);
      return users[index];
    },

    deleteUser(id) {
      const index = findUserIndex(id);
      if (index === -1) {
        throw new Error('User not found');
      }
      return users.splice(index, 1)[0];
    }
  };
})();

// Usage
const user1 = UserModule.addUser('Alice', 'alice@example.com');
const user2 = UserModule.addUser('Bob', 'bob@example.com');

console.log(UserModule.getAllUsers()); // [{ id: 1, ... }, { id: 2, ... }]
console.log(UserModule.getUser(1));    // { id: 1, name: 'Alice', ... }

// Cannot access private variables
console.log(UserModule.users);  // undefined
```

## Design Pattern Checklist

**When choosing a pattern, ask**:

- Does this pattern solve my specific problem?
- Will it reduce complexity or add unnecessary abstraction?
- Do team members understand this pattern?
- Is the pattern appropriate for the problem scale?
- Are there simpler alternatives?

**Common anti-patterns to avoid**:

- Over-engineering simple problems
- Using patterns for the sake of using patterns
- Copy-pasting patterns without understanding
- Ignoring JavaScript idioms in favor of classical OOP
- Creating unnecessary abstraction layers

## References

**Related Skills**:

- Main JavaScript skill: `data/javascript/SKILL.md`
- Language Mode: `data/javascript/language/SKILL.md`
- Performance Mode: `data/javascript/performance/SKILL.md`
- TypeScript Mode: `data/javascript/typescript/SKILL.md`
- Functional Mode: `data/javascript/functional/SKILL.md`

**Resources**:

- "Learning JavaScript Design Patterns" by Addy Osmani
- "Design Patterns: Elements of Reusable Object-Oriented Software" (Gang of Four)
- Refactoring.Guru (visual pattern catalog)
- "JavaScript Patterns" by Stoyan Stefanov
- patterns.dev (modern web patterns)
