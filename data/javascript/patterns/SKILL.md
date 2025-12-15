# JavaScript Patterns - Design Patterns & Architectural Solutions

## Overview

JavaScript Patterns mode provides expert guidance on design patterns, architectural patterns, and proven solutions for JavaScript/TypeScript applications. It helps you recognize pattern opportunities, implement Gang of Four (GoF) patterns, organize code with module patterns, design event-driven systems, manage state effectively, and apply best practices for scalable, maintainable applications.

**Philosophy**: Patterns solve problems - apply design patterns when they reduce complexity, not for their own sake. **Prioritize functional patterns with composable functions and simple data structures over class-based object-oriented approaches**. Recognize when a pattern fits naturally.

## Requirements

**No external dependencies required**

Works with any JavaScript/TypeScript codebase. Patterns are implemented in vanilla JS/TS without framework dependencies.

## Core Capabilities

**Functional Patterns** (PREFERRED - composable functions):

- Higher-Order Functions (functions that take/return functions)
- Function Composition (combine small functions into larger ones)
- Currying & Partial Application (pre-fill function arguments)
- Pure Functions (no side effects, predictable outputs)
- Immutable Data Transformations (map, filter, reduce)
- Function Pipelines (data flows through transformations)
- Closures for Encapsulation (private state without classes)
- Strategy via Functions (pass behavior as functions)
- Decorator via Higher-Order Functions (wrap functions to add behavior)

**Data Structure Patterns** (simple, composable):

- Plain Objects (simple key-value maps)
- Arrays & Array Methods (map, filter, reduce, etc.)
- Unique values (Set)
- Discriminated Unions (tagged objects for state)
- Nested Data Structures (trees, graphs as plain objects/arrays)
- Record/Dictionary Patterns (lookups without classes)

**Classical OOP Patterns** (use sparingly when functional approach is insufficient):

- Factory Functions (preferred over classes for object creation)
- Singleton via Module (single instance through module system)
- Observer/PubSub (event subscription)
- Adapter (interface conversion)
- Proxy (wrapper for control/enhancement)
- State Machine (using discriminated unions preferred)

**Module Patterns**:

- ES6 Modules (import/export - PREFERRED)
- Module as Namespace (group related functions)
- Dependency Injection via Function Parameters (pass dependencies explicitly)
- Closures for Module Privacy (avoid classes when possible)

**Async Patterns** (functional approach):

- Promise composition (chaining transformations)
- Async/await with pure functions
- Parallel execution (Promise.all, Promise.allSettled)
- Sequential execution via reduce or for-await
- Retry with backoff (higher-order function)
- Error handling with Either/Result types
- Async pipelines

**Architectural Patterns** (functional-first approach):

- Unidirectional Data Flow (Flux/Redux with reducers as pure functions)
- Event-driven with Functions (pub/sub using plain functions)
- Layered architecture with Function Modules
- Plugin via Higher-Order Functions
- Composable Middleware (function composition)
- Functional Core, Imperative Shell (isolate I/O from logic)

**State Management Patterns** (immutable, functional):

- Reducers (pure functions: (state, action) => newState)
- State machines using discriminated unions
- Immutable updates (spread operators, structural sharing)
- Lenses/Optics for nested updates
- Event sourcing (state derived from event history)
- Transducers for efficient state transformations

**Error Handling Patterns** (functional approach):

- Result/Either types ({ ok: true, value } or { ok: false, error })
- Maybe/Option types (handle null/undefined functionally)
- Railway-oriented programming (chain Result types)
- Error middleware as pure functions
- Explicit error returns (avoid throwing when possible)
- Validation with composable validators

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

**Functional Mode** (PREFERRED INTEGRATION):

- **This mode now prioritizes functional patterns by default**
- Classical OOP patterns have simpler functional equivalents:
  - Strategy → Functions as first-class values
  - Decorator → Higher-order functions
  - Command → Plain objects with functions
  - Factory → Factory functions returning plain objects
  - State → Discriminated unions with pure transitions
- Functional patterns: Functor, Monad, Lens, Transducer
- Functional reactive programming patterns
- **Consult Functional Mode for deeper FP concepts**

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

// PREFERRED: Functional Strategy Pattern (functions as first-class values)
const formatters = {
  json: (data) => JSON.stringify(data),
  xml: (data) => convertToXML(data),
  csv: (data) => convertToCSV(data)
};

// Simple data processing with function lookup
const processData = (data, formatType) => {
  const formatter = formatters[formatType];
  if (!formatter) throw new Error(`Unknown format: ${formatType}`);
  return formatter(data);
};

// Or with higher-order function for partial application
const createProcessor = (formatter) => (data) => formatter(data);

// Usage
const jsonProcessor = createProcessor(formatters.json);
jsonProcessor(data);

// Or direct invocation
processData(data, 'json');
```

### Trade-offs

Explain benefits and costs:

- **Benefits**: Simple data structures, extensible (add formatters to object), testable (pure functions), composable, no class overhead
- **Costs**: Minimal - just plain objects and functions
- **When to use**: Multiple algorithms, need to swap at runtime, want extensibility without classes

### Verification

How to confirm pattern improves design:

- Code is easier to extend (add features without modifying existing code)
- Responsibilities are clearer
- Testing is simpler (test strategies in isolation)
- Team understands the pattern

## Examples

### Example 1: Function Composition & Pipelines

**Build complex transformations from simple functions**:

```javascript
// Simple composable functions
const addTax = (rate) => (price) => price * (1 + rate);
const applyDiscount = (discount) => (price) => price * (1 - discount);
const formatCurrency = (price) => `$${price.toFixed(2)}`;

// Compose functions (right to left)
const compose = (...fns) => (x) => fns.reduceRight((acc, fn) => fn(acc), x);

// Pipe functions (left to right - more readable)
const pipe = (...fns) => (x) => fns.reduce((acc, fn) => fn(acc), x);

// Build pricing pipeline
const calculateFinalPrice = pipe(
  applyDiscount(0.1),    // 10% discount
  addTax(0.08),          // 8% tax
  formatCurrency         // format as currency
);

// Usage
const basePrice = 100;
console.log(calculateFinalPrice(basePrice)); // "$99.00"

// Data transformations with pipelines
const processUserData = pipe(
  (users) => users.filter(u => u.active),
  (users) => users.map(u => ({ ...u, name: u.name.toUpperCase() })),
  (users) => users.sort((a, b) => a.name.localeCompare(b.name))
);

const users = [
  { name: 'alice', active: true },
  { name: 'bob', active: false },
  { name: 'charlie', active: true }
];

console.log(processUserData(users));
// [{ name: 'ALICE', active: true }, { name: 'CHARLIE', active: true }]
```

### Example 2: Higher-Order Functions for Patterns

**Decorator pattern without classes**:

```javascript
// Base function
const greet = (name) => `Hello, ${name}!`;

// Decorators as higher-order functions
const withLogging = (fn) => (...args) => {
  console.log(`Calling with args:`, args);
  const result = fn(...args);
  console.log(`Result:`, result);
  return result;
};

const withTiming = (fn) => (...args) => {
  const start = Date.now();
  const result = fn(...args);
  console.log(`Execution time: ${Date.now() - start}ms`);
  return result;
};

const withCache = (fn) => {
  const cache = new Map();
  return (...args) => {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
};

// Compose decorators
const enhancedGreet = pipe(
  withCache,
  withTiming,
  withLogging
)(greet);

// Usage
enhancedGreet('Alice');
```

### Example 3: State Management with Plain Objects

**Immutable state updates with reducers**:

```javascript
// State is plain object
const initialState = {
  count: 0,
  todos: [],
  user: null
};

// Actions are plain objects with type discriminator
const createAction = (type, payload) => ({ type, payload });

// Reducer is pure function: (state, action) => newState
const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };

    case 'ADD_TODO':
      return {
        ...state,
        todos: [...state.todos, action.payload]
      };

    case 'TOGGLE_TODO':
      return {
        ...state,
        todos: state.todos.map(todo =>
          todo.id === action.payload
            ? { ...todo, completed: !todo.completed }
            : todo
        )
      };

    case 'SET_USER':
      return { ...state, user: action.payload };

    default:
      return state;
  }
};

// Simple store using closures
const createStore = (reducer, initialState) => {
  let state = initialState;
  let listeners = [];

  return {
    getState: () => state,
    dispatch: (action) => {
      state = reducer(state, action);
      listeners.forEach(listener => listener(state));
    },
    subscribe: (listener) => {
      listeners.push(listener);
      return () => {
        listeners = listeners.filter(l => l !== listener);
      };
    }
  };
};

// Usage
const store = createStore(reducer, initialState);

store.subscribe((state) => {
  console.log('State updated:', state);
});

store.dispatch(createAction('INCREMENT'));
store.dispatch(createAction('ADD_TODO', { id: 1, text: 'Learn FP', completed: false }));
store.dispatch(createAction('TOGGLE_TODO', 1));
```

### Example 4: Result/Either Type for Error Handling

**Functional error handling without exceptions**:

```javascript
// Result type - simple discriminated union
const Ok = (value) => ({ ok: true, value });
const Err = (error) => ({ ok: false, error });

// Helper functions for Result type
const map = (fn) => (result) =>
  result.ok ? Ok(fn(result.value)) : result;

const flatMap = (fn) => (result) =>
  result.ok ? fn(result.value) : result;

const getOrElse = (defaultValue) => (result) =>
  result.ok ? result.value : defaultValue;

// Example usage - parse and validate user input
const parseJSON = (str) => {
  try {
    return Ok(JSON.parse(str));
  } catch (e) {
    return Err(`Invalid JSON: ${e.message}`);
  }
};

const validateUser = (user) =>
  user.name && user.email
    ? Ok(user)
    : Err('User must have name and email');

const normalizeUser = (user) =>
  Ok({ ...user, name: user.name.toLowerCase() });

// Compose operations that may fail
const processUserInput = pipe(
  parseJSON,
  flatMap(validateUser),
  flatMap(normalizeUser)
);

// Usage
const validInput = '{"name":"Alice","email":"alice@example.com"}';
const invalidInput = '{"name":"Bob"}';

const result1 = processUserInput(validInput);
console.log(result1); // Ok({ name: "alice", email: "alice@example.com" })

const result2 = processUserInput(invalidInput);
console.log(result2); // Err("User must have name and email")

// Extract value with default
console.log(getOrElse({ name: 'guest', email: '' })(result2));
```

### Example 5: Currying & Partial Application

**Pre-configure functions for reuse**:

```javascript
// Generic curry utility
const curry = (fn) => {
  const arity = fn.length;
  return function curried(...args) {
    if (args.length >= arity) return fn(...args);
    return (...moreArgs) => curried(...args, ...moreArgs);
  };
};

// Curried functions
const add = curry((a, b) => a + b);
const multiply = curry((a, b) => a * b);
const map = curry((fn, arr) => arr.map(fn));
const filter = curry((pred, arr) => arr.filter(pred));

// Create specialized functions via partial application
const add10 = add(10);
const double = multiply(2);
const isEven = (n) => n % 2 === 0;

console.log(add10(5));  // 15
console.log(double(5)); // 10

// Compose with curried functions
const numbers = [1, 2, 3, 4, 5];

const evenDoubled = pipe(
  filter(isEven),
  map(double)
);

console.log(evenDoubled(numbers)); // [4, 8]

// Point-free style (no explicit arguments)
const getActiveUserNames = pipe(
  filter(user => user.active),
  map(user => user.name),
  map(name => name.toUpperCase())
);

const users = [
  { name: 'alice', active: true },
  { name: 'bob', active: false },
  { name: 'charlie', active: true }
];

console.log(getActiveUserNames(users)); // ['ALICE', 'CHARLIE']
```

---

## Classical OOP Examples (Use When Functional Approach is Insufficient)

### Example 6: Singleton Pattern

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

### Example 7: Factory Pattern (Functional Alternative: Factory Functions)

**Prefer factory functions returning plain objects**:

```javascript
// Factory functions (no classes needed)
const createAdmin = (name) => ({
  name,
  role: 'admin',
  permissions: ['read', 'write', 'delete']
});

const createGuest = (name) => ({
  name,
  role: 'guest',
  permissions: ['read']
});

const createModerator = (name) => ({
  name,
  role: 'moderator',
  permissions: ['read', 'write']
});

// Factory lookup (simple object mapping)
const userFactories = {
  admin: createAdmin,
  guest: createGuest,
  moderator: createModerator
};

// Generic factory function
const createUser = (type, name) => {
  const factory = userFactories[type];
  if (!factory) throw new Error(`Unknown user type: ${type}`);
  return factory(name);
};

// Usage
const admin = createUser('admin', 'Alice');
const guest = createUser('guest', 'Bob');

console.log(admin.permissions); // ['read', 'write', 'delete']
console.log(guest.permissions); // ['read']

// Easy to add behavior via composition
const withTimestamp = (user) => ({
  ...user,
  createdAt: new Date()
});

const adminWithTimestamp = withTimestamp(createAdmin('Charlie'));
```

### Example 8: Observer Pattern (Functional Pub/Sub)

**Event subscription with closures (no classes)**:

```javascript
// Functional event emitter using closures
const createEventEmitter = () => {
  const events = {};

  return {
    on: (event, callback) => {
      if (!events[event]) events[event] = [];
      events[event].push(callback);

      // Return unsubscribe function
      return () => {
        events[event] = events[event].filter(cb => cb !== callback);
      };
    },

    emit: (event, ...args) => {
      if (!events[event]) return;
      events[event].forEach(callback => callback(...args));
    },

    once: (event, callback) => {
      const onceCallback = (...args) => {
        callback(...args);
        events[event] = events[event].filter(cb => cb !== onceCallback);
      };
      if (!events[event]) events[event] = [];
      events[event].push(onceCallback);
    }
  };
};

// Usage
const emitter = createEventEmitter();

const handleUserLogin = (user) => {
  console.log(`User logged in: ${user.name}`);
};

// Subscribe and get unsubscribe function
const unsubscribe = emitter.on('user:login', handleUserLogin);
emitter.on('user:login', (user) => {
  console.log(`Welcome ${user.name}!`);
});

emitter.emit('user:login', { name: 'Alice' });
// User logged in: Alice
// Welcome Alice!

// Clean unsubscribe
unsubscribe();
emitter.emit('user:login', { name: 'Bob' });
// Welcome Bob! (only second listener fires)
```

### Example 9: Command Pattern (Functional Alternative)

**Commands as plain objects with functions**:

```javascript
// State is simple - just the text
let editorText = '';

// Command creators return plain objects
const createAppendCommand = (text) => ({
  type: 'APPEND',
  text,
  execute: (state) => state + text,
  undo: (state) => state.slice(0, -text.length)
});

const createDeleteCommand = (length) => ({
  type: 'DELETE',
  length,
  execute: (state) => state.slice(0, -length),
  undo: (state, deletedText) => state + deletedText
});

// Command history with closures
const createCommandHistory = (initialState) => {
  let state = initialState;
  let history = [];
  let current = -1;

  return {
    getState: () => state,

    execute: (command) => {
      // Remove any commands after current position
      history = history.slice(0, current + 1);

      const previousState = state;
      state = command.execute(state);

      history.push({ command, previousState });
      current++;
    },

    undo: () => {
      if (current < 0) return;

      const { command, previousState } = history[current];
      state = previousState;
      current--;
    },

    redo: () => {
      if (current >= history.length - 1) return;

      current++;
      const { command } = history[current];
      state = command.execute(state);
    }
  };
};

// Usage
const history = createCommandHistory('');

history.execute(createAppendCommand('Hello '));
history.execute(createAppendCommand('World'));
console.log(history.getState()); // "Hello World"

history.undo();
console.log(history.getState()); // "Hello "

history.undo();
console.log(history.getState()); // ""

history.redo();
console.log(history.getState()); // "Hello "

history.redo();
console.log(history.getState()); // "Hello World"
```

### Example 10: State Machine (Functional with Discriminated Unions)

**State transitions with plain objects**:

```javascript
// States as discriminated unions (tagged objects)
const states = {
  draft: () => ({ status: 'draft' }),
  published: () => ({ status: 'published' }),
  archived: () => ({ status: 'archived' })
};

// State transitions as pure functions
const transitions = {
  publish: (post) => {
    if (post.state.status === 'draft') {
      console.log('Post published');
      return { ...post, state: states.published() };
    }
    if (post.state.status === 'archived') {
      console.log('Post restored and published');
      return { ...post, state: states.published() };
    }
    console.log('Already published');
    return post;
  },

  archive: (post) => {
    if (post.state.status === 'published') {
      console.log('Post archived');
      return { ...post, state: states.archived() };
    }
    console.log('Can only archive published posts');
    return post;
  },

  delete: (post) => {
    if (post.state.status === 'draft') {
      console.log('Draft deleted');
      return null;
    }
    if (post.state.status === 'archived') {
      console.log('Archived post deleted');
      return null;
    }
    console.log('Cannot delete published post');
    return post;
  }
};

// Factory function for posts
const createBlogPost = (title, content) => ({
  title,
  content,
  state: states.draft()
});

// Usage
let post = createBlogPost('My Post', 'Content here');

post = transitions.publish(post);  // Post published
post = transitions.archive(post);  // Post archived
post = transitions.delete(post);   // Archived post deleted (returns null)

// Alternative: State machine with reducer pattern
const postReducer = (post, action) => {
  switch (action.type) {
    case 'PUBLISH': return transitions.publish(post);
    case 'ARCHIVE': return transitions.archive(post);
    case 'DELETE': return transitions.delete(post);
    default: return post;
  }
};

let post2 = createBlogPost('Another Post', 'More content');
post2 = postReducer(post2, { type: 'PUBLISH' });
post2 = postReducer(post2, { type: 'ARCHIVE' });
```

### Example 11: Module Pattern (ES6 Modules Preferred)

**Encapsulation with ES6 modules (preferred) or closures**:

```javascript
// PREFERRED: ES6 Module (userModule.js)
// Private state and functions (not exported)
let users = [];
let nextId = 1;

const validateUser = (user) => {
  if (!user.name || !user.email) {
    throw new Error('Invalid user data');
  }
};

const findUserIndex = (id) =>
  users.findIndex(user => user.id === id);

// Public API (exported functions)
export const addUser = (name, email) => {
  const user = { id: nextId++, name, email };
  validateUser(user);
  users.push(user);
  return user;
};

export const getUser = (id) =>
  users.find(user => user.id === id);

export const getAllUsers = () => [...users];

export const updateUser = (id, updates) => {
  const index = findUserIndex(id);
  if (index === -1) throw new Error('User not found');
  users[index] = { ...users[index], ...updates };
  validateUser(users[index]);
  return users[index];
};

export const deleteUser = (id) => {
  const index = findUserIndex(id);
  if (index === -1) throw new Error('User not found');
  return users.splice(index, 1)[0];
};

// Usage (in another file)
import { addUser, getUser, getAllUsers } from './userModule.js';

const user1 = addUser('Alice', 'alice@example.com');
const user2 = addUser('Bob', 'bob@example.com');

console.log(getAllUsers()); // [{ id: 1, ... }, { id: 2, ... }]
console.log(getUser(1));    // { id: 1, name: 'Alice', ... }

// ALTERNATIVE: Closure-based module (if ES6 modules unavailable)
const createUserModule = () => {
  let users = [];
  let nextId = 1;

  const validateUser = (user) => {
    if (!user.name || !user.email) throw new Error('Invalid user data');
  };

  return {
    addUser: (name, email) => {
      const user = { id: nextId++, name, email };
      validateUser(user);
      users.push(user);
      return user;
    },
    getUser: (id) => users.find(user => user.id === id),
    getAllUsers: () => [...users]
  };
};

const userModule = createUserModule();
```

## Design Pattern Checklist

**When choosing a pattern, ask**:

**Can I solve this with plain functions and data structures?** (Try this first!)

- Does this pattern solve my specific problem?
- Will it reduce complexity or add unnecessary abstraction?
  
**Am I reaching for classes when functions would suffice?**

- Do team members understand this pattern?
- Is the pattern appropriate for the problem scale?
- Are there simpler alternatives?

**Functional-first approach**:

1. **Start with functions and plain objects** - Most problems can be solved with composable functions
2. **Use discriminated unions over class hierarchies** - Tagged objects are simpler
3. **Prefer immutable data transformations** - map, filter, reduce over mutation
4. **Leverage closures for encapsulation** - No need for private class fields
5. **Higher-order functions over strategy classes** - Pass functions directly
6. **Use ES6 modules for organization** - Not class-based namespaces

**Common anti-patterns to avoid**:

- Over-engineering simple problems with classes
- Using patterns for the sake of using patterns
- Reaching for OOP when functional approach is simpler
- Creating class hierarchies when plain objects suffice
- Ignoring JavaScript's functional capabilities
- Creating unnecessary abstraction layers
- Premature optimization with complex patterns

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
