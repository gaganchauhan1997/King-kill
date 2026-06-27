# Engineering Knowledge Graph

## Purpose
Structured ontology for organizing engineering knowledge across all domains. Enables systematic retrieval, pattern matching, and cross-domain reasoning.

## Node Types

### 1. Technology Nodes
```json
{
  "id": "tech:react:18",
  "type": "technology",
  "category": "frontend-framework",
  "name": "React",
  "version": "18.x",
  "paradigm": ["component-based", "declarative", "functional"],
  "maturity": "stable",
  "ecosystem_maturity": 5,
  "learning_curve": "moderate",
  "team_availability": "high",
  "use_cases": ["spa", "dashboard", "interactive-ui"],
  "avoid_for": ["seo-critical-static", "simple-pages", "rapid-prototype"],
  "complexity_score": 3,
  "performance_profile": {
    "bundle_size": "medium",
    "runtime_overhead": "low",
    "rendering": "client-side-hydration",
    "concurrent_features": true
  }
}
```

### 2. Pattern Nodes
```json
{
  "id": "pattern:api-gateway",
  "type": "pattern",
  "category": "architectural",
  "name": "API Gateway",
  "classification": "structural",
  "problem": "Need unified entry point for multiple services",
  "solution": "Single entry point handling routing, auth, rate limiting",
  "applicability": ["microservices", "multi-team", "multi-protocol"],
  "consequences": {
    "benefits": ["centralized-cross-cutting", "simplified-clients", "protocol-translation"],
    "liabilities": ["single-point-of-failure", "additional-latency", "complexity"]
  },
  "related_patterns": ["pattern:bff", "pattern:load-balancer", "pattern:service-mesh"],
  "confidence": 5
}
```

### 3. Quality Attribute Nodes
```json
{
  "id": "qa:scalability",
  "type": "quality-attribute",
  "name": "Scalability",
  "dimensions": ["horizontal", "vertical", "functional"],
  "measurement": {
    "throughput": "requests/second",
    "capacity": "concurrent users",
    "growth": "linear vs exponential resource needs"
  },
  "tactics": [
    "load-balancing",
    "caching",
    "async-processing",
    "sharding",
    "auto-scaling",
    "stateless-design"
  ],
  "anti_patterns": ["shared-state", "synchronous-chains", "database-sessions"]
}
```

### 4. Domain Nodes
```json
{
  "id": "domain:e-commerce",
  "type": "domain",
  "name": "E-Commerce",
  "subdomains": ["catalog", "cart", "checkout", "inventory", "payment", "fulfillment"],
  "ubiquitous_language": {
    "product": "sellable item with variants",
    "order": "confirmed purchase intent",
    "fulfillment": "picking, packing, shipping process"
  },
  "common_integrations": ["payment-gateway", "shipping-provider", "tax-calculation", "inventory-management"],
  "regulatory": ["PCI-DSS", "consumer-protection", "tax-compliance"]
}
```

### 5. Decision Nodes
```json
{
  "id": "decision:frontend-framework-2024",
  "type": "decision",
  "question": "Which frontend framework for enterprise SPA?",
  "context": ["team-size", "performance-requirements", "seo-needs", "existing-skills"],
  "options": ["react", "vue", "angular", "svelte", "solid"],
  "criteria": ["ecosystem", "performance", "learning-curve", "hiring", "tooling"],
  "default_for": {
    "enterprise-large-team": "react",
    "performance-critical": "solid",
    "gradual-migration": "vue",
    "full-framework-needed": "angular"
  }
}
```

## Edge Types

- `implements`: Pattern implements Quality Attribute
- `enables`: Technology enables Pattern
- `contradicts`: Pattern/Technology conflicts with another
- `composes`: Component composed of other components
- `depends_on`: Runtime or build dependency
- `alternative_to`: Substitutable option
- `optimizes_for`: Designed to improve specific QA
- `degrades`: Negatively impacts specific QA

## Query Patterns

### Pattern 1: Technology Selection
```
MATCH (t:Technology)-[:optimizes_for]->(qa:QualityAttribute)
WHERE qa.name IN $requirements
AND t.maturity = "stable"
RETURN t, COLLECT(qa) as quality_matches
ORDER BY SIZE(quality_matches) DESC
```

### Pattern 2: Pattern Discovery
```
MATCH (p:Pattern)-[:implements]->(qa:QualityAttribute)
WHERE qa.name = $target_quality
MATCH (p)-[:enables]-(t:Technology)
WHERE t.name = $existing_stack
RETURN p, qa, t
```

### Pattern 3: Architecture Validation
```
MATCH (d:Decision)<-[:addresses]-(p:Pattern)
WHERE d.id = $decision_id
MATCH (p)-[:contradicts]-(conflict:Pattern)
RETURN p, conflict
```

## Domain Categories

| Category | Domains |
|----------|---------|
| Web | SPA, SSR, Static, PWA, Real-time |
| API | REST, GraphQL, gRPC, WebSocket, Webhook |
| Data | OLTP, OLAP, Streaming, Cache, Search |
| Infrastructure | Serverless, Container, VM, Edge |
| AI/ML | Training, Inference, RAG, Agent |
| Security | AuthZ, AuthN, Encryption, Audit |
| Integration | ETL, ESB, Event Bus, Message Queue |
