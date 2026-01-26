# MCP Tools Reference

Complete reference for claude-flow v3 MCP tools.

> **Note**: Tools are accessed via MCP with prefix `mcp__claude-flow__` (or `mcp__claude-flow_alpha__` for alpha).
> Slashes in tool names become underscores (e.g., `agent/spawn` → `agent_spawn`).

---

## 📊 Quick Stats

- **Total MCP Tools**: 230+
- **Core Tools**: 40+ (most commonly used)
- **Advanced Tools**: 190+ (specialized features)
- **Categories**: 16 tool categories

## 📑 Table of Contents

### Core Tools (Most Used - 40+ tools)
- [V2 Compatibility Tools](#v2-compatibility-tools-recommended) - Swarm, Agent, Memory basics
- [Swarm Management](#swarm-management) - init, status, shutdown, health
- [Agent Management](#agent-management) - spawn, list, terminate, status
- [Task Management](#task-management) - create, list, update, complete
- [Memory Tools](#memory-tools) - store, search, retrieve, list
- [Hooks System](#hooks-system) - pre/post-edit, worker dispatch, intelligence
- [Session Management](#session-management) - save, restore, list
- [System Tools](#system-tools) - status, metrics, health

### Advanced Tools (190+ tools)
- [Browser Automation](#browser-automation-tools) - 20+ Puppeteer/Playwright tools
- [Federation](#federation-tools) - Multi-agent coordination (8 tools)
- [DAA (Distributed Adaptive Agents)](#daa-distributed-adaptive-agents-tools) - Self-learning agents (8 tools)
- [Terminal Automation](#terminal-automation-tools) - Shell execution (5 tools)
- [Progress Tracking](#progress-tracking-tools) - V3 implementation tracking (4 tools)
- [Transfer Store](#transfer-store-tools-ipfs-based) - IPFS pattern marketplace (10 tools)
- [Coordination](#coordination-tools) - Topology, load balancing, consensus (7 tools)
- [Claims System](#claims-system-tools) - Task coordination & handoffs (12 tools)

### Reference
- [Notes](#notes) - Tool naming, parameters, locations

💡 **Tip**: Use Ctrl+F (Cmd+F) to search for specific tools by name or category.

---

## V2 Compatibility Tools (Recommended)

These tools use underscore naming for backward compatibility.

### Swarm Management

```javascript
// Initialize swarm
mcp__claude-flow__swarm_init {
  topology: "mesh",           // mesh, hierarchical, hierarchical-mesh, ring, star, adaptive
  maxAgents: 5,               // 1-100, default: 5
  strategy: "balanced"        // balanced, specialized, adaptive
}

// Get swarm status
mcp__claude-flow__swarm_status {
  detailed: false,            // Include detailed agent info
  verbose: false              // Alias for detailed
}

// Shutdown swarm
mcp__claude-flow__swarm_shutdown {
  graceful: true,             // Wait for agents to finish
  timeout: 30000              // Timeout in milliseconds
}

// Get swarm health
mcp__claude-flow__swarm_health {
  deep: false,                // Deep health check
  timeout: 5000               // Timeout in milliseconds
}
```

### Agent Management

```javascript
// Spawn agent
mcp__claude-flow__agent_spawn {
  agentType: "coder",         // coder, researcher, tester, architect, etc.
  id: "agent-123",            // Optional custom ID
  config: {},                 // Agent-specific config
  priority: "normal"          // low, normal, high, critical
}

// List agents
mcp__claude-flow__agent_list {
  status: "active",           // active, idle, terminated, all
  agentType: "coder",         // Filter by type
  limit: 100                  // Max results
}

// Get agent status (use instead of agent_metrics)
mcp__claude-flow__agent_status {
  agentId: "agent-123",       // Required agent ID
  includeMetrics: true,       // Include performance metrics
  includeHistory: false       // Include task history
}

// Get agent health
mcp__claude-flow__agent_health {
  agentId: "agent-123",       // Optional - check all if omitted
  deep: false                 // Deep health check
}

// Update agent configuration
mcp__claude-flow__agent_update {
  agentId: "agent-123",       // Required agent ID
  config: {},                 // New configuration
  priority: "high"            // Updated priority
}
```

### Task Orchestration

```javascript
// Create task
mcp__claude-flow__task_create {
  title: "Build REST API",    // Task title
  description: "...",         // Task description
  type: "feature",            // feature, bug, refactor, etc.
  priority: "high",           // low, normal, high, critical
  assignTo: "agent-123",      // Optional agent assignment
  dependencies: []            // Task IDs this depends on
}

// Get task status
mcp__claude-flow__task_status {
  taskId: "task-123"          // Required task ID
}

// List tasks
mcp__claude-flow__task_list {
  status: "pending",          // pending, in_progress, completed, all
  priority: "high",           // Filter by priority
  limit: 50                   // Max results
}

// Complete task
mcp__claude-flow__task_complete {
  taskId: "task-123",         // Required task ID
  result: {}                  // Task result data
}

// Update task
mcp__claude-flow__task_update {
  taskId: "task-123",         // Required task ID
  status: "in_progress",      // New status
  progress: 50                // Progress percentage
}

// Cancel task
mcp__claude-flow__task_cancel {
  taskId: "task-123",         // Required task ID
  reason: "No longer needed"  // Cancellation reason
}

```

### Memory

```javascript
// Memory usage/operations
mcp__claude-flow__memory_usage {
  namespace: "default",
  detailed: false
}
```

### Neural/Intelligence

```javascript
// Neural status
mcp__claude-flow__neural_status {
  component: "all"            // all, embeddings, patterns, models
}

// Train neural patterns
mcp__claude-flow__neural_train {
  pattern: "code_review",
  data: {},
  options: {}
}

// Get neural patterns
mcp__claude-flow__neural_patterns {
  category: "all",
  limit: 50
}

// Make predictions
mcp__claude-flow__neural_predict {
  input: "task description",
  model: "default"
}

// Compress neural model
mcp__claude-flow__neural_compress {
  targetSize: "small"         // small, medium, large
}

// Optimize neural performance
mcp__claude-flow__neural_optimize {
  target: "latency"           // latency, accuracy, memory
}
```

### Benchmarking & Features

```javascript
// Run benchmarks
mcp__claude-flow__benchmark_run {
  suite: "all",               // all, memory, neural, swarm, io
  iterations: 10,
  warmup: true
}

// Detect features
mcp__claude-flow__features_detect {}
```

### Embeddings

```javascript
// Initialize embeddings system
mcp__claude-flow__embeddings_init {
  model: "default",           // default, openai, local
  dimensions: 384
}

// Generate embeddings
mcp__claude-flow__embeddings_generate {
  text: "user authentication system",
  model: "default"
}

// Compare similarity between texts
mcp__claude-flow__embeddings_compare {
  text1: "user authentication",
  text2: "login system",
  metric: "cosine"            // cosine, euclidean, poincare
}

// Search embeddings
mcp__claude-flow__embeddings_search {
  query: "authentication",
  namespace: "code",
  limit: 10,
  threshold: 0.7
}

// Neural substrate operations
mcp__claude-flow__embeddings_neural {
  action: "status"            // status, init, drift, consolidate, adapt
}

// Hyperbolic embedding operations
mcp__claude-flow__embeddings_hyperbolic {
  action: "convert",          // status, convert, distance
  embedding: [0.1, 0.2, ...]  // For convert/distance actions
}

// Get embeddings system status
mcp__claude-flow__embeddings_status {}
```

---

## V3 Tools (Slash Format)

These are the native V3 tools using slash naming.

### Agent Tools

```javascript
// Spawn agent (V3)
mcp__claude-flow__agent_spawn {  // or agent/spawn
  agentType: "coder",
  id: "optional-id",
  config: {},
  priority: "normal",
  metadata: {}
}

// List agents (V3)
mcp__claude-flow__agent_list {   // or agent/list
  status: "active",
  agentType: "coder",
  limit: 100,
  offset: 0
}

// Terminate agent (V3)
mcp__claude-flow__agent_terminate {  // or agent/terminate
  agentId: "agent-123",
  graceful: true,
  reason: "Task completed"
}

// Agent status (V3)
mcp__claude-flow__agent_status {  // or agent/status
  agentId: "agent-123",
  includeMetrics: false,
  includeHistory: false
}
```

### Swarm Tools

```javascript
// Initialize swarm (V3)
mcp__claude-flow__swarm_init {   // or swarm/init
  topology: "mesh",
  maxAgents: 15,
  config: {
    loadBalancing: true,
    autoScaling: false
  }
}

// Swarm status (V3)
mcp__claude-flow__swarm_status { // or swarm/status
  includeAgents: true,
  includeMetrics: false,
  includeTopology: false
}

// Shutdown swarm (V3)
mcp__claude-flow__swarm_shutdown {  // or swarm/shutdown
  graceful: true,              // Wait for agents to finish
  timeout: 30000               // Timeout in milliseconds
}

// Get swarm health (V3)
mcp__claude-flow__swarm_health {   // or swarm/health
  deep: false,                 // Deep health check
  timeout: 5000                // Timeout in milliseconds
}
```

### Task Tools

```javascript
// Create task
mcp__claude-flow__tasks_create {
  title: "Implement feature",
  description: "...",
  type: "feature",
  priority: "high",
  assignTo: "agent-123",
  dependencies: []
}

// List tasks
mcp__claude-flow__tasks_list {
  status: "pending",
  priority: "high",
  assignedTo: "agent-123",
  limit: 50
}

// Task status
mcp__claude-flow__tasks_status {
  taskId: "task-123",
  includeHistory: false
}

// Cancel task
mcp__claude-flow__tasks_cancel {
  taskId: "task-123",
  reason: "No longer needed"
}

// Assign task
mcp__claude-flow__tasks_assign {
  taskId: "task-123",
  agentId: "agent-456"
}

// Update task
mcp__claude-flow__tasks_update {
  taskId: "task-123",
  status: "in_progress",
  progress: 50
}

// Task dependencies
mcp__claude-flow__tasks_dependencies {
  taskId: "task-123",
  action: "add",
  dependsOn: ["task-100"]
}

// Task results
mcp__claude-flow__tasks_results {
  taskId: "task-123"
}
```

### Memory Tools

```javascript
// Store memory
mcp__claude-flow__memory_store {
  key: "api_design",
  content: { pattern: "REST" },
  category: "backend",
  ttl: 3600,
  tags: ["design", "api"]
}

// Search memory
mcp__claude-flow__memory_search {
  query: "authentication",
  category: "backend",
  limit: 10,
  threshold: 0.7,
  semantic: true
}

// List memory
mcp__claude-flow__memory_list {
  category: "backend",
  prefix: "api_",
  limit: 100
}
```

### Session Tools

```javascript
// Save session
mcp__claude-flow__session_save {
  name: "api-development",
  description: "REST API implementation",
  includeAgents: true,
  includeTasks: true,
  includeMemory: true
}

// Restore session
mcp__claude-flow__session_restore {
  sessionId: "session-123"
}

// List sessions
mcp__claude-flow__session_list {
  limit: 20,
  includeMetadata: true
}
```

### Config Tools

```javascript
// Get config value
mcp__claude-flow__config_get {
  key: "swarm.maxAgents",     // Config key path
  namespace: "default"        // Config namespace
}

// Set config value
mcp__claude-flow__config_set {
  key: "swarm.maxAgents",     // Config key path
  value: 10,                  // New value
  namespace: "default"        // Config namespace
}

// List config keys
mcp__claude-flow__config_list {
  namespace: "default",       // Config namespace
  prefix: "swarm."            // Filter by prefix
}

// Reset config to defaults
mcp__claude-flow__config_reset {
  namespace: "default",       // Config namespace
  key: "swarm.maxAgents"      // Optional specific key
}

// Export config to file
mcp__claude-flow__config_export {
  path: "./claude-flow.json", // Export path
  namespace: "default",       // Config namespace
  format: "json"              // json, yaml
}

// Import config from file
mcp__claude-flow__config_import {
  path: "./claude-flow.json", // Import path
  namespace: "default",       // Config namespace
  merge: true                 // Merge with existing
}
```

### System Tools

```javascript
// System status
mcp__claude-flow__system_status {
  includeAgents: true,
  includeTasks: true,
  includeMemory: false,
  includeConnections: false
}

// System metrics
mcp__claude-flow__system_metrics {
  timeRange: "1h",            // 1h, 6h, 24h, 7d
  includeHistogram: false,
  components: ["all"]         // agents, tasks, memory, swarm, all
}

// System health
mcp__claude-flow__system_health {
  deep: false,
  timeout: 5000,
  components: []
}

// System info
mcp__claude-flow__system_info {
  includeEnv: false,
  includeVersions: true,
  includeCapabilities: true
}
```

### Hooks Tools

```javascript
// Pre-edit hook
mcp__claude-flow__hooks_pre-edit {
  filePath: "src/app.ts",
  content: "...",
  operation: "modify"
}

// Post-edit hook
mcp__claude-flow__hooks_post-edit {
  filePath: "src/app.ts",
  oldContent: "...",
  newContent: "...",
  success: true
}

// Pre-command hook
mcp__claude-flow__hooks_pre-command {
  command: "npm test",
  args: [],
  workingDir: "/project"
}

// Post-command hook
mcp__claude-flow__hooks_post-command {
  command: "npm test",
  exitCode: 0,
  stdout: "...",
  stderr: ""
}

// Route hook
mcp__claude-flow__hooks_route {
  task: "Review code",
  context: {}
}

// Explain hook
mcp__claude-flow__hooks_explain {
  topic: "authentication",
  context: {}
}

// Pretrain hook
mcp__claude-flow__hooks_pretrain {
  pattern: "code_style",
  examples: [],
  options: {}
}

// Metrics hook
mcp__claude-flow__hooks_metrics {
  category: "all",
  timeRange: "1h"
}

// List hooks
mcp__claude-flow__hooks_list {
  type: "all",
  enabled: true
}
```

### Worker Tools

```javascript
// Dispatch worker
mcp__claude-flow__worker_dispatch {
  trigger: "map",             // map, ultralearn, optimize, audit, testgaps, etc.
  context: "target path or description",
  priority: "normal"
}

// Worker status
mcp__claude-flow__worker_status {
  workerId: "worker-123"
}

// Cancel worker
mcp__claude-flow__worker_cancel {
  workerId: "worker-123",
  reason: "No longer needed"
}

// Detect triggers (auto-detect appropriate worker for context)
mcp__claude-flow__hooks_worker-detect {
  context: {},                // Context for trigger detection
  threshold: 0.7              // Confidence threshold
}

// List workers
mcp__claude-flow__hooks_worker-list {}
```


---

## Available Agent Types

```
Core Development:
  coder, reviewer, tester, planner, researcher

Swarm Coordination:
  hierarchical-coordinator, mesh-coordinator, adaptive-coordinator
  collective-intelligence-coordinator, swarm-memory-manager

Consensus & Distributed:
  byzantine-coordinator, raft-manager, gossip-coordinator
  consensus-builder, crdt-synchronizer, quorum-manager, security-manager

Performance & Optimization:
  perf-analyzer, performance-benchmarker, task-orchestrator
  memory-coordinator, smart-agent

SPARC Methodology:
  sparc-coord, sparc-coder, specification, pseudocode
  architecture, refinement

Specialized Development:
  backend-dev, frontend-dev, mobile-dev, ml-developer
  cicd-engineer, api-docs, system-architect, code-analyzer

V3 Specialized:
  queen-coordinator, security-architect, security-auditor
  memory-specialist, swarm-specialist, integration-architect
  performance-engineer, core-architect, test-architect, project-coordinator
```

---

## Worker Types

```
ultralearn    - Deep knowledge acquisition
optimize      - Performance optimization
consolidate   - Memory cleanup
predict       - Predictive preloading
audit         - Security vulnerability scanning (CRITICAL)
map           - Codebase architecture mapping
preload       - Resource cache warming
deepdive      - Deep code analysis
document      - Auto-documentation
refactor      - Refactoring suggestions
benchmark     - Performance benchmarking
testgaps      - Test coverage analysis
```

---

## CLI Tools (Additional)

These tools are defined in `@claude-flow/cli/src/mcp-tools/` and provide extended functionality.

### GitHub Tools (Local State)

> **Note**: These provide LOCAL STATE MANAGEMENT for workflow coordination.
> For actual GitHub API calls, use `gh` CLI or GitHub MCP server.

```javascript
// Analyze repository (local state)
mcp__claude-flow__github_repo_analyze {
  owner: "owner",
  repo: "repo",
  branch: "main",
  deep: false
}

// Manage pull requests (local state)
mcp__claude-flow__github_pr_manage {
  action: "list",           // list, create, review, merge, close
  owner: "owner",
  repo: "repo",
  prNumber: 123,
  title: "PR title",
  branch: "feature",
  baseBranch: "main"
}

// Track issues (local state)
mcp__claude-flow__github_issue_track {
  action: "list",           // list, create, update, close, assign
  owner: "owner",
  repo: "repo",
  issueNumber: 456,
  title: "Issue title",
  labels: ["bug"],
  assignees: ["user1"]
}

// Manage workflows (local state)
mcp__claude-flow__github_workflow {
  action: "list",           // list, trigger, status, cancel
  owner: "owner",
  repo: "repo",
  workflowId: "ci.yml",
  ref: "main"
}

// Get metrics (simulated)
mcp__claude-flow__github_metrics {
  owner: "owner",
  repo: "repo",
  metric: "all",            // all, commits, contributors, traffic, releases
  timeRange: "week"
}
```

### Analyze Tools (Diff Analysis)

```javascript
// Full diff analysis
mcp__claude-flow__analyze_diff {
  diff: "git diff content",
  context: "PR review"
}

// Quick risk assessment
mcp__claude-flow__analyze_diff-risk {
  diff: "git diff content"
}

// Classify change type
mcp__claude-flow__analyze_diff-classify {
  diff: "git diff content"
}

// Suggest reviewers
mcp__claude-flow__analyze_diff-reviewers {
  diff: "git diff content",
  team: ["user1", "user2"]
}

// File risk assessment
mcp__claude-flow__analyze_file-risk {
  filePath: "src/core/engine.ts"
}

// Quick diff statistics
mcp__claude-flow__analyze_diff-stats {
  diff: "git diff content"
}
```

### Claims Tools

```javascript
// Claim an issue
mcp__claude-flow__claims_claim {
  issueId: "issue-123",
  claimant: "agent:coder-1:coder",   // or "human:user-1:Alice"
  context: "Working on authentication"
}

// Release a claim
mcp__claude-flow__claims_release {
  issueId: "issue-123",
  claimant: "agent:coder-1:coder",
  reason: "Task completed"
}

// Request handoff
mcp__claude-flow__claims_handoff {
  issueId: "issue-123",
  from: "agent:coder-1:coder",
  to: "human:user-1:Alice",
  reason: "Needs human review",
  progress: 80
}

// Accept handoff
mcp__claude-flow__claims_accept-handoff {
  issueId: "issue-123",
  claimant: "human:user-1:Alice"
}

// Update status
mcp__claude-flow__claims_status {
  issueId: "issue-123",
  status: "active",         // active, paused, blocked, review-requested, completed
  note: "Making progress",
  progress: 50
}

// List claims
mcp__claude-flow__claims_list {
  status: "active",         // active, paused, blocked, stealable, completed, all
  claimant: "coder",
  agentType: "coder"
}

// Mark as stealable
mcp__claude-flow__claims_mark-stealable {
  issueId: "issue-123",
  reason: "overloaded",     // overloaded, stale, blocked-timeout, voluntary
  preferredTypes: ["tester"],
  context: "50% complete, needs testing"
}

// Steal an issue
mcp__claude-flow__claims_steal {
  issueId: "issue-123",
  stealer: "agent:tester-1:tester"
}

// List stealable issues
mcp__claude-flow__claims_stealable {
  agentType: "tester"
}

// Get agent load
mcp__claude-flow__claims_load {
  agentId: "coder-1",
  agentType: "coder"
}

// Get claims board
mcp__claude-flow__claims_board {}

// Rebalance loads
mcp__claude-flow__claims_rebalance {
  dryRun: true,
  targetUtilization: 0.7
}
```

### Security Tools (AIDefence)

```javascript
// Scan for threats
mcp__claude-flow__aidefence_scan {
  input: "text to scan",
  quick: false              // Quick scan mode
}

// Deep analysis
mcp__claude-flow__aidefence_analyze {
  input: "text to analyze",
  searchSimilar: true,
  k: 5
}

// Get statistics
mcp__claude-flow__aidefence_stats {}

// Learn from feedback
mcp__claude-flow__aidefence_learn {
  input: "original input",
  wasAccurate: true,
  verdict: "confirmed threat",
  threatType: "prompt-injection",
  mitigationStrategy: "block",   // block, sanitize, warn, log, escalate, transform, redirect
  mitigationSuccess: true
}

// Quick safety check
mcp__claude-flow__aidefence_is_safe {
  input: "text to check"
}

// Check for PII
mcp__claude-flow__aidefence_has_pii {
  input: "text with potential PII"
}
```

---

## Notes

1. **Tool Name Conversion**: When accessing via MCP, slashes become underscores
   - `agent/spawn` → `mcp__claude-flow__agent_spawn`
   - `claims/claim` → `mcp__claude-flow__claims_claim`
   - `analyze/diff` → `mcp__claude-flow__analyze_diff`

2. **V2 vs V3**: V2 tools are deprecated but maintained for compatibility
   - Prefer V3 tools for new development

3. **Server Prefix**: The prefix depends on how the MCP server is registered
   - `claude-flow` → `mcp__claude-flow__`
   - `claude-flow_alpha` → `mcp__claude-flow_alpha__`

4. **Parameters**: All parameters shown are the actual parameters from source code

5. **GitHub Tools**: Provide LOCAL STATE only for workflow coordination
   - Use `gh` CLI or GitHub MCP server for actual API calls

6. **Tool Locations**: Tools are defined in two locations:
   - `v3/mcp/tools/` - Core MCP tools
   - `v3/@claude-flow/cli/src/mcp-tools/` - Extended CLI tools


---

## Browser Automation Tools

### Browser Session Management

```javascript
// Open browser and navigate
mcp__claude-flow__browser_open {
  url: "https://example.com",
  headless: true,              // Run without GUI
  viewport: {
    width: 1920,
    height: 1080
  }
}

// Close browser
mcp__claude-flow__browser_close {}

// Take screenshot
mcp__claude-flow__browser_screenshot {
  path: "screenshot.png",
  fullPage: true
}

// Get page snapshot
mcp__claude-flow__browser_snapshot {
  includeHTML: true,
  includeMetadata: true
}
```

### Browser Navigation

```javascript
// Navigate back
mcp__claude-flow__browser_back {}

// Navigate forward
mcp__claude-flow__browser_forward {}

// Reload page
mcp__claude-flow__browser_reload {
  ignoreCache: false
}

// Get current URL
mcp__claude-flow__browser_get-url {}

// Get page title
mcp__claude-flow__browser_get-title {}
```

### Browser Interactions

```javascript
// Click element
mcp__claude-flow__browser_click {
  selector: "button#submit",
  waitFor: "navigation"        // Wait for navigation after click
}

// Fill input field
mcp__claude-flow__browser_fill {
  selector: "input[name='email']",
  value: "user@example.com"
}

// Type text
mcp__claude-flow__browser_type {
  selector: "textarea",
  text: "Hello world",
  delay: 100                   // Delay between keystrokes in ms
}

// Press key
mcp__claude-flow__browser_press {
  key: "Enter"
}

// Hover over element
mcp__claude-flow__browser_hover {
  selector: ".dropdown"
}

// Select dropdown option
mcp__claude-flow__browser_select {
  selector: "select#country",
  value: "US"
}

// Check checkbox
mcp__claude-flow__browser_check {
  selector: "input[type='checkbox']"
}

// Uncheck checkbox
mcp__claude-flow__browser_uncheck {
  selector: "input[type='checkbox']"
}

// Scroll
mcp__claude-flow__browser_scroll {
  x: 0,
  y: 500
}
```

### Browser Data Extraction

```javascript
// Get text content
mcp__claude-flow__browser_get-text {
  selector: "h1"
}

// Get input value
mcp__claude-flow__browser_get-value {
  selector: "input#username"
}

// Wait for element
mcp__claude-flow__browser_wait {
  selector: "div.loaded",
  timeout: 30000
}

// Evaluate JavaScript
mcp__claude-flow__browser_eval {
  script: "return document.title"
}

// List active sessions
mcp__claude-flow__browser_session-list {}
```

---

## Federation Tools

> **⚠️ Note**: Federation is **NOT a topology**. Federation tools coordinate **multiple independent swarms**, each with their own topology (mesh, hierarchical, etc.). Think of it as "swarm-of-swarms" coordination.

### Agent Federation

```javascript
// Join federation (connect swarm to multi-swarm network)
mcp__claude-flow__federation_join {
  federationId: "fed-123",
  agentId: "agent-456",
  capabilities: ["coder", "tester"]
}

// Leave federation
mcp__claude-flow__federation_leave {
  federationId: "fed-123",
  agentId: "agent-456"
}

// List federations
mcp__claude-flow__federation_list {
  status: "active"             // active, inactive, all
}

// Get federation status
mcp__claude-flow__federation_status {
  federationId: "fed-123"
}

// Broadcast to federation
mcp__claude-flow__federation_broadcast {
  federationId: "fed-123",
  message: "Task completed",
  priority: "high"
}

// Request capability
mcp__claude-flow__federation_request {
  federationId: "fed-123",
  capability: "security-audit",
  timeout: 30000
}

// Offer capability
mcp__claude-flow__federation_offer {
  federationId: "fed-123",
  capability: "code-review",
  capacity: 5
}

// Sync federation state
mcp__claude-flow__federation_sync {
  federationId: "fed-123"
}
```

---

## DAA (Distributed Adaptive Agents) Tools

### Agent Creation and Management

```javascript
// Create DAA agent
mcp__claude-flow__daa_agent_create {
  id: "learner-001",
  name: "Adaptive Learner",
  type: "coder",
  cognitivePattern: "adaptive",  // convergent, divergent, lateral, systems, critical, adaptive
  learningRate: 0.1,
  enableMemory: true
}

// Adapt agent behavior
mcp__claude-flow__daa_agent_adapt {
  agentId: "learner-001",
  feedback: {
    outcome: "success",
    metrics: { accuracy: 0.95 }
  }
}

// Get learning status
mcp__claude-flow__daa_learning_status {
  agentId: "learner-001"
}

// Share knowledge
mcp__claude-flow__daa_knowledge_share {
  sourceAgentId: "learner-001",
  targetAgentId: "learner-002",
  knowledgeType: "patterns"
}

// Get cognitive pattern
mcp__claude-flow__daa_cognitive_pattern {
  agentId: "learner-001"
}

// Get performance metrics
mcp__claude-flow__daa_performance_metrics {
  agentId: "learner-001",
  timeRange: "24h"
}
```

### Workflow Management

```javascript
// Create workflow
mcp__claude-flow__daa_workflow_create {
  name: "adaptive-pipeline",
  steps: [
    { agent: "researcher", action: "analyze" },
    { agent: "coder", action: "implement" },
    { agent: "tester", action: "validate" }
  ],
  adaptive: true
}

// Execute workflow
mcp__claude-flow__daa_workflow_execute {
  workflowId: "workflow-123",
  input: { task: "Build REST API" }
}
```

---

## Terminal Automation Tools

```javascript
// Create terminal session
mcp__claude-flow__terminal_create {
  shell: "bash",               // bash, zsh, powershell
  cwd: "/home/user/project"
}

// Execute command
mcp__claude-flow__terminal_execute {
  sessionId: "term-123",
  command: "npm install",
  timeout: 60000
}

// List terminal sessions
mcp__claude-flow__terminal_list {}

// Close terminal
mcp__claude-flow__terminal_close {
  sessionId: "term-123"
}

// Get terminal history
mcp__claude-flow__terminal_history {
  sessionId: "term-123",
  limit: 50
}
```

---

## Progress Tracking Tools

```javascript
// Check progress
mcp__claude-flow__progress_check {
  feature: "v3-migration",
  detailed: true
}

// Sync progress
mcp__claude-flow__progress_sync {
  source: "local",
  target: "remote"
}

// Get progress summary
mcp__claude-flow__progress_summary {
  groupBy: "category"          // category, priority, status
}

// Watch progress
mcp__claude-flow__progress_watch {
  feature: "v3-migration",
  interval: 5000               // Poll interval in ms
}
```

---

## Transfer Store Tools (IPFS-based)

### Pattern Marketplace

```javascript
// Search patterns
mcp__claude-flow__transfer_store-search {
  query: "authentication",
  tags: ["security", "oauth"],
  limit: 10
}

// Get pattern info
mcp__claude-flow__transfer_store-info {
  cid: "Qm..."                 // IPFS content ID
}

// Download pattern
mcp__claude-flow__transfer_store-download {
  cid: "Qm...",
  destination: "./patterns/"
}

// Get featured patterns
mcp__claude-flow__transfer_store-featured {
  category: "security"
}

// Get trending patterns
mcp__claude-flow__transfer_store-trending {
  timeRange: "7d"
}
```

### Plugin Registry

```javascript
// Search plugins
mcp__claude-flow__transfer_plugin-search {
  query: "code-formatter",
  limit: 10
}

// Get plugin info
mcp__claude-flow__transfer_plugin-info {
  pluginId: "plugin-123"
}

// Get featured plugins
mcp__claude-flow__transfer_plugin-featured {}

// Get official plugins
mcp__claude-flow__transfer_plugin-official {}
```

### IPFS Integration

```javascript
// Resolve IPFS path
mcp__claude-flow__transfer_ipfs-resolve {
  path: "/ipfs/Qm.../pattern.json"
}

// Detect PII in data
mcp__claude-flow__transfer_detect-pii {
  content: "User email: user@example.com",
  types: ["email", "phone", "ssn"]
}
```

---

## Coordination Tools

```javascript
// Set topology
mcp__claude-flow__coordination_topology {
  action: "set",               // set, get
  type: "hierarchical-mesh",
  consensusAlgorithm: "raft",
  maxNodes: 20,
  redundancy: 2
}

// Load balancing
mcp__claude-flow__coordination_load_balance {
  action: "set",               // set, get
  algorithm: "adaptive",       // round-robin, least-connections, weighted, adaptive
  weights: {}
}

// Sync coordination
mcp__claude-flow__coordination_sync {
  source: "agent-123",
  targets: ["agent-456", "agent-789"]
}

// Manage coordination node
mcp__claude-flow__coordination_node {
  action: "join",              // join, leave, status
  nodeId: "node-123"
}

// Consensus decision
mcp__claude-flow__coordination_consensus {
  proposal: "Deploy to production",
  timeout: 30000,
  threshold: 0.66
}

// Orchestrate tasks
mcp__claude-flow__coordination_orchestrate {
  tasks: [
    { agent: "coder", task: "Implement feature" },
    { agent: "tester", task: "Write tests" }
  ],
  parallel: true
}

// Get coordination metrics
mcp__claude-flow__coordination_metrics {
  includeLatency: true,
  includeTopology: true
}
```

---

## Claims System Tools

### Claims Management

```javascript
// Claim task
mcp__claude-flow__claims_claim {
  taskId: "task-123",
  agentId: "agent-456",
  reason: "Best suited for this task"
}

// Release claim
mcp__claude-flow__claims_release {
  claimId: "claim-789"
}

// Handoff claim
mcp__claude-flow__claims_handoff {
  claimId: "claim-789",
  toAgentId: "agent-999",
  reason: "Need specialist"
}

// Accept handoff
mcp__claude-flow__claims_accept-handoff {
  handoffId: "handoff-123"
}

// Get claim status
mcp__claude-flow__claims_status {
  claimId: "claim-789"
}

// List claims
mcp__claude-flow__claims_list {
  agentId: "agent-456",
  status: "active"             // active, pending, completed, all
}

// Mark as stealable
mcp__claude-flow__claims_mark-stealable {
  claimId: "claim-789",
  timeout: 300000              // Time before others can steal
}

// Steal claim
mcp__claude-flow__claims_steal {
  claimId: "claim-789",
  agentId: "agent-999",
  justification: "Original agent inactive"
}

// List stealable claims
mcp__claude-flow__claims_stealable {}

// Load claims from file
mcp__claude-flow__claims_load {
  path: "./claims.json"
}

// Get claims board
mcp__claude-flow__claims_board {}

// Rebalance claims
mcp__claude-flow__claims_rebalance {
  strategy: "workload"         // workload, capability, priority
}
```
   - `worker_triggers`, `worker_results`, `worker_stats`, `worker_context` - See Worker Tools section
