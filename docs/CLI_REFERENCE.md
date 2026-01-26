# CLI Commands Reference

Complete reference for claude-flow v3 CLI commands.

> **Usage**: `npx claude-flow@alpha <command> [options]`
>
> **Help**: `npx claude-flow@alpha --help` or `npx claude-flow@alpha <command> --help`

---

## 📊 Quick Stats

- **Total Commands**: 50+
- **Core Commands**: 15 (most commonly used)
- **Advanced Commands**: 35+ (specialized features)
- **Categories**: 12 command categories

## 📑 Table of Contents

### Core Commands (Most Used)
- [Swarm Commands](#swarm-commands) - Quick task orchestration
- [Hive-Mind Commands](#hive-mind-commands) - Complex project coordination
- [Memory Commands](#memory-commands) - Store, search, retrieve data
- [Hooks Commands](#hooks-commands) - Session, intelligence, workers
- [Agent Commands](#agent-commands) - Spawn, list, manage agents
- [Init Commands](#init-commands) - Project initialization

### Advanced Commands
- [Analyze Commands](#analyze-commands) - Code analysis (AST, MinCut, Louvain)
- [Route Commands](#route-commands) - Q-Learning task routing
- [Config Commands](#config-commands) - Configuration management
- [Claims Commands](#claims-commands) - Task claiming and handoffs
- [Progress Commands](#progress-commands) - Implementation tracking
- [Doctor Commands](#doctor-commands) - System diagnostics

### Utility Commands
- [Completions](#completions) - Shell completion setup
- [Deployment](#deployment) - Production deployment
- [Transfer Store](#transfer-store) - IPFS pattern marketplace
- [Plugins](#plugins) - Plugin registry

### Reference
- [Global Options](#global-options) - Flags available on all commands
- [Environment Variables](#environment-variables) - Configuration via env

💡 **Tip**: Use `Ctrl+F` (Cmd+F) to search for specific commands.

---

## Swarm Commands

Quick task orchestration for focused work.

### swarm

```bash
# Basic swarm execution
npx claude-flow@alpha swarm "Build a REST API"

# With strategy
npx claude-flow@alpha swarm "Research cloud architecture" --strategy research

# With options
npx claude-flow@alpha swarm "Build feature" \
  --strategy development \
  --max-agents 5 \
  --parallel \
  --monitor
```

**Options**:
| Flag | Description | Default |
|------|-------------|---------|
| `--strategy <type>` | auto, research, development, analysis | auto |
| `--max-agents <n>` | Maximum agents | 5 |
| `--timeout <minutes>` | Execution timeout | 60 |
| `--research` | Enable research capabilities | false |
| `--parallel` | Enable parallel execution | false |
| `--review` | Enable peer review | false |
| `--monitor` | Real-time monitoring | false |
| `--background` | Run in background | false |
| `--distributed` | Distributed coordination | false |
| `--memory-namespace <ns>` | Memory namespace | swarm |
| `--dry-run` | Show config without executing | false |

### swarm init

```bash
# Initialize swarm with topology
npx claude-flow@alpha swarm init --topology mesh --max-agents 5
```

**Options**:
| Flag | Description | Default |
|------|-------------|---------|
| `--topology <type>` | mesh, hierarchical, ring, star, hierarchical-mesh, adaptive | mesh |
| `--max-agents <n>` | Maximum agents | 5 |

### swarm start

```bash
# Start swarm with objective
npx claude-flow@alpha swarm start -o "Build REST API" -s development --parallel
```

**Options**:
| Flag | Description |
|------|-------------|
| `-o, --objective <task>` | Task objective |
| `-s, --strategy <type>` | Execution strategy |
| `--parallel` | Parallel execution |

### swarm status

```bash
npx claude-flow@alpha swarm status
```

### swarm stop

```bash
npx claude-flow@alpha swarm stop
```

### SPARC Phases via Swarm

SPARC methodology is implemented by including the phase in the objective:

```bash
# Specification phase
npx claude-flow@alpha swarm start -o "SPARC Specification: user auth" -s development

# Pseudocode phase
npx claude-flow@alpha swarm start -o "SPARC Pseudocode: user auth" -s development

# Architecture phase
npx claude-flow@alpha swarm start -o "SPARC Architecture: user auth" -s development

# Completion phase
npx claude-flow@alpha swarm start -o "SPARC Completion: user auth" -s development
```

For TDD/Refinement phase, use hive-mind (see below).

---

## Hive-Mind Commands

Complex project coordination with persistent state.

### hive-mind init

```bash
# Interactive wizard
npx claude-flow@alpha hive-mind wizard

# Direct initialization
npx claude-flow@alpha hive-mind init -t hierarchical-mesh -c byzantine -m 15
```

**Options**:
| Flag | Description | Default |
|------|-------------|---------|
| `-t, --topology <type>` | mesh, hierarchical, ring, star, hierarchical-mesh, adaptive | hierarchical |
| `-c, --consensus <algo>` | byzantine, raft, gossip | raft |
| `-m, --max-agents <n>` | Maximum agents | 8 |
| `-n, --name <string>` | Swarm name | auto |
| `-q, --queen-mode <mode>` | centralized, distributed | centralized |
| `--consensus-threshold` | Consensus threshold | 0.66 |
| `--auto-spawn` | Auto-spawn initial agents | false |

### hive-mind spawn

```bash
# Spawn by type
npx claude-flow@alpha hive-mind spawn coder
npx claude-flow@alpha hive-mind spawn researcher --batch 3

# Launch Claude Code with hive-mind
npx claude-flow@alpha hive-mind spawn --claude -o "Build REST API"
```

**Options**:
| Flag | Description | Default |
|------|-------------|---------|
| `[type]` | Agent type: coordinator, researcher, coder, analyst, etc. | worker |
| `-n, --name <string>` | Custom agent name | auto |
| `-s, --swarm-id <id>` | Target swarm ID | current |
| `-b, --batch <n>` | Spawn multiple agents | 1 |
| `-i, --interactive` | Interactive spawn mode | false |
| `--auto-assign` | Auto-assign to tasks | false |
| `--claude` | Launch Claude Code session | false |
| `-o, --objective <task>` | Task objective (with --claude) | - |

### SPARC Refinement (TDD) via Hive-Mind

```bash
# TDD/Refinement requires coordination - use hive-mind
npx claude-flow@alpha hive-mind init -t mesh -m 5
npx claude-flow@alpha hive-mind spawn --claude -o "SPARC Refinement (TDD): user auth"
```

### hive-mind task

```bash
npx claude-flow@alpha hive-mind task "Implement authentication"
```

### hive-mind status

```bash
npx claude-flow@alpha hive-mind status
```

### hive-mind shutdown

```bash
# Graceful shutdown
npx claude-flow@alpha hive-mind shutdown
```


---

## Memory Commands

Store, search, and retrieve data.

### memory store

```bash
# Store text
npx claude-flow@alpha memory store -k "api/auth" --value "JWT implementation"

# Store with namespace
npx claude-flow@alpha memory store -k "pattern" --value "singleton" -n backend

# Store as vector
npx claude-flow@alpha memory store -k "pattern/singleton" --vector
```

**Options**:
| Flag | Description | Default |
|------|-------------|---------|
| `-k, --key <string>` | Storage key (required) | - |
| `--value <string>` | Value to store (required) | - |
| `-n, --namespace <string>` | Memory namespace | default |
| `--ttl <seconds>` | Time to live | - |
| `--tags <string>` | Comma-separated tags | - |
| `--vector` | Store as vector embedding | false |

### memory search

```bash
# Semantic search
npx claude-flow@alpha memory search -q "authentication" --type semantic

# With options
npx claude-flow@alpha memory search -q "pattern" -n backend --limit 10 --threshold 0.7
```

**Options**:
| Flag | Description | Default |
|------|-------------|---------|
| `-q, --query <string>` | Search query (required) | - |
| `-n, --namespace <string>` | Memory namespace | - |
| `--type <type>` | semantic, keyword | semantic |
| `--limit <n>` | Maximum results | 10 |
| `--threshold <float>` | Similarity threshold (0.0-1.0) | 0.7 |

### memory retrieve

```bash
npx claude-flow@alpha memory retrieve -k "api/auth"
npx claude-flow@alpha memory get -k "api/auth" -n backend
```

**Alias**: `memory get`

**Options**:
| Flag | Description | Default |
|------|-------------|---------|
| `-k, --key <string>` | Storage key (required) | - |
| `-n, --namespace <string>` | Memory namespace | default |

### memory list

```bash
npx claude-flow@alpha memory list
npx claude-flow@alpha memory list -n backend --limit 20
```

### memory delete

```bash
npx claude-flow@alpha memory delete -k "old/key"
```

---

## Hooks Commands

Session management, intelligence, and worker orchestration.

### hooks session-restore

```bash
# Restore latest session
npx claude-flow@alpha hooks session-restore --session-id "latest"

# Restore specific session
npx claude-flow@alpha hooks session-restore --session-id "session-abc123"
```

### hooks session-end

```bash
npx claude-flow@alpha hooks session-end --export-metrics true --save-state true
```

### hooks pre-edit

```bash
npx claude-flow@alpha hooks pre-edit -f "src/utils.ts"
npx claude-flow@alpha hooks pre-edit -f "src/api.ts" -o refactor
```

**Options**:
| Flag | Description | Default |
|------|-------------|---------|
| `-f, --file <path>` | File path (required) | - |
| `-o, --operation <type>` | create, update, delete, refactor | update |
| `-c, --context <string>` | Additional context | - |

### hooks post-edit

```bash
npx claude-flow@alpha hooks post-edit -f "src/api.ts" --outcome success
```

### hooks pre-task

```bash
npx claude-flow@alpha hooks pre-task --description "Implement feature X"
```

### hooks post-task

```bash
npx claude-flow@alpha hooks post-task --task-id "task-123"
```

### hooks intelligence

```bash
# Show intelligence status
npx claude-flow@alpha hooks intelligence --showStatus true

# Enable features
npx claude-flow@alpha hooks intelligence --enableSona true --enableMoe true --enableHnsw true
```

### hooks worker-dispatch

```bash
# Dispatch background worker
npx claude-flow@alpha hooks worker-dispatch --trigger map --context "src/"
npx claude-flow@alpha hooks worker-dispatch --trigger testgaps --context "src/"
npx claude-flow@alpha hooks worker-dispatch --trigger audit --priority critical
```

**Available workers**: map, ultralearn, testgaps, audit, benchmark, document, refactor, optimize, consolidate, predict, preload, deepdive

### hooks worker-status

```bash
npx claude-flow@alpha hooks worker-status
```

### hooks worker-list

```bash
npx claude-flow@alpha hooks worker-list
```

---

## Agent Commands

Spawn and manage agents.

### agent spawn

```bash
npx claude-flow@alpha agent spawn coder
npx claude-flow@alpha agent spawn researcher --model opus
```

### agent list

```bash
npx claude-flow@alpha agent list
```

### agent status

```bash
npx claude-flow@alpha agent status --agent-id "agent-123"
```

### agent terminate

```bash
npx claude-flow@alpha agent terminate --agent-id "agent-123"
```

---

## Init Commands

Project and environment initialization.

### init

```bash
# Interactive initialization
npx claude-flow@alpha init

# With wizard
npx claude-flow@alpha init wizard
```

### init config

```bash
npx claude-flow@alpha init config
```

---

## Analyze Commands

Code analysis with advanced algorithms.

### analyze

```bash
# Full analysis
npx claude-flow@alpha analyze src/

# AST analysis
npx claude-flow@alpha analyze ast src/utils.ts

# Dependency analysis
npx claude-flow@alpha analyze deps src/
```

### analyze ast

```bash
npx claude-flow@alpha analyze ast src/api.ts --format json
```

Parse and analyze Abstract Syntax Tree using tree-sitter.

### analyze mincut

```bash
npx claude-flow@alpha analyze mincut src/
```

MinCut algorithm for finding module boundaries.

### analyze louvain

```bash
npx claude-flow@alpha analyze louvain src/
```

Louvain algorithm for detecting module communities.

### analyze circular

```bash
npx claude-flow@alpha analyze circular src/
```

Detect circular dependencies.

### analyze complexity

```bash
npx claude-flow@alpha analyze complexity src/
```

---

## Route Commands

Q-Learning based intelligent task routing.

### route

```bash
npx claude-flow@alpha route "Implement authentication"
```

### route task

```bash
npx claude-flow@alpha route task "Build REST API" --coverage-aware
```

### route agent

```bash
npx claude-flow@alpha route agent --task-type security
```

### route learn

```bash
npx claude-flow@alpha route learn --outcome success --task-type coding
```

### route stats

```bash
npx claude-flow@alpha route stats
```

---

## Config Commands

Configuration management.

### config get

```bash
npx claude-flow@alpha config get topology
npx claude-flow@alpha config get --all
```

### config set

```bash
npx claude-flow@alpha config set topology mesh
npx claude-flow@alpha config set maxAgents 10
```

### config list

```bash
npx claude-flow@alpha config list
```

### config reset

```bash
npx claude-flow@alpha config reset
```

### config export

```bash
npx claude-flow@alpha config export --output config.json
```

### config import

```bash
npx claude-flow@alpha config import --file config.json
```

---

## Claims Commands

Task claiming and handoff coordination.

### claims list

```bash
npx claude-flow@alpha claims list
npx claude-flow@alpha claims list --status pending
```

### claims claim

```bash
npx claude-flow@alpha claims claim --task-id "task-123"
```

### claims release

```bash
npx claude-flow@alpha claims release --claim-id "claim-456"
```

### claims handoff

```bash
npx claude-flow@alpha claims handoff --claim-id "claim-456" --to-agent "agent-789"
```

### claims board

```bash
npx claude-flow@alpha claims board
```

---

## Progress Commands

Track V3 implementation progress.

### progress

```bash
npx claude-flow@alpha progress
```

### progress check

```bash
npx claude-flow@alpha progress check --feature v3-migration
```

### progress summary

```bash
npx claude-flow@alpha progress summary --group-by category
```

---

## Doctor Commands

System diagnostics and health checks.

### doctor

```bash
# Run all checks
npx claude-flow@alpha doctor

# With auto-fix
npx claude-flow@alpha doctor --fix
```

**Checks performed** (13 total):
- Node.js version
- npm/pnpm availability
- MCP server connectivity
- Memory backend health
- Agent pool status
- Configuration validity
- And more...

**Options**:
| Flag | Description |
|------|-------------|
| `--fix` | Attempt to auto-fix issues |
| `--verbose` | Detailed output |

---

## Completions

Shell completion setup.

```bash
# Generate for bash
npx claude-flow@alpha completions bash >> ~/.bashrc

# Generate for zsh
npx claude-flow@alpha completions zsh >> ~/.zshrc

# Generate for fish
npx claude-flow@alpha completions fish > ~/.config/fish/completions/claude-flow.fish

# Generate for PowerShell
npx claude-flow@alpha completions powershell >> $PROFILE
```

---

## Deployment

Production deployment management.

```bash
npx claude-flow@alpha deployment status
npx claude-flow@alpha deployment create --env production
```

---

## Transfer Store

IPFS-based pattern marketplace.

```bash
# Search patterns
npx claude-flow@alpha transfer-store search "authentication"

# Get pattern info
npx claude-flow@alpha transfer-store info <cid>

# Download pattern
npx claude-flow@alpha transfer-store download <cid> --output ./patterns/
```

---

## Plugins

Plugin registry management.

```bash
# Search plugins
npx claude-flow@alpha plugins search "formatter"

# Install plugin
npx claude-flow@alpha plugins install <plugin-id>

# List installed
npx claude-flow@alpha plugins list
```

---

## Global Options

Available on all commands:

| Flag | Description |
|------|-------------|
| `-h, --help` | Show help |
| `-v, --version` | Show version |
| `--verbose` | Verbose output |
| `--format <type>` | Output format: text, json |
| `--no-color` | Disable colored output |

---

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `CLAUDE_FLOW_CONFIG` | Config file path | .claude-flow.json |
| `CLAUDE_FLOW_MEMORY_BACKEND` | Memory backend | sqlite |
| `CLAUDE_FLOW_LOG_LEVEL` | Log level | info |
| `CLAUDE_FLOW_MCP_PORT` | MCP server port | 3000 |

---

## Source Reference

CLI commands are defined in:
```
C:\Users\USER\projects\claude-flow\v3\@claude-flow\cli\src\commands\
```

Key source files:
- `swarm.ts` - Swarm commands
- `hive-mind.ts` - Hive-mind commands
- `memory.ts` - Memory commands
- `hooks.ts` - Hooks commands
- `agent.ts` - Agent commands
- `analyze.ts` - Analyze commands
- `route.ts` - Route commands
- `config.ts` - Config commands
- `claims.ts` - Claims commands
- `doctor.ts` - Doctor commands
- `completions.ts` - Shell completions
