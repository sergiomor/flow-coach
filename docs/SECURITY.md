# Security Configuration Reference

Complete guide to security features in claude-flow v3.

> **CLI Reference**: For CLI commands including `hooks worker-dispatch --trigger audit`, see [CLI_REFERENCE.md](CLI_REFERENCE.md).
>
> **MCP Reference**: For AIDefence and security MCP tools, see [MCP_REFERENCE.md](MCP_REFERENCE.md).

---

## Overview

Security in claude-flow v3 is provided through multiple layers:

| Layer | Component | Purpose |
|-------|-----------|---------|
| **Agent** | `security-manager` | Code review, vulnerability detection |
| **Worker** | `audit` | Background vulnerability scanning |
| **MCP Tools** | AIDefence suite | PII detection, prompt injection, threat analysis |
| **Consensus** | Byzantine | Fault-tolerant, handles malicious nodes |

---

## When Security Is Recommended

Flow-coach automatically recommends security configuration when your task involves:

| Task Type | Security Level | Recommendations |
|-----------|---------------|-----------------|
| Code generation | **HIGH** | `audit` worker + `security-manager` agent |
| Self-learning agents | **HIGH** | AIDefence (prevent bad pattern learning) |
| Authentication/auth | **CRITICAL** | Full suite + `opus` model for security agent |
| User input handling | **HIGH** | AIDefence PII detection |
| API development | **MEDIUM** | `audit` worker |
| Database operations | **MEDIUM** | `audit` worker |
| File system operations | **MEDIUM** | `audit` worker |
| Documentation only | **LOW** | Optional |
| Research/exploration | **LOW** | Optional |

---

## Decision Tree: Security Selection

```
Does your task involve...
|
|-- Code generation?
|   +-- ✅ Enable audit worker
|   +-- ✅ Add security-manager agent
|   +-- Recommended: auditBeforeCommit: true
|
|-- Self-learning agents (DAA)?
|   +-- ✅ Enable AIDefence (prevent learning bad patterns)
|   +-- ✅ Enable audit worker
|   +-- ✅ Add security-manager with critical cognitive pattern
|
|-- Authentication/authorization?
|   +-- ✅ CRITICAL: Full security suite
|   +-- ✅ security-manager with opus model
|   +-- ✅ audit worker with critical priority
|   +-- ✅ AIDefence for credential detection
|
|-- User input handling?
|   +-- ✅ AIDefence PII detection
|   +-- ✅ Prompt injection scanning
|
|-- API/database operations?
|   +-- ✅ audit worker
|   +-- Consider: security-manager for sensitive endpoints
|
|-- File system operations?
|   +-- ✅ audit worker
|   +-- Check for path traversal vulnerabilities
|
|-- Documentation only?
|   +-- ❌ Security optional (low risk)
|
+-- Research/exploration only?
    +-- ❌ Security optional (no code changes)
```

---

## Security Components

### 1. security-manager Agent

Specialized agent for security review and vulnerability detection.

```javascript
mcp__claude-flow__agent_spawn {
  agentType: "security-manager",
  model: "opus",              // Complex reasoning for security
  task: "Security audit all code changes",
  domain: "security"
}
```

**Capabilities**:
- Code vulnerability scanning
- Authentication flow review
- Input validation checking
- Dependency security analysis
- OWASP Top 10 detection

**Recommended Pipeline**:
```
system-architect → coder → code-analyzer → security-manager → reviewer
```

---

### 2. audit Background Worker

Background vulnerability scanning with **CRITICAL** priority.

```javascript
mcp__claude-flow__hooks_worker-dispatch {
  trigger: "audit",
  context: "src/auth",       // Target path/module
  priority: "critical",      // Always critical for security
  background: true
}
```

| Setting | Value |
|---------|-------|
| Duration | 45 seconds |
| Priority | **CRITICAL** (highest) |
| Triggers on | Security-sensitive code, before releases |

**Use Cases**:
- Before git commits
- After code generation
- Before deployments
- On security-sensitive modules

---

### 3. AIDefence MCP Tools

Fast, real-time security scanning.

#### aidefence_scan

Comprehensive security scan.

```javascript
mcp__claude-flow__aidefence_scan {
  input: "<code or text to scan>",
  checks: ["injection", "pii", "jailbreak", "malicious"]
}
// Returns: { safe: false, threats: [...], recommendations: [...] }
```

**Checks for**:
- Prompt injection attempts
- Jailbreak patterns
- PII (emails, SSNs, API keys, passwords)
- Malicious code patterns
- Command injection

#### aidefence_is_safe

Quick boolean safety check (fastest).

```javascript
mcp__claude-flow__aidefence_is_safe {
  input: "<text to check>"
}
// Returns: { safe: true } or { safe: false, reason: "..." }
```

#### aidefence_has_pii

Detect personally identifiable information.

```javascript
mcp__claude-flow__aidefence_has_pii {
  input: "<text to scan>"
}
// Returns: { hasPii: true, types: ["email", "api_key"], locations: [...] }
```

**Detects**:
- Email addresses
- Phone numbers
- Social Security Numbers
- Credit card numbers
- API keys and tokens
- Passwords
- IP addresses

#### aidefence_analyze

Deep threat analysis with recommendations.

```javascript
mcp__claude-flow__aidefence_analyze {
  input: "<code or text>",
  verbose: true
}
// Returns: { threats: [...], severity: "high", recommendations: [...] }
```

#### aidefence_learn

Learn from new threat patterns (for self-learning systems).

```javascript
mcp__claude-flow__aidefence_learn {
  pattern: "<threat pattern>",
  category: "injection",
  severity: "high"
}
```

---

### 4. Byzantine Consensus

For distributed systems that need fault tolerance against malicious nodes.

```javascript
mcp__claude-flow__coordination_topology {
  action: "set",
  consensusAlgorithm: "byzantine"
}
```

**Features**:
- Handles malicious nodes
- f < n/3 fault tolerance
- Cryptographic verification
- Consensus despite adversaries

**Use when**:
- High-security environments
- Untrusted agent sources
- Critical decision coordination

---

## Configuration Examples

### Minimal Security (Code Generation)

```json
{
  "workers": ["audit"],
  "security": {
    "auditBeforeCommit": true
  }
}
```

### Medium Security (API Development)

```json
{
  "agents": [
    {
      "type": "security-manager",
      "model": "sonnet",
      "capabilities": ["review", "security"]
    }
  ],
  "workers": ["audit", "testgaps"],
  "security": {
    "auditBeforeCommit": true,
    "scanPii": true
  }
}
```

### Full Security (Self-Learning / Auth Systems)

```json
{
  "agents": [
    {
      "id": "security-001",
      "name": "Security Auditor",
      "type": "security-manager",
      "cognitivePattern": "critical",
      "learningRate": 0.05,
      "model": "opus",
      "capabilities": ["review", "security", "vulnerability-scan"]
    }
  ],
  "workers": ["audit", "testgaps", "optimize"],
  "security": {
    "aidefence": true,
    "scanPii": true,
    "detectInjection": true,
    "auditBeforeCommit": true,
    "blockOnCritical": true
  },
  "consensus": "byzantine"
}
```

---

## Security Hooks

### Pre-Edit Hook (Scan Before Writing)

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "cat | jq -r '.tool_input.content // empty' | npx claude-flow@alpha security scan --stdin"
          }
        ]
      }
    ]
  }
}
```

> **Note**: AIDefence tools (`aidefence_scan`, `aidefence_is_safe`, etc.) are MCP tools only, not CLI commands. Use `security scan` for CLI scanning.

### Pre-Commit Hook (Audit Before Commit)

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "cat | jq -r '.tool_input.command // empty' | grep -q 'git commit' && npx claude-flow@alpha hooks worker-dispatch --trigger audit --priority critical --wait"
          }
        ]
      }
    ]
  }
}
```

---

## Security Levels Reference

| Level | Components | Use Case |
|-------|------------|----------|
| **NONE** | - | Documentation, research only |
| **LOW** | `audit` worker | Simple code changes |
| **MEDIUM** | `audit` + `security-manager` | API, database work |
| **HIGH** | Full suite without byzantine | Code generation, user input |
| **CRITICAL** | Full suite + byzantine | Auth systems, self-learning |

---

## Best Practices

### 1. Always Enable Security for Code Generation

```javascript
// When task involves writing code
if (task.involvesCodeGeneration) {
  config.workers.push("audit");
  config.security.auditBeforeCommit = true;
}
```

### 2. Use opus Model for Security-Critical Reviews

```javascript
{
  type: "security-manager",
  model: "opus"  // Don't use haiku for security
}
```

### 3. Scan Self-Learning Agent Inputs

```javascript
// Before storing learned patterns
mcp__claude-flow__aidefence_scan {
  input: learnedPattern,
  checks: ["malicious", "injection"]
}
```

### 4. Block on Critical Vulnerabilities

```json
{
  "security": {
    "blockOnCritical": true  // Halt execution if critical issue found
  }
}
```

### 5. Review Before Deployment

```javascript
// Pre-deployment checklist
mcp__claude-flow__hooks_worker-dispatch { trigger: "audit", priority: "critical" }
mcp__claude-flow__hooks_worker-dispatch { trigger: "testgaps" }
```

---

## Troubleshooting

### Security Scan Not Running

```javascript
// Check worker status
mcp__claude-flow__hooks_worker-status {}

// Force dispatch
mcp__claude-flow__hooks_worker-dispatch {
  trigger: "audit",
  priority: "critical",
  background: false  // Wait for results
}
```

### AIDefence Not Available

```javascript
// Check AIDefence status
mcp__claude-flow__aidefence_stats {}

// Initialize if needed
mcp__claude-flow__aidefence_init {}
```

### False Positives

```javascript
// Analyze with verbose mode
mcp__claude-flow__aidefence_analyze {
  input: "<flagged content>",
  verbose: true
}
// Review recommendations and adjust thresholds
```

---

## Security Checklist for Flow-Coach

When flow-coach recommends security, it ensures:

- [ ] Security assessment shown in Phase 1
- [ ] Security level determined based on task type
- [ ] Appropriate agents recommended (security-manager)
- [ ] Appropriate workers enabled (audit)
- [ ] AIDefence configured if needed
- [ ] Consensus algorithm selected if distributed
- [ ] Hooks configured for pre-commit scanning
- [ ] Model routing uses opus for security tasks
