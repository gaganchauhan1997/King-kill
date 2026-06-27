# Reasoning Pipeline

## Purpose
Explicit chain-of-thought framework with self-reflection, uncertainty quantification, and multi-step validation. Prevents shallow reasoning and ensures thorough analysis.

## Core Pipeline

### Stage 1: Problem Decomposition

Decompose any request into atomic sub-problems:

```
INPUT: User request
OUTPUT: [SubProblem1, SubProblem2, ... SubProblemN]

For each sub-problem:
  - Type: [implementation|design|analysis|debugging|optimization]
  - Dependencies: [prerequisite sub-problems]
  - Complexity: [low|medium|high]
  - Confidence requirement: [speculative|standard|critical]
```

**Decomposition rules:**
- Max 7 sub-problems per level (cognitive limit)
- If > 7, create intermediate groupings
- Identify dependencies to establish execution order
- Flag high-confidence-requirement items for additional verification

### Stage 2: Context Assembly

Gather relevant context for each sub-problem:

```
CONTEXT_SOURCES:
  1. Knowledge Graph nodes (matching domain/pattern/technology)
  2. Previous conversation history
  3. Uploaded files and documentation
  4. Reference modules from this skill
  5. Web search results (when needed)
  6. Code repositories (when linked)
```

**Context prioritization:**
1. User's explicit requirements (highest priority)
2. Domain-specific patterns from knowledge graph
3. Technology-specific best practices
4. General engineering principles
5. Cross-domain analogies (lowest priority)

### Stage 3: Option Generation

For design decisions, generate at least 3 approaches:

```
APPROACH_GENERATION:
  1. Conservative: Proven, battle-tested approach
  2. Balanced: Modern with proven components
  3. Progressive: Cutting-edge with higher risk
  4. Hybrid: Combines elements from above

For each approach:
  - Implementation outline
  - Key tradeoffs
  - Risk assessment
  - Resource requirements
  - Timeline estimate
```

### Stage 4: Analysis

Apply structured analysis to each option:

**Technical Analysis:**
- Scalability assessment (horizontal, vertical, functional)
- Performance characteristics (latency, throughput, resource usage)
- Security implications (attack surface, data flow risks)
- Maintainability evaluation (complexity, coupling, cohesion)
- Testing strategy (unit, integration, e2e, property-based)

**Organizational Analysis:**
- Team capability match
- Learning curve impact
- Hiring market considerations
- Existing infrastructure compatibility
- Migration complexity

**Financial Analysis:**
- Implementation cost
- Infrastructure cost (compute, storage, network)
- Licensing cost
- Maintenance cost (annual estimate)
- Training cost

### Stage 5: Decision with Confidence

Select approach using decision engine (see 02-decision-engine.md):

```
DECISION_OUTPUT:
  - Selected approach
  - Confidence score (1-5)
  - Key assumptions
  - Conditions that would change decision
  - Risk mitigation steps
```

### Stage 6: Implementation Planning

Break selected approach into executable steps:

```
IMPLEMENTATION_PLAN:
  Phase 1: Foundation
    - Step 1: [specific action]
    - Step 2: [specific action]
    - Validation: [how to verify]
  
  Phase 2: Core
    - Step 3: [specific action]
    - Step 4: [specific action]
    - Validation: [how to verify]
  
  Phase 3: Polish
    - Step 5: [specific action]
    - Final validation: [comprehensive verification]
```

### Stage 7: Self-Review

Before finalizing, apply critique:

```
SELF_REVIEW_CHECKLIST:
  [ ] Did I understand the actual problem (not just symptoms)?
  [ ] Did I consider at least 3 approaches?
  [ ] Did I identify all key tradeoffs?
  [ ] Did I check for contradictions?
  [ ] Did I consider edge cases?
  [ ] Did I assess security implications?
  [ ] Did I consider performance at scale?
  [ ] Did I plan for failure modes?
  [ ] Did I consider maintenance burden?
  [ ] Did I verify no hallucinated APIs/patterns?
  [ ] Did I match abstraction level to request?
  [ ] Did I state confidence honestly?
```

If any check fails, return to relevant stage.

## Reflection Mechanism

### During Execution

After each major section of output:

```
REFLECTION_PROMPT:
  "Review what was just written. Is it:
   - Technically accurate?
   - Complete for the stated requirements?
   - Consistent with previous sections?
   - At the right abstraction level?
   - Free of hallucination?"
```

### Post-Completion

After full response:

```
FINAL_REVIEW:
  1. Verify all code compiles (if code was generated)
  2. Check all links/references exist (if referenced)
  3. Confirm no contradictions between sections
  4. Validate consistency with stated constraints
  5. Ensure all user requirements are addressed
```

## Uncertainty Quantification

### Confidence Levels

| Level | Meaning | Action Required |
|-------|---------|-----------------|
| **Certain** (95%+) | Industry standard, widely validated | Proceed |
| **High** (80-95%) | Well-documented, clear tradeoffs | Proceed, note caveats |
| **Moderate** (60-80%) | Reasonable approach, context matters | State assumptions |
| **Low** (40-60%) | Multiple valid approaches | Present options |
| **Uncertain** (<40%) | Insufficient information | Request clarification |

### Expression of Uncertainty

Always specify:
1. **What is certain**: Established facts, documented behavior
2. **What is likely**: Best estimate based on evidence
3. **What is unknown**: Gaps requiring investigation
4. **What depends on context**: Conditional recommendations

## Cognitive Bias Checklist

Before finalizing any recommendation, check for:

- [ ] **Recency bias**: Over-weighting recent trends
- [ ] **Authority bias**: Assuming popular = correct
- [ ] **Confirmation bias**: Seeking evidence for preferred answer
- [ ] **Anchoring**: Over-relying on first information received
- [ ] **Availability bias**: Over-valuing memorable examples
- [ ] **Overconfidence**: Stating uncertain things as facts
- [ ] **Halo effect**: Extending one positive attribute to all
- [ ] **Sunk cost**: Favoring continued investment in past decisions
