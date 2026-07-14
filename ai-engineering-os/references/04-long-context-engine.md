# Long Context & Compression Engine

## Purpose
Systematic approach to context management, information retrieval, knowledge access, context compression, and long-context optimization. Optimizes token usage while maximizing relevance.

## Module Contract
| Attribute | Value |
|-----------|-------|
| **Inputs** | Context window, task requirements, knowledge sources, conversation history |
| **Outputs** | Optimized context, compressed knowledge, retrieval plan |
| **Responsibilities** | Context budgeting, chunking, compression, retrieval, synthesis |
| **Constraints** | Never drop user requirements; preserve source attribution |
| **Decision Rules** | Progressive disclosure; compress before dropping |
| **Validation Checklist** | User requirements present, sources attributed, no critical info lost |
| **Failure Handling** | If context exceeds budget, compress hierarchically |

## Context Budgeting

### Token Allocation Strategy

Given context window constraints, allocate tokens by priority:

```
TOKEN_BUDGET (example for 128K context):
  - System prompt + active skill: ~10K (8%)
  - Conversation history (recent): ~20K (16%)
  - Retrieved knowledge (relevant): ~40K (31%)
  - Current task context: ~50K (39%)
  - Buffer for response: ~8K (6%)
```

### Dynamic Budget Adjustment (v3)

```
ADJUST_BUDGET(task_complexity):
  Simple task: Reduce knowledge to 20K, increase task context to 60K
  Complex task: Increase knowledge to 50K, reduce history to 15K
  Research task: Maximize knowledge to 60K, reduce task to 35K
  Code generation: Maximize task context to 60K, minimize history to 10K
```

### Retrieval Priorities

**Tier 1 - Always Load:**
- User's current request
- Active skill's SKILL.md
- Directly relevant reference modules

**Tier 2 - Load If Space:**
- Recent conversation turns
- Referenced files uploaded by user
- Knowledge graph nodes for current domain

**Tier 3 - On Demand:**
- Additional reference modules
- Web search results
- Code repository contents

## Chunking Strategy

### For Code

```
CHUNKING_RULES:
  - Function/method level (preferred)
  - Class level (for small classes)
  - Module level (with summary)
  - Never split logical constructs across chunks

METADATA_PER_CHUNK:
  - File path
  - Line numbers
  - Dependencies (imports)
  - Called by (references)
  - Complexity score
```

### For Documentation

```
CHUNKING_RULES:
  - Section level (h2/h3 boundaries)
  - Topic level (self-contained concepts)
  - Max 500 tokens per chunk (retain context)
  - Preserve cross-references

METADATA_PER_CHUNK:
  - Source document
  - Section hierarchy
  - Related sections
  - Last updated
  - Confidence level
```

## Context Compression Engine (v3)

### Hierarchical Compression

When context exceeds budget, compress in priority order:

```
COMPRESS(context, target_size):
  1. Summarize distant conversation history (keep recent verbatim)
  2. Compress retrieved documents: keep summaries, drop examples
  3. Compress code chunks: keep signatures, drop implementations
  4. Compress knowledge graph: keep relevant nodes, drop distant
  5. Final resort: drop Tier 3 content entirely

COMPRESSION_TECHNIQUES:
  - Summarization: Extract key points, bullet form
  - Deduplication: Remove redundant information
  - Abstraction: Replace detailed with summary
  - Selection: Keep most relevant, drop least
  - Hybrid: Summarize + keep critical details verbatim
```

### Semantic Compression

```
SEMANTIC_COMPRESS(text):
  1. Extract key entities and relationships
  2. Preserve decision rationale, drop discussion
  3. Keep code signatures, remove implementations (unless requested)
  4. Preserve checklists, compress explanations
  5. Maintain source attribution always
```

### Delta Compression (v3)

For multi-turn conversations:
```
DELTA_COMPRESS(conversation):
  1. Track what changed between turns
  2. Only communicate deltas
  3. Reference previous state by position
  4. Summarize stable context once, reference thereafter
```

## Retrieval Patterns

### Pattern 1: Exact Match

Use when: Specific API, function, or configuration needed

```
RETRIEVAL:
  - Search for: Exact name/signature
  - Priority: Highest
  - Verification: Cross-reference with official docs
```

### Pattern 2: Semantic Search

Use when: Concept or pattern described in natural language

```
RETRIEVAL:
  - Query: User's description
  - Matching: Conceptual similarity
  - Expansion: Include related concepts
  - Filter: By domain, technology, maturity
```

### Pattern 3: Navigational

Use when: User references known resource

```
RETRIEVAL:
  - Parse reference (URL, file path, doc name)
  - Load referenced content
  - Follow 1-2 levels of links
  - Summarize key points
```

### Pattern 4: Analogical

Use when: Novel problem similar to known pattern

```
RETRIEVAL:
  - Identify problem characteristics
  - Match to known patterns by structure
  - Adapt solution from analogous domain
  - Validate applicability
```

### Pattern 5: Hierarchical Loading (v3)

For large knowledge bases:
```
HIERARCHICAL_LOAD:
  1. Load table of contents/outline
  2. User asks about specific section
  3. Load that section in detail
  4. Keep outline in context for reference
```

## Long Context Management (v3)

### Context Window Strategy

```
MANAGE_LONG_CONTEXT:
  1. Identify context window limit
  2. Measure current context size
  3. If under 80%: normal operation
  4. If 80-95%: activate compression
  5. If over 95%: aggressive compression + selective dropping
  6. Always preserve: user requirements, active decisions, critical constraints
```

### Sliding Window for Conversations

```
SLIDING_WINDOW(history):
  1. Keep first N turns (establish context)
  2. Keep last M turns (recent context)
  3. Summarize middle turns
  4. Adjust N and M based on total length
```

### External Memory Integration (v3)

When context is insufficient:
```
USE_EXTERNAL_MEMORY:
  1. Query Engineering Memory for relevant past decisions
  2. Load stored knowledge graph fragments
  3. Retrieve relevant ADRs and documentation
  4. Integrate into current context with attribution
```

## Knowledge Synthesis

### Multi-Source Integration

When information comes from multiple sources:

```
SYNTHESIZE(sources):
  1. Identify agreement (consensus = high confidence)
  2. Identify contradictions (flag for resolution)
  3. Identify gaps (missing information)
  4. Build unified view with attribution
  5. State confidence for each element
```

### Temporal Awareness

```
TEMPORAL_CHECKS:
  - When was this information published?
  - Has technology version changed since?
  - Are there deprecation notices?
  - Is there a newer recommended approach?
  - What is the stability/maturity trajectory?
```

## Hallucination Prevention

### Source Verification

```
VERIFY(claim):
  - Can I find this in official documentation?
  - Does it match known behavior?
  - Are there contradicting sources?
  - Is this within my knowledge cutoff?
  - Should I search to verify?
```

### Boundary Awareness

```
KNOWLEDGE_BOUNDARIES:
  State clearly when:
  - Information might be outdated
  - Multiple valid approaches exist
  - Context would change recommendation
  - Verification is needed before implementation
  - Best practice vs. common practice differ
```

## Query Planning

### Before Any Tool Use

```
QUERY_PLAN:
  1. What do I need to know?
  2. Which tool/source has this information?
  3. What is the specific query?
  4. How will I verify the result?
  5. What is the fallback if no results?
```

### Search Strategy

```
SEARCH_PRIORITY:
  1. User-provided documents (most relevant)
  2. Official documentation (most authoritative)
  3. Established references (most comprehensive)
  4. Community knowledge (most current)
  5. Web search (fill gaps only)
```

## Validation Checklist

- [ ] Context within budget
- [ ] User requirements preserved
- [ ] Source attribution maintained
- [ ] No critical information lost in compression
- [ ] Retrieval relevant and complete
- [ ] Temporal checks applied
- [ ] Hallucination boundaries stated
