# Architecture Decision Engine

## Purpose
Structured decision-making framework for technology selection, architecture choices, and design decisions with constraint solving, reasoning, and risk assessment. Replaces intuition with repeatable, auditable processes.

## Module Contract
| Attribute | Value |
|-----------|-------|
| **Inputs** | Decision context, constraints, options, criteria |
| **Outputs** | Decision recommendation, rationale, risk assessment, ADR |
| **Responsibilities** | Option generation, evaluation, constraint satisfaction, decision documentation |
| **Constraints** | Minimum 3 options unless domain is genuinely constrained |
| **Decision Rules** | Weighted scoring with sensitivity analysis; prefer reversible decisions |
| **Validation Checklist** | All options evaluated, sensitivity analysis performed, risks documented |
| **Failure Handling** | If no option satisfies hard constraints, report constraint conflict |

## Decision Framework

### Step 1: Context Mapping

Define the decision scope:

```json
{
  "decision_id": "unique-id",
  "category": "technology|architecture|pattern|infrastructure",
  "scope": {
    "organization_size": "startup|mid|enterprise",
    "team_expertise": "novice|intermediate|expert",
    "team_size": "small(2-5)|medium(6-15)|large(16+)",
    "time_constraints": "prototype|mvp|production",
    "budget_constraints": "minimal|moderate|unlimited"
  },
  "constraints": {
    "hard": ["must-haves"],
    "soft": ["nice-to-haves"],
    "anti": ["must-avoids"]
  },
  "success_criteria": ["measurable outcomes"],
  "reversibility": "fully|partially|irreversible"
}
```

### Step 2: Option Generation

Generate comprehensive option set:

1. **Obvious choices** - Market leaders, defaults
2. **Emerging options** - Growing adoption, modern approaches
3. **Niche options** - Specialized for specific constraints
4. **Composite options** - Hybrid approaches

Minimum 3 options unless domain is genuinely constrained.

### Step 3: Criteria Definition

Define weighted criteria based on context:

| Criterion | Weight | Measurement |
|-----------|--------|-------------|
| Performance | varies | Benchmarks, latency, throughput |
| Scalability | varies | Growth headroom, resource efficiency |
| Maintainability | varies | Complexity metrics, documentation |
| Security | varies | Attack surface, vulnerability history |
| Cost | varies | TCO, licensing, infrastructure |
| Team Fit | varies | Learning curve, existing skills |
| Ecosystem | varies | Package availability, community size |
| Maturity | varies | Version stability, breaking change history |

### Step 4: Evaluation Matrix

```json
{
  "options": ["option_a", "option_b", "option_c"],
  "criteria": {
    "performance": { "weight": 0.25, "max_score": 5 },
    "scalability": { "weight": 0.20, "max_score": 5 },
    "maintainability": { "weight": 0.20, "max_score": 5 },
    "security": { "weight": 0.15, "max_score": 5 },
    "cost": { "weight": 0.10, "max_score": 5 },
    "team_fit": { "weight": 0.10, "max_score": 5 }
  },
  "scores": {
    "option_a": { "performance": 4, "scalability": 3, "maintainability": 5, "security": 4, "cost": 3, "team_fit": 5 },
    "option_b": { "performance": 5, "scalability": 5, "maintainability": 3, "security": 3, "cost": 2, "team_fit": 2 },
    "option_c": { "performance": 3, "scalability": 4, "maintainability": 4, "security": 5, "cost": 4, "team_fit": 4 }
  }
}
```

Calculation: weighted_score = SUM(score * weight) for each option

### Step 5: Sensitivity Analysis

Test decision stability:
1. Vary weights +/- 20% - does winner change?
2. Remove single criteria - does winner change?
3. Add pessimistic estimates - does winner change?

If decision is sensitive to small changes, flag for additional analysis.

### Step 6: Risk Assessment

For top 2 options:

```json
{
  "risk_analysis": {
    "adoption_risk": "probability × impact of low adoption",
    "technical_risk": "probability × impact of technical limitation",
    "organizational_risk": "probability × impact of team/skill mismatch",
    "vendor_risk": "probability × impact of vendor/platform failure",
    "migration_risk": "probability × impact of migration complexity"
  }
}
```

## Constraint Solver (v3)

### Hard Constraint Satisfaction
The constraint solver ensures all hard constraints are satisfied before scoring:

```
SOLVE(constraints, options):
  1. Filter options: eliminate any that violate hard constraints
  2. If no options remain: report constraint conflict
  3. Score remaining options against soft constraints
  4. Rank by: soft constraint satisfaction × weighted criteria scores
  5. Return ranked options with constraint satisfaction report
```

### Constraint Conflict Resolution
When hard constraints conflict:
```
RESOLVE_CONFLICT(constraints):
  1. Identify conflicting constraint pairs
  2. Calculate relaxation impact for each constraint
  3. Recommend minimum relaxation to achieve feasibility
  4. Present tradeoff: "Relax constraint X by Y% to enable solution Z"
  5. Request human decision on which constraint to relax
```

### Constraint Types
| Type | Example | Enforcement |
|------|---------|-------------|
| Budget | Cost < $X/month | Hard - eliminate exceeding options |
| Compliance | Must be SOC 2 | Hard - eliminate non-compliant options |
| Performance | p95 < 200ms | Hard - eliminate failing options |
| Preference | Prefer open source | Soft - weight in scoring |
| Timeline | Deliver in 3 months | Soft - weight in scoring |

## Decision Documentation

```markdown
## Decision: [Title]

### Status
- Proposed | Accepted | Deprecated | Superseded by [ADR-XXX]

### Context
[What is the issue that we're seeing that is motivating this decision?]

### Constraints
| Type | Constraint | Satisfaction |
|------|-----------|-------------|
| Hard | ... | Met / Violated |
| Soft | ... | Met / Partial / Violated |

### Decision
[What is the change that we're proposing or have agreed to implement?]

### Constraint Analysis
[How constraints influenced the decision]

### Consequences
[What becomes easier or more difficult to do and any risks introduced]

### Alternatives Considered
| Option | Pros | Cons | Why Rejected | Score |
|--------|------|------|--------------|-------|
| ...    | ...  | ...  | ...          | ...   |

### Risk Assessment
| Risk Type | Probability | Impact | Mitigation |
|-----------|-------------|--------|------------|
| ...       | ...         | ...    | ...        |

### Sensitivity Analysis
[Is the decision stable under weight variation?]

### Compliance
- [ ] Security review passed
- [ ] Performance requirements met
- [ ] Cost constraints satisfied
- [ ] Team capability assessment complete
- [ ] Migration plan documented
- [ ] Hard constraints verified
```

## Common Decision Patterns

### Frontend Framework Selection
| Context | Recommendation | Confidence |
|---------|---------------|------------|
| Large team, enterprise | React + TypeScript | 5 |
| Performance critical, small team | Solid.js or Svelte | 4 |
| Full framework needed | Angular or Next.js | 4 |
| Gradual migration | Vue.js | 4 |
| Rapid prototyping | None (vanilla + htmx) | 4 |

### API Style Selection
| Context | Recommendation | Confidence |
|---------|---------------|------------|
| Public API, multiple clients | REST + OpenAPI | 5 |
| Complex data relationships | GraphQL | 4 |
| Internal microservices | gRPC | 5 |
| Real-time updates | WebSocket + REST | 4 |
| Simple CRUD | REST | 5 |

### Database Selection
| Context | Recommendation | Confidence |
|---------|---------------|------------|
| General purpose | PostgreSQL | 5 |
| Document store | MongoDB (if flexible schema needed) | 4 |
| High write throughput | ScyllaDB or Cassandra | 4 |
| Caching | Redis | 5 |
| Search | Elasticsearch or Meilisearch | 4 |
| Time series | TimescaleDB or InfluxDB | 4 |

### Deployment Model
| Context | Recommendation | Confidence |
|---------|---------------|------------|
| Startup, simple app | Vercel/Netlify/Railway | 5 |
| Container orchestration | Kubernetes (EKS/GKE) | 5 |
| Serverless functions | AWS Lambda + API Gateway | 4 |
| Enterprise on-prem | Kubernetes + Helm | 4 |
| Edge deployment | Cloudflare Workers | 4 |

## Anti-Patterns

- **Analysis paralysis**: Spending more time deciding than implementing
- **Resume-driven development**: Choosing based on trendiness
- **Golden hammer**: Using familiar tools for all problems
- **Premature optimization**: Over-engineering before validation
- **Not invented here**: Rejecting proven solutions
- **Permanent prototyping**: Never committing to a decision

## Validation Checklist

- [ ] At least 3 options generated (or domain-constrained justification)
- [ ] All hard constraints satisfied
- [ ] Weighted scoring applied consistently
- [ ] Sensitivity analysis performed
- [ ] Risk assessment completed for top options
- [ ] ADR template populated
- [ ] Decision stability confirmed under weight variation
- [ ] Constraint satisfaction documented
