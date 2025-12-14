# TypeScript - Type System Mastery & Type-Safe Architecture

## Overview

TypeScript mode provides expert guidance on type system design, advanced TypeScript patterns, and type-level programming. It helps you build type-safe systems, master generics and utility types, implement sophisticated type guards, design type architectures, and migrate JavaScript codebases to TypeScript incrementally and safely.

**Philosophy**: Type safety as enabler - types prevent bugs without sacrificing flexibility. Use the type system to make illegal states unrepresentable.

## Requirements

**TypeScript compiler required** (version 4.0+ recommended for modern features)

Works with any TypeScript project configuration and integrates with all major JavaScript runtimes and frameworks.

## Core Capabilities

**Type System Fundamentals**:

- Primitive types (string, number, boolean, null, undefined, symbol, bigint)
- Object types (interfaces, type aliases, index signatures)
- Array and tuple types (fixed-length, optional elements, rest elements)
- Literal types (string literals, numeric literals, boolean literals)
- Union and intersection types
- Type aliases vs interfaces (when to use each)
- `any`, `unknown`, `never`, `void` (proper usage)

**Generic Types**:

- Generic functions (type parameters, constraints)
- Generic interfaces and classes
- Generic constraints (`extends`, multiple constraints)
- Default type parameters
- Conditional type constraints
- Variance (covariance, contravariance, invariance)
- Higher-kinded types (type-level programming)

**Utility Types**:

- Built-in utilities (`Partial`, `Required`, `Readonly`, `Pick`, `Omit`, `Extract`, `Exclude`, `Record`, `NonNullable`, `ReturnType`, `Parameters`, `ConstructorParameters`, `InstanceType`, `ThisType`, `Awaited`)
- Custom utility type creation
- Utility type composition
- Type transformation patterns

**Type Guards and Narrowing**:

- Type predicates (`is` keyword)
- `typeof` type guards
- `instanceof` type guards
- Discriminated unions (tagged unions)
- `in` operator narrowing
- Truthiness narrowing
- Equality narrowing
- Control flow analysis

**Advanced Type Features**:

- Conditional types (`T extends U ? X : Y`)
- Mapped types (`{ [K in keyof T]: ... }`)
- Template literal types
- Recursive types
- Index types (`keyof`, indexed access types)
- `infer` keyword
- Type manipulation with `as`

**Type Safety Patterns**:

- Branded types (nominal typing simulation)
- Builder pattern with type safety
- State machines with literal types
- Option/Maybe type pattern
- Result type pattern (for error handling)
- Type-safe event emitters
- Type-safe routing

**Migration Strategies**:

- JavaScript to TypeScript incremental migration
- `allowJs` and `checkJs` configuration
- JSDoc type annotations
- Declaration files (`.d.ts`)
- Ambient types and modules
- Third-party library types (@types packages)
- Gradual strict mode adoption

## When to Use

**Use TypeScript Mode for**:

- Designing type-safe APIs and interfaces
- Complex generic type implementations
- TypeScript migration planning and execution
- Type system architecture decisions
- Type error debugging and resolution
- Creating reusable typed libraries
- Type-level programming and metaprogramming
- Improving IDE autocomplete and intellisense
- Preventing runtime errors through static analysis
- Documenting code contracts with types

**Don't use TypeScript Mode for**:

- Runtime behavior questions (use Language Mode)
- Performance optimization (use Performance Mode, though types can affect perf)
- Pure JavaScript projects without type requirements
- Framework-specific type problems (React FC, Vue defineComponent, etc.)
- Build configuration issues (tsconfig.json troubleshooting is OK)

## Integration with Other Modes

**Language Mode**:

- TypeScript builds on JavaScript semantics
- Modern JavaScript features + type annotations = TypeScript
- Understanding JavaScript helps model types correctly

**Performance Mode**:

- Type assertions can affect runtime performance
- Type narrowing can help V8 optimize better
- Excessive type complexity can slow compilation

**Functional Mode**:

- Typed pure functions are easier to reason about
- Generics enable type-safe functional utilities
- Discriminated unions support algebraic data types

**Patterns Mode**:

- Design patterns benefit from type safety
- Type system enforces pattern contracts
- Some patterns are TypeScript-specific (decorators, mixins)

## Standard Workflow

### 1. Analyze Type Requirements

**Understand the domain**:

- What data shapes exist in the system?
- What operations are performed on data?
- What invariants must be maintained?
- What states are valid/invalid?

### 2. Design Type Architecture

**Create type system structure**:

- Define core domain types
- Identify shared/reusable types
- Design generic abstractions
- Use discriminated unions for variants
- Apply utility types for transformations

### 3. Implement Type Safety

**Apply types systematically**:

- Add type annotations to functions
- Define interfaces for objects
- Use generics for reusable logic
- Implement type guards for narrowing
- Leverage utility types

### 4. Validate and Refine

**Ensure type correctness**:

- Fix type errors
- Improve type inference
- Reduce `any` usage
- Add stricter checks
- Verify edge cases are typed

### 5. Document and Maintain

**Keep types maintainable**:

- Document complex types with comments
- Extract reusable utilities
- Keep type definitions close to usage
- Version type definitions for public APIs

## Output Format

### Context

- What code/API was analyzed
- Type safety goals
- Current type coverage level

### Type Analysis

Categorized findings:

- **CRITICAL**: `any` types in critical paths, unsafe type assertions, missing error handling types
- **HIGH**: Weak types allowing invalid states, missing generics, poor type inference
- **MEDIUM**: Redundant type annotations, utility type opportunities, type organization issues
- **LOW**: Type alias vs interface preferences, comment documentation

### Type Design

Recommended type structures with examples:

```typescript
// Before: Weak types allow invalid states
interface User {
  id: string;
  name: string;
  status: string; // Too broad - any string allowed
  metadata?: any;  // Unsafe
}

// After: Strong types make illegal states unrepresentable
type UserStatus = 'active' | 'inactive' | 'suspended';

interface UserMetadata {
  lastLogin: Date;
  preferences: Record<string, boolean>;
}

interface User {
  id: string;
  name: string;
  status: UserStatus; // Only valid statuses allowed
  metadata: UserMetadata; // Fully typed
}
```

### Migration Strategy

For JavaScript to TypeScript conversion:

1. Enable TypeScript with `allowJs: true`
2. Rename `.js` to `.ts` incrementally (start with leaf modules)
3. Add `// @ts-check` to enable type checking in JS files
4. Fix type errors module by module
5. Enable strict mode gradually (`--strict` or individual flags)
6. Remove `any` types systematically

### Verification

How to confirm type safety:

- TypeScript compiler with strict mode enabled
- No `any` types (or minimal, explicitly justified)
- All errors handled with Result/Option types
- IDE provides accurate autocomplete
- Refactoring is safe (rename, extract, move)

## Examples

### Example 1: Advanced Generic Constraints

**Type-safe object property getter**:

```typescript
// Get nested property value with type safety
function getProperty<T, K1 extends keyof T>(obj: T, key1: K1): T[K1];
function getProperty<T, K1 extends keyof T, K2 extends keyof T[K1]>(
  obj: T,
  key1: K1,
  key2: K2
): T[K1][K2];
function getProperty<
  T,
  K1 extends keyof T,
  K2 extends keyof T[K1],
  K3 extends keyof T[K1][K2]
>(obj: T, key1: K1, key2: K2, key3: K3): T[K1][K2][K3];

function getProperty(obj: any, ...keys: string[]): any {
  return keys.reduce((acc, key) => acc?.[key], obj);
}

// Usage - fully type-safe
const user = {
  profile: {
    address: {
      city: 'San Francisco'
    }
  }
};

const city = getProperty(user, 'profile', 'address', 'city'); // string
// getProperty(user, 'profile', 'invalid'); // Error: invalid key
```

### Example 2: Discriminated Unions for State Management

**Type-safe state machine**:

```typescript
// Define all possible states as discriminated union
type FetchState<T> =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success'; data: T }
  | { status: 'error'; error: Error };

// Type-safe state handlers
function handleFetchState<T>(state: FetchState<T>): string {
  switch (state.status) {
    case 'idle':
      return 'Not started';
    case 'loading':
      return 'Loading...';
    case 'success':
      return `Data: ${state.data}`; // state.data is available
    case 'error':
      return `Error: ${state.error.message}`; // state.error is available
  }
  // Exhaustiveness checking - compiler ensures all cases handled
}

// Usage
const state1: FetchState<string> = { status: 'success', data: 'Hello' };
const state2: FetchState<string> = { status: 'loading' };
// const invalid: FetchState<string> = { status: 'success' }; // Error: missing data
```

### Example 3: Conditional Types and Utility Creation

**Smart type extraction**:

```typescript
// Extract all function property names from a type
type FunctionPropertyNames<T> = {
  [K in keyof T]: T[K] extends Function ? K : never;
}[keyof T];

// Extract all non-function property names
type NonFunctionPropertyNames<T> = {
  [K in keyof T]: T[K] extends Function ? never : K;
}[keyof T];

// Create type with only function properties
type FunctionProperties<T> = Pick<T, FunctionPropertyNames<T>>;

// Create type with only non-function properties
type NonFunctionProperties<T> = Pick<T, NonFunctionPropertyNames<T>>;

// Usage
interface User {
  id: string;
  name: string;
  save(): Promise<void>;
  delete(): Promise<void>;
}

type UserMethods = FunctionProperties<User>;
// { save(): Promise<void>; delete(): Promise<void>; }

type UserData = NonFunctionProperties<User>;
// { id: string; name: string; }
```

### Example 4: Branded Types for Type Safety

**Nominal typing simulation**:

```typescript
// Create branded types to prevent mixing similar primitives
type Brand<K, T> = K & { __brand: T };

type UserId = Brand<string, 'UserId'>;
type PostId = Brand<string, 'PostId'>;
type Email = Brand<string, 'Email'>;

// Constructor functions with validation
function UserId(id: string): UserId {
  if (!id.startsWith('user_')) {
    throw new Error('Invalid user ID format');
  }
  return id as UserId;
}

function Email(email: string): Email {
  if (!email.includes('@')) {
    throw new Error('Invalid email format');
  }
  return email as Email;
}

// Type-safe functions
function getUser(id: UserId): User { /* ... */ }
function getPost(id: PostId): Post { /* ... */ }

// Usage
const userId = UserId('user_123');
const email = Email('user@example.com');

getUser(userId); // OK
// getUser('user_123'); // Error: string is not UserId
// getUser(postId); // Error: PostId is not UserId
```

### Example 5: Type-Safe Event Emitter

**Events with typed payloads**:

```typescript
// Define event map
interface Events {
  'user:login': { userId: string; timestamp: Date };
  'user:logout': { userId: string };
  'post:created': { postId: string; authorId: string };
}

// Type-safe event emitter
class TypedEventEmitter<T extends Record<string, any>> {
  private listeners: {
    [K in keyof T]?: Array<(payload: T[K]) => void>;
  } = {};

  on<K extends keyof T>(event: K, callback: (payload: T[K]) => void): void {
    if (!this.listeners[event]) {
      this.listeners[event] = [];
    }
    this.listeners[event]!.push(callback);
  }

  emit<K extends keyof T>(event: K, payload: T[K]): void {
    this.listeners[event]?.forEach(callback => callback(payload));
  }
}

// Usage
const emitter = new TypedEventEmitter<Events>();

emitter.on('user:login', (payload) => {
  // payload is typed as { userId: string; timestamp: Date }
  console.log(`User ${payload.userId} logged in at ${payload.timestamp}`);
});

emitter.emit('user:login', {
  userId: 'user_123',
  timestamp: new Date()
}); // OK

// emitter.emit('user:login', { userId: 'user_123' }); // Error: missing timestamp
// emitter.on('invalid:event', () => {}); // Error: unknown event
```

### Example 6: Mapped Types for Transformation

**Create derived types automatically**:

```typescript
// Make all properties optional recursively
type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};

// Make all properties readonly recursively
type DeepReadonly<T> = {
  readonly [P in keyof T]: T[P] extends object ? DeepReadonly<T[P]> : T[P];
};

// Convert all methods to return Promises
type Promisify<T> = {
  [K in keyof T]: T[K] extends (...args: infer A) => infer R
    ? (...args: A) => Promise<R>
    : T[K];
};

// Usage
interface Config {
  server: {
    port: number;
    host: string;
  };
  database: {
    url: string;
  };
}

type PartialConfig = DeepPartial<Config>;
// All properties optional at all levels

type ReadonlyConfig = DeepReadonly<Config>;
// All properties readonly at all levels

interface API {
  getUser(id: string): User;
  deletePost(id: string): boolean;
}

type AsyncAPI = Promisify<API>;
// {
//   getUser(id: string): Promise<User>;
//   deletePost(id: string): Promise<boolean>;
// }
```

### Example 7: Result Type Pattern for Error Handling

**Type-safe error handling without exceptions**:

```typescript
// Result type - either success with data or failure with error
type Result<T, E = Error> =
  | { ok: true; value: T }
  | { ok: false; error: E };

// Helper constructors
const Ok = <T>(value: T): Result<T, never> => ({ ok: true, value });
const Err = <E>(error: E): Result<never, E> => ({ ok: false, error });

// Type-safe functions returning Result
function parseJSON<T>(json: string): Result<T, string> {
  try {
    const value = JSON.parse(json) as T;
    return Ok(value);
  } catch (error) {
    return Err(`Invalid JSON: ${error}`);
  }
}

function divide(a: number, b: number): Result<number, string> {
  if (b === 0) {
    return Err('Division by zero');
  }
  return Ok(a / b);
}

// Usage - explicit error handling required
const result = divide(10, 2);
if (result.ok) {
  console.log(result.value); // number
} else {
  console.error(result.error); // string
}

// Composing Results
function safeDivideAndParse(a: number, b: number, json: string): Result<{ quotient: number; data: any }, string> {
  const divResult = divide(a, b);
  if (!divResult.ok) return divResult;

  const parseResult = parseJSON(json);
  if (!parseResult.ok) return parseResult;

  return Ok({ quotient: divResult.value, data: parseResult.value });
}
```

## TypeScript Strict Mode Checklist

**`strict: true` enables**:

- `strictNullChecks`: null/undefined must be explicitly handled
- `strictFunctionTypes`: stricter function type checking
- `strictBindCallApply`: typed bind/call/apply
- `strictPropertyInitialization`: class properties must be initialized
- `noImplicitThis`: `this` must be explicitly typed
- `alwaysStrict`: emit "use strict" in output
- `useUnknownInCatchVariables`: catch variables are `unknown` instead of `any`

**Additional strict flags to consider**:

- `noImplicitAny`: no implicit `any` types
- `noImplicitReturns`: all code paths must return
- `noFallthroughCasesInSwitch`: switch cases must break/return
- `noUncheckedIndexedAccess`: array/object access returns `T | undefined`
- `noUnusedLocals`: error on unused local variables
- `noUnusedParameters`: error on unused function parameters

## References

**Related Skills**:

- Main JavaScript skill: `data/javascript/SKILL.md`
- Language Mode: `data/javascript/language/SKILL.md`
- Performance Mode: `data/javascript/performance/SKILL.md`
- Functional Mode: `data/javascript/functional/SKILL.md`
- Patterns Mode: `data/javascript/patterns/SKILL.md`

**Resources**:

- TypeScript Handbook (official documentation)
- TypeScript Deep Dive by Basarat Ali Syed
- Type Challenges (practice advanced types)
- DefinitelyTyped (@types packages)
- Matt Pocock's TypeScript tips
