# Impact Analysis Engine

## Purpose
Predict and assess the effects of changes across the system. Identifies blast radius, dependency impacts, and cascading effects before implementation.

## Module Contract
| Attribute | Value |
|-----------|-------|
| **Inputs** | Proposed change, repository map, dependency graph, system model |
| **Outputs** | Impact report, blast radius, risk assessment, mitigation plan |
| **Responsibilities** | Change impact prediction, blast radius analysis, risk identification |
| **Constraints** | Analysis is predictive; actual impact may differ |
| **Decision Rules** | Always run impact analysis before modifying existing code |
| **Validation Checklist** | All dependencies traced, blast radius mapped, risks identified |
| **Failure Handling** | If dependencies unknown, flag as high-risk and recommend discovery |

## Impact Analysis Framework

### Change Classification
```json
{
  "change": {
    "type": "api_change|database_migration|dependency_update|refactor|feature_addition",
    "scope": "isolated|component|system_wide",
    "risk_level": "low|medium|high|critical",
    "rollback_complexity": "simple|moderate|complex"
  }
}
```

### Blast Radius Analysis
```
ANALYZE_BLAST_RADIUS(change, system):
  1. Identify direct impact:
     - Modified files/components
     - Direct callers/users
     - Database schema changes
  
  2. Identify indirect impact:
     - Transitive dependencies
     - Downstream consumers
     - Integration points
  
  3. Identify systemic impact:
     - Performance effects
     - Security implications
     - Operational changes
  
  4. Map rollback requirements
  
  5. Estimate testing scope
```

### Impact Dimensions

| Dimension | Analysis | Output |
|-----------|----------|--------|
| **Code Impact** | Files changed, callers affected | List of affected components |
| **Data Impact** | Schema changes, migrations | Migration plan, data compatibility |
| **API Impact** | Contract changes, consumers | Breaking change assessment |
| **Performance Impact** | Latency, throughput, resources | Performance delta estimate |
| **Security Impact** | Attack surface, auth changes | Security review requirements |
| **Operational Impact** | Deployment, monitoring, runbooks | Ops changes needed |
| **Team Impact** | Onboarding, knowledge transfer | Communication plan |
| **Cost Impact** | Infrastructure, licensing | Cost delta estimate |

## Dependency Impact Tracing

```
TRACE_IMPACT(change_point):
  1. Find all direct dependents
  2. For each dependent, find its dependents
  3. Continue until leaf nodes
  4. Categorize by impact type:
     - Breaking: Must change
     - Behavior change: Should verify
     - No impact: Document as safe
  5. Generate impact tree
```

## Risk Assessment

### Change Risk Matrix
| Change Type | Low Risk | Medium Risk | High Risk |
|-------------|----------|-------------|-----------|
| **API Change** | Additive only | Behavior change | Contract break |
| **Database** | Index addition | Column addition | Schema refactor |
| **Dependency** | Patch update | Minor update | Major update |
| **Refactor** | Internal only | Cross-component | Core logic |
| **Feature** | Behind flag | New endpoint | Core flow change |

### Rollback Assessment
```
ASSESS_ROLLBACK(change):
  1. Can change be rolled back? (yes/no/partial)
  2. Rollback time estimate
  3. Data compatibility on rollback
  4. Rollback testing requirements
  5. Hotfix path if rollback fails
```

## Impact Report Template

```markdown
## Impact Analysis: [Change Description]

### Change Summary
- **Type**: ...
- **Scope**: ...
- **Risk Level**: ...

### Blast Radius
```
[Affected components diagram]
```

### Impact by Dimension
| Dimension | Impact | Severity | Mitigation |
|-----------|--------|----------|------------|
| Code | ... | ... | ... |
| Data | ... | ... | ... |
| API | ... | ... | ... |
| Performance | ... | ... | ... |
| Security | ... | ... | ... |
| Operations | ... | ... | ... |

### Testing Requirements
- [ ] Unit tests for changed components
- [ ] Integration tests for affected flows
- [ ] Performance regression tests
- [ ] Rollback procedure tested

### Rollback Plan
1. [Step 1]
2. [Step 2]

### Approval Requirements
- [ ] Code review
- [ ] Architecture review (if high impact)
- [ ] Security review (if security impact)
- [ ] Performance review (if perf impact)
```

## Validation Checklist

- [ ] Change classified correctly
- [ ] Blast radius fully mapped
- [ ] All impact dimensions assessed
- [ ] Dependencies traced
- [ ] Rollback plan defined
- [ ] Testing scope identified
- [ ] Risk level assigned
- [ ] Mitigations documented
