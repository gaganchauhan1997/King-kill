# Reasoning & Autonomous Planning Engine

## Purpose
Explicit chain-of-thought framework with autonomous planning, self-reflection, uncertainty quantification, and multi-step validation. Prevents shallow reasoning and ensures thorough analysis. Enables self-directed execution workflows.

## Module Contract
| Attribute | Value |
|-----------|-------|
| **Inputs** | User request, context, available engines, constraints |
| **Outputs** | Execution plan, reasoning trace, confidence assessment |
| **Responsibilities** | Problem decomposition, planning, reasoning, self-review |
| **Constraints** | Max 7 sub-problems per level (cognitive limit) |
| **Decision Rules** | Always generate plan before execution; self-review before delivery |
| **Validation Checklist** | Plan reviewed, reasoning trace complete, confidence stated |
| **Failure Handling** | If planning fails, fall back to sequential execution with checkpoints |

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

### Stage 3: Autonomous Plan Generation (v3)

Generate execution plan automatically:

```
PLAN_GENERATION:
  1. Map sub-problems to engines
  2. Identify dependencies and execution order
  3. Determine which engines can run in parallel
  4. Allocate confidence requirements per step
  5. Define validation checkpoints
  6. Generate rollback points for reversible steps

PLAN_OUTPUT:
  phases:
    - name: "Foundation"
      engines: [01, 12, 13]
      outputs: [context, memory, constraints]
      checkpoint: "Context sufficient for reasoning"
    - name: "Reasoning"
      engines: [02, 03, 14]
      outputs: [decisions, plans, simulations]
      checkpoint: "Decisions validated"
    - name: "Execution"
      engines: [domain-specific]
      outputs: [design, code, analysis]
      checkpoint: "Output meets quality gates"
    - name: "Validation"
      engines: [05, 16, 10]
      outputs: [quality_score, consensus, documentation]
      checkpoint: "Quality gates passed"
```

### Stage 4: Option Generation

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

### Stage 5: Analysis

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

### Stage 6: Decision with Confidence

Select approach using decision engine (see 02-architecture-decision-engine.md):

```
DECISION_OUTPUT:
  - Selected approach
  - Confidence score (1-5)
  - Key assumptions
  - Conditions that would change decision
  - Risk mitigation steps
```

### Stage 7: Implementation Planning

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

### Stage 8: Self-Review (v3 Enhanced)

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
  [ ] Did I generate an autonomous plan? (v3)
  [ ] Did I identify rollback points? (v3)
  [ ] Did I assess change impact? (v3)
```

If any check fails, return to relevant stage.

## Autonomous Execution Protocol (v3)

### Plan Execution
```
EXECUTE(plan):
  For each phase in plan:
    1. Load required engines
    2. Execute engine workflows
    3. Validate checkpoint
    4. If checkpoint fails: apply recovery or request guidance
    5. Store results in Engineering Memory
    6. Proceed to next phase
```

### Adaptive Planning
If execution deviates from plan:
```
ADAPT(plan, deviation):
  1. Assess deviation severity
  2. If minor: adjust plan, continue
  3. If major: regenerate plan from current state
  4. If critical: halt execution, request human guidance
  5. Document adaptation rationale
```

### Parallel Execution
When engines are independent:
```
PARALLEL_EXECUTE(engines):
  1. Identify independent engine sets
  2. Execute in parallel
  3. Collect results
  4. Apply Multi-Agent Consensus if conflicts detected
  5. Synthesize unified output
```

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
   - Free of hallucination?
   - Aligned with the execution plan? (v3)"
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
  6. Verify plan was followed or adaptation documented (v3)
  7. Store learnings in Engineering Memory (v3)
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
- [ ] **Planning fallacy**: Underestimating time/complexity (v3)
- [ ] **Automation bias**: Over-trusting automated recommendations (v3)

## Validation Checklist

- [ ] Problem correctly decomposed
- [ ] Context fully assembled
- [ ] At least 3 approaches considered
- [ ] Autonomous plan generated (v3)
- [ ] Dependencies identified and ordered
- [ ] Checkpoints defined
- [ ] Rollback points identified (v3)
- [ ] Self-review completed
- [ ] Confidence stated
- [ ] Biases checked
- [ ] Plan is executable and validated
