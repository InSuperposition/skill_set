# JavaScript Functional - Functional Programming Mastery

## Overview

JavaScript Functional mode provides expert guidance on functional programming principles, patterns, and practices in JavaScript. It helps you design pure functions, enforce immutability, compose functions, apply higher-order functions, and leverage functional patterns like currying, partial application, functors, and monads for predictable, testable, and maintainable code.

**Philosophy**: Functional when appropriate - immutability and purity improve reliability and testability. Choose functional patterns when they add value and clarity.

## Requirements

**No external dependencies required**

Works with vanilla JavaScript and TypeScript. Functional libraries (Ramda, Lodash/FP) are optional but not required.

## Core Capabilities

**Pure Functions**:

- No side effects (no mutations, no I/O, no randomness)
- Deterministic (same inputs always produce same output)
- Referential transparency (can be replaced with its return value)
- Easier to test, reason about, and parallelize
- Memoization and caching opportunities

**Immutability Patterns**:

- Immutable data structures (never mutate)
- Spread operators for shallow copies (`...`)
- Object.freeze() for shallow immutability
- Structural sharing for efficiency
- Copy-on-write semantics
- Persistent data structures (for deep immutability)

**Function Composition**:

- Combine simple functions to build complex behaviors
- Pipe (left-to-right composition)
- Compose (right-to-left composition)
- Point-free style (tacit programming)
- Function chaining

**Higher-Order Functions**:

- Functions that take functions as arguments
- Functions that return functions
- Built-in HOFs (map, filter, reduce, find, some, every)
- Custom HOF creation
- Function factories

**Currying and Partial Application**:

- Transform multi-arg functions to single-arg chains
- Partial application (fixing some arguments)
- Auto-currying utilities
- Use cases (dependency injection, configuration)

**Functional Patterns**:

- Functors (mappable containers)
- Monads (chainable containers with flatMap)
- Maybe/Option type (null safety)
- Either/Result type (error handling)
- List operations (map, filter, reduce)
- Lazy evaluation with generators
- Transducers (composable transformations)

**Declarative Programming**:

- Express "what" not "how"
- Replace loops with map/filter/reduce
- Use descriptive function names
- Minimize state changes
- Prefer expressions over statements

## When to Use

**Use Functional Mode for**:

- Building predictable, testable code
- Complex data transformations
- State management (Redux, immutable state)
- Reducing bugs through immutability
- Parallel processing preparation
- Mathematical/algorithmic problems
- Event stream processing (RxJS-style)
- Avoiding null/undefined bugs with Maybe
- Type-safe error handling with Either/Result
- Code requiring high reliability

**Don't use Functional Mode for**:

- OOP-heavy codebases (wrong paradigm)
- Performance-critical imperative code (measure first)
- Simple CRUD operations (might be overkill)
- UI event handlers requiring state mutation
- Low-level systems programming
- Code where side effects are unavoidable and central

## Integration with Other Modes

**Language Mode**:

- Closures are foundational for currying and partial application
- Arrow functions enable concise higher-order functions
- Iterators/Generators support lazy evaluation
- Destructuring simplifies function parameters

**Performance Mode**:

- Pure functions enable better optimization and caching
- Immutability has performance tradeoffs (copying overhead)
- Lazy evaluation with generators improves efficiency
- Structural sharing minimizes copy costs

**TypeScript Mode**:

- Typed pure functions are easier to reason about
- Generics enable type-safe functional utilities
- Discriminated unions support algebraic data types
- Type-safe functors and monads

**Patterns Mode**:

- Many functional patterns (Strategy, Command) are higher-order functions
- Observer pattern with functional reactive programming
- Functional design patterns vs OOP patterns

## Standard Workflow

### 1. Identify Side Effects and Mutations

**Analyze current code**:

- Where does code mutate state?
- Where are side effects happening?
- Which functions are impure?
- Can impure code be isolated to boundaries?

### 2. Refactor to Pure Functions

**Extract pure logic**:

- Separate pure logic from side effects
- Make functions deterministic
- Remove dependencies on external state
- Accept all inputs as parameters
- Return new values instead of mutating

### 3. Apply Immutability

**Ensure data immutability**:

- Use const instead of let/var
- Never mutate objects or arrays
- Use spread/rest for copies
- Consider Object.freeze() for extra safety
- Use immutable data libraries if needed

### 4. Compose Functions

**Build complex behaviors from simple parts**:

- Break down complex operations into small functions
- Compose functions using pipe or compose
- Prefer composition over inheritance
- Use point-free style where it improves readability

### 5. Verify Benefits

**Confirm improvements**:

- Are functions easier to test?
- Is reasoning about code flow clearer?
- Are bugs reduced (especially null/state bugs)?
- Is code more reusable?

## Output Format

### Context

- What code was analyzed
- Functional programming goals
- Current purity and immutability level

### Functional Analysis

Categorized findings:

- **CRITICAL**: Dangerous mutations, race conditions, shared mutable state
- **HIGH**: Significant impurity, unnecessary side effects, hidden mutations
- **MEDIUM**: Moderate improvements possible, partial immutability, composition opportunities
- **LOW**: Style improvements, point-free opportunities, naming

### Refactoring Plan

Step-by-step transformation:

1. Identify and isolate side effects
2. Extract pure functions
3. Apply immutability patterns
4. Compose functions
5. Add functional utilities (map, filter, reduce)
6. Consider advanced patterns (Maybe, Either)

### Pure Function Examples

Before/after showing functional transformation:

```javascript
// Before: Impure (mutates array, depends on external state)
let total = 0;
function addToCart(items, item) {
  items.push(item); // Mutation!
  total += item.price; // Side effect!
  return items;
}

// After: Pure (no mutations, no side effects)
function addToCart(items, item) {
  return [...items, item]; // New array
}

function calculateTotal(items) {
  return items.reduce((sum, item) => sum + item.price, 0);
}
```

### Verification

How to confirm improvements:

- Unit tests are simpler (no mocks needed for pure functions)
- Functions can be tested in isolation
- Memoization can be applied safely
- Parallel execution is safe

## Examples

### Example 1: Pure vs Impure Functions

**Impure (side effects, mutations, non-deterministic)**:

```javascript
let count = 0;

// Mutates external state
function incrementCount() {
  count++; // Side effect
  return count;
}

// Non-deterministic (depends on Date)
function getTimestamp() {
  return new Date().getTime(); // Side effect
}

// Mutates input
function addProperty(obj, key, value) {
  obj[key] = value; // Mutation
  return obj;
}
```

**Pure (no side effects, deterministic)**:

```javascript
// Pure - returns new value
function increment(count) {
  return count + 1;
}

// Pure - accepts timestamp as parameter
function formatTimestamp(timestamp) {
  return new Date(timestamp).toISOString();
}

// Pure - returns new object
function addProperty(obj, key, value) {
  return { ...obj, [key]: value };
}
```

### Example 2: Immutability Patterns

**Array operations without mutation**:

```javascript
const numbers = [1, 2, 3, 4, 5];

// ❌ MUTATING (bad)
numbers.push(6);        // Mutates original
numbers.pop();          // Mutates original
numbers.splice(1, 1);   // Mutates original
numbers.sort();         // Mutates original

// ✅ IMMUTABLE (good)
const added = [...numbers, 6];           // New array
const removed = numbers.slice(0, -1);    // New array
const spliced = [...numbers.slice(0, 1), ...numbers.slice(2)]; // New array
const sorted = [...numbers].sort();      // New array (copy first)
```

**Object operations without mutation**:

```javascript
const user = { name: 'Alice', age: 30 };

// ❌ MUTATING (bad)
user.email = 'alice@example.com'; // Mutates original
delete user.age;                  // Mutates original

// ✅ IMMUTABLE (good)
const withEmail = { ...user, email: 'alice@example.com' }; // New object
const withoutAge = { ...user, age: undefined };             // New object
// Or better:
const { age, ...withoutAge2 } = user;                       // New object
```

**Nested object updates**:

```javascript
const state = {
  user: {
    profile: {
      name: 'Alice',
      address: { city: 'SF' }
    }
  }
};

// ❌ MUTATING (bad)
state.user.profile.address.city = 'NYC';

// ✅ IMMUTABLE (good)
const newState = {
  ...state,
  user: {
    ...state.user,
    profile: {
      ...state.user.profile,
      address: {
        ...state.user.profile.address,
        city: 'NYC'
      }
    }
  }
};
```

### Example 3: Function Composition

**Pipe (left-to-right composition)**:

```javascript
const pipe = (...fns) => (initialValue) =>
  fns.reduce((value, fn) => fn(value), initialValue);

// Simple functions
const double = (x) => x * 2;
const increment = (x) => x + 1;
const square = (x) => x * x;

// Compose them
const transform = pipe(
  double,     // 5 * 2 = 10
  increment,  // 10 + 1 = 11
  square      // 11 * 11 = 121
);

console.log(transform(5)); // 121
```

**Compose (right-to-left composition)**:

```javascript
const compose = (...fns) => (initialValue) =>
  fns.reduceRight((value, fn) => fn(value), initialValue);

// Same result, different order
const transform = compose(
  square,     // Applied last
  increment,  // Applied second
  double      // Applied first
);

console.log(transform(5)); // 121
```

**Point-free style (tacit programming)**:

```javascript
// With explicit parameters
const getUserNames = (users) => users.map(user => user.name);

// Point-free (no explicit parameters)
const getName = (user) => user.name;
const getUserNames = (users) => users.map(getName);

// Even more concise with partial application
const map = (fn) => (array) => array.map(fn);
const getUserNames = map(getName);
```

### Example 4: Currying and Partial Application

**Manual currying**:

```javascript
// Regular function
function add(a, b, c) {
  return a + b + c;
}

// Curried version
function addCurried(a) {
  return function(b) {
    return function(c) {
      return a + b + c;
    };
  };
}

// Usage
console.log(add(1, 2, 3));              // 6
console.log(addCurried(1)(2)(3));       // 6

// Partial application
const add1 = addCurried(1);
const add1and2 = add1(2);
console.log(add1and2(3)); // 6
```

**Auto-curry utility**:

```javascript
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    } else {
      return function(...nextArgs) {
        return curried.apply(this, args.concat(nextArgs));
      };
    }
  };
}

// Use curry
const add = curry((a, b, c) => a + b + c);

console.log(add(1)(2)(3));    // 6
console.log(add(1, 2)(3));    // 6
console.log(add(1)(2, 3));    // 6
console.log(add(1, 2, 3));    // 6
```

**Practical use case - configuration**:

```javascript
const log = curry((level, timestamp, message) => {
  console.log(`[${level}] ${timestamp}: ${message}`);
});

// Create specialized loggers
const info = log('INFO');
const error = log('ERROR');

// Use them
info(Date.now(), 'Application started');
error(Date.now(), 'Connection failed');
```

### Example 5: Maybe Monad (Null Safety)

**Maybe type implementation**:

```javascript
class Maybe {
  constructor(value) {
    this.value = value;
  }

  static of(value) {
    return new Maybe(value);
  }

  isNothing() {
    return this.value === null || this.value === undefined;
  }

  map(fn) {
    return this.isNothing() ? Maybe.of(null) : Maybe.of(fn(this.value));
  }

  flatMap(fn) {
    return this.isNothing() ? Maybe.of(null) : fn(this.value);
  }

  getOrElse(defaultValue) {
    return this.isNothing() ? defaultValue : this.value;
  }
}

// Usage - no null checks needed!
const user = { name: 'Alice', address: { city: 'SF' } };

const city = Maybe.of(user)
  .map(u => u.address)      // Maybe { city: 'SF' }
  .map(a => a.city)         // Maybe 'SF'
  .map(c => c.toUpperCase()) // Maybe 'SF'
  .getOrElse('UNKNOWN');     // 'SF'

const missing = Maybe.of(null)
  .map(u => u.address)      // Maybe null
  .map(a => a.city)         // Maybe null (no error!)
  .getOrElse('UNKNOWN');     // 'UNKNOWN'
```

### Example 6: Either Monad (Error Handling)

**Either type implementation**:

```javascript
class Either {
  constructor(value, isRight = true) {
    this.value = value;
    this.isRight = isRight;
  }

  static right(value) {
    return new Either(value, true);
  }

  static left(value) {
    return new Either(value, false);
  }

  map(fn) {
    return this.isRight ? Either.right(fn(this.value)) : this;
  }

  flatMap(fn) {
    return this.isRight ? fn(this.value) : this;
  }

  getOrElse(defaultValue) {
    return this.isRight ? this.value : defaultValue;
  }

  fold(leftFn, rightFn) {
    return this.isRight ? rightFn(this.value) : leftFn(this.value);
  }
}

// Usage - explicit error handling
function parseJSON(json) {
  try {
    return Either.right(JSON.parse(json));
  } catch (error) {
    return Either.left(error.message);
  }
}

function getProperty(obj, key) {
  return obj[key]
    ? Either.right(obj[key])
    : Either.left(`Property ${key} not found`);
}

// Compose operations
const result = parseJSON('{"name":"Alice"}')
  .flatMap(obj => getProperty(obj, 'name'))
  .map(name => name.toUpperCase());

result.fold(
  error => console.error('Error:', error),
  value => console.log('Success:', value)  // Success: ALICE
);
```

### Example 7: Transducers (Composable Transformations)

**Efficient data transformation**:

```javascript
// Without transducers (creates intermediate arrays)
const result = [1, 2, 3, 4, 5, 6, 7, 8]
  .map(x => x * 2)       // [2, 4, 6, 8, 10, 12, 14, 16]
  .filter(x => x > 5)    // [6, 8, 10, 12, 14, 16]
  .map(x => x - 1);      // [5, 7, 9, 11, 13, 15]

// With transducers (single pass, no intermediate arrays)
const map = (fn) => (reducer) => (acc, val) => reducer(acc, fn(val));
const filter = (predicate) => (reducer) => (acc, val) =>
  predicate(val) ? reducer(acc, val) : acc;

const transduce = (xform, reducer, init, collection) => {
  const transformedReducer = xform(reducer);
  return collection.reduce(transformedReducer, init);
};

const xform = compose(
  map(x => x * 2),
  filter(x => x > 5),
  map(x => x - 1)
);

const result = transduce(
  xform,
  (acc, val) => [...acc, val],
  [],
  [1, 2, 3, 4, 5, 6, 7, 8]
);
// [5, 7, 9, 11, 13, 15] - Same result, more efficient
```

## Functional Programming Principles

**✅ DO**:

- Write pure functions whenever possible
- Embrace immutability (const, spread, no mutations)
- Compose small functions into larger ones
- Use declarative array methods (map, filter, reduce)
- Avoid side effects or isolate them to boundaries
- Prefer expressions over statements
- Use descriptive function names
- Apply currying for reusability
- Handle null/undefined with Maybe
- Handle errors with Either/Result

**❌ DON'T**:

- Mutate function arguments
- Rely on external mutable state
- Use side effects in pure functions
- Use loops when map/filter/reduce work
- Ignore functional patterns in OOP-heavy code
- Force functional style where it hurts readability
- Create overly abstract code for simple problems
- Use point-free style to the point of obscurity

## References

**Related Skills**:

- Main JavaScript skill: `data/javascript/SKILL.md`
- Language Mode: `data/javascript/language/SKILL.md`
- Performance Mode: `data/javascript/performance/SKILL.md`
- TypeScript Mode: `data/javascript/typescript/SKILL.md`
- Patterns Mode: `data/javascript/patterns/SKILL.md`

**Resources**:

- "Functional-Light JavaScript" by Kyle Simpson
- "Professor Frisby's Mostly Adequate Guide to Functional Programming"
- Ramda.js (functional library)
- Lodash/FP (functional programming variant)
- Fantasy Land specification (algebraic types)
