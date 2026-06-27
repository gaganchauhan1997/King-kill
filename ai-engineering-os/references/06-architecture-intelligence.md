# Architecture Intelligence

## Purpose
Comprehensive architectural pattern library with selection guidance, tradeoff analysis, and implementation strategies.

## Architectural Styles

### Monolithic

```json
{
  "style": "monolithic",
  "structure": "single-deployable-unit",
  "best_for": ["small-team", "rapid-development", "simple-domain", "early-stage"],
  "characteristics": {
    "deployment": "single-unit",
    "scaling": "vertical or full-unit-horizontal",
    "data": "shared-database",
    "communication": "in-process",
    "complexity": "low"
  },
  "tradeoffs": {
    "pros": ["simple-development", "easy-testing", "fast-deployment", "low-overhead"],
    "cons": ["limited-scalability", "tight-coupling", "technology-lock-in", "slow-release-cycle"]
  }
}
```

### Microservices

```json
{
  "style": "microservices",
  "structure": "independent-deployable-services",
  "best_for": ["large-team", "complex-domain", "independent-scaling", "multi-tech"],
  "characteristics": {
    "deployment": "independent",
    "scaling": "per-service",
    "data": "database-per-service",
    "communication": "async-messaging-or-sync-api",
    "complexity": "high"
  },
  "tradeoffs": {
    "pros": ["independent-scaling", "technology-diversity", "team-autonomy", "fault-isolation"],
    "cons": ["distributed-complexity", "network-latency", "data-consistency", "operational-overhead"]
  },
  "prerequisites": ["devops-culture", "monitoring", "ci-cd", "service-discovery"]
}
```

### Modular Monolith

```json
{
  "style": "modular-monolith",
  "structure": "single-deployable-with-internal-modules",
  "best_for": ["growing-team", "evolving-domain", "future-microservices"],
  "characteristics": {
    "deployment": "single-unit",
    "scaling": "full-unit (for now)",
    "data": "separate-schemas-or-databases",
    "communication": "in-process-with-boundaries",
    "complexity": "medium"
  },
  "tradeoffs": {
    "pros": ["simpler-ops", "clear-boundaries", "migration-path", "team-structure-prep"],
    "cons": ["partial-coupling", "database-complexity", "not-full-team-autonomy"]
  }
}
```

### Serverless

```json
{
  "style": "serverless",
  "structure": "function-as-a-service-with-managed-infra",
  "best_for": ["event-driven", "variable-load", "quick-start", "cost-optimize"],
  "characteristics": {
    "deployment": "function-level",
    "scaling": "auto-zero-to-infinity",
    "data": "managed-services",
    "communication": "events-or-api",
    "complexity": "medium"
  },
  "tradeoffs": {
    "pros": ["no-server-management", "auto-scaling", "pay-per-use", "fast-development"],
    "cons": ["cold-start", "vendor-lock-in", "execution-limits", "debugging-complexity"]
  }
}
```

### Event-Driven

```json
{
  "style": "event-driven",
  "structure": "producers-consumers-with-event-bus",
  "best_for": ["async-workflows", "multi-system-integration", "temporal-decoupling"],
  "characteristics": {
    "deployment": "mixed",
    "scaling": "consumer-group-based",
    "data": "event-sourcing-or-cqrs",
    "communication": "async-events",
    "complexity": "high"
  },
  "tradeoffs": {
    "pros": ["loose-coupling", "scalability", "extensibility", "audit-trail"],
    "cons": ["eventual-consistency", "complex-debugging", "schema-evolution", "ordering-challenges"]
  }
}
```

## Pattern Catalog

### Data Patterns

| Pattern | Problem | Solution | Tradeoffs |
|---------|---------|----------|-----------|
| CQRS | Read/write contention | Separate models for reads/writes | Complexity, eventual consistency |
| Event Sourcing | Audit requirement, state reconstruction | Store events, derive state | Complexity, storage growth |
| Saga | Distributed transaction | Sequence of local transactions | No immediate consistency, compensation logic |
| Outbox | Reliable message publishing | Atomic DB + message write | Additional table, polling |
| Sharding | Database scalability | Partition data across instances | Cross-shard queries, rebalancing |
| Materialized View | Complex query performance | Pre-computed read models | Staleness, update complexity |

### Communication Patterns

| Pattern | Problem | Solution | Tradeoffs |
|---------|---------|----------|-----------|
| API Gateway | Client service explosion | Single entry point | Additional hop, SPOF risk |
| BFF | Multiple client needs | Backend per frontend | Duplication, team coordination |
| Circuit Breaker | Cascading failures | Fail fast when downstream down | Complexity, tuning needed |
| Retry + Backoff | Transient failures | Automatic retry with delay | Latency, thundering herd |
| Bulkhead | Resource exhaustion | Isolate failure domains | Resource underutilization |
| Strangler Fig | Legacy migration | Incrementally replace system | Dual maintenance period |

### Structural Patterns

| Pattern | Problem | Solution | Tradeoffs |
|---------|---------|----------|-----------|
| Hexagonal | Framework coupling | Ports and adapters | Boilerplate, learning curve |
| Clean Architecture | Dependency management | Layered with dependency rule | Overhead for simple cases |
| Vertical Slice | Feature coupling | Features as vertical slices | Duplication, larger services |
| Plugin | Extensibility | Extension points | Interface design, security |
| Sidecar | Cross-cutting concerns | Separate container for concerns | Resource overhead |

## Selection Decision Tree

```
START
  |
  +-- Team size?
  |     +-- 1-3: Monolith or Serverless
  |     +-- 4-10: Modular Monolith
  |     +-- 11+: Microservices or Modular Monolith
  |
  +-- Domain complexity?
  |     +-- Simple: Monolith
  |     +-- Moderate: Modular Monolith
  |     +-- Complex: Microservices
  |
  +-- Scaling requirements?
  |     +-- Uniform: Monolith
  |     +-- Component-specific: Microservices
  |     +-- Spiky/Variable: Serverless
  |
  +-- Time to market?
  |     +-- Critical: Monolith or Serverless
  |     +-- Standard: Modular Monolith
  |     +-- Can invest: Microservices
  |
  +-- Existing infrastructure?
  |     +-- Minimal: Serverless or PaaS
  |     +-- Moderate: Modular Monolith
  |     +-- Mature: Full choice
```

## Quality Attribute Scenarios

Define requirements as scenarios:

```json
{
  "scenario": "Under normal operation, the system processes 10,000 orders/hour with <200ms p95 latency",
  "stimulus": "10,000 orders/hour sustained load",
  "source": "Customer purchase flow",
  "artifact": "Order processing pipeline",
  "environment": "Normal operation",
  "response": "Process orders successfully",
  "measure": "p95 latency < 200ms, 99.9% success rate"
}
```

## Architecture Review Checklist

- [ ] All functional requirements trace to components
- [ ] All quality attributes have scenarios
- [ ] Failure modes identified with mitigations
- [ ] Security boundaries defined
- [ ] Data flow documented
- [ ] Scalability limits understood
- [ ] Monitoring strategy defined
- [ ] Deployment architecture specified
- [ ] Migration path documented (if applicable)
- [ ] Cost model estimated
