# JavaScript Language - Modern JavaScript Mastery (ES2015+)

## Overview

JavaScript Language mode provides expert guidance on modern JavaScript features, async patterns, and core language semantics. It helps you understand and apply ES2015+ features, master asynchronous programming, implement advanced language features (Proxies, Iterators, Generators), and make informed decisions about module systems and code organization.

**Philosophy**: Modern standards first - prioritize ES2015+ features over legacy patterns for maintainability and future-proofing.

## Requirements

**No external dependencies required**

Works with any JavaScript runtime (browser, Node.js, Deno, Bun) and any JavaScript/TypeScript codebase.

## Core Capabilities

**ES2015+ Features**:

- Modules (ESM, CommonJS, dynamic imports, import maps)
- Destructuring (objects, arrays, nested, default values, rest)
- Spread/Rest operators (arrays, objects, function parameters)
- Template literals (string interpolation, tagged templates)
- Arrow functions (syntax, lexical `this`, when to use/avoid)
- Classes (syntax, inheritance, private fields, static members)
- Enhanced object literals (shorthand, computed properties, methods)

**Async Programming Mastery**:

- Promises (creation, chaining, error handling, Promise.all/race/allSettled)
- Async/await (syntax, error handling, parallel execution, gotchas)
- Event loop understanding (call stack, task queue, microtasks, macrotasks)
- Async iteration (for-await-of, async generators)
- Migration patterns (callbacks → Promises → async/await)

**Advanced Language Features**:

- Proxies (traps, handlers, use cases: validation, logging, reactivity)
- Symbols (unique values, well-known symbols, private-ish properties)
- Iterators (iteration protocol, custom iterators, iterable objects)
- Generators (function*, yield, two-way communication, lazy evaluation)
- WeakMap/WeakSet (memory-conscious collections, private data)
- Reflect API (meta-programming, consistent object manipulation)

**Scope and Execution**:

- Closures (lexical scope, data privacy, module patterns)
- Execution context (global, function, eval contexts)
- `this` binding (implicit, explicit, arrow functions, classes)
- Hoisting (var, let, const, function, class declarations)
- Temporal Dead Zone (TDZ) and block scoping

**Module Systems**:

- ESM (ES Modules): import/export, default vs named, tree-shaking
- CommonJS: require/module.exports, circular dependencies
- Dynamic imports: import(), code splitting, lazy loading
- Module resolution, import maps, and bare specifiers

## When to Use

**Use Language Mode for**:

- Modernizing legacy JavaScript code to ES2015+
- Understanding async patterns and event loop behavior
- Implementing advanced features (Proxies, Symbols, Iterators, Generators)
- Debugging closure, scope, or `this` binding issues
- Making module system architectural decisions
- Migrating between module systems (CommonJS ↔ ESM)
- Understanding JavaScript semantics and core behavior
- Code review focusing on modern JavaScript usage
- Teaching or learning modern JavaScript best practices

**Don't use Language Mode for**:

- Performance optimization (use Performance Mode)
- Type safety and TypeScript features (use TypeScript Mode)
- Framework-specific questions (React hooks, Vue composition API, etc.)
- Build tool configuration (webpack, vite, rollup)
- Node.js-specific APIs (fs, http, path, etc.)
- Browser-specific APIs (DOM, fetch, localStorage, etc.)

## Integration with Other Modes

**Performance Mode**:

- Understanding closures helps prevent memory leaks
- Event loop knowledge enables async optimization
- Generator knowledge supports efficient iteration strategies

**TypeScript Mode**:

- Modern JavaScript syntax + type annotations = robust TypeScript
- Understanding JavaScript semantics improves type modeling
- Module systems affect TypeScript configuration

**Functional Mode**:

- Closures are foundational for currying and partial application
- Arrow functions enable concise higher-order functions
- Iterators/Generators support lazy functional patterns

**Patterns Mode**:

- Modules enable various module patterns
- Classes and Proxies support multiple design patterns
- Closures underpin several JavaScript patterns

## Standard Workflow

### 1. Analyze Current Code

**Identify patterns and opportunities**:

- What JavaScript version/features are currently used?
- Are there legacy patterns that could be modernized?
- Are async patterns consistent and error-handled?
- Are advanced features used appropriately or misused?

### 2. Identify Modernization Opportunities

**Surface specific improvements**:

- Replace callbacks with Promises or async/await
- Use destructuring to simplify code
- Apply const/let instead of var
- Convert to arrow functions where appropriate
- Use template literals for string building
- Simplify with spread/rest operators

### 3. Recommend Modern Alternatives

**Provide specific guidance**:

- Show before/after code examples
- Explain benefits and tradeoffs
- Note browser/runtime compatibility if relevant
- Highlight potential gotchas or edge cases

### 4. Provide Examples and Patterns

**Demonstrate best practices**:

- Show idiomatic modern JavaScript
- Include error handling patterns
- Demonstrate common use cases
- Provide anti-patterns to avoid

### 5. Verify and Validate

**Ensure correctness**:

- Are semantics preserved in refactoring?
- Is error handling maintained or improved?
- Are edge cases handled?
- Is browser/runtime compatibility acceptable?

## Output Format

### Context

- What code was analyzed and why
- Current JavaScript patterns identified
- Modernization goal or focus area

### Findings

Categorized opportunities for improvement:

- **CRITICAL**: Code that will break in modern environments, serious semantic errors
- **HIGH**: Significant modernization opportunities, async anti-patterns, scope bugs
- **MEDIUM**: Moderate improvements, consistency issues, minor anti-patterns
- **LOW**: Style improvements, small optimizations, idiomatic preferences

### Recommendations

Specific, actionable improvements with code examples:

```javascript
// Before (legacy pattern)
var self = this;
setTimeout(function() {
  console.log(self.name);
}, 1000);

// After (modern pattern)
setTimeout(() => {
  console.log(this.name);
}, 1000);
```

### Migration Path

For significant refactoring:

1. Step-by-step transformation plan
2. Order of operations (least to most risky)
3. Testing strategy at each step
4. Rollback options if issues arise

### Verification

How to confirm improvements:

- Unit tests to verify behavior preservation
- Linting rules to enforce modern patterns
- Browser/runtime compatibility checks
- Manual testing of critical paths

## Examples

### Example 1: Modernizing Async Code

**Before (callback hell)**:

```javascript
function getUserData(userId, callback) {
  fetchUser(userId, function(err, user) {
    if (err) return callback(err);
    fetchPosts(user.id, function(err, posts) {
      if (err) return callback(err);
      fetchComments(posts[0].id, function(err, comments) {
        if (err) return callback(err);
        callback(null, { user, posts, comments });
      });
    });
  });
}
```

**After (async/await)**:

```javascript
async function getUserData(userId) {
  const user = await fetchUser(userId);
  const posts = await fetchPosts(user.id);
  const comments = await fetchComments(posts[0].id);
  return { user, posts, comments };
}
```

**Benefits**: Flattened structure, consistent error handling (try/catch), easier to read and maintain.

### Example 2: Using Destructuring and Spread

**Before**:

```javascript
function createUser(config) {
  var name = config.name;
  var email = config.email;
  var role = config.role || 'user';

  var defaults = { active: true, verified: false };
  var user = {};
  for (var key in defaults) {
    user[key] = defaults[key];
  }
  user.name = name;
  user.email = email;
  user.role = role;

  return user;
}
```

**After**:

```javascript
function createUser({ name, email, role = 'user' }) {
  const defaults = { active: true, verified: false };
  return { ...defaults, name, email, role };
}
```

**Benefits**: Destructuring with defaults, spread operator, const instead of var, more concise and readable.

### Example 3: Implementing an Iterator

**Custom iterable object**:

```javascript
const range = {
  from: 1,
  to: 5,

  [Symbol.iterator]() {
    let current = this.from;
    const last = this.to;

    return {
      next() {
        if (current <= last) {
          return { value: current++, done: false };
        } else {
          return { done: true };
        }
      }
    };
  }
};

// Usage
for (const num of range) {
  console.log(num); // 1, 2, 3, 4, 5
}

// Array destructuring works too
const [first, second] = range;
console.log(first, second); // 1, 2
```

### Example 4: Using Generators for Lazy Evaluation

**Infinite sequence with generator**:

```javascript
function* fibonacci() {
  let [prev, curr] = [0, 1];
  while (true) {
    yield curr;
    [prev, curr] = [curr, prev + curr];
  }
}

// Take first 10 Fibonacci numbers
const fib = fibonacci();
const first10 = Array.from({ length: 10 }, () => fib.next().value);
console.log(first10); // [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```

**Benefits**: Lazy evaluation, memory efficient, infinite sequences without infinite loops.

### Example 5: Proxy for Validation

**Validation proxy**:

```javascript
function createValidatedUser(target) {
  return new Proxy(target, {
    set(obj, prop, value) {
      if (prop === 'age') {
        if (typeof value !== 'number' || value < 0) {
          throw new TypeError('Age must be a positive number');
        }
      }
      if (prop === 'email') {
        if (!value.includes('@')) {
          throw new TypeError('Invalid email format');
        }
      }
      obj[prop] = value;
      return true;
    }
  });
}

const user = createValidatedUser({});
user.age = 25; // OK
user.email = 'user@example.com'; // OK
user.age = -5; // TypeError: Age must be a positive number
```

**Benefits**: Runtime validation, transparent object wrapping, reusable validation logic.

## References

**Related Skills**:

- Main JavaScript skill: `data/javascript/SKILL.md`
- Performance Mode: `data/javascript/performance/SKILL.md`
- TypeScript Mode: `data/javascript/typescript/SKILL.md`
- Functional Mode: `data/javascript/functional/SKILL.md`
- Patterns Mode: `data/javascript/patterns/SKILL.md`

**Language Specifications**:

- ECMAScript 2015+ specifications (TC39)
- MDN Web Docs for JavaScript reference
- JavaScript.info for comprehensive tutorials
