# Troubleshooting Guide

Common issues and solutions for claude-flow v3.

> **CLI Reference**: For complete CLI command documentation, see [CLI_REFERENCE.md](CLI_REFERENCE.md).
>
> **MCP Reference**: For all MCP tools, see [MCP_REFERENCE.md](MCP_REFERENCE.md).

---

## Quick Reference

| Issue | Solution |
|-------|----------|
| Agents not coordinating | Check topology matches task type |
| Memory not persisting | Use hive-mind, not swarm |
| Slow searches | Initialize HNSW: `embeddings_init` |
| Session lost | Use `hooks session-restore --session-id latest` |
| Hooks not firing | Run `npx claude-flow@alpha hooks init --force` |
| Intelligence not learning | Check `hooks_intelligence_stats` |
| Model routing failing | Check `hooks_model-stats` |
| Workers not spawning | Check `hooks_worker-status` |
| Browser automation fails | Ensure Playwright installed |
| Claims conflicts | Use `claims_board` to visualize |

---

## Detailed Solutions

### Agents Not Coordinating

**Symptoms**: Agents working independently, not sharing state.

**Causes**:
1. Wrong topology for task type
2. Memory not configured
3. Consensus not enabled

**Solutions**:

```javascript
// 1. Check current topology
mcp__claude-flow__coordination_topology { action: "get" }

// 2. Switch to mesh for better coordination
mcp__claude-flow__coordination_topology {
  action: "set",
  type: "mesh"
}

// 3. Enable memory sharing
mcp__claude-flow__hive-mind_memory {
  action: "set",
  key: "shared_context",
  value: { ... }
}
```

---

### Memory Not Persisting

**Symptoms**: Data lost between sessions.

**Causes**:
1. Using swarm instead of hive-mind
2. Session not saved
3. Memory not configured for persistence

**Solutions**:

```bash
# Use hive-mind for persistence
npx claude-flow@alpha init wizard

# Or save session explicitly
npx claude-flow@alpha hooks session-end --save-state true
```

```javascript
// MCP: Save session
mcp__claude-flow__session_save {
  name: "my-project",
  includeMemory: true,
  includeTasks: true
}
```

---

### Slow Searches

**Symptoms**: Memory/pattern searches taking too long.

**Causes**:
1. HNSW not initialized
2. Too many entries without indexing
3. Embeddings not enabled

**Solutions**:

```javascript
// 1. Initialize embeddings with HNSW
mcp__claude-flow__embeddings_init {
  model: "all-MiniLM-L6-v2",
  hyperbolic: true,
  cacheSize: 256
}

// 2. Enable HNSW in intelligence layer
mcp__claude-flow__hooks_intelligence {
  enableHnsw: true
}

// 3. Check status
mcp__claude-flow__embeddings_status {}
```

---

### Session Lost

**Symptoms**: Can't resume previous work.

**Solutions**:

```bash
# Restore latest session
npx claude-flow@alpha hooks session-restore --session-id "latest"

# List available sessions
npx claude-flow@alpha session list
```

```javascript
// MCP: Restore session
mcp__claude-flow__hooks_session-restore {
  sessionId: "latest",
  restoreAgents: true,
  restoreTasks: true
}

// Or list sessions first
mcp__claude-flow__session_list {}
```

---

### Hooks Not Firing

**Symptoms**: Pre/post hooks not executing.

**Solutions**:

```bash
# Reinitialize hooks
npx claude-flow@alpha hooks init --force --template standard

# Verify hooks are registered
npx claude-flow@alpha hooks list
```

```javascript
// MCP: List hooks
mcp__claude-flow__hooks_list {}

// Initialize if empty
mcp__claude-flow__hooks_init {
  template: "standard",
  force: true
}
```

---

### Intelligence Not Learning

**Symptoms**: No improvement from SONA trajectories.

**Causes**:
1. Trajectories not ended properly
2. Learning not triggered
3. EWC++ not consolidating

**Solutions**:

```javascript
// 1. Check intelligence status
mcp__claude-flow__hooks_intelligence_stats { detailed: true }

// 2. Force learning cycle
mcp__claude-flow__hooks_intelligence_learn {
  consolidate: true
}

// 3. Ensure trajectories are complete
mcp__claude-flow__hooks_intelligence_trajectory-end {
  trajectoryId: "traj-xxx",
  success: true
}
```

---

### Model Routing Failing

**Symptoms**: Tasks not routed to appropriate models.

**Solutions**:

```javascript
// 1. Check routing stats
mcp__claude-flow__hooks_model-stats {}

// 2. Manual route to verify
mcp__claude-flow__hooks_model-route {
  task: "your task description"
}

// 3. Record correct outcome to improve
mcp__claude-flow__hooks_model-outcome {
  task: "your task",
  model: "sonnet",
  outcome: "success"
}
```

---

### Workers Not Spawning

**Symptoms**: Background workers not starting.

**Solutions**:

```javascript
// 1. Check worker status
mcp__claude-flow__hooks_worker-status {
  includeCompleted: true
}

// 2. List available workers
mcp__claude-flow__hooks_worker-list {}

// 3. Dispatch with explicit priority
mcp__claude-flow__hooks_worker-dispatch {
  trigger: "map",
  priority: "high",
  background: false  // Wait for completion
}
```

---

### Browser Automation Fails

**Symptoms**: Browser actions not working.

**Solutions**:

```bash
# Install Playwright
npm install playwright
npx playwright install
```

```javascript
// Check browser session
mcp__claude-flow__browser_session-list {}

// Open new session
mcp__claude-flow__browser_open {
  url: "https://example.com",
  waitUntil: "networkidle"
}

// Take screenshot to debug
mcp__claude-flow__browser_screenshot {
  fullPage: true
}
```

---

### Claims Conflicts

**Symptoms**: Multiple agents claiming same issue, handoff issues.

**Solutions**:

```javascript
// 1. View claims board
mcp__claude-flow__claims_board {}

// 2. Check for conflicts
mcp__claude-flow__claims_list { status: "all" }

// 3. Release conflicting claim
mcp__claude-flow__claims_release {
  issueId: "issue-xxx",
  claimant: "agent-123",
  reason: "Conflict resolution"
}

// 4. Rebalance if needed
mcp__claude-flow__claims_rebalance {
  dryRun: true,
  targetUtilization: 0.7
}
```

---

### Swarm Not Starting

**Symptoms**: Swarm initialization fails.

**Solutions**:

```javascript
// 1. Check system status
mcp__claude-flow__system_status { verbose: true }

// 2. Check health
mcp__claude-flow__system_health { deep: true }

// 3. Reset if corrupted
mcp__claude-flow__system_reset {
  component: "swarm",
  confirm: true
}

// 4. Reinitialize
mcp__claude-flow__swarm_init {
  topology: "mesh",
  maxAgents: 5
}
```

---

### Performance Issues

**Symptoms**: Slow operations, high memory usage.

**Solutions**:

```javascript
// 1. Run performance report
mcp__claude-flow__performance_report {
  timeRange: "1h",
  format: "detailed"
}

// 2. Detect bottlenecks
mcp__claude-flow__performance_bottleneck {
  deep: true
}

// 3. Apply optimizations
mcp__claude-flow__performance_optimize {
  target: "all"
}

// 4. Run consolidate worker
mcp__claude-flow__hooks_worker-dispatch {
  trigger: "consolidate",
  priority: "high"
}
```

---

### Configuration Issues

**Symptoms**: Settings not applying, unexpected behavior.

**Solutions**:

```javascript
// 1. Export current config
mcp__claude-flow__config_export { scope: "project" }

// 2. List all settings
mcp__claude-flow__config_list {
  scope: "project",
  includeDefaults: true
}

// 3. Reset specific key
mcp__claude-flow__config_reset {
  key: "swarm.maxAgents",
  scope: "project"
}

// 4. Or reset all
mcp__claude-flow__config_reset {
  scope: "project"
}
```

---

## Diagnostic Commands

### Full System Diagnostic

```javascript
// System status
mcp__claude-flow__system_status { verbose: true }

// Health check
mcp__claude-flow__system_health { deep: true, fix: true }

// Metrics
mcp__claude-flow__system_metrics { category: "all" }

// Intelligence status
mcp__claude-flow__hooks_intelligence { showStatus: true }

// Embeddings status
mcp__claude-flow__embeddings_status {}
```

### CLI Diagnostic

```bash
# Check version
npx claude-flow@alpha --version

# System info
npx claude-flow@alpha system info

# Health check
npx claude-flow@alpha system health --deep

# List all resources
npx claude-flow@alpha swarm status
npx claude-flow@alpha hive-mind status
npx claude-flow@alpha memory list
```

---

## Getting Help

1. **Check logs**: Look for error messages in terminal output
2. **Enable verbose**: Add `--verbose` to CLI commands
3. **Check status**: Use `system_status` with `verbose: true`
4. **Reset and retry**: Use `system_reset` for clean state
5. **Documentation**: https://github.com/ruvnet/claude-flow
