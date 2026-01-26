# Integration Configuration Reference

Complete guide to integration features in claude-flow v3: GitHub, Diff Analysis, Claims System, and Session Management.

> **CLI Reference**: For complete CLI command documentation, see [CLI_REFERENCE.md](CLI_REFERENCE.md).
>
> **MCP Reference**: For all MCP tools, see [MCP_REFERENCE.md](MCP_REFERENCE.md).

---

> **Note**: GitHub tools provide LOCAL STATE MANAGEMENT for workflow coordination.
> For actual GitHub API operations, use `gh` CLI or the GitHub MCP server.

---

## Overview

Integration features connect claude-flow with external systems and workflows:

| Category | Components | Purpose |
|----------|------------|---------|
| **GitHub** | PR, Issues, Workflows, Metrics | Repository management (local state) |
| **Diff Analysis** | Risk assessment, Reviewers, Stats | Code change analysis |
| **Claims** | Board, Handoffs, Load Balancing | Human/agent coordination |
| **Sessions** | Save, Restore, Persistence | State management |

---

## GitHub Integration

> **Note**: These tools provide LOCAL STATE for workflow coordination.
> For actual GitHub API calls, use `gh` CLI or the GitHub MCP server.

### When Auto-Recommended

| Task Type | Auto-Enabled |
|-----------|--------------|
| PR/pull request mentioned | `github_pr_manage` + `analyze_diff` |
| Code review task | `analyze_diff` + reviewer suggestion |
| CI/CD workflow | `github_workflow` tools |
| Release/deployment | `github_pr_manage` + `github_workflow` |
| Issue tracking | `github_issue_track` |
| Repository analysis | `github_repo_analyze` |

### Decision Tree

```
Does your task mention...
|
|-- PR/pull request?
|   +-- ✅ github_pr_manage (create, review, merge)
|   +-- ✅ analyze_diff (risk assessment)
|   +-- ✅ analyze_diff-reviewers (suggest reviewers)
|
|-- Code review?
|   +-- ✅ analyze_diff (comprehensive analysis)
|   +-- ✅ analyze_diff-risk (quick risk check)
|   +-- ✅ analyze_diff-reviewers (suggest experts)
|
|-- CI/CD workflow?
|   +-- ✅ github_workflow (list, trigger, status)
|   +-- Monitor workflow runs
|
|-- Release/deployment?
|   +-- ✅ github_pr_manage (create release PR)
|   +-- ✅ github_workflow (trigger deploy)
|   +-- ✅ Pre-release checklist workers
|
|-- Issue tracking?
|   +-- ✅ github_issue_track (create, update, assign)
|
|-- Repository analysis?
|   +-- ✅ github_repo_analyze (deep analysis)
|
+-- None of the above?
    +-- ❌ GitHub integration optional
```

### GitHub MCP Tools

#### Repository Analysis

```javascript
mcp__claude-flow__github_repo_analyze {
  owner: "org",
  repo: "repo-name",
  branch: "main",
  deep: true
}
```

#### PR Management

```javascript
// Create PR
mcp__claude-flow__github_pr_manage {
  action: "create",
  owner: "org",
  repo: "repo-name",
  title: "Feature: User Authentication",
  body: "Implementation details...",
  branch: "feature/auth",
  baseBranch: "main"
}

// List PRs
mcp__claude-flow__github_pr_manage {
  action: "list",
  owner: "org",
  repo: "repo-name",
  state: "open"
}

// Review PR
mcp__claude-flow__github_pr_manage {
  action: "review",
  owner: "org",
  repo: "repo-name",
  prNumber: 123,
  event: "APPROVE",
  body: "LGTM!"
}

// Merge PR
mcp__claude-flow__github_pr_manage {
  action: "merge",
  owner: "org",
  repo: "repo-name",
  prNumber: 123,
  mergeMethod: "squash"
}
```

#### Issue Tracking

```javascript
// Create issue
mcp__claude-flow__github_issue_track {
  action: "create",
  owner: "org",
  repo: "repo-name",
  title: "Bug: Login fails on mobile",
  body: "Description...",
  labels: ["bug", "high-priority"],
  assignees: ["user1"]
}

// Update issue
mcp__claude-flow__github_issue_track {
  action: "update",
  owner: "org",
  repo: "repo-name",
  issueNumber: 456,
  state: "closed"
}
```

#### Workflow Management

```javascript
// List workflows
mcp__claude-flow__github_workflow {
  action: "list",
  owner: "org",
  repo: "repo-name"
}

// Trigger workflow
mcp__claude-flow__github_workflow {
  action: "trigger",
  owner: "org",
  repo: "repo-name",
  workflowId: "ci.yml",
  ref: "main"
}

// Check status
mcp__claude-flow__github_workflow {
  action: "status",
  owner: "org",
  repo: "repo-name",
  runId: 12345
}
```

#### Metrics

```javascript
mcp__claude-flow__github_metrics {
  owner: "org",
  repo: "repo-name",
  metric: "all",  // commits, contributors, traffic, releases
  timeRange: "30d"
}
```

---

## Diff Analysis

### When Auto-Recommended

| Task Type | Auto-Enabled |
|-----------|--------------|
| Code review | `analyze_diff` (full analysis) |
| PR creation | `analyze_diff-risk` + `analyze_diff-reviewers` |
| Security review | `analyze_file-risk` |
| Pre-merge check | `analyze_diff-stats` |

### Diff Analysis Tools

#### Full Diff Analysis

```javascript
mcp__claude-flow__analyze_diff {
  ref: "HEAD~1",              // or "main..feature"
  includeFileRisks: true,
  includeReviewers: true,
  useRuVector: true           // AI-enhanced analysis
}
```

**Output**:
```
Diff Analysis:
- Files changed: 12
- Additions: 450
- Deletions: 120
- Risk level: MEDIUM

High-risk files:
1. src/auth/oauth.ts (security-sensitive)
2. src/db/migrations/001.sql (schema change)

Suggested reviewers:
1. @security-team (auth changes)
2. @dba-team (migration)
```

#### Quick Risk Assessment

```javascript
mcp__claude-flow__analyze_diff-risk {
  ref: "main..feature"
}
// Returns: { riskLevel: "medium", reasons: [...] }
```

#### Suggest Reviewers

```javascript
mcp__claude-flow__analyze_diff-reviewers {
  ref: "HEAD",
  limit: 5
}
// Returns: [{ user: "@alice", reason: "auth expert" }, ...]
```

#### File Risk Assessment

```javascript
mcp__claude-flow__analyze_file-risk {
  path: "src/auth/service.js",
  status: "modified",
  additions: 50,
  deletions: 10
}
// Returns: { risk: "high", reasons: ["security-sensitive", "high churn"] }
```

#### Diff Statistics

```javascript
mcp__claude-flow__analyze_diff-stats {
  ref: "HEAD"
}
// Returns: { files: 12, additions: 450, deletions: 120, ... }
```

---

## Claims System

### When Auto-Recommended

| Workflow Need | Auto-Enabled |
|---------------|--------------|
| Human approval gates | Claims system |
| Review cycles | Claims with handoffs |
| Multi-stage approval | Claims board |
| Agent-to-human handoff | Claims coordination |

### Decision Tree

```
Does your workflow need...
|
|-- Human approval gates?
|   +-- ✅ Enable claims system
|   +-- Configure approval checkpoints
|
|-- Review cycles?
|   +-- ✅ Enable claims with handoff
|   +-- Agent completes work → Claims handoff → Human reviews
|
|-- Multi-stage approval?
|   +-- ✅ Enable claims_board visualization
|   +-- Track approval status across stages
|
|-- Agent-to-human handoff?
|   +-- ✅ Enable claims coordination
|   +-- Define handoff triggers
|
+-- Fully autonomous?
    +-- ❌ Claims optional
```

### Claims MCP Tools

#### View Claims Board

```javascript
mcp__claude-flow__claims_board {}
```

**Output**:
```
Claims Board:
┌────────────────┬─────────┬──────────┬─────────────┐
│ Claim          │ Status  │ Owner    │ Waiting On  │
├────────────────┼─────────┼──────────┼─────────────┤
│ Auth PR Review │ PENDING │ agent-1  │ @alice      │
│ Deploy Staging │ BLOCKED │ agent-2  │ @ops-team   │
│ Schema Change  │ APPROVED│ agent-3  │ -           │
└────────────────┴─────────┴──────────┴─────────────┘
```

#### List Claims

```javascript
mcp__claude-flow__claims_list {
  status: "pending",  // pending, approved, rejected, all
  owner: "agent-1"
}
```

### Claims Configuration

```json
{
  "claims": {
    "enabled": true,
    "approvalGates": [
      {
        "name": "Security Review",
        "trigger": "security-sensitive files changed",
        "approvers": ["@security-team"]
      },
      {
        "name": "Deploy Approval",
        "trigger": "deployment workflow",
        "approvers": ["@ops-team", "@tech-lead"]
      }
    ],
    "autoHandoff": true,
    "notifyOnClaim": true
  }
}
```

---

## Session Management

### When Auto-Enabled

| Mode Selected | Session Management |
|---------------|-------------------|
| HIVE-MIND | ✅ AUTO-ENABLED |
| DAA (self-learning) | ✅ AUTO-ENABLED |
| SWARM | ❌ Optional (unless "resume" mentioned) |

### Decision Tree

```
Which mode was selected?
|
|-- HIVE-MIND?
|   +-- ✅ AUTO-ENABLE session management
|   +-- hooks session-restore --session-id "latest"
|   +-- hooks session-end --export-metrics true
|   +-- Auto-save on exit
|   +-- Resume capability enabled
|
|-- DAA (self-learning)?
|   +-- ✅ AUTO-ENABLE persistent sessions
|   +-- Learning state preserved
|   +-- Knowledge persisted across sessions
|
|-- SWARM (quick task)?
|   +-- ❌ Sessions optional (ephemeral by default)
|   |
|   +-- Unless "resume later" mentioned?
|       +-- ✅ Enable sessions
```

### Session MCP Tools

#### Save Session

```javascript
mcp__claude-flow__session_save {
  name: "api-development",
  description: "REST API implementation project",
  includeAgents: true,
  includeTasks: true,
  includeMemory: true
}
```

#### Restore Session

```javascript
mcp__claude-flow__session_restore {
  name: "api-development"
}
```

#### List Sessions

```javascript
mcp__claude-flow__session_list {
  limit: 10,
  sortBy: "date"
}
```

#### Session Info

```javascript
mcp__claude-flow__session_info {
  sessionId: "session-123"
}
```

### Session Hooks (CLI)

```bash
# Restore previous session
npx claude-flow@alpha hooks session-restore --session-id "latest"

# End session with export
npx claude-flow@alpha hooks session-end --export-metrics true --save-state true
```

### Session Configuration

```json
{
  "session": {
    "persistence": true,
    "autoSave": true,
    "saveInterval": 300,  // seconds
    "includeAgents": true,
    "includeTasks": true,
    "includeMemory": true,
    "exportMetricsOnEnd": true
  }
}
```

---

## Worker Patterns (Auto-Trigger)

Flow-coach auto-triggers worker patterns based on task type:

### New Project Setup

**Trigger**: New codebase, "new project", "getting started"

```javascript
// Auto-triggered sequence
[Pattern: new-project-setup]:
  1. mcp__claude-flow__worker_dispatch { trigger: "map", context: "src/" }
  2. mcp__claude-flow__worker_dispatch { trigger: "ultralearn", context: "project domain" }
  3. mcp__claude-flow__worker_dispatch { trigger: "testgaps", context: "src/" }
```

### Pre-Release Checklist

**Trigger**: "release", "deploy", "production"

```javascript
// Auto-triggered sequence
[Pattern: pre-release]:
  1. mcp__claude-flow__worker_dispatch { trigger: "audit", context: "full codebase", priority: "critical" }
  2. mcp__claude-flow__worker_dispatch { trigger: "benchmark", context: "api endpoints" }
  3. mcp__claude-flow__worker_dispatch { trigger: "document", context: "src/" }
  4. mcp__claude-flow__worker_dispatch { trigger: "testgaps", context: "src/" }
```

### Technical Debt Sprint

**Trigger**: "refactor", "technical debt", "cleanup"

```javascript
// Auto-triggered sequence
[Pattern: tech-debt]:
  1. mcp__claude-flow__worker_dispatch { trigger: "map", context: "src/" }
  2. mcp__claude-flow__worker_dispatch { trigger: "refactor", context: "src/legacy" }
  3. mcp__claude-flow__worker_dispatch { trigger: "testgaps", context: "src/" }
  4. mcp__claude-flow__worker_dispatch { trigger: "optimize", context: "performance hotspots" }
```

### Complex Investigation

**Trigger**: "debug", "investigate", "analyze issue"

```javascript
// Auto-triggered sequence
[Pattern: investigation]:
  1. mcp__claude-flow__worker_dispatch { trigger: "deepdive", context: "problematic module" }
  2. mcp__claude-flow__worker_dispatch { trigger: "map", context: "src/" }
  3. mcp__claude-flow__worker_dispatch { trigger: "benchmark", context: "core module" }
```

---

## Configuration Examples

### Full Integration (Enterprise)

```json
{
  "github": {
    "enabled": true,
    "autoAnalyzeDiff": true,
    "suggestReviewers": true,
    "createPROnComplete": false
  },
  "claims": {
    "enabled": true,
    "approvalGates": [
      { "name": "Security Review", "approvers": ["@security"] },
      { "name": "Deploy Approval", "approvers": ["@ops"] }
    ]
  },
  "session": {
    "persistence": true,
    "autoSave": true,
    "exportMetricsOnEnd": true
  },
  "workerPatterns": {
    "autoTrigger": true,
    "patterns": ["new-project-setup", "pre-release", "tech-debt"]
  }
}
```

### Minimal Integration (Solo Developer)

```json
{
  "github": {
    "enabled": true,
    "autoAnalyzeDiff": true
  },
  "session": {
    "persistence": true,
    "autoSave": true
  }
}
```

---

## Integration Checklist for Flow-Coach

When flow-coach recommends integration, it ensures:

- [ ] GitHub level assessed in Phase 1
- [ ] Diff analysis enabled for code reviews
- [ ] Claims system enabled when approval needed
- [ ] Session management auto-enabled for HIVE-MIND/DAA
- [ ] Worker patterns auto-triggered based on task
- [ ] Appropriate tools configured in final config
