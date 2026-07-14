# Refactoring Intelligence Engine

## Purpose
Structured approach to codebase refactoring: strategy selection, risk assessment, migration planning, and execution guidance. Ensures refactoring improves rather than destabilizes the system.

## Module Contract
| Attribute | Value |
|-----------|-------|
| **Inputs** | Codebase analysis, technical debt inventory, goals, constraints |
| **Outputs** | Refactoring plan, risk assessment, step-by-step guide, validation criteria |
| **Responsibilities** | Strategy selection, risk assessment, migration planning, execution guidance |
| **Constraints** | Never refactor without tests; never big-bang without rollback plan |
| **Decision Rules** | Prefer incremental refactoring; always preserve behavior |
| **Validation Checklist** | Tests pass at each step, behavior preserved, debt reduced |
| **Failure Handling** | If refactoring too risky, recommend alternative approaches |

## Refactoring Strategy Selection

### Strategy Decision Tree
```
REFACTORING_GOAL?
├── Improve readability → Extract method, rename, simplify
├── Reduce complexity → Extract class, decompose conditional
├── Remove duplication → Extract common code, introduce abstraction
├── Improve performance → Optimize hot paths, caching, lazy loading
├── Modernize patterns → Gradual pattern adoption
├── Architectural change → Strangler Fig, branch by abstraction
├── Dependency update → Compatibility layer, gradual migration
└── Tech stack change → Adapter pattern, incremental replacement
```

### Strategy Patterns

| Strategy | When | Risk | Approach |
|----------|------|------|----------|
| **Sprout Method** | Add new feature in clean way | Low | New code in new method/class |
| **Sprout Class** | New responsibility | Low | New class, gradually migrate |
| **Wrap Method** | Change existing method | Low | Wrap with new implementation |
| **Wrap Class** | Replace class behavior | Medium | Adapter/Facade pattern |
| **Extract Component** | Modularize | Medium | Extract with interface boundary |
| **Strangler Fig** | Replace subsystem | High | Incremental replacement |
| **Branch by Abstraction** | Large-scale change | High | Abstract, then switch |

## Risk Assessment

### Refactoring Risk Factors
```json
{
  "risk_assessment": {
    "code_coverage": {
      "value": "75%",
      "risk": "medium",
      "mitigation": "Add tests before refactoring"
    },
    "complexity": {
      "value": "high cyclomatic",
      "risk": "high",
      "mitigation": "Pre-refactor complexity reduction"
    },
    "dependencies": {
      "value": "15 callers",
      "risk": "high",
      "mitigation": "Interface extraction first"
    },
    "criticality": {
      "value": "payment processing",
      "risk": "critical",
      "mitigation": "Feature flag + extensive testing"
    }
  }
}
```

### Risk Mitigation Hierarchy
1. **Tests first** - Ensure comprehensive test coverage
2. **Feature flags** - Allow instant rollback
3. **Incremental** - Small, reviewable changes
4. **Interface boundaries** - Extract interfaces to isolate changes
5. **Parallel implementations** - Run old and new side-by-side

## Migration Planning

### Migration Steps Template
```markdown
## Refactoring Plan: [Description]

### Current State
[What the code looks like now]

### Target State
[What the code should look like]

### Migration Steps
| Step | Action | Validation | Risk |
|------|--------|------------|------|
| 1 | Add tests | Tests pass | Low |
| 2 | Extract interface | Compilation passes | Low |
| 3 | Create new implementation | Tests pass | Medium |
| 4 | Feature flag integration | Toggle works | Medium |
| 5 | Gradual rollout | Monitoring clean | High |
| 6 | Remove old code | Tests pass | Medium |

### Rollback Plan
[How to revert at each step]

### Success Criteria
- [ ] All tests pass
- [ ] Performance maintained or improved
- [ ] Code coverage maintained
- [ ] No regression in monitoring
```

## Validation During Refactoring

### Per-Step Validation
```
VALIDATE_STEP(step):
  1. All existing tests pass
  2. New tests added for changed code
  3. Performance benchmarks maintained
  4. Static analysis passes
  5. Code review completed
  6. Integration tests pass
```

### Post-Refactoring Validation
```
VALIDATE_REFACTORING(result):
  1. Behavior unchanged (black-box testing)
  2. Metrics improved:
     - Complexity reduced
     - Duplication reduced
     - Testability improved
  3. No performance regression
  4. No security degradation
  5. Documentation updated
```

## Validation Checklist

- [ ] Refactoring goal clearly defined
- [ ] Strategy selected with rationale
- [ ] Risk assessment completed
- [ ] Tests in place before starting
- [ ] Migration plan with rollback
- [ ] Feature flags considered for risky changes
- [ ] Validation criteria defined
- [ ] Step-by-step execution plan ready
