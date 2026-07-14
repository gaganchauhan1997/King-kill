# Engineering DNA Engine

## Purpose
Capture and apply organizational engineering fingerprint: standards, preferences, conventions, historical decisions, and team capabilities. Ensures consistency across all engineering outputs.

## Module Contract
| Attribute | Value |
|-----------|-------|
| **Inputs** | Organizational standards, team capabilities, historical decisions, codebase patterns |
| **Outputs** | Engineering DNA profile, consistency constraints, team capability assessment |
| **Responsibilities** | Profile organization, enforce consistency, assess capabilities |
| **Constraints** | DNA profile must evolve with organization; never be static |
| **Decision Rules** | When DNA conflicts with best practice, document rationale for deviation |
| **Validation Checklist** | DNA profile complete, consistent, current |
| **Failure Handling** | If DNA unknown, use industry defaults and flag for definition |

## DNA Profile Structure

```json
{
  "engineering_dna": {
    "organization": {
      "name": "Org Name",
      "size": "startup|mid|enterprise",
      "engineering_culture": "description"
    },
    "technology_stack": {
      "primary_languages": ["TypeScript", "Python"],
      "frameworks": ["React", "Node.js", "FastAPI"],
      "databases": ["PostgreSQL", "Redis"],
      "infrastructure": ["AWS", "Kubernetes", "Docker"],
      "preferred_tools": ["GitHub Actions", "Datadog", "Sentry"]
    },
    "standards": {
      "code_style": "organization-specific or industry-standard",
      "architecture_pattern": "modular-monolith|microservices|etc",
      "api_style": "REST|GraphQL|gRPC",
      "testing_requirements": "coverage_thresholds, test_types",
      "documentation_requirements": "adr_required, api_docs_required"
    },
    "preferences": {
      "explicit_over_implicit": true,
      "type_safety": "strict|lenient",
      "error_handling": "fail-fast|graceful-degradation",
      "optimization_priority": "readability|performance",
      "dependency_philosophy": "minimal|batteries-included"
    },
    "constraints": {
      "compliance_requirements": ["SOC2", "GDPR"],
      "budget_constraints": "description",
      "team_constraints": "size, expertise",
      "technical_constraints": ["legacy_systems", "vendor_lockin"]
    },
    "historical_decisions": [
      {
        "decision": "Why we chose X over Y",
        "rationale": "Context at the time",
        "current_status": "still_valid|under_review|deprecated"
      }
    ]
  }
}
```

## DNA Detection from Repository

```
DETECT_DNA(repository):
  1. Analyze technology stack from dependency files
  2. Detect code style from existing code
  3. Identify architecture pattern from structure
  4. Infer testing standards from test files
  5. Extract conventions from naming patterns
  6. Identify documentation practices
  7. Detect security practices
  8. Infer performance priorities
```

## DNA Consistency Enforcement

```
ENFORCE_CONSISTENCY(output, dna):
  1. Check technology stack alignment
  2. Verify code style compliance
  3. Confirm architecture pattern adherence
  4. Validate testing standards
  5. Check documentation requirements
  6. Flag deviations with rationale
  7. Suggest alignment changes
```

## Team Capability Assessment

```json
{
  "team_capabilities": {
    "languages": {
      "TypeScript": { "level": "expert", "count": 5 },
      "Python": { "level": "intermediate", "count": 3 }
    },
    "domains": {
      "frontend": { "level": "expert", "coverage": "full" },
      "backend": { "level": "expert", "coverage": "full" },
      "devops": { "level": "intermediate", "coverage": "partial" },
      "security": { "level": "beginner", "coverage": "none" }
    },
    "capacity": {
      "sprint_velocity": "story points",
      "availability": "percentage",
      "bottlenecks": ["areas with insufficient capacity"]
    }
  }
}
```

## DNA Evolution

```
EVOLVE_DNA(dna, new_information):
  1. Compare new information with existing DNA
  2. Identify changes in technology stack
  3. Detect standard evolution
  4. Update capability assessment
  5. Archive deprecated decisions
  6. Version the DNA profile
  7. Communicate changes to team
```

## Validation Checklist

- [ ] DNA profile complete and current
- [ ] Technology stack accurate
- [ ] Standards documented
- [ ] Preferences explicit
- [ ] Constraints identified
- [ ] Team capabilities assessed
- [ ] Historical decisions archived
- [ ] Consistency enforcement active
