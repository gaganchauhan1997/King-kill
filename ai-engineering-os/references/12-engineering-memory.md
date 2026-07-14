# Engineering Memory Engine

## Purpose
Persistent context system that stores decisions, learnings, patterns, and outcomes across sessions. Enables continuous improvement by building organizational knowledge over time.

## Module Contract
| Attribute | Value |
|-----------|-------|
| **Inputs** | Decisions, outcomes, learnings, patterns, errors |
| **Outputs** | Retrieved memories, learned patterns, recommendations based on history |
| **Responsibilities** | Store, retrieve, synthesize, and learn from engineering decisions |
| **Constraints** | Never store secrets; always verify memory before using |
| **Decision Rules** | Prefer recent, high-confidence memories; flag conflicting memories |
| **Validation Checklist** | Memories have source attribution, confidence scores, timestamps |
| **Failure Handling** | If memory retrieval fails, operate with current context only |

## Memory Types

### Decision Memory
```json
{
  "memory_type": "decision",
  "decision_id": "unique-id",
  "context": "What led to this decision",
  "options_considered": ["option1", "option2", "option3"],
  "selected_option": "chosen option",
  "rationale": "Why this option was chosen",
  "confidence_at_time": 4,
  "outcome": "What happened",
  "outcome_confidence": 5,
  "lessons_learned": ["lesson1", "lesson2"],
  "timestamp": "2024-01-01T00:00:00Z",
  "tags": ["architecture", "database", "microservices"]
}
```

### Pattern Memory
```json
{
  "memory_type": "pattern",
  "pattern_name": "Repository Pattern",
  "context": "When and where it was applied",
  "effectiveness": "high|medium|low",
  "applicability_conditions": ["condition1", "condition2"],
  "caveats": ["caveat1"],
  "related_patterns": ["pattern1", "pattern2"],
  "timestamp": "2024-01-01T00:00:00Z",
  "source": "project-name"
}
```

### Error Memory
```json
{
  "memory_type": "error",
  "error_type": "category",
  "description": "What went wrong",
  "root_cause": "Why it happened",
  "resolution": "How it was fixed",
  "prevention": "How to prevent recurrence",
  "timestamp": "2024-01-01T00:00:00Z",
  "severity": "critical|major|minor",
  "tags": ["deployment", "database", "security"]
}
```

### Learning Memory
```json
{
  "memory_type": "learning",
  "insight": "What was learned",
  "context": "When this insight applies",
  "confidence": 4,
  "verification_status": "verified|unverified|deprecated",
  "timestamp": "2024-01-01T00:00:00Z",
  "source": "project-name or experience"
}
```

## Memory Operations

### Store
```
STORE(memory):
  1. Validate memory structure
  2. Add timestamp and source attribution
  3. Calculate initial confidence
  4. Tag with relevant categories
  5. Check for conflicts with existing memories
  6. Store with conflict flag if applicable
```

### Retrieve
```
RETRIEVE(query):
  1. Match by tags, context, and recency
  2. Filter by confidence threshold
  3. Sort by relevance × recency × confidence
  4. Check for conflicting memories
  5. Return with confidence scores and conflict warnings
```

### Synthesize
```
SYNTHESIZE(memories):
  1. Group related memories
  2. Identify trends and patterns
  3. Flag contradictions
  4. Generate insights
  5. Update confidence based on outcomes
```

### Update
```
UPDATE(memory_id, outcome):
  1. Retrieve original memory
  2. Record outcome
  3. Update confidence based on outcome
  4. Add lessons learned
  5. If memory proven wrong, mark as deprecated
```

## Memory Retrieval Patterns

### By Context
```
RETRIEVE_BY_CONTEXT(task_description):
  - Match tags to task domain
  - Match technology stack
  - Match architectural patterns
  - Return most relevant memories
```

### By Recency
```
RETRIEVE_RECENT(time_window):
  - Return memories from last N days
  - Weighted by recency
  - Filter by relevance
```

### By Confidence
```
RETRIEVE_CONFIDENT(min_confidence):
  - Only return memories with confidence >= threshold
  - Prioritize verified learnings
```

## Memory Confidence Lifecycle

```
INITIAL_CAPTURE: Confidence = 3 (reasonable, unverified)
  ↓ After positive outcome
VERIFIED: Confidence = 4-5 (confirmed by experience)
  ↓ After repeated positive outcomes
ESTABLISHED: Confidence = 5 (highly reliable)
  ↓ If contradicted by new evidence
DEPRECATED: Confidence = 1-2 (outdated or wrong)
```

## Validation Checklist

- [ ] All memories have source attribution
- [ ] Confidence scores assigned
- [ ] Timestamps recorded
- [ ] Tags applied consistently
- [ ] Conflicts detected and flagged
- [ ] Outcomes tracked
- [ ] Deprecated memories marked
- [ ] No secrets stored in memory
