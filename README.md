# Flow Coach

> Interactive **claude-flow v3** orchestration coach with automated recommendations for security, testing, documentation, GitHub integration, and more.

**Version**: v3 only. CLI source: `@claude-flow/cli/src/commands/`

---

## Installation

Drop the `flow-coach` directory into your `.claude/skills/` directory, then invoke with:
```bash
/flow-coach <your task description>
```

---

## Quick Start

### Invoke the Skill

```bash
/flow-coach <your task description>
```

**Examples:**
```bash
/flow-coach Build a REST API with authentication
/flow-coach Fix the login bug and add regression tests
/flow-coach Set up CI/CD pipeline for our project
```

---

## How Auto-Triggers Work

Flow Coach analyzes your task description and **automatically enables features** based on keywords. Most real tasks trigger multiple features simultaneously.

### Trigger Keywords Reference

| Keyword Detected | Features Auto-Enabled |
|------------------|----------------------|
| `auth`, `login`, `OAuth`, `password` | Security (CRITICAL) + Testing + Sessions + **SPARC (full)** |
| `API`, `REST`, `endpoint` | Documentation + Testing + Security + **SPARC (full)** |
| `implement`, `build`, `create`, `new feature` | Testing + Security + **SPARC (full cycle)** |
| `fix`, `bug`, `issue` | Testing (regression) + **SPARC (refinement only)** |
| `refactor`, `cleanup`, `debt` | Tech-debt pattern + Testing + **SPARC (architecture→completion)** |
| `deploy`, `release`, `production` | Pre-release pattern + Claims + GitHub |
| `new project`, `new codebase` | New-project pattern (map → ultralearn → testgaps) |
| `learn`, `improve`, `adapt` | DAA mode + Intelligence + Security |
| `TDD`, `test-first` | tdd-london-swarm agent + **SPARC (refinement phase)** |
| `PR`, `pull request` | GitHub integration + Diff analysis |
| `multiple teams`, `cross-team` | HIERARCHICAL-MESH topology |
| `prototype`, `quick`, `spike` | SWARM mode + **SPARC (pseudocode→refinement)** |
| `documentation`, `docs` | **SPARC (completion only)** |
| `explain`, `understand`, `analyze` | SWARM mode only (no code = no SPARC) |

### Priority Levels

| Level | When Applied | Example |
|-------|--------------|---------|
| **CRITICAL** | Auth, payments, security-sensitive | `audit` worker runs first |
| **HIGH** | Code generation, API work | `testgaps` + `security-manager` |
| **MEDIUM** | General implementation | `testgaps` worker |
| **LOW** | Research, documentation-only | Optional features |

---
This is the MINIMAL configuration for pure research tasks.
```

---

## Complete Coaching Process (15 Phases)

```
Phase 1:  Task Assessment          → Evaluates 10 dimensions (includes SPARC)
Phase 2:  SPARC Methodology        → Full / Partial / Skip based on task type
Phase 3:  Mode Selection           → SWARM / HIVE-MIND / DAA
Phase 4:  Topology Selection       → MESH / HIERARCHICAL / RING / STAR
Phase 5:  Agent Selection          → ~15 core + custom agents (SPARC agents if needed)
Phase 6:  Memory Configuration     → AgentDB / ReasoningBank / Hybrid
Phase 7:  Intelligence Layer       → SONA / MoE / HNSW / Flash Attention
Phase 8:  Model Routing            → Haiku / Sonnet / Opus auto-selection
Phase 9:  Background Workers       → 12 available workers
Phase 10: Security Configuration   → Audit / AIDefence / scanning
Phase 11: Testing Configuration    → testgaps / tester / TDD
Phase 12: Documentation Config     → document worker / api-docs
Phase 13: Integration Config       → GitHub / Diff / Claims / Sessions
Phase 14: Command Generation       → Complete config for review
Phase 15: User Decision            → Execute / Modify / Add / Remove
```

---

## Task Assessment (10 Dimensions)

When you describe a task, Flow Coach generates:

```
TASK ASSESSMENT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Complexity:     ████████░░ (80%) Complex
Duration:       ██████░░░░ (60%) Medium
Coordination:   █████████░ (90%) High
Memory Needs:   ████████░░ (80%) Persistent
Intelligence:   ███████░░░ (70%) Neural
Security:       ████████░░ (80%) Required (code generation)
Testing:        ████████░░ (80%) Required (code generation)
Documentation:  █████░░░░░ (50%) Recommended
GitHub:         ███░░░░░░░ (30%) Optional
SPARC:          ████████░░ (80%) Full cycle (new feature)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Recommendation: HIVE-MIND with MESH topology + SONA intelligence
SPARC Phases:   Specification → Pseudocode → Architecture → Refinement → Completion
Auto-enabled:   testgaps, audit, session management, map worker
```

---

## SPARC Methodology (Phase 2)

Flow Coach automatically recommends the appropriate SPARC phases based on task type:

### SPARC Decision Tree

```
What type of task is this?
|
|-- New feature / Implement / Build / Create?
|   +-- FULL SPARC CYCLE
|       Specification → Pseudocode → Architecture → Refinement → Completion
|       Agents: specification, pseudocode, architecture, tdd-london-swarm, refinement
|
|-- Bug fix / Issue / Error?
|   +-- SPARC REFINEMENT ONLY
|       Skip to: Refinement (with regression testing)
|       Agents: tester, coder, reviewer
|
|-- Refactoring / Cleanup / Tech debt?
|   +-- SPARC FROM ARCHITECTURE
|       Architecture → Refinement → Completion
|       Agents: architecture, coder, tester, reviewer
|
|-- Prototype / Spike / Quick test?
|   +-- SPARC LITE
|       Pseudocode → Refinement (skip Spec & full Arch)
|       Agents: pseudocode, coder, tester
|
|-- Documentation only?
|   +-- SPARC COMPLETION ONLY
|       Completion phase only
|       Agents: api-docs, documenter
|
+-- Research / Analysis / Exploration?
    +-- NO SPARC (not code generation)
        Use: researcher, analyst agents
```

### SPARC Phase Summary

| Task Type | SPARC Phases | Key Agents |
|-----------|--------------|------------|
| **New Feature** | Full (S→P→A→R→C) | specification, architecture, tdd-london-swarm |
| **Bug Fix** | Refinement only | tester, coder, reviewer |
| **Refactoring** | A→R→C | architecture, coder, tester |
| **Prototype** | P→R | pseudocode, coder |
| **Documentation** | Completion | api-docs, documenter |
| **Research** | None | researcher, analyst |

### SPARC Phase Details

| Phase | Purpose | Output |
|-------|---------|--------|
| **S**pecification | Requirements, constraints, acceptance criteria | Requirements doc |
| **P**seudocode | Algorithm design, logic flow, data structures | Logic diagrams |
| **A**rchitecture | System design, components, interfaces | Architecture doc |
| **R**efinement | TDD implementation, iterative improvement | Tested code |
| **C**ompletion | Integration, documentation, deployment prep | Production-ready |

### SPARC Execution Commands

Use swarm/hive-mind with the SPARC phase in the objective:

```bash
# Specification phase
npx claude-flow@alpha swarm start -o "SPARC Specification: user authentication" -s development

# Pseudocode phase
npx claude-flow@alpha swarm start -o "SPARC Pseudocode: user authentication" -s development

# Architecture phase
npx claude-flow@alpha swarm start -o "SPARC Architecture: user authentication" -s development

# Refinement phase (TDD) - use hive-mind for complex coordination
npx claude-flow@alpha hive-mind spawn --claude -o "SPARC Refinement (TDD): user authentication"

# Completion phase
npx claude-flow@alpha swarm start -o "SPARC Completion: user authentication" -s development
```

### Background Worker Commands

```bash
npx claude-flow@alpha hooks worker-dispatch --trigger map --context "src/"
npx claude-flow@alpha hooks worker-dispatch --trigger testgaps --context "src/"
npx claude-flow@alpha hooks worker-dispatch --trigger audit --priority critical
npx claude-flow@alpha hooks worker-dispatch --trigger document --context "src/"
```

---

## User Decision Menu (16 Options)

```
[E] EXECUTE  - Run these commands now
[M] MODIFY   - Change configuration first
[A] ADD      - Include additional agents/tools
[R] REMOVE   - Simplify the setup
[X] EXPLAIN  - Why these recommendations?
[S] SAVE     - Save config for later
[L] LEARN    - Teach me these patterns
[I] INTEL    - Configure intelligence layer
[W] WORKERS  - Configure background workers
[B] BROWSER  - Add browser automation
[C] CLAIMS   - Set up human/agent coordination
[P] SECURITY - Configure security (audit, AIDefence)
[T] TESTING  - Configure testing (testgaps, TDD)
[D] DOCS     - Configure documentation
[G] GITHUB   - Configure GitHub integration
[N] SESSION  - Configure session management
```

---

## Coaching Shortcuts (25 Available)

| Shortcut | Action |
|----------|--------|
| `"just recommend"` | Skip questions, get best config |
| `"explain more"` | Deeper teaching on any concept |
| `"simpler setup"` | Minimal configuration |
| `"full power"` | Maximum agents and features |
| `"compare options"` | See alternatives side-by-side |
| `"enable intelligence"` | Turn on full RuVector suite |
| `"dispatch workers"` | Show background worker options |
| `"v3 features"` | List all v3-exclusive features |
| `"enable security"` | Turn on full security suite |
| `"security audit"` | Add security-manager + audit worker |
| `"scan for vulnerabilities"` | Dispatch audit worker immediately |
| `"enable testing"` | Turn on testgaps + tester agent |
| `"enable tdd"` | Turn on TDD workflow |
| `"check coverage"` | Dispatch testgaps worker immediately |
| `"enable docs"` | Turn on document worker + api-docs |
| `"map codebase"` | Dispatch map worker immediately |
| `"new project setup"` | Auto-trigger: map → ultralearn → testgaps + Full SPARC |
| `"pre-release check"` | Auto-trigger: audit → benchmark → document |
| `"enable github"` | Turn on GitHub integration |
| `"enable sessions"` | Turn on session persistence |
| `"enable claims"` | Turn on human/agent coordination |
| `"enable sparc"` | Turn on full SPARC methodology (all 5 phases) |
| `"sparc lite"` | Quick SPARC (Pseudocode → Refinement only) |
| `"sparc refactor"` | SPARC for refactoring (Architecture → Completion) |
| `"skip sparc"` | Disable SPARC methodology recommendations |

---

## Orchestration Modes

| Mode | Best For | Auto-Features |
|------|----------|---------------|
| **SWARM** | Quick tasks (< 1 hour) | Ephemeral, no sessions |
| **HIVE-MIND** | Complex projects | Sessions auto-enabled, persistent memory |
| **DAA** | Self-learning systems | Full intelligence suite, knowledge sharing |

---

## Topologies

| Topology | Structure | Best For |
|----------|-----------|----------|
| **MESH** | Everyone ↔ Everyone | High collaboration (3-8 agents) |
| **HIERARCHICAL** | Queen → Leads → Workers | Large teams (up to 20 agents) |
| **RING** | A → B → C → D → A | Pipeline processing |
| **STAR** | Central hub ↔ All | Centralized control |
| **HIERARCHICAL-MESH** | Queen → Teams (mesh within) | Multiple teams |

---

## Background Workers (12 Available)

| Worker | Purpose | Priority |
|--------|---------|----------|
| `ultralearn` | Deep knowledge acquisition | Normal |
| `optimize` | Performance optimization | High |
| `consolidate` | Memory cleanup | Low |
| `predict` | Predictive preloading | Normal |
| `audit` | Security vulnerability scanning | **CRITICAL** |
| `map` | Codebase architecture mapping | Normal |
| `preload` | Resource cache warming | Low |
| `deepdive` | Deep code analysis | Normal |
| `document` | Auto-documentation | Normal |
| `refactor` | Refactoring suggestions | Normal |
| `benchmark` | Performance benchmarking | Normal |
| `testgaps` | Test coverage analysis | Normal |

---

## Documentation Files

```
.claude/skills/flow-coach/
├── SKILL.md              # Main skill file
├── README.md             # This documentation
└── docs/
    ├── CLI_REFERENCE.md  # All 50+ CLI commands
    ├── MCP_REFERENCE.md  # All 230+ MCP tools
    ├── AGENTS.md         # ~15 core + unlimited custom agents
    ├── TOPOLOGIES.md     # Mode & topology guide
    ├── INTELLIGENCE.md   # Neural features (SONA, MoE, HNSW)
    ├── WORKERS.md        # 12 background workers
    ├── SECURITY.md       # Security configuration
    ├── TESTING.md        # Testing configuration
    ├── INTEGRATION.md    # GitHub, Diff, Claims, Sessions
    └── TROUBLESHOOTING.md # Common issues & solutions
```

---

## Version History

| Version | Changes |
|---------|---------|
| **2.0.0** | Added 3 new phases, 7 decision trees, 10 shortcuts, auto-recommendations |
| **1.1.0** | Added Security Configuration phase |
| **1.0.0** | Initial release with 10 phases |

---

## Compatibility

- **Claude-Flow**: v3.0.0-alpha+
- **Claude Code**: Latest
- **MCP Servers**: claude-flow@alpha

---

## Core Principle

**Flow Coach NEVER auto-executes.** Every command is displayed for review, and you always choose what happens next.

```
[E] EXECUTE  ← Nothing runs until you choose this
```

---

## Quick Reference Card

```
┌─────────────────────────────────────────────────────────────────┐
│                      FLOW COACH QUICK REF                       │
├─────────────────────────────────────────────────────────────────┤
│ INVOKE:     /flow-coach <task>                                  │
│                                                                 │
│ MODES:      SWARM (quick) | HIVE-MIND (complex) | DAA (learning)│
│                                                                 │
│ SPARC:      Full (S→P→A→R→C) for new features                   │
│             Refinement only for bug fixes                       │
│             A→R→C for refactoring                               │
│             P→R for prototypes                                  │
│             None for research (no code generation)              │
│                                                                 │
│ SHORTCUTS:  "just recommend" | "full power" | "simpler setup"   │
│             "enable security" | "enable testing" | "enable tdd" │
│             "enable sparc" | "sparc lite" | "skip sparc"        │
│             "enable docs" | "enable github" | "map codebase"    │
│             "new project setup" | "pre-release check"           │
│                                                                 │
│ MENU:       [E]xecute [M]odify [A]dd [R]emove [X]plain         │
│             [S]ave [L]earn [I]ntel [W]orkers [B]rowser         │
│             [C]laims [P]security [T]esting [D]ocs [G]ithub     │
│             [N]session                                          │
│                                                                 │
│ COMPOUND AUTO-TRIGGERS (multiple features per task):            │
│   New feature → Full SPARC + testing + security + docs          │
│   Auth work   → Full SPARC + security + testing + sessions      │
│   REST API    → Full SPARC + docs + testing + security          │
│   Bug fix     → SPARC Refinement + testing + regression         │
│   Refactor    → SPARC A→R→C + testing + security                │
│   Prototype   → SPARC Lite (P→R) + minimal testing              │
│   Deploy      → pre-release pattern + claims + github           │
│   Research    → SWARM only (no SPARC, no code generation)       │
└─────────────────────────────────────────────────────────────────┘
```

---

## Real-World Examples

Each example shows exactly what Flow Coach enables and why:

### 1. Authentication System

**Task**: `/flow-coach implement OAuth2 authentication`

**Keywords detected**: `implement` (code gen) + `OAuth` + `authentication` (security-critical)

```
Mode: HIVE-MIND (complex, multi-component)
Topology: MESH (agents collaborate on auth flows)

Auto-Enabled:
├── Security (CRITICAL priority)
│   ├── security-manager agent (opus model - complex reasoning)
│   ├── audit worker (runs before any commit)
│   └── AIDefence (credential & token detection)
│
├── Testing (HIGH priority)
│   ├── testgaps worker (coverage analysis)
│   └── tester agent (auth flow tests)
│
├── Documentation
│   └── document worker (auth flow diagrams)
│
└── Sessions
    └── Auto-enabled (HIVE-MIND requires persistence)
```

---

### 2. REST API Development

**Task**: `/flow-coach build a REST API for user management`

**Keywords detected**: `build` (code gen) + `REST API` (docs required) + `user` (data handling)

```
Mode: HIVE-MIND (multi-endpoint system)
Topology: MESH (architect + backend + tester collaborate)

Auto-Enabled:
├── Documentation (HIGH priority)
│   ├── api-docs agent (OpenAPI/Swagger generation)
│   └── document worker (endpoint documentation)
│
├── Testing
│   ├── testgaps worker
│   └── tester agent (integration tests for endpoints)
│
├── Security
│   └── audit worker (input validation, SQL injection checks)
│
└── Sessions
    └── Auto-enabled (HIVE-MIND)
```

---

### 3. Bug Fix with Regression Test

**Task**: `/flow-coach fix the login bug and add regression tests`

**Keywords detected**: `fix` (bug) + `login` (auth-related) + `tests` (explicit)

```
Mode: SWARM (focused, single-purpose task)
Topology: STAR (coder at center, tester validates)

Auto-Enabled:
├── Testing (HIGH priority)
│   ├── testgaps worker (find untested paths around bug)
│   └── tester agent (write regression test FIRST)
│
└── Security
    └── audit worker (login = auth-sensitive area)

NOT Auto-Enabled:
├── Documentation: ❌ (bug fix, not new feature)
├── Sessions: ❌ (SWARM = ephemeral)
└── Intelligence: ❌ (no learning required)
```

---

### 4. Production Deployment

**Task**: `/flow-coach prepare for production deployment`

**Keywords detected**: `production` + `deployment` (pre-release triggers)

```
Mode: HIVE-MIND (critical, multi-stage)
Topology: HIERARCHICAL (production-validator leads)

Auto-Enabled:
├── Pre-Release Pattern (runs sequentially)
│   ├── 1. audit worker (CRITICAL - security scan first)
│   ├── 2. benchmark worker (performance validation)
│   ├── 3. document worker (changelog, release notes)
│   └── 4. testgaps worker (coverage gate)
│
├── Agents
│   └── production-validator (final readiness check)
│
├── GitHub Integration
│   └── Workflow trigger (deployment pipeline)
│
├── Claims System
│   └── Approval gates (security team, ops team)
│
└── Sessions
    └── Persistent (track deployment state)
```

---

### 5. New Project Onboarding

**Task**: `/flow-coach I'm new to this codebase, help me understand it`

**Keywords detected**: `new to` + `codebase` (onboarding pattern)

```
Mode: SWARM (exploration, no code changes)
Topology: STAR (researcher at center)

Auto-Enabled:
├── New Project Pattern (runs sequentially)
│   ├── 1. map worker (directory structure, dependencies)
│   ├── 2. ultralearn worker (domain knowledge extraction)
│   └── 3. testgaps worker (coverage baseline)
│
└── Workers
    └── deepdive (analyze complex modules)

NOT Auto-Enabled:
├── Security: ❌ (no code changes)
├── Testing: ❌ (no implementation)
├── Documentation: ❌ (exploration only)
└── GitHub: ❌ (no PR work)
```

---

### 6. Self-Learning System

**Task**: `/flow-coach build agents that learn from past solutions`

**Keywords detected**: `build` (code gen) + `learn` + `past solutions` (DAA triggers)

```
Mode: DAA (Decentralized Autonomous Agents - required for learning)
Topology: MESH (all agents share knowledge)

Auto-Enabled:
├── Intelligence Layer (FULL SUITE)
│   ├── SONA optimizer (neural optimization)
│   ├── MoE routing (expert selection)
│   ├── HNSW indexing (fast similarity search)
│   ├── Neural embeddings (384-dim vectors)
│   └── EWC++ (prevent catastrophic forgetting)
│
├── Security (CRITICAL for learning systems)
│   └── AIDefence (prevent learning malicious patterns)
│
├── Testing
│   └── testgaps worker (validate learned behaviors)
│
└── Sessions
    └── Persistent (learning state MUST be preserved)
```

---

### 7. Refactoring Authentication

**Task**: `/flow-coach refactor the authentication module`

**Keywords detected**: `refactor` (tech-debt) + `authentication` (security-critical)

```
Mode: HIVE-MIND (structural changes require coordination)
Topology: MESH (architect + coder + reviewer collaborate)

Auto-Enabled:
├── Technical Debt Pattern (runs sequentially)
│   ├── 1. map worker (current architecture snapshot)
│   ├── 2. refactor worker (improvement suggestions)
│   ├── 3. testgaps worker (ensure coverage maintained)
│   └── 4. optimize worker (performance after refactor)
│
├── Security (CRITICAL - auth module)
│   └── audit worker (runs before AND after changes)
│
├── Testing
│   └── testgaps (baseline BEFORE, validation AFTER)
│
└── Sessions
    └── Auto-enabled (track refactoring progress)
```

---

### 8. Research Task (Minimal Triggers)

**Task**: `/flow-coach explain how the routing works`

**Keywords detected**: `explain` (research, no code)

```
Mode: SWARM (quick exploration)
Topology: STAR (researcher at center)

Auto-Enabled:
├── Workers
│   ├── map (understand structure)
│   └── deepdive (analyze routing logic)
│
└── Agent
    └── researcher (exploration specialist)

NOT Auto-Enabled:
├── Security: ❌ (no code changes)
├── Testing: ❌ (no implementation)
├── Documentation: ❌ (not creating docs)
├── GitHub: ❌ (no PR/CI work)
├── Intelligence: ❌ (no learning needed)
└── Sessions: ❌ (SWARM = ephemeral)
