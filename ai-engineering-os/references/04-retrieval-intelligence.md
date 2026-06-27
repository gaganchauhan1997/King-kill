# Retrieval Intelligence

## Purpose
Systematic approach to context management, information retrieval, and knowledge access. Optimizes token usage while maximizing relevance.

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

## Context Compression

### When to Compress

- Retrieved content > 50% of available context
- Multiple large documents loaded
- Previous conversation > 10 turns
- Working with large codebases

### Compression Techniques

**Summarization:**
```
COMPRESS(original):
  - Extract key points (bullet form)
  - Remove examples (keep one representative)
  - Remove redundant explanations
  - Preserve code signatures, remove implementations
  - Keep decision rationale, remove discussion
```

**Hierarchical Loading:**
```
LOAD(content):
  1. Load table of contents/outline
  2. User asks about specific section
  3. Load that section in detail
  4. Keep outline in context for reference
```

**Delta Updates:**
```
UPDATE(context):
  - Track what changed between turns
  - Only communicate deltas
  - Reference previous state by position
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
