# Background Workers Reference

Complete guide to the 12 background workers in claude-flow v3.

> **CLI Reference**: For CLI commands including `hooks worker-dispatch`, see [CLI_REFERENCE.md](CLI_REFERENCE.md).
>
> **MCP Reference**: For MCP worker tools, see [MCP_REFERENCE.md](MCP_REFERENCE.md).

---

## Overview

Background workers run automated analysis and optimization tasks.

**Performance Targets**:
- Trigger detection: <5ms
- Worker spawn: <50ms
- Max concurrent: 10

---

## Available Workers

| Worker | Purpose | Duration | Priority |
|--------|---------|----------|----------|
| `ultralearn` | Deep knowledge acquisition | 60s | Normal |
| `optimize` | Performance optimization | 30s | High |
| `consolidate` | Memory cleanup | 20s | Low |
| `predict` | Predictive preloading | 15s | Normal |
| `audit` | Security vulnerability scanning | 45s | **CRITICAL** |
| `map` | Codebase architecture mapping | 30s | Normal |
| `preload` | Resource cache warming | 10s | Low |
| `deepdive` | Deep code analysis | 60s | Normal |
| `document` | Auto-documentation | 45s | Normal |
| `refactor` | Code refactoring suggestions | 30s | Normal |
| `benchmark` | Performance benchmarking | 60s | Normal |
| `testgaps` | Test coverage analysis | 30s | Normal |

---

## Worker Details

### ultralearn

Deep knowledge acquisition from codebase and context.

```javascript
mcp__claude-flow__worker_dispatch {
  trigger: "ultralearn",
  context: "REST API authentication patterns",
  priority: "normal"
}
```

**Use when**: Starting a new project or exploring unfamiliar domain.

---

### optimize

Performance optimization analysis and suggestions.

```javascript
mcp__claude-flow__worker_dispatch {
  trigger: "optimize",
  context: "src/api/handlers",
  priority: "high"
}
```

**Use when**: Performance issues detected or before production deployment.

---

### consolidate

Memory cleanup and garbage collection.

```javascript
mcp__claude-flow__worker_dispatch {
  trigger: "consolidate",
  context: "memory cleanup",
  priority: "low"
}
```

**Use when**: Memory usage is high or after long sessions.

---

### predict

Predictive preloading of likely-needed resources.

```javascript
mcp__claude-flow__worker_dispatch {
  trigger: "predict",
  context: "user-authentication-flow",
  priority: "normal"
}
```

**Use when**: Starting a task with predictable resource needs.

---

### audit

Security vulnerability scanning.

```javascript
mcp__claude-flow__worker_dispatch {
  trigger: "audit",
  context: "src/auth",
  priority: "critical"
}
```

**Use when**: Working on security-sensitive code, before releases.

**Priority**: Always **CRITICAL** - security issues need immediate attention.

---

### map

Codebase architecture mapping and visualization.

```javascript
mcp__claude-flow__worker_dispatch {
  trigger: "map",
  context: "src/",
  priority: "normal"
}
```

**Use when**: Understanding new codebase or documenting architecture.

---

### preload

Resource cache warming for faster access.

```javascript
mcp__claude-flow__worker_dispatch {
  trigger: "preload",
  context: "test fixtures",
  priority: "low"
}
```

**Use when**: Before running tests or builds.

---

### deepdive

Deep code analysis for complex understanding.

```javascript
mcp__claude-flow__worker_dispatch {
  trigger: "deepdive",
  context: "src/core/engine.ts",
  priority: "normal"
}
```

**Use when**: Debugging complex issues or understanding intricate code.

---

### document

Auto-documentation generation.

```javascript
mcp__claude-flow__worker_dispatch {
  trigger: "document",
  context: "src/api",
  priority: "normal"
}
```

**Use when**: After implementing features, before releases.

---

### refactor

Code refactoring suggestions.

```javascript
mcp__claude-flow__worker_dispatch {
  trigger: "refactor",
  context: "src/legacy",
  priority: "normal"
}
```

**Use when**: Technical debt cleanup, code quality improvement.

---

### benchmark

Performance benchmarking.

```javascript
mcp__claude-flow__worker_dispatch {
  trigger: "benchmark",
  context: "api endpoints",
  priority: "normal"
}
```

**Use when**: Before/after optimization, performance regression testing.

---

### testgaps

Test coverage analysis.

```javascript
mcp__claude-flow__worker_dispatch {
  trigger: "testgaps",
  context: "src/",
  priority: "normal"
}
```

**Use when**: Improving test coverage, identifying untested code.

---

## Worker Management

### List Available Triggers

```javascript
mcp__claude-flow__worker_triggers {}
// Returns: { triggers: [{ name, description, priority, estimatedDuration, capabilities }] }
```

### Check Status

```javascript
mcp__claude-flow__worker_status {
  workerId: "worker-xxx"
}
// Returns: { found, workerId, trigger, status, progress, phase, startedAt, completedAt, result, error }
```

### Cancel Worker

```javascript
mcp__claude-flow__worker_cancel {
  workerId: "worker-xxx"
}
// Returns: { cancelled, workerId, reason }
```

### Auto-Detect from Text

```javascript
mcp__claude-flow__worker_detect {
  text: "analyze security vulnerabilities in the auth module"
}
// Returns: { detected: [...], confidence, recommended }
```

### Get Worker Results

```javascript
mcp__claude-flow__worker_results {
  sessionId: "session-xxx",
  trigger: "audit",           // Optional filter
  status: "completed",        // Optional: pending, running, completed, failed, cancelled
  limit: 10
}
```

### Get Worker Stats

```javascript
mcp__claude-flow__worker_stats {}
// Returns: { total, pending, running, completed, failed, cancelled, byTrigger }
```

---

## Workflow Patterns

### Pattern 1: New Project Setup

```javascript
// 1. Map the codebase first
mcp__claude-flow__worker_dispatch {
  trigger: "map",
  context: "src/",
  priority: "high"
}

// 2. Deep learn the domain
mcp__claude-flow__worker_dispatch {
  trigger: "ultralearn",
  context: "project domain and patterns"
}

// 3. Check test coverage
mcp__claude-flow__worker_dispatch {
  trigger: "testgaps",
  context: "src/"
}
```

### Pattern 2: Pre-Release Checklist

```javascript
// 1. Security audit
mcp__claude-flow__worker_dispatch {
  trigger: "audit",
  context: "full codebase",
  priority: "critical"
}

// 2. Performance benchmark
mcp__claude-flow__worker_dispatch {
  trigger: "benchmark",
  context: "api endpoints"
}

// 3. Auto-documentation
mcp__claude-flow__worker_dispatch {
  trigger: "document",
  context: "src/"
}
```

### Pattern 3: Technical Debt Sprint

```javascript
// 1. Map current architecture
mcp__claude-flow__worker_dispatch {
  trigger: "map",
  context: "src/"
}

// 2. Get refactoring suggestions
mcp__claude-flow__worker_dispatch {
  trigger: "refactor",
  context: "src/legacy"
}

// 3. Identify test gaps
mcp__claude-flow__worker_dispatch {
  trigger: "testgaps",
  context: "src/"
}

// 4. Optimize hotspots
mcp__claude-flow__worker_dispatch {
  trigger: "optimize",
  context: "performance hotspots"
}
```

### Pattern 4: Complex Investigation

```javascript
// 1. Deep dive into problematic code
mcp__claude-flow__worker_dispatch {
  trigger: "deepdive",
  context: "src/core/problematic-module.ts"
}

// 2. Map dependencies
mcp__claude-flow__worker_dispatch {
  trigger: "map",
  context: "src/core/"
}

// 3. Benchmark current performance
mcp__claude-flow__worker_dispatch {
  trigger: "benchmark",
  context: "core module"
}
```

---

## Priority Guidelines

| Priority | Use For | Examples |
|----------|---------|----------|
| **critical** | Security, blocking issues | `audit` |
| **high** | Important, time-sensitive | `optimize` before deploy |
| **normal** | Standard tasks | Most workers |
| **low** | Background, non-urgent | `consolidate`, `preload` |

---

## Concurrent Execution

**Max concurrent workers**: 10

When dispatching multiple workers:

```javascript
// All dispatched in parallel
[Single Message]:
  mcp__claude-flow__worker_dispatch { trigger: "map", context: "src/" }
  mcp__claude-flow__worker_dispatch { trigger: "testgaps", context: "src/" }
  mcp__claude-flow__worker_dispatch { trigger: "audit", context: "security scan", priority: "critical" }
  mcp__claude-flow__worker_dispatch { trigger: "document", context: "src/" }
```

Workers will execute concurrently up to the limit, with higher priority workers taking precedence.
