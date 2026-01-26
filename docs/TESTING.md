# Testing Configuration Reference

Complete guide to testing features in claude-flow v3.

> **CLI Reference**: For CLI commands including `hooks worker-dispatch --trigger testgaps`, see [CLI_REFERENCE.md](CLI_REFERENCE.md).
>
> **MCP Reference**: For worker MCP tools, see [MCP_REFERENCE.md](MCP_REFERENCE.md).

---

## Overview

Testing in claude-flow v3 is provided through multiple components:

| Component | Type | Purpose |
|-----------|------|---------|
| `testgaps` | Worker | Test coverage analysis |
| `tester` | Agent | Test creation and validation |
| `tdd-london-swarm` | Agent | TDD with mock-driven development |
| `production-validator` | Agent | Production readiness validation |

---

## When Testing Is Auto-Recommended

Flow-coach automatically recommends testing configuration when your task involves:

| Task Type | Testing Level | Auto-Enabled |
|-----------|--------------|--------------|
| Code generation | **HIGH** | `testgaps` + `tester` |
| TDD mentioned | **HIGH** | `tdd-london-swarm` + `testgaps` |
| Bug fix | **MEDIUM** | `testgaps` (find untested paths) |
| Refactoring | **HIGH** | `testgaps` (maintain coverage) |
| API development | **HIGH** | `tester` (API tests) |
| Feature implementation | **MEDIUM** | `testgaps` on completion |
| Documentation only | **LOW** | Optional |
| Research/exploration | **LOW** | Optional |

---

## Decision Tree: Testing Selection

```
Does your task involve...
|
|-- Code generation?
|   +-- ✅ Enable testgaps worker
|   +-- ✅ Add tester agent
|   +-- Auto-run testgaps after code written
|
|-- TDD/test-first mentioned?
|   +-- ✅ Enable tdd-london-swarm agent
|   +-- ✅ Enable testgaps worker
|   +-- Write tests BEFORE implementation
|
|-- Bug fix?
|   +-- ✅ Enable testgaps (find untested paths)
|   +-- Identify test gaps around bug location
|   +-- Write regression test
|
|-- Refactoring?
|   +-- ✅ Enable testgaps (ensure coverage maintained)
|   +-- Run before refactoring (baseline)
|   +-- Run after refactoring (verify)
|
|-- API development?
|   +-- ✅ Enable tester agent (API tests)
|   +-- Integration tests for endpoints
|   +-- Contract testing
|
|-- Documentation only?
|   +-- ❌ Testing optional
|
+-- Research/exploration only?
    +-- ❌ Testing optional
```

---

## Testing Components

### 1. testgaps Worker

Background test coverage analysis.

```javascript
mcp__claude-flow__hooks_worker-dispatch {
  trigger: "testgaps",
  context: "src/",
  priority: "normal",
  background: true
}
```

| Setting | Value |
|---------|-------|
| Duration | 30 seconds |
| Priority | Normal |
| Output | Coverage report + gap identification |

**Use Cases**:
- Before starting code changes (baseline)
- After code changes (verify coverage)
- Before releases (coverage gate)
- During refactoring (maintain coverage)

**Output Example**:
```
Coverage Analysis:
- Overall: 78%
- src/auth/: 45% ⚠️ LOW
- src/api/: 92% ✅
- src/utils/: 85% ✅

Untested Paths:
1. src/auth/oauth.ts:45-67 (OAuth callback)
2. src/auth/session.ts:23-34 (Session refresh)
3. src/api/errors.ts:12-20 (Error handling)
```

---

### 2. tester Agent

Specialized agent for test creation and validation.

```javascript
mcp__claude-flow__agent_spawn {
  agentType: "tester",
  model: "sonnet",
  task: "Create comprehensive tests for auth module",
  domain: "testing"
}
```

**Capabilities**:
- Unit test creation
- Integration test creation
- Test fixture generation
- Assertion validation
- Mock creation
- Test organization

**Recommended Pipeline**:
```
coder → tester → reviewer
```

---

### 3. tdd-london-swarm Agent

TDD specialist using London School (mock-driven) approach.

```javascript
mcp__claude-flow__agent_spawn {
  agentType: "tdd-london-swarm",
  model: "sonnet",
  task: "Implement user authentication using TDD",
  domain: "testing"
}
```

**Approach**:
1. Write failing test first
2. Write minimal code to pass
3. Refactor
4. Repeat

**Best For**:
- New feature development
- Clean architecture enforcement
- Dependency injection patterns

---

### 4. production-validator Agent

Production readiness validation.

```javascript
mcp__claude-flow__agent_spawn {
  agentType: "production-validator",
  model: "opus",
  task: "Validate application is production-ready",
  domain: "testing"
}
```

**Checks**:
- Test coverage thresholds
- Error handling completeness
- Security validation
- Performance benchmarks
- Documentation completeness
- Deployment configuration

---

## Configuration Examples

### Minimal Testing (Code Changes)

```json
{
  "workers": ["testgaps"],
  "testing": {
    "runOnComplete": true
  }
}
```

### Standard Testing (Feature Development)

```json
{
  "agents": [
    {
      "type": "tester",
      "model": "sonnet",
      "task": "Create and maintain tests"
    }
  ],
  "workers": ["testgaps"],
  "testing": {
    "coverageThreshold": 80,
    "runOnComplete": true,
    "failOnLowCoverage": false
  }
}
```

### Full TDD (Test-First Development)

```json
{
  "agents": [
    {
      "type": "tdd-london-swarm",
      "model": "sonnet",
      "task": "Lead TDD development"
    },
    {
      "type": "tester",
      "model": "sonnet",
      "task": "Validate test quality"
    }
  ],
  "workers": ["testgaps"],
  "testing": {
    "tddMode": true,
    "coverageThreshold": 90,
    "runOnComplete": true,
    "failOnLowCoverage": true,
    "requireTestsFirst": true
  }
}
```

### Pre-Release Validation

```json
{
  "agents": [
    {
      "type": "production-validator",
      "model": "opus",
      "task": "Validate production readiness"
    }
  ],
  "workers": ["testgaps", "audit", "benchmark"],
  "testing": {
    "coverageThreshold": 85,
    "failOnLowCoverage": true,
    "securityScan": true,
    "performanceBaseline": true
  }
}
```

---

## Testing Workflows

### Workflow 1: Code-First with Coverage Check

```
1. coder writes implementation
2. testgaps analyzes coverage
3. tester creates missing tests
4. reviewer validates
```

```javascript
// Auto-triggered after code changes
[PostToolUse - Edit|Write]:
  if (file matches "src/**/*.ts") {
    mcp__claude-flow__hooks_worker-dispatch { trigger: "testgaps" }
  }
```

### Workflow 2: TDD (Test-First)

```
1. tdd-london-swarm writes failing test
2. coder writes minimal implementation
3. testgaps verifies coverage
4. refactor if needed
5. repeat
```

### Workflow 3: Bug Fix with Regression Test

```
1. testgaps identifies untested paths around bug
2. tester writes regression test (should fail)
3. coder fixes bug
4. test passes
5. testgaps verifies no coverage regression
```

---

## Auto-Trigger Rules

Flow-coach auto-enables testing based on task analysis:

| Detected Keyword | Auto-Enabled |
|-----------------|--------------|
| "implement", "create", "build" | `testgaps` + `tester` |
| "TDD", "test-first", "test-driven" | `tdd-london-swarm` |
| "fix", "bug", "issue" | `testgaps` |
| "refactor", "restructure" | `testgaps` (before + after) |
| "API", "endpoint", "REST" | `tester` (API tests) |
| "release", "deploy", "production" | `production-validator` |

---

## Testing Best Practices

### 1. Always Check Coverage Before Changes

```javascript
// Before starting work
mcp__claude-flow__hooks_worker-dispatch {
  trigger: "testgaps",
  context: "src/",
  priority: "high"
}
```

### 2. Use TDD for New Features

```json
{
  "agents": [{ "type": "tdd-london-swarm" }],
  "testing": { "tddMode": true, "requireTestsFirst": true }
}
```

### 3. Verify Coverage After Refactoring

```javascript
// After refactoring complete
mcp__claude-flow__hooks_worker-dispatch {
  trigger: "testgaps",
  context: "refactored-module/",
  priority: "high",
  background: false  // Wait for results
}
```

### 4. Gate Releases on Coverage

```json
{
  "testing": {
    "coverageThreshold": 85,
    "failOnLowCoverage": true
  }
}
```

---

## Troubleshooting

### Low Coverage Detected

```javascript
// Get detailed coverage report
mcp__claude-flow__hooks_worker-dispatch {
  trigger: "testgaps",
  context: "src/",
  priority: "high",
  background: false
}

// Spawn tester to fill gaps
mcp__claude-flow__agent_spawn {
  agentType: "tester",
  task: "Create tests for uncovered paths in auth module"
}
```

### TDD Workflow Not Working

```javascript
// Ensure TDD mode is enabled
config: {
  "testing": {
    "tddMode": true,
    "requireTestsFirst": true
  }
}

// Use correct agent
mcp__claude-flow__agent_spawn {
  agentType: "tdd-london-swarm",  // Not regular tester
  task: "..."
}
```

---

## Testing Checklist for Flow-Coach

When flow-coach recommends testing, it ensures:

- [ ] Testing level assessed in Phase 1
- [ ] testgaps worker enabled for code tasks
- [ ] tester agent added when appropriate
- [ ] TDD workflow enabled when requested
- [ ] Coverage thresholds configured
- [ ] Auto-trigger rules applied
- [ ] Pre-release validation if deploying
