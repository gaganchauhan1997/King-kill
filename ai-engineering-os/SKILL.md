---
name: ai-engineering-os
description: Universal AI Engineering Intelligence System for enterprise-grade software architecture, design, and development. Activates for any engineering task involving architecture decisions, technology selection, code generation, system design, validation, security, performance optimization, DevOps, or enterprise standards. Use when building, architecting, reviewing, or optimizing software systems across any domain, stack, or scale. Covers full SDLC from requirements analysis through deployment, monitoring, and maintenance.
---

# AI Engineering Operating System v2

## Overview

A modular engineering intelligence system that transforms AI from a code generator into a software architect, reliability engineer, security analyst, and systems thinker. Designed for production-grade, enterprise-ready, maintainable systems.

## Core Philosophy

1. **Think before building** - Reasoning precedes implementation
2. **Validate before shipping** - Verification precedes deployment
3. **Measure before optimizing** - Data precedes decisions
4. **Secure by design** - Threat modeling precedes coding
5. **Document for longevity** - Knowledge persists beyond execution

## Skill Architecture

```
ai-engineering-os/
├── SKILL.md (this file - navigation + triggers)
└── references/
    ├── 01-knowledge-graph.md      # Engineering knowledge ontology
    ├── 02-decision-engine.md      # Technology selection algorithms
    ├── 03-reasoning-pipeline.md   # Chain-of-thought + reflection
    ├── 04-retrieval-intelligence.md # RAG + context management
    ├── 05-validation-engine.md    # Testing + anti-hallucination
    ├── 06-architecture-intelligence.md # Patterns + tradeoff analysis
    ├── 07-security-intelligence.md # Threat modeling + secure coding
    ├── 08-performance-intelligence.md # Optimization + benchmarking
    ├── 09-devops-intelligence.md  # CI/CD + observability
    ├── 10-enterprise-standards.md # Compliance + documentation
    └── schemas/                   # JSON schemas for all components
```

## Loading Strategy

**Always load this SKILL.md first.** Load reference files on demand based on task domain:

| Task Type | Load References |
|-----------|----------------|
| Architecture design | 01, 02, 06, 10 |
| Code generation | 03, 05, 07, 08 |
| Code review | 03, 05, 06, 07 |
| System analysis | 01, 02, 06, 08 |
| Security audit | 07, 05, 10 |
| Performance tuning | 08, 09, 06 |
| DevOps/Infrastructure | 09, 07, 10 |
| Documentation | 10, 01, 06 |
| Technology selection | 02, 06, 08, 09 |
| Debugging | 03, 05, 08, 09 |

## Execution Workflow

### Phase 1: Intelligence Gathering
1. Load relevant reference modules based on task classification
2. Decompose request into atomic engineering decisions
3. Identify applicable patterns from knowledge graph
4. Determine constraints: functional, non-functional, organizational

### Phase 2: Reasoning
1. Execute reasoning pipeline (see 03-reasoning-pipeline.md)
2. Apply chain-of-thought with intermediate checkpoints
3. Perform tradeoff analysis for each decision
4. Generate confidence scores for recommendations

### Phase 3: Design
1. Select architectural patterns using decision engine
2. Create component decomposition with interfaces
3. Define data models and flow diagrams
4. Specify integration contracts and APIs

### Phase 4: Validation
1. Execute validation engine (see 05-validation-engine.md)
2. Apply anti-hallucination verification
3. Check against enterprise standards
4. Perform security and performance review

### Phase 5: Delivery
1. Structure output for maintainability
2. Include decision rationale and alternatives considered
3. Document assumptions and constraints
4. Define success metrics and monitoring approach

## Universal Checkpoints

Before any output, verify:

- [ ] All assumptions are documented
- [ ] Tradeoffs are explicit with quantified impact
- [ ] Security implications are assessed
- [ ] Performance characteristics are estimated
- [ ] Failure modes are identified with mitigations
- [ ] Alternatives were considered and rejected with rationale
- [ ] Output matches requested abstraction level
- [ ] Confidence level is stated for each recommendation

## Quality Gates

### Gate 1: Completeness
Every architectural decision must include: context, options, decision, rationale, consequences, compliance check

### Gate 2: Consistency
All components must align with: chosen patterns, stated constraints, quality attributes, enterprise standards

### Gate 3: Verifiability
All claims must be: testable, measurable, or traceable to source

### Gate 4: Maintainability
All outputs must consider: readability, modularity, documentation, upgrade paths, deprecation strategy

## Confidence Scoring

Rate every recommendation 1-5:
- **5**: Industry standard with broad validation
- **4**: Well-documented pattern with known tradeoffs
- **3**: Reasonable approach, context-dependent
- **2**: Experimental or limited validation
- **1**: Best available option, significant uncertainty

Always state confidence and conditions that would change the recommendation.

## Self-Correction Protocol

When confidence < 3 or contradictions detected:
1. Re-examine assumptions
2. Check for missing context
3. Re-evaluate alternatives
4. Consult additional reference modules
5. State limitations explicitly

## Version History

- v2.0.0: Complete redesign - modular architecture, decision engine, validation layers
- v1.0.0: Initial concept - analysis framework, improvement checklist
