# Performance Intelligence

## Purpose
Systematic performance engineering: measurement, optimization, benchmarking, capacity planning, and predictive performance analysis.

## Module Contract
| Attribute | Value |
|-----------|-------|
| **Inputs** | System design, code, performance requirements, resource constraints |
| **Outputs** | Performance assessment, optimization plan, capacity projections |
| **Responsibilities** | Measurement, optimization, benchmarking, prediction, capacity planning |
| **Constraints** | No optimization without measurement; no premature optimization |
| **Decision Rules** | Measure first, optimize highest-impact bottlenecks |
| **Validation Checklist** | Baseline established, bottlenecks identified, optimizations measured |
| **Failure Handling** | If measurement not possible, estimate with confidence bounds |

## Performance Measurement

### Key Metrics

| Metric | Definition | Target | Measurement |
|--------|-----------|--------|-------------|
| **Latency** | Time to complete request | Context-dependent | p50, p95, p99 |
| **Throughput** | Requests per second | Meet demand + headroom | Load testing |
| **Error Rate** | Failed request percentage | <0.1% | Monitoring |
| **Resource Utilization** | CPU/Memory/IO usage | <80% sustained | APM tools |
| **Availability** | Uptime percentage | 99.9% - 99.999% | Monitoring |

### Latency Budgets

```json
{
  "latency_budgets": {
    "total_p95": "200ms",
    "breakdown": {
      "dns_lookup": "10ms",
      "tls_handshake": "20ms",
      "network_latency": "30ms",
      "load_balancer": "5ms",
      "application_processing": "100ms",
      "database_query": "30ms",
      "response_serialization": "5ms"
    }
  }
}
```

## Bottleneck Analysis

### Common Bottlenecks

| Layer | Symptoms | Diagnosis | Solutions |
|-------|----------|-----------|-----------|
| **Database** | Slow queries, high CPU | Query analysis, EXPLAIN | Indexing, query rewrite, caching |
| **Network** | Latency spikes, timeouts | Traceroute, bandwidth test | CDN, connection pooling, compression |
| **Application** | High CPU, memory growth | Profiling, heap dumps | Algorithm optimization, caching |
| **External APIs** | Variable latency | Response time tracking | Circuit breaker, caching, async |
| **Memory** | GC pressure, OOM | Heap analysis, GC logs | Object pooling, streaming, limits |
| **Disk I/O** | High await time | iostat, disk profiling | SSD, batching, async writes |

### Profiling Strategy

```
PROFILE_SYSTEM:
  1. Identify slow transactions from APM
  2. Reproduce in controlled environment
  3. Profile CPU (hotspots, flame graphs)
  4. Profile Memory (allocations, retention)
  5. Profile I/O (disk, network)
  6. Analyze database (slow query log, execution plans)
  7. Correlate findings
  8. Prioritize by impact × effort
```

## Performance Prediction (v3)

### Predictive Performance Modeling

Estimate performance before implementation:

```
PREDICT_PERFORMANCE(design):
  1. Identify critical path
  2. Estimate latency per component:
     - Database: query count × estimated query time
     - Network: round trips × latency
     - Processing: algorithm complexity × data size
     - External: API call count × API latency
  3. Sum critical path latency
  4. Estimate throughput: 1 / total_latency × parallelism
  5. Identify likely bottlenecks
  6. Recommend optimizations before coding
```

### Performance Estimation Framework

| Component | Base Latency | Scaling Factor | Estimation Method |
|-----------|-------------|----------------|-------------------|
| DB read (indexed) | 1-5ms | × query count | Query plan estimate |
| DB write | 5-20ms | × write count | Transaction complexity |
| DB scan (unindexed) | 100ms-10s | × table size | Table size × row time |
| Cache hit | 0.1-1ms | × hit count | Network + deserialize |
| Cache miss | DB read + 1ms | × miss rate | DB time + overhead |
| HTTP call (internal) | 5-20ms | × call count | Network latency |
| HTTP call (external) | 50-500ms | × call count | External API SLA |
| Serialization | 0.1-10ms | × payload size | Payload size / throughput |

### Capacity Prediction

```
PREDICT_CAPACITY(requirements):
  1. Define peak traffic: requests/second
  2. Estimate resource per request:
     - CPU: ms/request
     - Memory: MB/request
     - DB: queries/request
     - Network: KB/request
  3. Calculate instance requirements:
     instances = peak_traffic × resource_per_request / instance_capacity
  4. Apply safety factor (1.5x)
  5. Estimate cost
```

## Optimization Strategies

### Database Optimization

1. **Indexing strategy**
   - Covering indexes for frequent queries
   - Partial indexes for filtered queries
   - Avoid over-indexing (write penalty)

2. **Query optimization**
   - N+1 query elimination
   - Batch operations
   - Read replicas for read-heavy workloads
   - Connection pooling

3. **Caching strategy**
   ```
   CACHE_HIERARCHY:
     L1: Application cache (in-memory, local)
     L2: Distributed cache (Redis, Memcached)
     L3: CDN (static assets, edge)
     L4: Database cache (query cache, buffer pool)
   
   CACHING_PATTERNS:
     - Cache-aside: Application manages cache
     - Write-through: Write to cache + DB
     - Write-behind: Async DB write
     - Read-through: Cache loads from DB on miss
   ```

### Application Optimization

1. **Async processing**
   - Non-blocking I/O
   - Message queues for background work
   - Event-driven updates

2. **Resource management**
   - Object pooling (DB connections, HTTP clients)
   - Streaming for large data
   - Pagination for large collections
   - Compression (gzip, brotli)

3. **Algorithm efficiency**
   - O(n) vs O(n²) matters at scale
   - Early termination
   - Lazy evaluation
   - Batch processing

### Frontend Optimization

1. **Loading performance**
   - Code splitting (route-based, component-based)
   - Lazy loading (images, components, data)
   - Preloading critical resources
   - Resource hints (preload, prefetch, preconnect)

2. **Runtime performance**
   - Minimize re-renders
   - Virtual scrolling for large lists
   - Debounce/throttle event handlers
   - Web Workers for heavy computation

3. **Network optimization**
   - HTTP/2 or HTTP/3
   - Compression (brotli preferred)
   - CDN for static assets
   - Service Worker for caching

## Benchmarking

### Benchmark Design

```
BENCHMARK_DESIGN:
  1. Define what to measure (latency, throughput, both)
  2. Establish baseline (current system or competitor)
  3. Create realistic test data
  4. Simulate realistic load patterns
  5. Isolate variables (change one thing at a time)
  6. Run sufficient iterations (statistical significance)
  7. Document environment (hardware, network, config)
```

### Load Testing Patterns

| Pattern | Use Case | Tool Examples |
|---------|----------|--------------|
| **Smoke** | Verify basic functionality | Simple script |
| **Load** | Expected load validation | k6, Artillery |
| **Stress** | Breaking point identification | k6, Locust |
| **Spike** | Sudden traffic handling | k6, JMeter |
| **Endurance** | Memory leak detection | Long-running k6 |
| **Soak** | Extended period stability | k6, custom |

### Performance Testing Checklist

- [ ] Test data is representative (size, distribution)
- [ ] Cache is warm (not cold start measurements)
- [ ] Network conditions simulated (if relevant)
- [ ] Concurrent users realistic
- [ ] Error scenarios included
- [ ] Metrics collected: latency (p50, p95, p99), throughput, errors, resource usage
- [ ] Results are reproducible

## Capacity Planning

### Formula

```
REQUIRED_CAPACITY = (Peak_Traffic × Safety_Factor) / Unit_Capacity

Where:
  Safety_Factor = 1.5 (50% headroom)
  Unit_Capacity = Throughput per instance
```

### Scaling Strategies

| Strategy | When | How |
|----------|------|-----|
| **Vertical** | Single node optimization | More CPU, RAM, faster disk |
| **Horizontal** | Distributed load | More instances, load balancer |
| **Functional** | Component-specific scaling | Separate services |
| **Auto-scaling** | Variable load | CPU/memory/trigger-based |

### Capacity Planning Template

```markdown
## Capacity Plan: [System]

### Current State
- Peak traffic: [X] requests/minute
- Current instances: [N]
- Current utilization: [X]%

### Growth Projections
- 3 months: [X]% increase
- 6 months: [X]% increase
- 12 months: [X]% increase

### Predicted Requirements (v3)
| Component | Current | 3mo | 6mo | 12mo |
|-----------|---------|-----|-----|------|
| App servers | ...     | ... | ... | ...  |
| Database    | ...     | ... | ... | ...  |
| Cache       | ...     | ... | ... | ...  |

### Scaling Triggers
- Scale out when: [metric > threshold]
- Scale in when: [metric < threshold]
- Max instances: [N]
- Min instances: [N]

### Cost Estimate
| Resource | Monthly Cost |
|----------|-------------|
| ...      | ...         |
```

## Performance Review Checklist

- [ ] Baseline established
- [ ] Bottlenecks identified
- [ ] Optimization prioritized by impact
- [ ] Changes measured against baseline
- [ ] No regressions in other metrics
- [ ] Monitoring in place for ongoing tracking
- [ ] Alerting thresholds configured
- [ ] Performance predicted for changes (v3)
