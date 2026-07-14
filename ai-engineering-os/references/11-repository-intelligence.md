# Repository Intelligence Engine

## Purpose
Analyze existing codebases to understand structure, dependencies, patterns, technical debt, and health. Provides intelligence about the current state of a repository to inform architectural decisions, refactoring, and improvements.

## Module Contract
| Attribute | Value |
|-----------|-------|
| **Inputs** | Repository files, directory structure, dependency manifests, source code |
| **Outputs** | Repository map, dependency graph, health assessment, pattern inventory |
| **Responsibilities** | Structure analysis, dependency mapping, pattern detection, health scoring |
| **Constraints** | Read-only analysis; never modify without explicit instruction |
| **Decision Rules** | Always analyze before recommending changes to existing code |
| **Validation Checklist** | All files catalogued, dependencies mapped, patterns identified |
| **Failure Handling** | If repository is too large, sample and extrapolate with confidence bounds |

## Repository Structure Analysis

### Directory Mapping
```
MAP_REPOSITORY(repo):
  1. Catalog all directories and files
  2. Identify project type (language, framework, build tool)
  3. Map to known architecture patterns
  4. Identify entry points (main, index, app)
  5. Locate configuration files
  6. Identify test structure
  7. Map documentation files
```

### Language and Framework Detection
| Indicator | Detection Method |
|-----------|-----------------|
| package.json | Node.js / JavaScript |
| requirements.txt / pyproject.toml | Python |
| Cargo.toml | Rust |
| go.mod | Go |
| pom.xml / build.gradle | Java |
| Gemfile | Ruby |
| composer.json | PHP |
| .csproj / .sln | C# |

### Architecture Pattern Detection
```
DETECT_PATTERNS(repo):
  - Monolithic: Single entry point, shared database
  - Microservices: Multiple service directories, API contracts
  - Modular Monolith: Clear module boundaries, single deployable
  - Serverless: Function definitions, event triggers
  - Event-Driven: Event handlers, message queue configs
  - Layered: domain/application/infrastructure separation
  - Clean/Hexagonal: ports and adapters, dependency inversion
```

## Dependency Analysis

### Dependency Graph
```json
{
  "dependencies": {
    "direct": ["dep1", "dep2"],
    "transitive": ["dep1-a", "dep1-b", "dep2-a"],
    "dev": ["test-framework", "linter"],
    "external": ["third-party-api"],
    "internal": ["shared-library", "common-module"]
  },
  "dependency_health": {
    "outdated_count": 5,
    "vulnerable_count": 2,
    "abandoned_count": 1,
    "healthy_percentage": 85
  }
}
```

### Dependency Risk Assessment
| Risk Level | Criteria | Action |
|------------|----------|--------|
| **Critical** | Known CVE, no patch available | Immediate replacement required |
| **High** | Major version behind, deprecation announced | Plan migration within sprint |
| **Medium** | Minor version behind, stable | Update in next maintenance window |
| **Low** | Patch version behind | Update with next deployment |

## Code Health Scoring

### Health Dimensions
```json
{
  "repository_health": {
    "maintainability": {
      "code_complexity": "cyclomatic_complexity_avg",
      "duplication_percentage": "5%",
      "file_organization": "scored 1-5",
      "naming_consistency": "scored 1-5"
    },
    "test_coverage": {
      "overall_percentage": "75%",
      "by_module": {"core": "90%", "utils": "60%"},
      "test_types": {"unit": true, "integration": true, "e2e": false}
    },
    "documentation": {
      "readme_quality": "scored 1-5",
      "api_documentation": "scored 1-5",
      "inline_comments": "scored 1-5",
      "adr_count": 3
    },
    "security": {
      "secret_exposure": "none|low|medium|high",
      "dependency_vulns": "count",
      "security_headers": "present|partial|missing"
    },
    "performance": {
      "database_query_patterns": "analyzed",
      "caching_strategy": "present|missing",
      "async_patterns": "used|not_used"
    }
  }
}
```

### Health Score Calculation
```
HEALTH_SCORE = (maintainability × 0.25) + (test_coverage × 0.25) + (documentation × 0.20) + (security × 0.20) + (performance × 0.10)

Scale: 0-100
90-100: Excellent
70-89: Good
50-69: Needs improvement
<50: Critical attention required
```

## Pattern Inventory

### Detected Patterns
```
INVENTORY_PATTERNS(repo):
  1. Design patterns: Singleton, Factory, Repository, Strategy, etc.
  2. Architecture patterns: MVC, MVP, Clean Architecture, CQRS
  3. Integration patterns: API Gateway, BFF, Circuit Breaker
  4. Data patterns: Repository, Unit of Work, DAO
  5. Anti-patterns: God Class, Spaghetti Code, Copy-Paste
  6. Testing patterns: AAA, Given-When-Then, Page Object
```

### Technical Debt Identification
```
IDENTIFY_TECH_DEBT(repo):
  - Code smells: Long methods, large classes, deep nesting
  - Outdated dependencies
  - Missing tests for critical paths
  - Documentation gaps
  - Hardcoded values
  - TODO/FIXME comments
  - Bypassed quality gates
  - Workarounds and hacks
```

## Repository Intelligence Report Template

```markdown
## Repository Intelligence Report: [Repo Name]

### Overview
- **Language/Framework**: ...
- **Architecture Pattern**: ...
- **Repository Size**: ... files, ... lines of code
- **Health Score**: .../100

### Structure
```
[Directory tree with key files highlighted]
```

### Dependencies
| Category | Count | Risk Level |
|----------|-------|------------|
| Direct | ... | ... |
| Outdated | ... | ... |
| Vulnerable | ... | ... |

### Health Assessment
| Dimension | Score | Notes |
|-----------|-------|-------|
| Maintainability | ... | ... |
| Test Coverage | ... | ... |
| Documentation | ... | ... |
| Security | ... | ... |
| Performance | ... | ... |

### Detected Patterns
- [Pattern 1]: [Location] - [Assessment]
- [Pattern 2]: [Location] - [Assessment]

### Technical Debt
| Item | Severity | Location | Recommendation |
|------|----------|----------|----------------|
| ...  | ...      | ...      | ...            |

### Recommendations
1. [Priority 1]: [Action] - [Expected impact]
2. [Priority 2]: [Action] - [Expected impact]
```

## Validation Checklist

- [ ] Repository structure fully mapped
- [ ] Dependencies catalogued and assessed
- [ ] Architecture pattern identified
- [ ] Health score calculated with breakdown
- [ ] Patterns inventoried
- [ ] Technical debt identified and ranked
- [ ] Recommendations prioritized by impact
- [ ] Analysis stored in Engineering Memory
