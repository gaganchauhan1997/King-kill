# Validation Engine

## Purpose
Multi-layer validation system for catching errors, preventing hallucinations, and ensuring quality. Applies at generation time, review time, and run time.

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

### Architecture Output
- [ ] Components identified
- [ ] Interfaces defined
- [ ] Data flow documented
- [ ] Failure modes addressed
- [ ] Scalability considered
- [ ] Security boundaries defined
- [ ] Deployment model specified

### Configuration Output
- [ ] Valid format
- [ ] Required fields present
- [ ] Environment-specific values parameterized
- [ ] Secrets externalized
- [ ] Defaults sensible
- [ ] Documentation comments present
