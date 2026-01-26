# Decision Trees Reference

Quick-reference decision trees for claude-flow v3 orchestration configuration.

> **CLI Reference**: For complete CLI command documentation, see [CLI_REFERENCE.md](CLI_REFERENCE.md).
>
> **MCP Reference**: For all MCP tools, see [MCP_REFERENCE.md](MCP_REFERENCE.md).

**Navigation**: [SPARC Methodology](#sparc-methodology) | [Mode](#mode-selection) | [Topology](#topology-selection) | [Security](#security-configuration) | [Testing](#testing-configuration) | [Documentation](#documentation-configuration) | [Codebase Mapping](#codebase-mapping) | [Session Management](#session-management) | [GitHub](#github-integration) | [Worker Patterns](#worker-patterns-auto-trigger) | [Claims](#claims-system) | [Browser Automation](#browser-automation-configuration)

---

## SPARC Methodology

```
What type of task is this?
|
|-- New feature / Implement / Build / Create?
|   +-- FULL SPARC CYCLE
|       Phases: Specification → Pseudocode → Architecture → Refinement → Completion
|       Agents: specification, pseudocode, architecture, tdd-london-swarm, refinement
|
|-- Bug fix / Issue / Error?
|   +-- SPARC REFINEMENT ONLY
|       Phases: Refinement (with regression testing)
|       Agents: tester, coder, reviewer
|
|-- Refactoring / Cleanup / Tech debt?
|   +-- SPARC FROM ARCHITECTURE
|       Phases: Architecture → Refinement → Completion
|       Agents: architecture, coder, tester, reviewer
|
|-- Prototype / Spike / Quick test?
|   +-- SPARC LITE
|       Phases: Pseudocode → Refinement
|       Agents: pseudocode, coder, tester
|
|-- Documentation only?
|   +-- SPARC COMPLETION ONLY
|       Phases: Completion
|       Agents: api-docs, documenter
|
+-- Research / Analysis / Exploration?
    +-- NO SPARC (not code generation)
        Agents: researcher, analyst
```

> See [SPARC.md](SPARC.md) for execution commands.
> Detailed SPARC reference: [SPARC.md](SPARC.md)

---

## Mode Selection

```
Is your task...
|-- Quick/focused (< 1 hour)? --> SWARM
|-- Complex/multi-day? ---------> HIVE-MIND
|-- Need to resume later? ------> HIVE-MIND (persistent)
|-- Self-learning required? ----> DAA (autonomous)
+-- Distributed/fault-tolerant? -> HIVE-MIND + byzantine
```

---

## Topology Selection

```
How do your agents need to communicate?
|
|-- Everyone talks to everyone?
|   +-- MESH topology
|       Best for: High collaboration (3-8 agents)
|
|-- Clear boss/worker structure?
|   +-- HIERARCHICAL topology
|       Best for: Large teams, clear delegation
|
|-- Work flows in stages?
|   +-- RING topology
|       Best for: Pipelines, sequential processing
|
|-- Central coordinator needed?
|   +-- STAR topology
|       Best for: Centralized control, aggregation
|
+-- Multiple teams with cross-team needs?
    +-- HIERARCHICAL-MESH (hybrid)
        Best for: Enterprise, complex systems
```

> Detailed topology information: [TOPOLOGIES.md](TOPOLOGIES.md)

---

## Security Configuration

```
Does your task involve...
|-- Code generation? -------------------> Enable audit worker + security-manager
|-- Self-learning agents? --------------> Enable AIDefence (prevent bad pattern learning)
|-- Authentication/authorization? ------> Enable security-manager + audit (CRITICAL)
|-- User input handling? ---------------> Enable AIDefence PII detection
|-- API/database operations? -----------> Enable audit worker
|-- File system operations? ------------> Enable audit worker
|-- Documentation only? ----------------> Security optional
+-- Research/exploration only? ---------> Security optional
```

> Detailed security configuration: [SECURITY.md](SECURITY.md)

---

## Testing Configuration

```
Does your task involve...
|-- Code generation? -------------------> testgaps worker + tester agent
|-- TDD/test-first mentioned? ----------> tdd-london-swarm agent + testgaps
|-- Bug fix? ---------------------------> testgaps (find untested paths)
|-- Refactoring? -----------------------> testgaps (ensure coverage maintained)
|-- API development? -------------------> tester agent (API tests)
|-- Documentation only? ----------------> Testing optional
+-- Research/exploration only? ---------> Testing optional
```

> Detailed testing configuration: [TESTING.md](TESTING.md)

---

## Documentation Configuration

```
Does your task involve...
|-- API development? -------------------> document worker + api-docs agent
|-- Feature implementation? ------------> document worker (on completion)
|-- Public library/package? ------------> documenter agent + document worker
|-- Release preparation? ---------------> document worker (auto-trigger)
|-- Internal tooling? ------------------> document worker (recommended)
|-- Prototype/experiment? --------------> Documentation optional
+-- Research only? ---------------------> Documentation optional
```

---

## Codebase Mapping

```
Is this...
|-- New codebase/project? --------------> map worker FIRST (before other work)
|-- Large refactoring task? ------------> map worker (understand structure)
|-- Architecture design? ---------------> map worker + system-architect
|-- Understanding existing code? -------> map + deepdive workers
|-- Debugging complex issue? -----------> deepdive + map workers
|-- Simple feature addition? -----------> Mapping optional
+-- Documentation only? ----------------> Mapping optional
```

---

## Session Management

```
Which mode was selected?
|-- HIVE-MIND? -------------------------> AUTO-ENABLE session management
|   |-- session-start with daemon
|   |-- session-end with export
|   +-- Auto-save on exit
|-- DAA (self-learning)? ---------------> AUTO-ENABLE persistent sessions
|-- SWARM (quick task)? ----------------> Sessions optional (ephemeral)
|   +-- Unless "resume later" mentioned -> Enable sessions
```

---

## GitHub Integration

```
Does your task mention...
|-- PR/pull request? -------------------> github_pr_manage + analyze_diff
|-- Code review? -----------------------> analyze_diff + reviewers suggestion
|-- CI/CD workflow? --------------------> github_workflow tools
|-- Release/deployment? ----------------> github_pr_manage + workflow
|-- Issue tracking? --------------------> github_issue_track
|-- Repository analysis? ---------------> github_repo_analyze
|-- None of the above? -----------------> GitHub integration optional
```

> Detailed integration configuration: [INTEGRATION.md](INTEGRATION.md)

---

## Worker Patterns (Auto-Trigger)

```
What type of task is this?

NEW PROJECT SETUP:
|-- New codebase? ----------------------> Auto-trigger pattern:
|   1. map (codebase structure)
|   2. ultralearn (domain knowledge)
|   3. testgaps (coverage baseline)

PRE-RELEASE CHECKLIST:
|-- Release/deploy mentioned? ----------> Auto-trigger pattern:
|   1. audit (security - CRITICAL)
|   2. benchmark (performance)
|   3. document (auto-docs)

TECHNICAL DEBT SPRINT:
|-- Refactoring mentioned? -------------> Auto-trigger pattern:
|   1. map (current architecture)
|   2. refactor (suggestions)
|   3. testgaps (ensure coverage)
|   4. optimize (performance)

COMPLEX INVESTIGATION:
|-- Debugging/investigation? -----------> Auto-trigger pattern:
|   1. deepdive (problematic code)
|   2. map (dependencies)
|   3. benchmark (performance baseline)
```

> Detailed worker configuration: [WORKERS.md](WORKERS.md)

---

## Claims System

```
Does your workflow need...
|-- Human approval gates? --------------> Claims system + claims_board
|-- Review cycles? ---------------------> Claims with handoff configuration
|-- Multi-stage approval? --------------> Claims board visualization
|-- Agent-to-human handoff? ------------> Claims system
|-- Fully autonomous? ------------------> Claims optional
```

> Detailed claims configuration: [INTEGRATION.md](INTEGRATION.md)

---

## Browser Automation Configuration

```
Does your task involve...
|-- End-to-end testing? ---------------> Browser automation tools
|   Tools: browser_open, browser_click, browser_fill, browser_screenshot
|   Agent: tester with browser capabilities
|-- Web scraping / Data extraction? ---> Browser automation tools
|-- Visual regression testing? --------> browser_screenshot + browser_snapshot
|-- Form automation / UI testing? -----> browser_fill, browser_type, browser_click
|-- Screenshot capture? ---------------> browser_screenshot
|-- Unit/integration tests only? ------> Browser automation optional
+-- API testing only? -----------------> Browser automation optional
```

**Trigger keywords**: end-to-end, E2E, browser test, web automation, visual testing, screenshot, form automation, UI testing, web application tests

---

**Back to**: [SKILL.md](../SKILL.md) | [Main Documentation](TOPOLOGIES.md)
