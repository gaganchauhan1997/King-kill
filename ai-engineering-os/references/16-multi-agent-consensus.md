# Multi-Agent Consensus Engine

## Purpose
Cross-engine validation and agreement mechanism. When multiple intelligence engines produce recommendations, this engine resolves conflicts and builds consensus.

## Module Contract
| Attribute | Value |
|-----------|-------|
| **Inputs** | Recommendations from multiple engines, confidence scores, context |
| **Outputs** | Consensus recommendation, dissent documentation, confidence assessment |
| **Responsibilities** | Conflict detection, consensus building, disagreement resolution |
| **Constraints** | Never override security engine without human review |
| **Decision Rules** | Weight by confidence × domain relevance; security wins ties |
| **Validation Checklist** | All engines heard, conflicts resolved, rationale documented |
| **Failure Handling** | If consensus impossible, present options with human escalation path |

## Consensus Framework

### Conflict Detection
```
DETECT_CONFLICTS(engine_outputs):
  1. Compare recommendations across engines
  2. Identify direct contradictions
  3. Identify partial disagreements
  4. Categorize by severity:
     - Blocking: Recommendations are mutually exclusive
     - Warning: Different approaches with tradeoffs
     - Info: Minor variations in implementation
```

### Consensus Building
```
BUILD_CONSENSUS(outputs):
  1. Gather all recommendations with confidence scores
  2. Identify areas of agreement
  3. For disagreements:
     a. Identify underlying assumptions
     b. Determine which assumption is more valid
     c. Apply domain expertise weighting
  4. Generate unified recommendation
  5. Document dissenting views
```

### Weighting Algorithm
```
WEIGHT(engine, recommendation):
  base_confidence = engine.confidence_score
  domain_relevance = relevance_to_task(engine.domain, task)
  track_record = engine.historical_accuracy
  recency = engine.knowledge_recency
  
  final_weight = base_confidence × domain_relevance × track_record × recency
```

## Resolution Strategies

### Strategy 1: Weighted Vote
```
VOTE(outputs):
  - Each engine votes with confidence-weighted score
  - Winner is highest weighted recommendation
  - Document vote distribution
```

### Strategy 2: Compromise
```
COMPROMISE(outputs):
  - Find common ground between recommendations
  - Merge compatible elements
  - Document what each engine contributed
```

### Strategy 3: Escalation
```
ESCALATE(outputs):
  - When consensus cannot be reached
  - Present all options with tradeoffs
  - Recommend default with rationale
  - Flag for human decision
```

### Special Rules
```
SECURITY_WINS:
  - Security engine has veto power on safety issues
  - Security concerns always require explicit resolution
  - Never auto-override security warnings

COST_AWARENESS:
  - Cost optimization engine provides budget context
  - Cost concerns are advisory, not blocking
  - Cost vs quality tradeoffs require explicit decision
```

## Consensus Report Template

```markdown
## Multi-Agent Consensus Report

### Participating Engines
| Engine | Recommendation | Confidence | Weight |
|--------|---------------|------------|--------|
| ...    | ...           | ...        | ...    |

### Areas of Agreement
- [Agreement 1]
- [Agreement 2]

### Resolved Conflicts
| Conflict | Resolution | Rationale |
|----------|-----------|-----------|
| ...      | ...       | ...       |

### Outstanding Dissent
| Engine | View | Why Overridden |
|--------|------|----------------|
| ...    | ...  | ...            |

### Final Consensus
[Unified recommendation]

### Confidence
[Overall confidence with justification]

### Human Review Required
[Yes/No - with explanation]
```

## Validation Checklist

- [ ] All relevant engines participated
- [ ] Conflicts detected and categorized
- [ ] Resolution strategy applied
- [ ] Dissent documented
- [ ] Security concerns addressed
- [ ] Final consensus justified
- [ ] Human escalation path defined if needed
