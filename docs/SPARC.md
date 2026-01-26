# SPARC Methodology Reference

Structured development with claude-flow orchestration.

> **CLI Reference**: For complete CLI command documentation, see [CLI_REFERENCE.md](CLI_REFERENCE.md).

---

## Overview

SPARC = **S**pecification → **P**seudocode → **A**rchitecture → **R**efinement → **C**ompletion

Each phase has dedicated agents and can be orchestrated via swarm/hive-mind workflows.

> **Note**: SPARC is a methodology implemented via swarm/hive-mind commands.

---

## Phase 1: Specification

**Purpose:** Define requirements and constraints.

**Recommended approach (v3):**
```bash
# Initialize swarm with specification agent
npx claude-flow@alpha swarm init --topology star --max-agents 3
npx claude-flow@alpha swarm start -o "Specification: your feature" -s development
```

| Agent | Role |
|-------|------|
| `specification` | Requirements analysis |
| `researcher` | Domain research |

**Output:** Clear requirements document, acceptance criteria.

---

## Phase 2: Pseudocode

**Purpose:** Design algorithms before implementation.

**Recommended approach (v3):**
```bash
# Use swarm with pseudocode/planner agents
npx claude-flow@alpha swarm init --topology star --max-agents 3
npx claude-flow@alpha swarm start -o "Pseudocode: your feature" -s development
```

| Agent | Role |
|-------|------|
| `pseudocode` | Algorithm design |
| `planner` | Task decomposition |

**Output:** Step-by-step logic, data flow diagrams.

---

## Phase 3: Architecture

**Purpose:** System design and component structure.

**Recommended approach (v3):**
```bash
# Use swarm with architecture agents
npx claude-flow@alpha swarm init --topology star --max-agents 3
npx claude-flow@alpha swarm start -o "Architecture: your feature" -s development
```

| Agent | Role |
|-------|------|
| `architecture` | System design |
| `system-architect` | High-level patterns |

**Output:** Component diagrams, API contracts, database schema.

---

## Phase 4: Refinement (TDD)

**Purpose:** Test-driven implementation with iterative improvement.

**Recommended approach (v3):**
```bash
# Use hive-mind for TDD workflow (requires coordination)
npx claude-flow@alpha hive-mind init -t mesh -m 5
npx claude-flow@alpha hive-mind spawn --claude -o "TDD: your feature"
```

| Agent | Role |
|-------|------|
| `tester` | Write tests first |
| `coder` | Implement to pass tests |
| `reviewer` | Quality validation |
| `tdd-london-swarm` | Full TDD workflow |

**Cycle:**
```
Write Test → Run (Fail) → Implement → Run (Pass) → Refactor
```

**Output:** Tested, working code with coverage.

---

## Phase 5: Completion

**Purpose:** Integration, documentation, deployment readiness.

**Recommended approach (v3):**
```bash
# Use swarm with integration agents
npx claude-flow@alpha swarm init --topology star --max-agents 3
npx claude-flow@alpha swarm start -o "Integration: your feature" -s development
```

| Agent | Role |
|-------|------|
| `refinement` | Final polish |
| `production-validator` | Deployment checks |
| `api-docs` | Documentation |

**Output:** Production-ready, documented feature.

---

## Quick Reference: SPARC via Swarm/Hive-Mind


| SPARC Phase | V3 Approach |
|-------------|-------------|
| Specification | `swarm start -o "Specification: feature"` with `specification` agent |
| Pseudocode | `swarm start -o "Pseudocode: feature"` with `pseudocode` agent |
| Architecture | `swarm start -o "Architecture: feature"` with `architecture` agent |
| Refinement (TDD) | `hive-mind spawn --claude -o "TDD: feature"` with `tdd-london-swarm` |
| Completion | `swarm start -o "Integration: feature"` with `refinement` agent |
| Full Pipeline | Use hive-mind with multiple SPARC agents (see below) |

---

## SPARC with Hive-Mind

For complex features, combine SPARC phases with persistent orchestration:

```bash
# Restore persistent session
npx claude-flow@alpha hooks session-restore --session-id "latest"

# Initialize hive-mind for SPARC pipeline
npx claude-flow@alpha hive-mind init --topology hierarchical --max-agents 5

# Spawn SPARC-related agents
npx claude-flow@alpha hive-mind spawn architect
npx claude-flow@alpha hive-mind spawn coder
npx claude-flow@alpha hive-mind spawn tester

# Submit SPARC task
npx claude-flow@alpha hive-mind task "SPARC: your feature"

# Resume later (via session restore)
npx claude-flow@alpha hooks session-restore --session-id "latest"
```

---

## Phase-Agent Mapping

```
SPECIFICATION ──► specification, researcher
       │
       ▼
PSEUDOCODE ────► pseudocode, planner
       │
       ▼
ARCHITECTURE ──► architecture, system-architect
       │
       ▼
REFINEMENT ────► tester, coder, reviewer, tdd-london-swarm
       │
       ▼
COMPLETION ────► refinement, production-validator, api-docs
```

---

## When to Use Each Phase

| Scenario | Start At | Skip |
|----------|----------|------|
| New feature | Specification | None |
| Bug fix | Refinement | Spec, Pseudo, Arch |
| Refactoring | Architecture | Spec, Pseudo |
| Documentation | Completion | All coding phases |
| Quick prototype | Pseudocode | Spec |
