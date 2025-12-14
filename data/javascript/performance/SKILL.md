# JavaScript Performance - Runtime Optimization & V8 Mastery

## Overview

JavaScript Performance mode provides expert guidance on optimizing JavaScript runtime performance through V8 engine understanding, algorithmic analysis, memory management, and evidence-based optimization. It helps you identify bottlenecks, understand V8 internals, analyze algorithmic complexity, and make data-driven performance improvements.

**Philosophy**: Performance through understanding - optimize based on V8 internals and measurements, not myths or cargo-cult patterns. Measure first, optimize second.

## Requirements

**No external dependencies required**

Works with any V8-based runtime (Chrome, Node.js, Deno, Bun) and provides general JavaScript performance guidance applicable to other engines (SpiderMonkey, JavaScriptCore).

## Core Capabilities

**V8 Engine Internals**:

- JIT compilation (Ignition interpreter, TurboFan optimizer)
- Hidden classes (object shapes, inline caching)
- Inline caching (monomorphic, polymorphic, megamorphic)
- Deoptimization detection and prevention
- Optimization tiers and bailouts
- Function optimization (hot functions, inlining)
- Object allocation and representation

**Memory Management**:

- Garbage collection (generational GC, scavenge, mark-sweep, mark-compact)
- Memory leaks (closures, event listeners, global references, DOM detachment)
- Heap analysis (shallow vs retained size, dominators)
- Memory profiling (allocation timeline, heap snapshots)
- Memory-efficient data structures
- WeakMap/WeakSet for memory-conscious caching
- Object pooling and reuse strategies

**Algorithmic Analysis**:

- Big O complexity (time and space)
- Algorithm selection (search, sort, graph, dynamic programming)
- Data structure performance characteristics (Array, Map, Set, Object)
- Cache-friendly algorithms and data layouts
- Amortized analysis for collection operations

**Event Loop Optimization**:

- Event loop phases (timers, I/O callbacks, setImmediate, close callbacks)
- Microtasks vs macrotasks (Promise.then vs setTimeout)
- Long task prevention (chunking, requestIdleCallback, Web Workers)
- Async scheduling strategies
- Blocking detection and resolution

**Bundle Optimization**:

- Code splitting strategies
- Tree-shaking and dead code elimination
- Lazy loading and dynamic imports
- Module federation
- Bundle size analysis and reduction

**Profiling and Measurement**:

- Chrome DevTools Performance panel
- Node.js profiler (--prof, --prof-process, clinic.js)
- Benchmark.js and microbenchmarking
- Real User Monitoring (RUM) metrics
- Core Web Vitals (LCP, FID, CLS)

## When to Use

**Use Performance Mode for**:

- Application is slow or unresponsive
- High memory usage or memory leaks
- Understanding V8 optimization behavior
- Optimizing hot code paths identified by profiling
- Algorithm/data structure selection decisions
- Event loop blocking investigation
- Bundle size reduction
- Performance regression debugging
- Performance-sensitive features (games, animations, data viz)
- Capacity planning and scalability analysis

**Don't use Performance Mode for**:

- Premature optimization without measurements
- Framework-specific performance (React re-renders, Vue reactivity)
- Network/API performance (different domain)
- Build time performance (webpack/vite compilation)
- Database query optimization
- Simple code that doesn't need optimization

## Integration with Other Modes

**Language Mode**:

- Closures can cause memory leaks if misused
- Event loop understanding enables async optimization
- Generator knowledge supports efficient iteration

**TypeScript Mode**:

- Type assertions can affect runtime performance
- Generic types may influence JIT optimization
- Type narrowing can help V8 optimize better

**Functional Mode**:

- Pure functions enable better optimization
- Immutability has performance tradeoffs
- Lazy evaluation with generators improves efficiency

**Patterns Mode**:

- Some patterns are inherently more efficient (Flyweight, Object Pool)
- Pattern choice affects memory and CPU characteristics
- Architectural patterns impact scalability

## Standard Workflow

### 1. Establish Baseline

**Measure current performance**:

- Profile the application (CPU, memory, network)
- Identify slow operations and hot paths
- Collect quantitative metrics (latency, throughput, memory usage)
- Define performance goals and targets

### 2. Identify Bottlenecks

**Analyze findings**:

- CPU bottlenecks (hot functions, algorithmic complexity)
- Memory bottlenecks (allocations, leaks, GC pressure)
- Event loop bottlenecks (blocking operations)
- I/O bottlenecks (file system, network, database)

### 3. Understand V8 Behavior

**Check optimization status**:

- Are functions being optimized or deoptimized?
- Are objects monomorphic or polymorphic?
- Is inline caching effective?
- Are there hidden class mismatches?

### 4. Optimize

**Apply targeted improvements**:

- Algorithmic improvements (better Big O complexity)
- Data structure changes (Array → Set, Object → Map)
- V8-friendly patterns (monomorphic functions, stable object shapes)
- Memory optimizations (reduce allocations, fix leaks)
- Async optimizations (batching, parallelization)

### 5. Benchmark and Verify

**Measure impact**:

- Re-run profiling and benchmarks
- Compare before/after metrics
- Ensure correctness is preserved
- Check for performance regression in other areas

## Output Format

### Context

- What code/feature was analyzed
- Performance goals and targets
- Current baseline metrics

### Performance Analysis

Categorized findings:

- **CRITICAL**: Severe bottlenecks causing user-visible slowness, memory leaks causing crashes
- **HIGH**: Significant inefficiencies, poor algorithmic complexity, excessive allocations
- **MEDIUM**: Moderate improvements possible, minor memory waste, bundling opportunities
- **LOW**: Micro-optimizations, theoretical improvements with minimal real-world impact

### Bottlenecks

Specific problem areas with evidence:

```md
Hot Path: processUserData (35% of CPU time)

- Called 10,000 times per second
- O(n²) algorithm due to nested loops
- Causing event loop blocking (200ms frames)
```

### Optimizations

Recommended improvements with code examples and expected impact:

```javascript
// Before: O(n²) - 250ms for 1000 items
function findDuplicates(arr) {
  const duplicates = [];
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] === arr[j] && !duplicates.includes(arr[i])) {
        duplicates.push(arr[i]);
      }
    }
  }
  return duplicates;
}

// After: O(n) - 2ms for 1000 items (125x faster)
function findDuplicates(arr) {
  const seen = new Set();
  const duplicates = new Set();
  for (const item of arr) {
    if (seen.has(item)) {
      duplicates.add(item);
    } else {
      seen.add(item);
    }
  }
  return Array.from(duplicates);
}
```

**Expected Impact**: 125x faster, reduces CPU time from 35% to <1%, eliminates event loop blocking.

### Before/After Metrics

Quantitative comparison:

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| Execution time | 250ms | 2ms | 125x faster |
| Memory allocated | 500KB | 50KB | 10x reduction |
| GC pressure | High | Low | - |
| Event loop blocking | Yes (200ms) | No | - |

### Verification Plan

How to confirm improvements:

- Benchmark suite comparing before/after performance
- Profiling to verify CPU time reduction
- Memory profiling to confirm allocation reduction
- Load testing to ensure scalability
- Real-world user metrics (if available)

## Examples

### Example 1: V8 Deoptimization

**Problem**: Function is being deoptimized repeatedly

**Diagnosis**:

```javascript
// Check optimization status (Node.js with --allow-natives-syntax)
function add(a, b) {
  return a + b;
}

%OptimizeFunctionOnNextCall(add);
add(1, 2);
console.log(%GetOptimizationStatus(add)); // Optimized

add(1, "2"); // Different types!
console.log(%GetOptimizationStatus(add)); // Deoptimized
```

**Solution**: Keep function monomorphic (same input types)

```javascript
function addNumbers(a, b) {
  // Type check or use TypeScript for compile-time enforcement
  if (typeof a !== 'number' || typeof b !== 'number') {
    throw new TypeError('Arguments must be numbers');
  }
  return a + b;
}

function addStrings(a, b) {
  return String(a) + String(b);
}
```

### Example 2: Hidden Class Optimization

**Problem**: Object shape changes causing hidden class mismatch

**Before (slow - creates multiple hidden classes)**:

```javascript
function createPoint(x, y, z) {
  const point = {};
  point.x = x; // Hidden class 1
  if (y !== undefined) {
    point.y = y; // Hidden class 2
  }
  if (z !== undefined) {
    point.z = z; // Hidden class 3 or 4
  }
  return point;
}
```

**After (fast - single hidden class)**:

```javascript
function createPoint(x, y = 0, z = 0) {
  // Always same shape, always same property order
  return { x, y, z };
}
```

### Example 3: Memory Leak Prevention

**Problem**: Event listeners causing memory leak

**Before (leaks memory)**:

```javascript
class DataFetcher {
  constructor() {
    this.data = new Array(100000); // Large array

    // Event listener holds reference to this.data
    document.addEventListener('click', () => {
      console.log(this.data.length);
    });
  }
}

// Even after instance is no longer needed, the event listener
// keeps it alive, preventing garbage collection
```

**After (properly cleaned up)**:

```javascript
class DataFetcher {
  constructor() {
    this.data = new Array(100000);
    this.handleClick = this.handleClick.bind(this);
    document.addEventListener('click', this.handleClick);
  }

  handleClick() {
    console.log(this.data.length);
  }

  destroy() {
    document.removeEventListener('click', this.handleClick);
    this.data = null; // Allow GC
  }
}

// Call destroy() when done
const fetcher = new DataFetcher();
// ... use fetcher ...
fetcher.destroy(); // Now can be garbage collected
```

### Example 4: Event Loop Optimization

**Problem**: Long task blocking event loop

**Before (blocks for ~1 second)**:

```javascript
function processLargeDataset(data) {
  const results = [];
  for (let i = 0; i < data.length; i++) {
    results.push(expensiveOperation(data[i]));
  }
  return results;
}

// Blocks UI for entire processing duration
processLargeDataset(largeArray);
```

**After (chunked, non-blocking)**:

```javascript
async function processLargeDataset(data, chunkSize = 100) {
  const results = [];

  for (let i = 0; i < data.length; i += chunkSize) {
    const chunk = data.slice(i, i + chunkSize);

    // Process chunk
    for (const item of chunk) {
      results.push(expensiveOperation(item));
    }

    // Yield to event loop every chunk
    await new Promise(resolve => setTimeout(resolve, 0));
  }

  return results;
}

// UI stays responsive during processing
await processLargeDataset(largeArray);
```

### Example 5: Efficient Data Structure Selection

**Problem**: Array.includes() is slow for large arrays

**Before (O(n) lookup - slow for large arrays)**:

```javascript
const validIds = [/* thousands of IDs */];

function isValidId(id) {
  return validIds.includes(id); // O(n) linear search
}

// Called frequently - very slow!
```

**After (O(1) lookup - fast)**:

```javascript
const validIds = new Set([/* thousands of IDs */]);

function isValidId(id) {
  return validIds.has(id); // O(1) hash lookup
}

// 100x-1000x faster for large datasets
```

### Example 6: Lazy Evaluation with Generators

**Problem**: Processing entire array when only few items needed

**Before (processes everything eagerly)**:

```javascript
function getFirstFiveEvenNumbers(numbers) {
  return numbers
    .filter(n => n % 2 === 0)
    .slice(0, 5);
}

// If array is huge, filter processes everything before slice
getFirstFiveEvenNumbers(hugeArray);
```

**After (stops early with generator)**:

```javascript
function* filterEven(numbers) {
  for (const n of numbers) {
    if (n % 2 === 0) {
      yield n;
    }
  }
}

function getFirstFiveEvenNumbers(numbers) {
  const result = [];
  for (const n of filterEven(numbers)) {
    result.push(n);
    if (result.length === 5) break; // Stop early!
  }
  return result;
}

// Processes only until 5 evens are found
getFirstFiveEvenNumbers(hugeArray);
```

## V8 Optimization Checklist

**✅ DO**:

- Keep functions monomorphic (same input types)
- Initialize all properties in constructor (stable object shape)
- Use consistent property order
- Avoid `delete` operator (use `obj.prop = null` or Map)
- Use typed arrays for numeric data
- Prefer `for` loops over `forEach` in hot paths
- Use Map/Set for collections instead of objects
- Cache frequently accessed values
- Batch DOM updates
- Use object pooling for frequently created/destroyed objects

**❌ DON'T**:

- Mix types in function parameters
- Add properties after object creation
- Change property order across objects
- Use `arguments` object (use rest parameters)
- Use `eval` or `with`
- Create unnecessary closures in loops
- Allocate objects in tight loops
- Block the event loop with synchronous operations

## References

**Related Skills**:

- Main JavaScript skill: `data/javascript/SKILL.md`
- Language Mode: `data/javascript/language/SKILL.md`
- TypeScript Mode: `data/javascript/typescript/SKILL.md`
- Functional Mode: `data/javascript/functional/SKILL.md`
- Patterns Mode: `data/javascript/patterns/SKILL.md`

**Resources**:

- V8 blog (official V8 team insights)
- "What's up with monomorphism?" by Vyacheslav Egorov
- Chrome DevTools documentation
- Node.js performance documentation
- "JavaScript Performance" by MDN Web Docs
