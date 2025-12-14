---
name: skill-code-performance
description: Performance optimization and efficiency analysis. Identifies bottlenecks through algorithmic complexity analysis, database query optimization, caching strategies, and profiling. Focuses on measuring first, then optimizing high-impact areas with concrete performance metrics.
allowed-tools: Read
---

# Code Performance - Optimization and Efficiency

## Overview

Code Performance identifies performance bottlenecks, analyzes efficiency, and provides optimization strategies. It emphasizes measuring performance scientifically before optimizing, focusing efforts on high-impact areas rather than premature optimization.

This skill covers algorithmic complexity, database optimization, memory efficiency, and network/IO performance across all programming languages.

**Philosophy**: "Premature optimization is evil, but informed optimization is essential" - measure first, optimize what matters.

## Requirements

No external dependencies required.

Recommends profiling and benchmarking tools based on technology stack.

## Core Capabilities

**Performance Dimensions**:

- Algorithmic complexity (Big O analysis)
- Database optimization (N+1 queries, indexes, query efficiency)
- Memory management (leaks, large objects, streaming)
- Network and I/O (async operations, batching, caching)
- Caching strategies (application, HTTP, database)

**Bottleneck Identification**:

- Profiling slow code paths
- Database query analysis
- Memory leak detection
- Network latency issues

**Optimization Techniques**:

- Algorithm improvements (O(n²) → O(n log n))
- Database eager loading and indexing
- Caching frequently accessed data
- Async operations for I/O
- Batching API requests

**Performance Measurement**:

- Execution time profiling
- Memory usage tracking
- Database query count and duration
- Before/after comparison metrics

## When to Use

**Use performance for**:

- Identifying slow code paths
- Optimizing database queries
- Reducing memory usage
- Improving response times
- Scaling system for higher load
- Profiling before optimization
- Measuring optimization impact

**Don't use performance for**:

- Micro-optimizations without profiling
- Code that isn't a bottleneck
- Premature optimization
- Sacrificing readability for minimal gains

## Integration with Other Skills

**first-principles**: Root cause analysis

- Question why code is slow
- Understand fundamental performance requirements
- Verify optimization necessity
- Example: "Is this really a bottleneck or just feels slow?"

**code-reviewer**: Performance reviews

- Identify performance issues in PRs
- Suggest optimizations early
- Catch obvious inefficiencies
- Example: "This N+1 query will be slow at scale"

**code-auditor**: Performance audits

- System-wide performance assessment
- Identify hotspots requiring optimization
- Prioritize optimization efforts
- Example: "Dashboard query takes 8 seconds - optimize first"

**tech-stack-advisor**: Framework optimization

- Use framework performance features
- Language-specific optimization techniques
- Library performance characteristics
- Example: "Use Django's select_related for this query"

## Performance Analysis

### Algorithmic Complexity

**Big O Notation**:

- O(1) - Constant: Ideal (hash lookup)
- O(log n) - Logarithmic: Excellent (binary search)
- O(n) - Linear: Good (single loop)
- O(n log n) - Log-linear: Acceptable (efficient sorting)
- O(n²) - Quadratic: Bad (nested loops)
- O(2ⁿ) - Exponential: Terrible (avoid)

**Example - Nested Loops**:

```python
# Bad: O(n²)
for user in users:
    for order in orders:
        if order.user_id == user.id:
            process(order)

# Good: O(n)
orders_by_user = defaultdict(list)
for order in orders:
    orders_by_user[order.user_id].append(order)

for user in users:
    for order in orders_by_user[user.id]:
        process(order)
```

**Example - Inefficient Search**:

```python
# Bad: O(n) lookup
if item in item_list:  # Linear scan

# Good: O(1) lookup
if item in item_set:   # Hash lookup
```

### Database Performance

**N+1 Query Problem**:

```python
# Bad: 1 + N queries
users = User.query.all()          # 1 query
for user in users:
    posts = Post.query.filter_by(user_id=user.id).all()  # N queries

# Good: 1 query with JOIN
users = User.query.options(joinedload(User.posts)).all()
```

**Missing Indexes**:

```sql
-- Slow: Full table scan
SELECT * FROM orders WHERE user_id = 123;

-- Add index
CREATE INDEX idx_orders_user ON orders(user_id);

-- Fast: Index scan
SELECT * FROM orders WHERE user_id = 123;
```

**SELECT * Abuse**:

```sql
-- Bad: Fetching unnecessary columns
SELECT * FROM users;  -- 50 columns, only need 2

-- Good: Select only needed columns
SELECT id, name FROM users;
```

### Memory Optimization

**Memory Leaks**:

```python
# Bad: Growing global cache
_cache = {}  # Never cleaned, grows forever

# Good: LRU cache with size limit
from functools import lru_cache

@lru_cache(maxsize=1000)
def expensive_function(arg):
    ...
```

**Large Object Streaming**:

```python
# Bad: Loading entire file into memory
data = open('huge_file.txt').read()  # 10GB file

# Good: Stream processing
with open('huge_file.txt') as f:
    for line in f:  # One line at a time
        process(line)
```

### Network and I/O

**Synchronous Blocking**:

```python
# Bad: Sequential API calls (1500ms total)
result1 = fetch_api_1()  # 500ms
result2 = fetch_api_2()  # 500ms
result3 = fetch_api_3()  # 500ms

# Good: Parallel with async (500ms total)
results = await asyncio.gather(
    fetch_api_1(),
    fetch_api_2(),
    fetch_api_3()
)
```

**Chatty APIs**:

```python
# Bad: Multiple round trips
for user_id in user_ids:
    user = api.get_user(user_id)  # N API calls

# Good: Batch request
users = api.get_users_batch(user_ids)  # 1 API call
```

## Performance Profiling

**Python Profiling**:

```python
import cProfile
import pstats

profiler = cProfile.Profile()
profiler.enable()
slow_function()
profiler.disable()

stats = pstats.Stats(profiler)
stats.sort_stats('cumulative')
stats.print_stats(10)  # Top 10 slowest
```

**Database Profiling**:

```sql
-- PostgreSQL
EXPLAIN ANALYZE
SELECT * FROM orders WHERE user_id = 123;

-- MySQL
EXPLAIN SELECT * FROM orders WHERE user_id = 123;
```

## Caching Strategies

**Application Caching**:

```python
from redis import Redis
cache = Redis()

def get_user(user_id):
    # Check cache
    cached = cache.get(f"user:{user_id}")
    if cached:
        return json.loads(cached)

    # Fetch from database
    user = User.query.get(user_id)

    # Store in cache (1 hour)
    cache.setex(f"user:{user_id}", 3600, json.dumps(user))
    return user
```

**HTTP Caching**:

```python
@app.route('/static/image.jpg')
def serve_image():
    response = send_file('image.jpg')
    response.headers['Cache-Control'] = 'public, max-age=31536000'
    return response
```

## Examples

### Example 1: N+1 Query Optimization

**Before** (100,001 queries, 8 minutes):

```python
users = User.query.all()
for user in users:
    user.posts = Post.query.filter_by(user_id=user.id).all()
```

**After** (1 query, 0.5 seconds):

```python
users = User.query.options(joinedload(User.posts)).all()
```

**Performance Gain**: 1000x faster

### Example 2: Algorithm Optimization

**Before** (O(n²), 10 seconds for 1000 items):

```python
def find_duplicates(items):
    duplicates = []
    for i in range(len(items)):
        for j in range(i + 1, len(items)):
            if items[i] == items[j]:
                duplicates.append(items[i])
    return duplicates
```

**After** (O(n), 0.01 seconds for 1000 items):

```python
def find_duplicates(items):
    seen = set()
    duplicates = set()
    for item in items:
        if item in seen:
            duplicates.add(item)
        seen.add(item)
    return list(duplicates)
```

**Performance Gain**: 1000x faster

## Performance Checklist

**Database**:

- Indexes on query predicates
- No N+1 queries (use eager loading)
- Connection pooling configured
- Query result caching where appropriate
- SELECT only needed columns

**Algorithms**:

- No nested loops on large datasets
- Appropriate data structures (set vs list)
- Binary search instead of linear
- Memoization for expensive calculations

**Network**:

- Batch API requests
- Async for I/O operations
- CDN for static assets
- Compression enabled (gzip)
- HTTP/2 or HTTP/3

**Memory**:

- Stream large files
- LRU cache with limits
- Resources released (connections closed)
- No memory leaks

## Best Practices

**Measure First**:

1. Profile to find bottlenecks
2. Set performance targets
3. Optimize hot paths only
4. Measure improvement with numbers

**Low-Hanging Fruit**:

- Add database indexes
- Fix N+1 queries
- Add caching layer
- Use CDN for static files
- Enable compression

**Do**:

- Profile before optimizing
- Focus on actual bottlenecks
- Measure performance gains
- Set performance budgets
- Test at scale

**Don't**:

- Optimize without profiling
- Prematurely optimize
- Sacrifice readability for micro-optimization
- Ignore algorithmic complexity
- Guess where performance issues are

## See Also

Related code quality skills:

- code-reviewer: Performance review in PRs
- code-auditor: System-wide performance audits
- code-mentor: Teaching performance concepts

Related foundational skills:

- first-principles: Understanding performance requirements
- tech-stack-advisor: Framework-specific optimizations
