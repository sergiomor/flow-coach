# Agent Categories Reference

This document describes the agent types available in claude-flow v3.

> **CLI Reference**: For CLI commands including `agent spawn`, see [CLI_REFERENCE.md](CLI_REFERENCE.md).
>
> **MCP Reference**: For agent MCP tools, see [MCP_REFERENCE.md](MCP_REFERENCE.md).

---

## Understanding Agent Types

### Core Agent Types (~15 predefined)

These are **TypeScript-defined agent types** with specific behaviors, capabilities, and built-in coordination patterns. They have predefined roles and optimized prompts for their specialized tasks.

### Custom/Dynamic Agents (unlimited)

Claude-flow supports **dynamic agent creation** - you can spawn any agent type by name, and claude-flow will create it dynamically with the role you specify. This allows for:

- Domain-specific agents (e.g., `data-analyst`, `database-migrator`, `security-auditor`)
- Project-specific roles (e.g., `domain-researcher`, `api-designer`)
- Any custom agent type that fits your workflow

**Example - Creating a custom agent:**
```javascript
// This works even though "my-custom-agent" isn't predefined
mcp__claude-flow__agent_spawn {
  agentType: "my-custom-agent",
  model: "sonnet",
  task: "Perform specialized analysis for my domain"
}
```

> **Note**: Custom agents inherit base coordination capabilities but don't have the specialized prompts of core agents. For complex specialized behaviors, consider using a core agent type and customizing via the task description.

---

## Core Agent Types

The following sections list the **Core TypeScript-defined agents** (marked with **[Core]**) and **example custom agents** (marked with **[Example]**) that demonstrate common patterns.

---

## Core Development (5 agents) [Core]

| Agent | Purpose |
|-------|---------|
| `researcher` | Analysis and information gathering |
| `coder` | Implementation and coding |
| `tester` | Test creation and validation |
| `reviewer` | Code review and quality |
| `planner` | Task planning and decomposition |

---

## Architecture (7 agents) [Core]

| Agent | Purpose |
|-------|---------|
| `system-architect` | High-level system design |
| `backend-dev` | API and server development |
| `mobile-dev` | Mobile app development |
| `ml-developer` | Machine learning development |
| `api-docs` | Documentation generation |
| `code-analyzer` | Code quality analysis |
| `cicd-engineer` | CI/CD pipeline setup |

---

## SPARC Methodology (14 agents) [Core]

| Agent | Purpose |
|-------|---------|
| `sparc-coord` | SPARC workflow coordination |
| `sparc-coder` | SPARC implementation specialist |
| `specification` | Requirements specification |
| `pseudocode` | Algorithm design |
| `architecture` | System architecture |
| `refinement` | Code refinement |
| `sparc-architect` | Architecture mode |
| `sparc-tester` | Testing mode |
| `sparc-reviewer` | Review mode |
| `sparc-researcher` | Research mode |
| `sparc-debugger` | Debugging mode |
| `sparc-optimizer` | Optimization mode |
| `sparc-documenter` | Documentation mode |
| `sparc-innovator` | Innovation mode |

### SPARC Usage

> **Note**: SPARC is a methodology. Use swarm/hive-mind with SPARC agents (see [SPARC.md](SPARC.md)).

```bash
# Use swarm with SPARC agents
npx claude-flow@alpha swarm init --topology star --max-agents 3
npx claude-flow@alpha swarm start -o "Specification: your feature" -s development

# Use hive-mind for TDD workflow
npx claude-flow@alpha hive-mind init -t mesh -m 5
npx claude-flow@alpha hive-mind spawn --claude -o "TDD: your feature"
```

See [SPARC.md](SPARC.md) for complete methodology reference.

---

## Swarm Coordination (10 agents) [Mixed: Core + Example]

| Agent | Type | Purpose |
|-------|------|---------|
| `hierarchical-coordinator` | Core | Queen-led coordination |
| `mesh-coordinator` | Core | Peer-to-peer coordination |
| `adaptive-coordinator` | Core | Dynamic topology switching |
| `queen-coordinator` | Example | Hive-mind leadership |
| `worker-specialist` | Example | Task execution |
| `scout-explorer` | Example | Information reconnaissance |
| `swarm-memory-manager` | Core | Distributed memory |
| `collective-intelligence` | Example | Group decision-making |
| `task-orchestrator` | Core | Task distribution |
| `memory-coordinator` | Core | Memory synchronization |

---

## Consensus & Distributed (7 agents) [Mixed: Core + Example]

| Agent | Type | Purpose |
|-------|------|---------|
| `byzantine-coordinator` | Example | Fault-tolerant consensus |
| `raft-manager` | Example | Leader election |
| `gossip-coordinator` | Example | Eventually consistent |
| `quorum-manager` | Example | Membership management |
| `crdt-synchronizer` | Example | Conflict-free replication |
| `security-manager` | Core | Security protocols |
| `consensus-builder` | Example | General consensus |

### Consensus Algorithms

- **Raft**: Leader election, log replication (hierarchical)
- **Byzantine**: Fault-tolerant, handles malicious nodes
- **Gossip**: Eventually consistent, epidemic protocol
- **CRDT**: Conflict-free, automatic merge

---

## GitHub Integration (14 agents) [Mixed: Core + Example]

| Agent | Type | Purpose |
|-------|------|---------|
| `pr-manager` | Core | Pull request management |
| `code-review-swarm` | Core | Distributed code review |
| `issue-tracker` | Core | Issue management |
| `release-manager` | Core | Release coordination |
| `workflow-automation` | Core | GitHub Actions automation |
| `project-board-sync` | Example | Project board sync |
| `repo-architect` | Example | Repository architecture |
| `multi-repo-swarm` | Core | Multi-repository coordination |
| `swarm-pr` | Example | PR swarm operations |
| `swarm-issue` | Example | Issue swarm operations |
| `github-modes` | Core | GitHub integration modes |
| `pr-enhance` | Example | PR enhancement |
| `issue-triage` | Example | Issue triage |
| `github-swarm` | Core | Full GitHub swarm |

---

## Performance & Quality (9 agents) [Mixed: Core + Example]

| Agent | Type | Purpose |
|-------|------|---------|
| `perf-analyzer` | Core | Performance analysis |
| `performance-benchmarker` | Core | Benchmark execution |
| `production-validator` | Example | Production validation |
| `smart-agent` | Core | Intelligent task handling |
| `analyst` | Example | General analysis |
| `tdd-london-swarm` | Core | London-style TDD |
| `migration-planner` | Core | Migration planning |
| `base-template-generator` | Example | Template generation |
| `swarm-init` | Core | Swarm initialization |

---

## Agent Selection by Task Type

### For API Development
```
system-architect -> backend-dev -> tester -> reviewer -> api-docs
```

### For Feature Development
```
researcher -> specification -> coder -> tester -> reviewer
```

### For Bug Fixing
```
sparc-debugger -> code-analyzer -> coder -> tester
```

### For Architecture Review
```
system-architect -> code-analyzer -> sparc-reviewer -> security-manager
```

### For CI/CD Setup
```
cicd-engineer -> workflow-automation -> tester -> production-validator
```

### For Research
```
researcher -> sparc-researcher -> analyst -> sparc-documenter
```

---

## Agent Scaling Pattern

```
1 agent  -> Validate approach
3 agents -> Core team (research, code, test)
5 agents -> Standard swarm
8+ agents -> Hive-mind with specialists
```

---

## MCP Agent Spawn

```javascript
// Spawn a core agent via MCP
mcp__claude-flow__agent_spawn {
  agentType: "coder",
  model: "sonnet",  // haiku, sonnet, opus, inherit
  task: "Implement REST API endpoints"
}

// Spawn a CUSTOM agent - claude-flow creates it dynamically!
mcp__claude-flow__agent_spawn {
  agentType: "data-pipeline-engineer",  // Any name works
  model: "sonnet",
  task: "Design and implement ETL pipeline for user analytics"
}

// List agents
mcp__claude-flow__agent_list {}

// Check agent status
mcp__claude-flow__agent_status { agentId: "agent-123" }
```

---

## Creating Custom Agents

You can spawn **any custom agent type** by name. Claude-flow will create it dynamically with the role you specify.

### When to Use Custom Agents

- **Domain-specific work**: `data-analyst`, `research-specialist`, `domain-expert`
- **Project-specific roles**: `project-coordinator`, `method-validator`
- **Specialized tasks**: `schema-migrator`, `api-contract-designer`, `performance-tuner`

### Custom Agent Best Practices

1. **Use descriptive names**: `database-schema-designer` is better than `db-agent`
2. **Provide detailed tasks**: Custom agents rely on task descriptions for context
3. **Combine with core agents**: Use custom agents alongside core agents in swarms
4. **Consider using core agents first**: If a core agent fits, use it for optimized behavior

### Example Custom Agent Workflows

```javascript
// Domain-specific research team
mcp__claude-flow__agent_spawn { agentType: "methodology-researcher", task: "Research domain-specific methodologies" }
mcp__claude-flow__agent_spawn { agentType: "data-analyst", task: "Analyze statistical patterns" }
mcp__claude-flow__agent_spawn { agentType: "uncertainty-specialist", task: "Quantify parameter uncertainties" }

// Project-specific development team
mcp__claude-flow__agent_spawn { agentType: "api-designer", task: "Design REST endpoints for domain calculations" }
mcp__claude-flow__agent_spawn { agentType: "integration-specialist", task: "Integrate third-party APIs and services" }
```

---

## Summary: Core vs Custom Agents

| Aspect | Core Agents (~15) | Custom Agents (unlimited) |
|--------|-------------------|---------------------------|
| **Definition** | TypeScript-defined in claude-flow | Created dynamically at runtime |
| **Behavior** | Specialized prompts and capabilities | Inherits base coordination |
| **Use case** | Standard development workflows | Domain/project-specific needs |
| **Examples** | `coder`, `tester`, `researcher` | `data-analyst`, `data-migrator` |
| **Best for** | General tasks with proven patterns | Specialized domain expertise |
