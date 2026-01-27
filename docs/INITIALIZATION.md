# Claude-Flow v3 Initialization Guide

Complete guide to initialization requirements and prerequisite detection for claude-flow v3 orchestration.

---

## Overview

Claude-Flow v3 has **6 different initialization commands** that must be run in the correct order based on which features you're using. Flow Coach automatically detects prerequisites and includes the necessary init commands when you choose **[E] EXECUTE**.

**CRITICAL:** Flow Coach now checks `system status` FIRST before running any init commands to avoid unnecessary re-initialization and confusing error messages.

---

## Initialization Workflow (When [E] EXECUTE is chosen)

### **Step 1: Check System Status**
```bash
npx claude-flow@alpha system status
```

**Output shows:**
- Intelligence: active/inactive
- Embeddings: initialized/not initialized
- AgentDB: initialized/not initialized
- Hooks: enabled/disabled
- Active sessions

### **Step 2: Run Only Missing Initializations**

Flow Coach parses the status and:
- ✅ **Skips** components that are already initialized
- ✅ **Shows clear messages**: "✓ Already initialized, skipping..."
- ✅ **Runs init commands** only for missing components
- ✅ **Uses default providers** (ONNX for embeddings, no --provider flag)

### **Step 3: Display Status Summary**

```
Checking system status...
✓ Intelligence layer: Active (balanced mode)
✓ Embeddings: Initialized (ONNX all-MiniLM-L6-v2, 384D)
✓ AgentDB: Initialized (HNSW enabled)
✓ Hooks: Enabled (standard template)
✓ Session: Restored (session-1769446979339)

All prerequisites ready. Proceeding with orchestration...
```

---

## All Initialization Commands

| Command | Purpose | Required When |
|---------|---------|---------------|
| `hooks init` | Initialize hooks system | Using workers, sessions, or intelligence |
| `swarm init` | Initialize quick task swarm | SWARM mode (< 1 hour tasks) |
| `hive-mind init` | Initialize persistent hive | HIVE-MIND mode (complex/multi-day) |
| `embeddings init` | Vector search setup | Semantic search or Memory Needs = Persistent |
| `agentdb init` | HNSW database | Using AgentDB/RuVector features |
| `hooks intelligence init` | AI learning layer | SONA, MoE, Flash Attention, or EWC++ |

---

## Customizing Detection Thresholds

Flow Coach uses **default thresholds** to determine when to initialize features. These can be customized per-project.

### Default Thresholds

| Feature | Default | Meaning |
|---------|---------|---------|
| Intelligence | ≥50% | Moderately complex learning tasks |
| Memory Needs | ≥60% | Significant persistence requirements |
| Workers | ≥40% | Background task processing needed |

### How to Customize

Create or edit `.claude/settings.json` in your project root:

```json
{
  "skills": {
    "flow-coach": {
      "thresholds": {
        "intelligence": 70,
        "memory": 80,
        "workers": 50
      },
      "templates": {
        "hooks": "standard",
        "intelligence": "full"
      },
      "autoInit": true
    }
  }
}
```

### Configuration Options

**Thresholds (0-100):**
- `intelligence` - When to init intelligence layer (default: 50)
- `memory` - When to init embeddings/AgentDB (default: 60)
- `workers` - When to enable background workers (default: 40)

**Templates:**
- `hooks` - Hooks template: minimal | standard | full (default: standard)
- `intelligence` - Intelligence preset: basic | standard | full (default: standard)

**Auto-initialization:**
- `autoInit` - Automatically run init commands when [E] is chosen (default: true)

### Example Configurations

**Conservative (Resource-constrained):**
```json
{
  "thresholds": {
    "intelligence": 70,
    "memory": 80,
    "workers": 60
  }
}
```

**Aggressive (High-performance):**
```json
{
  "thresholds": {
    "intelligence": 40,
    "memory": 50,
    "workers": 30
  }
}
```

**Enterprise (Security-focused):**
```json
{
  "thresholds": {
    "intelligence": 60,
    "memory": 70,
    "workers": 50
  },
  "templates": {
    "hooks": "full"
  }
}
```

---

## Prerequisite Detection Rules

Flow Coach automatically detects which init commands are needed based on your task assessment and configured thresholds:

### **Intelligence Layer Prerequisites**

**Triggers:**
- Intelligence score ≥ 50% in task assessment
- Keywords: "learn", "pattern", "improve", "adaptive", "self-learning"
- Features: SONA, MoE, HNSW, Flash Attention, EWC++

**Required init (if not already active):**
```bash
# Check first
npx claude-flow@alpha system status | grep "Intelligence"

# If not active, initialize with defaults
npx claude-flow@alpha hooks intelligence init

# Optional: Enable specific features
npx claude-flow@alpha hooks intelligence init \
  --enable-sona true \
  --enable-moe true \
  --enable-hnsw true

# If already active, skip with message
echo "✓ Intelligence layer already active, skipping init"
```

---

### **Memory System Prerequisites**

**Triggers:**
- Memory Needs = "Persistent" in task assessment
- Memory Needs score ≥ 60%
- Keywords: "semantic search", "vector search", "remember", "store pattern"
- Features: Cross-session context, pattern storage, similarity search

**Required init (if not already initialized):**
```bash
# Check first
npx claude-flow@alpha system status | grep "Embeddings\|AgentDB"

# Vector embeddings - ALWAYS use default (ONNX, no --provider flag)
npx claude-flow@alpha embeddings init

# Optional: with hyperbolic embeddings
npx claude-flow@alpha embeddings init --hyperbolic true --cache-size 256

# AgentDB with HNSW indexing
npx claude-flow@alpha agentdb init --enable-hnsw true --quantization-bits 8

# If already initialized, skip with message
echo "✓ Embeddings already initialized (ONNX, 384D), skipping"
echo "✓ AgentDB already initialized, skipping"
```

**CRITICAL:** Never use `--provider ollama`, `--provider voyage`, or any other provider flag. The default ONNX provider works locally without API keys.

---

### **Hooks System Prerequisites**

**Triggers:**
- ANY background workers recommended (map, testgaps, audit, etc.)
- Session management features used
- Intelligence layer enabled
- Pre/post task automation

**Required init (if not already enabled):**
```bash
# Check first
npx claude-flow@alpha system status | grep "Hooks"

# Initialize with standard template
npx claude-flow@alpha hooks init --template standard

# If already initialized, skip with message
echo "✓ Hooks system already enabled, skipping init"
```

**Templates:**
- `minimal` - Basic hooks only
- `standard` - Common hooks (recommended)
- `full` - All available hooks

---

### **Session Management Prerequisites**

**Triggers:**
- HIVE-MIND mode selected
- Duration = "Medium" or "Long" in task assessment
- Keywords: "resume", "continue", "multi-day", "save progress"

**Required init:**
```bash
npx claude-flow@alpha hooks session-start \
  --project "project-name" \
  --restore-latest
```

---

## Checking Initialization Status

### **System Status Command**

**ALWAYS run this FIRST before any initialization:**

```bash
npx claude-flow@alpha system status
```

**Example Output:**
```
Claude Flow Status
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Intelligence:  ✓ Active (balanced mode)
Embeddings:    ✓ Initialized (ONNX all-MiniLM-L6-v2, 384D)
AgentDB:       ✓ Initialized (HNSW enabled, 8-bit quantization)
Hooks:         ✓ Enabled (standard template)
Sessions:      ✓ 1 active session (session-1769446979339)
Workers:       ✓ 12 workers loaded
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Parsing Logic:**
- ✓ = Component initialized and working
- ✗ = Component not initialized or failed
- - = Component not applicable

**Based on this output, Flow Coach will:**
1. ✅ **Skip** components showing ✓ (already initialized)
2. ❌ **Initialize** components showing ✗ (missing)
3. ℹ️ **Show clear messages** for each decision

---

## Complete Initialization Sequences

### **Scenario 1: Quick Task (SWARM)**

**No prerequisites needed:**
```bash
# Direct initialization
npx claude-flow@alpha swarm init --topology mesh --max-agents 5
npx claude-flow@alpha swarm start -o "Build feature" -s development
```

---

### **Scenario 2: Complex Project (HIVE-MIND)**

**With session management only:**
```bash
# 1. Initialize hive-mind
npx claude-flow@alpha hive-mind init -t hierarchical-mesh -c byzantine -m 15

# 2. Start session
npx claude-flow@alpha hooks session-start --project "my-project"

# 3. Spawn agents
npx claude-flow@alpha hive-mind spawn coder
```

---

### **Scenario 3: HIVE-MIND + Intelligence**

**With learning features (check-first pattern):**
```bash
# STEP 1: Check what's already initialized
npx claude-flow@alpha system status

# STEP 2: Initialize only missing components
# If hooks not enabled:
npx claude-flow@alpha hooks init --template standard

# If intelligence not active:
npx claude-flow@alpha hooks intelligence init

# STEP 3: Initialize hive-mind
npx claude-flow@alpha hive-mind init -t hierarchical-mesh -c byzantine -m 15

# STEP 4: Start session
npx claude-flow@alpha hooks session-start --project "my-project"

# STEP 5: Spawn agents (will use MoE routing if intelligence active)
npx claude-flow@alpha hive-mind spawn coder
```

**Flow Coach will show:**
```
Checking system status...
✓ Hooks: Already enabled, skipping init
✓ Intelligence: Already active, skipping init
→ Initializing HIVE-MIND with hierarchical-mesh topology...
✓ HIVE-MIND initialized successfully
✓ Session started: my-project
```

---

### **Scenario 4: HIVE-MIND + Intelligence + Memory**

**Full feature set (check-first pattern):**
```bash
# STEP 1: Check system status FIRST
npx claude-flow@alpha system status

# STEP 2: Initialize only missing components

# If hooks not enabled:
npx claude-flow@alpha hooks init --template standard

# If embeddings not initialized (NEVER use --provider flag):
npx claude-flow@alpha embeddings init
npx claude-flow@alpha agentdb init --enable-hnsw true

# If intelligence not active:
npx claude-flow@alpha hooks intelligence init

# STEP 3: Initialize hive-mind
npx claude-flow@alpha hive-mind init -t hierarchical-mesh -c byzantine -m 15

# STEP 4: Start session
npx claude-flow@alpha hooks session-start --project "my-project" --restore-latest

# STEP 5: Spawn agents with full capabilities
npx claude-flow@alpha hive-mind spawn coder
npx claude-flow@alpha hive-mind spawn tester
```

**Flow Coach will show:**
```
Checking system status...
✓ Hooks: Already enabled, skipping
✓ Embeddings: Already initialized (ONNX, 384D), skipping
✓ AgentDB: Already initialized (HNSW), skipping
✓ Intelligence: Already active, skipping
→ Initializing HIVE-MIND...
✓ Session restored: my-project (session-1769446979339)
→ Spawning agents...
✓ All prerequisites ready. System operational.
```

---

## Execution Flow When User Chooses [E]

```
1. Analyze task assessment scores and features
   ├─ Intelligence ≥ 50%? → May need hooks intelligence init
   ├─ Memory Needs ≥ 60%? → May need embeddings init + agentdb init
   ├─ Workers recommended? → May need hooks init
   └─ HIVE-MIND mode? → Need session management

2. **CHECK SYSTEM STATUS FIRST** (MANDATORY)
   └─ npx claude-flow@alpha system status

   Parse output:
   ├─ Intelligence: active ✓ OR inactive ✗
   ├─ Embeddings: initialized ✓ OR not initialized ✗
   ├─ AgentDB: initialized ✓ OR not initialized ✗
   ├─ Hooks: enabled ✓ OR disabled ✗
   └─ Sessions: active sessions listed

3. Generate init commands ONLY for missing components
   ├─ hooks init (if hooks ✗ AND workers/intelligence needed)
   ├─ embeddings init (if embeddings ✗ AND memory ≥60%)
   │   └─ NEVER use --provider flag (defaults to ONNX)
   ├─ agentdb init (if agentdb ✗ AND memory ≥60%)
   └─ hooks intelligence init (if intelligence ✗ AND intelligence ≥50%)

4. Show clear status messages for each decision
   ├─ "✓ Intelligence already active, skipping init"
   ├─ "✓ Embeddings already initialized (ONNX, 384D), skipping"
   ├─ "→ Initializing hooks system..."
   └─ "✓ Hooks initialized successfully"

5. Generate main initialization
   ├─ swarm init OR hive-mind init (always needed)
   └─ hooks session-start (if HIVE-MIND mode)

6. Generate agent spawning commands

7. Execute sequentially, showing progress
```

**CRITICAL RULES:**
- ✅ ALWAYS run `system status` FIRST
- ✅ SKIP components that show ✓ in status
- ✅ NEVER use `--provider` flag for embeddings
- ✅ Show clear ✓/→ messages for user feedback
- ❌ NEVER use `--force` flags unless user explicitly asks

---

## Checking Initialization Status

```bash
# Overall system health
npx claude-flow@alpha system status

# Deep health check
npx claude-flow@alpha system health --deep

# Check specific components
npx claude-flow@alpha hooks list                    # Hooks status
npx claude-flow@alpha embeddings status            # Embeddings status
npx claude-flow@alpha hooks intelligence --show-status  # Intelligence status
npx claude-flow@alpha memory list                  # Memory system status
```

---

## Common Initialization Patterns

### **Pattern: New Project Setup**
```bash
npx claude-flow@alpha hooks init --template standard
npx claude-flow@alpha hive-mind init -t mesh -m 5
npx claude-flow@alpha hooks session-start --project "new-project"
npx claude-flow@alpha hooks worker-dispatch --trigger map --context "."
```

### **Pattern: Learning System**
```bash
npx claude-flow@alpha hooks init --template standard
npx claude-flow@alpha embeddings init --hyperbolic true
npx claude-flow@alpha hooks intelligence init --enable-sona true --enable-moe true
npx claude-flow@alpha hive-mind init -t adaptive -m 10
```

### **Pattern: Security-Critical Project**
```bash
npx claude-flow@alpha hooks init --template full
npx claude-flow@alpha hive-mind init -t hierarchical-mesh -c byzantine -m 15
npx claude-flow@alpha hooks session-start --project "secure-app"
npx claude-flow@alpha hooks worker-dispatch --trigger audit --priority critical
```

---

## Troubleshooting

### **"Command not found" or "Module not initialized"**

**Solution:** Run the prerequisite init commands first.

```bash
# Check what's missing
npx claude-flow@alpha system status

# Reinitialize if needed
npx claude-flow@alpha hooks init --force --template standard
```

---

### **Memory/embeddings commands fail**

**Solution:** Initialize embeddings and AgentDB first.

```bash
npx claude-flow@alpha embeddings init --hyperbolic true
npx claude-flow@alpha agentdb init --enable-hnsw true
```

---

### **Intelligence features not working**

**Solution:** Initialize intelligence layer.

```bash
npx claude-flow@alpha hooks intelligence init --enable-sona true
```

---

## Quick Reference

| If Flow Coach Recommends... | You Need... |
|----------------------------|-------------|
| Intelligence: Neural | `hooks intelligence init` |
| Memory Needs: Persistent | `embeddings init` + `agentdb init` |
| Background Workers | `hooks init` |
| HIVE-MIND mode | `hive-mind init` + `hooks session-start` |
| SWARM mode only | `swarm init` (no prerequisites) |
| SONA/MoE/HNSW | `hooks init` + `hooks intelligence init` |
| Semantic Search | `embeddings init` + `agentdb init` |

---

**For detailed command syntax, see [CLI_REFERENCE.md](CLI_REFERENCE.md)**
