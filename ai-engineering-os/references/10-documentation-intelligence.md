# Documentation Intelligence

## Purpose
Organizational standards for code quality, documentation, collaboration, and long-term maintainability with intelligent documentation generation, analysis, and gap detection.

## Module Contract
| Attribute | Value |
|-----------|-------|
| **Inputs** | Code, architecture decisions, requirements, team standards |
| **Outputs** | Documentation, quality analysis, gap reports |
| **Responsibilities** | Documentation generation, standards enforcement, gap detection |
| **Constraints** | Documentation must be accurate and current; never generate stale docs |
| **Decision Rules** | README required for every project; ADR for every architectural decision |
| **Validation Checklist** | All templates populated, standards followed, gaps identified |
| **Failure Handling** | If standards conflict, use Engineering DNA or default to higher standard |

## Code Organization

### Repository Structure

```
project/
├── README.md                  # Overview, setup, architecture
├── docs/                      # Architecture Decision Records
│   └── adr/
├── src/                       # Source code
│   ├── domain/               # Business logic (independent of framework)
│   ├── application/          # Use cases, workflows
│   ├── infrastructure/       # External concerns (DB, API, messaging)
│   └── interface/            # Controllers, presenters, CLI
├── tests/                     # Test suites
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── scripts/                   # Automation scripts
├── config/                    # Configuration templates
├── docker/                    # Container definitions
└── .github/                   # CI/CD workflows
```

### Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| Files (code) | kebab-case | `user-service.ts` |
| Files (config) | kebab-case | `database-config.yaml` |
| Classes | PascalCase | `UserService` |
| Functions | camelCase | `getUserById` |
| Constants | SCREAMING_SNAKE_CASE | `MAX_RETRY_COUNT` |
| Variables | camelCase | `currentUser` |
| Boolean | is/has/should prefix | `isActive`, `hasPermission` |
| Database tables | snake_case, plural | `user_sessions` |
| API endpoints | kebab-case | `/api/v1/user-profiles` |
| Environment variables | SCREAMING_SNAKE_CASE | `DATABASE_URL` |

## Documentation Standards

### README.md Template

```markdown
# Project Name

## Overview
One-paragraph description of what this project does.

## Architecture
[Link to architecture diagram or ADR]

## Getting Started
### Prerequisites
- Requirement 1
- Requirement 2

### Installation
\`\`\`bash
# Step-by-step setup
\`\`\`

### Running Tests
\`\`\`bash
# Test command
\`\`\`

## Development
### Project Structure
[Overview of directory layout]

### Key Decisions
[Link to ADRs]

## Deployment
[Link to deployment docs]

## Monitoring
[Link to dashboards and runbooks]

## Contributing
[Link to contributing guidelines]
```

### Architecture Decision Records (ADRs)

```markdown
# ADR-XXX: [Title]

## Status
- Proposed | Accepted | Deprecated | Superseded by ADR-YYY

## Context
[What is the issue that we're seeing?]

## Decision
[What is the change that we're proposing?]

## Consequences
### Positive
- [Benefit 1]
- [Benefit 2]

### Negative
- [Tradeoff 1]
- [Tradeoff 2]

## Alternatives Considered
| Option | Pros | Cons | Decision |
|--------|------|------|----------|
| ...    | ...  | ...  | Rejected |

## Compliance
- [ ] Security review
- [ ] Performance impact assessed
- [ ] Cost impact assessed

## Notes
[Any additional context]
```

### API Documentation

```yaml
openapi: 3.0.0
info:
  title: API Name
  version: 1.0.0
paths:
  /resource:
    get:
      summary: List resources
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ResourceList'
components:
  schemas:
    ResourceList:
      type: object
      properties:
        items:
          type: array
          items:
            $ref: '#/components/schemas/Resource'
        total:
          type: integer
```

## Documentation Intelligence (v3)

### Automated Gap Detection

```
DETECT_DOC_GAPS(project):
  1. Check for README.md existence
  2. Check for ADRs (count architectural decisions vs ADRs)
  3. Check API documentation coverage
  4. Check code documentation (public APIs, complex logic)
  5. Check deployment documentation
  6. Check runbook existence
  7. Generate gap report with priority rankings
```

### Documentation Quality Analysis

```
ANALYZE_DOC_QUALITY(documentation):
  1. Completeness: Are all required sections present?
  2. Accuracy: Does it match the actual code/system?
  3. Currency: Is it up to date?
  4. Clarity: Is it understandable to target audience?
  5. Consistency: Does it follow organizational standards?
  6. Discoverability: Can users find what they need?
```

### Documentation Generation Triggers

| Trigger | Generated Document |
|---------|-------------------|
| New project created | README.md from template |
| Architectural decision made | ADR from template |
| API endpoint added | OpenAPI spec update |
| Deployment process defined | Deployment runbook |
| Incident resolved | Post-incident review doc |
| Service dependency added | Architecture diagram update |

## Code Review Standards

### Review Checklist

**For Author:**
- [ ] Self-review completed
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] ADR created (if architectural)
- [ ] Breaking changes documented
- [ ] Migration guide included (if needed)

**For Reviewer:**
- [ ] Understand the change and its motivation
- [ ] Verify business logic correctness
- [ ] Check error handling
- [ ] Verify test coverage
- [ ] Check security implications
- [ ] Verify no secrets or PII
- [ ] Confirm naming conventions followed
- [ ] Check performance implications

### Review Response Time

| Type | Target |
|------|--------|
| Critical bug fix | 1 hour |
| Feature PR | 4 hours |
| Refactoring | 8 hours |
| Documentation | 24 hours |

## Versioning Strategy

### Semantic Versioning

```
MAJOR.MINOR.PATCH

MAJOR: Breaking changes
MINOR: New features (backward compatible)
PATCH: Bug fixes (backward compatible)
```

### Version Bump Rules

| Change Type | Version Bump |
|-------------|-------------|
| API breaking change | MAJOR |
| New feature | MINOR |
| Bug fix | PATCH |
| Documentation only | None or PATCH |
| Performance improvement | MINOR |
| Dependency update | PATCH or MINOR |

### API Versioning

```
/api/v1/resource       # URL path versioning (recommended)
/api/v2/resource

Accept: application/vnd.api.v1+json  # Header versioning (alternative)
```

## Dependency Management

### Selection Criteria

| Criterion | Weight | Assessment |
|-----------|--------|------------|
| Maturity | High | Version > 1.0, stable release cycle |
| Maintenance | High | Active in last 6 months |
| Community | Medium | Stars, contributors, issue response |
| Size | Medium | Bundle impact |
| License | High | Compatible with project |
| Security | High | No known CVEs, security policy |

### Update Strategy

```
DEPENDENCY_UPDATES:
  Security patches: Immediate (automated)
  Bug fixes: Weekly batch
  Minor versions: Monthly review
  Major versions: Quarterly planning
  
  Process:
    1. Review changelog
    2. Check breaking changes
    3. Update in isolated branch
    4. Run full test suite
    5. Deploy to staging
    6. Monitor for 24 hours
    7. Deploy to production
```

## Team Collaboration

### Definition of Done

- [ ] Code implemented
- [ ] Unit tests pass (>80% coverage)
- [ ] Integration tests pass
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] ADR created (if applicable)
- [ ] No security vulnerabilities
- [ ] Performance acceptable
- [ ] Deployed to staging
- [ ] Product owner acceptance

### Communication Protocols

| Topic | Channel | Response Time |
|-------|---------|---------------|
| Production incident | Phone/Pager | Immediate |
| Urgent question | Slack DM | 1 hour |
| Code review | PR comment | 4 hours |
| General question | Slack channel | 24 hours |
| Announcement | Email/Slack | N/A |

## Compliance Requirements

### Data Handling

| Classification | Handling | Encryption | Retention |
|---------------|----------|------------|-----------|
| **Public** | Standard | TLS in transit | Standard |
| **Internal** | Need-to-know | TLS + at rest | Standard |
| **Confidential** | Authorized only | Full encryption | Defined period |
| **Restricted** | Minimal access | Full encryption + key management | Minimal period |

### Audit Requirements

- All production access logged
- Configuration changes tracked
- Data access auditable
- Security events alerted
- Regular access reviews (quarterly)

## Quality Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Code coverage | >80% | Test reports |
| Deploy frequency | Daily | CI/CD metrics |
| Lead time for changes | <3 days | Issue tracker |
| Change failure rate | <5% | Incident tracker |
| MTTR (recovery) | <1 hour | Incident tracker |
| Technical debt ratio | <10% | Static analysis |
| Documentation coverage | >90% public APIs | Doc generation |

## Validation Checklist

- [ ] All templates defined and current
- [ ] Naming conventions documented
- [ ] ADR template includes compliance section
- [ ] Documentation gap detection configured (v3)
- [ ] Code review standards defined
- [ ] Versioning strategy documented
- [ ] Dependency management process defined
- [ ] Definition of Done agreed upon
