# Quality Scoring Engine

## Purpose
Multi-layer validation system with automated quality scoring for catching errors, preventing hallucinations, and ensuring quality. Applies at generation time, review time, and run time. Provides quantitative quality assessment.

## Module Contract
| Attribute | Value |
|-----------|-------|
| **Inputs** | Generated output, requirements, standards, context |
| **Outputs** | Quality score, validation report, improvement recommendations |
| **Responsibilities** | Validation, scoring, anti-hallucination, quality assessment |
| **Constraints** | No false positives on security; never approve unverified APIs |
| **Decision Rules** | Score < 3 requires revision; security failures are blocking |
| **Validation Checklist** | All layers checked, score calculated, report generated |
| **Failure Handling** | If validation fails, provide specific remediation guidance |

## Validation Layers

### Layer 1: Syntax Validation

**Code Generation:**
```
VALIDATE_SYNTAX(code):
  - Correct language syntax
  - Balanced brackets/braces
  - Proper indentation
  - Valid identifiers
  - Type consistency (if typed language)
  - Import/require statements valid
```

**Configuration:**
```
VALIDATE_CONFIG(config):
  - Valid format (JSON/YAML/TOML)
  - Required fields present
  - Field types correct
  - No circular references
  - Values within valid ranges
```

### Layer 2: Semantic Validation

**Logic Verification:**
```
VALIDATE_LOGIC(code):
  - All branches reachable
  - No dead code
  - Variables initialized before use
  - Resource cleanup (close, dispose)
  - Error paths handled
  - Race conditions identified
```

**API Validation:**
```
VALIDATE_APIS(code):
  - Functions called exist
  - Arguments match signatures
  - Return values handled
  - Exceptions caught or documented
  - No deprecated API usage
  - Version compatibility checked
```

### Layer 3: Pattern Validation

**Architecture Compliance:**
```
VALIDATE_ARCHITECTURE(design):
  - Patterns correctly applied
  - Responsibilities properly separated
  - Dependencies flow correctly
  - No circular dependencies
  - Interface segregation followed
  - Dependency inversion applied
```

**Anti-Pattern Detection:**
```
DETECT_ANTI_PATTERNS(code):
  - God objects/classes
  - Spaghetti code
  - Copy-paste duplication
  - Magic numbers/strings
  - Hardcoded configuration
  - Tight coupling
  - Premature abstraction
  - Leaky abstractions
```

### Layer 4: Security Validation

See 07-security-intelligence.md for comprehensive security validation.

Quick checks:
```
VALIDATE_SECURITY(code):
  - No secrets in code
  - Input validation present
  - Output encoding applied
  - Authentication checked
  - Authorization enforced
  - SQL injection prevented
  - XSS prevented
  - CSRF protected
```

### Layer 5: Performance Validation

See 08-performance-intelligence.md for detailed performance analysis.

Quick checks:
```
VALIDATE_PERFORMANCE(code):
  - No N+1 queries
  - No memory leaks
  - No blocking in async context
  - Efficient data structures
  - Appropriate caching
  - Resource limits respected
```

## Quality Scoring (v3)

### Automated Scoring Matrix

Each layer contributes to overall quality score:

```
QUALITY_SCORE(output):
  syntax_score = validate_syntax(output)      // 0-100
  semantic_score = validate_logic(output)      // 0-100
  pattern_score = validate_architecture(output) // 0-100
  security_score = validate_security(output)    // 0-100 (blocking)
  performance_score = validate_performance(output) // 0-100

  // Weighted aggregate
  overall = (syntax × 0.15) + (semantic × 0.25) + (pattern × 0.20) + (security × 0.25) + (performance × 0.15)

  // Security is blocking: if security_score < 50, overall capped at 49
```

### Score Interpretation

| Score | Grade | Action |
|-------|-------|--------|
| 90-100 | A | Approve - excellent quality |
| 80-89 | B | Approve with minor notes |
| 70-79 | C | Accept with improvements noted |
| 60-69 | D | Revise - significant issues |
| 50-59 | F | Major revision required |
| < 50 | Blocked | Security or critical failure |

### Dimension Scoring

| Dimension | Weight | Checks |
|-----------|--------|--------|
| Correctness | 25% | Logic, syntax, API validity |
| Security | 25% | Secrets, injection, auth, encoding |
| Maintainability | 20% | Patterns, coupling, documentation |
| Performance | 15% | Efficiency, resource usage |
| Completeness | 15% | Requirements coverage, edge cases |

## Testing Strategy

### Test Generation

When generating code, also generate tests:

```
GENERATE_TESTS(code):
  Unit Tests:
    - Happy path for each function
    - Edge cases (null, empty, max values)
    - Error cases (exceptions, failures)
    - Boundary conditions
  
  Integration Tests:
    - Component interactions
    - API contract compliance
    - Data flow validation
  
  E2E Tests (if applicable):
    - Critical user flows
    - Cross-browser (frontend)
    - Cross-platform (mobile)
```

### Test Coverage Targets

| Component Type | Unit | Integration | E2E |
|----------------|------|-------------|-----|
| Core business logic | 90%+ | 70%+ | - |
| API endpoints | 80%+ | 80%+ | Key flows |
| UI components | 70%+ | 60%+ | Critical paths |
| Infrastructure | 60%+ | 80%+ | Deployment |
| Utilities | 80%+ | - | - |

## Anti-Hallucination Protocol

### API Verification Checklist

Before referencing any API, verify:
- [ ] Function/class name exists in actual library
- [ ] Parameters match actual signature
- [ ] Return type is correct
- [ ] Not deprecated in current version
- [ ] Works in specified environment

### Pattern Verification Checklist

Before recommending any pattern, verify:
- [ ] Pattern name is correct
- [ ] Implementation matches known pattern
- [ ] Applicability conditions are stated
- [ ] Tradeoffs are accurate
- [ ] Not confused with similar pattern

### Fact Verification Levels

| Level | Method | Confidence |
|-------|--------|------------|
| **Built-in** | In training data, no ambiguity | 95%+ |
| **Verifiable** | Can check with search/tool use | 90%+ |
| **Probable** | Consistent with known facts | 75% |
| **Speculative** | Reasonable but unverified | 50% |
| **Unknown** | Cannot assess | State explicitly |

## Review Framework

### Self-Review Protocol

Before delivering any code:

```
SELF_REVIEW:
  1. Read code as if reviewing peer's work
  2. Check against acceptance criteria
  3. Verify no missing error handling
  4. Check naming clarity
  5. Verify comments add value (not restate code)
  6. Check for testability
  7. Verify no TODO without ticket reference
  8. Check for consistency with codebase style
```

### Review Dimensions

| Dimension | Check |
|-----------|-------|
| Correctness | Does it do what it should? |
| Completeness | Are all requirements met? |
| Clarity | Is intent obvious? |
| Efficiency | Is performance acceptable? |
| Safety | Are errors handled? |
| Maintainability | Can it be changed easily? |
| Testability | Can it be verified? |
| Security | Is it safe from attacks? |
| Consistency | Does it match existing code? |
| Documentation | Is it well explained? |

## Error Classification

When issues found, classify severity:

| Severity | Definition | Action |
|----------|------------|--------|
| **Critical** | Security vulnerability, data loss, crash | Must fix before merge |
| **Major** | Incorrect behavior, significant performance issue | Must fix before merge |
| **Minor** | Code smell, style issue, missing optimization | Should fix |
| **Info** | Suggestion, alternative approach | Optional |

## Validation Checklist by Output Type

### Code Output
- [ ] Compiles/syntax valid
- [ ] All imports resolved
- [ ] Error handling present
- [ ] Input validation present
- [ ] No hardcoded secrets
- [ ] Tests included or testable
- [ ] Documentation/comments present
- [ ] Follows language conventions
- [ ] Quality score >= 70

### Architecture Output
- [ ] Components identified
- [ ] Interfaces defined
- [ ] Data flow documented
- [ ] Failure modes addressed
- [ ] Scalability considered
- [ ] Security boundaries defined
- [ ] Deployment model specified
- [ ] Quality score >= 75

### Configuration Output
- [ ] Valid format
- [ ] Required fields present
- [ ] Environment-specific values parameterized
- [ ] Secrets externalized
- [ ] Defaults sensible
- [ ] Documentation comments present
- [ ] Quality score >= 80

## Validation Checklist

- [ ] All 5 validation layers checked
- [ ] Quality score calculated
- [ ] Anti-hallucination protocol applied
- [ ] Security validation passed (non-blocking)
- [ ] Self-review completed
- [ ] Improvement recommendations generated (if score < 90)
