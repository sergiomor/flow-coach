# Learning Patterns Reference

Reusable patterns for effective claude-flow orchestration.

> **CLI Reference**: For complete CLI command documentation, see [CLI_REFERENCE.md](CLI_REFERENCE.md).

---

## Pattern 1: Progressive Agent Scaling

Start small, scale as complexity grows:

```
1 agent   → Validate approach, quick tasks
3 agents  → Core team (researcher, coder, tester)
5 agents  → Full team (+reviewer, +architect)
8+ agents → Complex systems (add specialists)
```

**When to scale up:**
- Task scope expands beyond initial estimate
- Agents waiting on each other (bottleneck)
- Quality issues requiring specialized review

**When to scale down:**
- Agents idle or duplicating work
- Communication overhead slowing progress
- Simple task over-engineered

---

## Pattern 2: Memory Namespacing

Organize knowledge by domain for clean separation:

```bash
--namespace backend    # API, database, auth
--namespace frontend   # UI, components, state
--namespace shared     # Cross-cutting concerns
--namespace research   # Analysis, findings
--namespace decisions  # Architecture decisions
```

**Benefits:**
- Agents query only relevant context
- Prevents knowledge pollution across domains
- Easier to debug and audit
- Supports team boundaries in large projects

**Example:**
```bash
# Store backend decision
npx claude-flow@alpha memory store auth_strategy "JWT with refresh tokens" \
  --namespace backend

# Query only backend knowledge
npx claude-flow@alpha memory search --type semantic "authentication" \
  --namespace backend --limit 5
```

---

## Pattern 3: Iterative Refinement

Build → Test → Learn → Improve cycle:

```
┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐
│  BUILD  │ ──► │  TEST   │ ──► │  LEARN  │ ──► │ IMPROVE │
└─────────┘     └─────────┘     └─────────┘     └─────────┘
     │                                               │
     └───────────────────────────────────────────────┘
```

**Implementation:**
1. Enable `post-task` hooks to capture patterns
2. Use `testgaps` worker to find coverage holes
3. Query memory for successful approaches
4. Apply learnings to next iteration

**Commands:**
```bash
# Enable learning hooks
npx claude-flow@alpha hooks post-task --task-id "current-task"

# Check what was learned
npx claude-flow@alpha hooks intelligence --showStatus true

# Apply to next task
npx claude-flow@alpha hooks pre-task --description "next task"
```

---

## Pattern 4: Topology Matching

Match topology to communication pattern:

| Communication Need | Topology | Example Task |
|-------------------|----------|--------------|
| Everyone shares everything | MESH | Pair programming, design review |
| Clear delegation chain | HIERARCHICAL | Large feature with subtasks |
| Sequential processing | RING | Data pipeline, build stages |
| Central coordination | STAR | Code review, approval workflow |
| Multiple teams | HIERARCHICAL-MESH | Enterprise projects |

**Anti-patterns to avoid:**
- MESH for simple sequential tasks (overhead)
- HIERARCHICAL for exploratory research (too rigid)
- RING when agents need to collaborate (blocking)
- STAR for highly parallel work (bottleneck)

---

## Pattern 5: Session Lifecycle

Manage long-running work effectively:

```
SESSION START                    SESSION END
     │                                │
     ▼                                ▼
┌─────────────┐               ┌─────────────┐
│ Restore     │               │ Export      │
│ - Memory    │               │ - Metrics   │
│ - Context   │               │ - Summary   │
│ - Agents    │               │ - State     │
└─────────────┘               └─────────────┘
```

**Best practices:**
```bash
# Always start with restore (picks up where you left off)
npx claude-flow@alpha hooks session-restore --session-id "latest"

# Initialize and work with HIVE-MIND
npx claude-flow@alpha hive-mind init --topology mesh
npx claude-flow@alpha hive-mind spawn coder
npx claude-flow@alpha hive-mind task "your task"

# End with export (preserves everything)
npx claude-flow@alpha hooks session-end --export-metrics true
```

---

## Pattern 6: Worker Orchestration

Combine background workers for common scenarios:

**New Project Setup:**
```
map → ultralearn → testgaps
```
- Map codebase structure first
- Learn domain patterns
- Establish test baseline

**Pre-Release Checklist:**
```
audit → benchmark → document
```
- Security scan (critical)
- Performance validation
- Auto-generate docs

**Technical Debt Sprint:**
```
map → refactor → testgaps → optimize
```
- Understand current state
- Get refactoring suggestions
- Ensure coverage maintained
- Optimize performance

---

## Quick Reference

| Pattern | When to Use | Key Command |
|---------|-------------|-------------|
| Progressive Scaling | Any multi-agent task | `--max-agents N` |
| Memory Namespacing | Multi-domain projects | `--namespace domain` |
| Iterative Refinement | Learning systems | `hooks post-task` |
| Topology Matching | Coordination design | `--topology type` |
| Session Lifecycle | Long-running work | `session-restore/end` |
| Worker Orchestration | Automated analysis | `hooks worker-dispatch` |

---

## Performance Tips

Quick optimizations for common bottlenecks:

| Goal | Solution | Command/Setting |
|------|----------|-----------------|
| Faster search | Use HNSW indexing | `hooks intelligence --enableHnsw true` |
| Reduce memory | Enable quantization | 4-32x reduction via RuVector |
| Parallel work | Mesh topology + agents | `--topology mesh --max-agents N` |
| Resume sessions | Hive-mind persistence | `hooks session-restore --session-id <id>` |
| Learn patterns | Enable SONA | `hooks intelligence --enableSona true` |
| Speed up routing | MoE expert routing | `hooks intelligence --enableMoe true` |
| Reduce latency | Flash Attention | O(N) memory, 2.49x-7.47x speedup |

**Performance Workers:**

| Worker | Purpose | When to Use |
|--------|---------|-------------|
| `benchmark` | Measure performance | Before/after optimization |
| `optimize` | Find improvements | Slow operations detected |
| `preload` | Cache frequently used | Large codebase |

**Anti-patterns to Avoid:**

| Anti-pattern | Problem | Solution |
|--------------|---------|----------|
| Too many agents | Communication overhead | Start with 3-5, scale up |
| Wrong topology | Blocking or wasted messages | Match to communication pattern |
| No namespacing | Memory pollution | Use `--namespace` consistently |
| Ephemeral for long work | Lost progress | Use HIVE-MIND, not SWARM |
