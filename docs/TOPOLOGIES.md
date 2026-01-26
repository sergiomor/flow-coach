# Topology Reference

Detailed guide for swarm topologies and orchestration modes.

> **CLI Reference**: For complete CLI command documentation, see [CLI_REFERENCE.md](CLI_REFERENCE.md).
>
> **MCP Reference**: For all MCP tools, see [MCP_REFERENCE.md](MCP_REFERENCE.md).

---

## Orchestration Modes

### SWARM (Quick Tasks)

**Best for**: Single features, quick fixes, research, experiments

```bash
# Step 1: Initialize swarm
npx claude-flow@alpha swarm init --topology mesh --max-agents 5

# Step 2: Start with objective
npx claude-flow@alpha swarm start -o "your task" -s development --parallel

# Check status
npx claude-flow@alpha swarm status
```

**Characteristics**:
- Two-step setup (init + start)
- Task-scoped memory (ephemeral)
- Temporary sessions
- Lightweight coordination

### HIVE-MIND (Complex Projects)

**Best for**: Multi-component systems, long projects, team coordination

```bash
# Step 1: Initialize hive-mind
npx claude-flow@alpha hive-mind init -t hierarchical-mesh -c byzantine -m 15

# Step 2: Spawn agents
npx claude-flow@alpha hive-mind spawn -n 5                    # 5 workers
npx claude-flow@alpha hive-mind spawn -n 2 -r specialist      # 2 specialists

# Step 3: Submit task
npx claude-flow@alpha hive-mind task "your project task"

# Or: Launch Claude Code with hive-mind coordination
npx claude-flow@alpha hive-mind spawn --claude -o "your project task"

# Check status
npx claude-flow@alpha hive-mind status
```

**Characteristics**:
- Interactive wizard setup
- Project-wide SQLite memory (persistent)
- Resumable sessions
- Queen-led coordination with consensus
- Role-based agents (queen, worker, specialist, scout)

### DAA (Decentralized Autonomous Agents)

**Best for**: Autonomous workflows, learning systems, self-improving agents

```javascript
mcp__claude-flow__daa_agent_create {
  id: "learner-001",
  name: "Adaptive Learner",
  type: "coder",
  cognitivePattern: "adaptive",
  learningRate: 0.1,
  enableMemory: true
}
```

**Characteristics**:
- Self-learning agents with adaptation
- Cognitive patterns (convergent, divergent, lateral, systems, critical, adaptive)
- Knowledge sharing between agents
- Workflow autonomy

---

## Swarm Topologies

### MESH (Fully Connected)

```
    A ---- B
   /|\    /|\
  / | \  / | \
 /  |  \/  |  \
C --+--XX--+-- D
 \  |  /\  |  /
  \ | /  \ | /
   \|/    \|/
    E ---- F
```

**Characteristics**:
- Every agent connects to every other
- Maximum collaboration potential
- Higher communication overhead
- O(n^2) connections

**Best for**:
- High collaboration tasks
- Shared context requirements
- Small-medium teams (3-8 agents)

**MCP Configuration**:
```javascript
mcp__claude-flow__swarm_init {
  topology: "mesh",
  maxAgents: 8
}
```

---

### RING (Circular)

```
    A --> B
    ^     |
    |     v
    F     C
    ^     |
    |     v
    E <-- D
```

**Characteristics**:
- Each agent connects to neighbors only
- Sequential processing
- Minimal overhead
- O(n) connections

**Best for**:
- Pipeline/sequential processing
- Data transformation chains
- Review workflows
- Stage-based work

**MCP Configuration**:
```javascript
mcp__claude-flow__swarm_init {
  topology: "ring",
  maxAgents: 10
}
```

---

### STAR (Hub and Spoke)

```
        W1
        |
   W5 - C - W2
       /|\
      / | \
    W4  |  W3
        W6
```

**Characteristics**:
- Central coordinator (C)
- All agents report to center
- Single source of truth
- Moderate overhead

**Best for**:
- Centralized control
- Aggregation tasks
- Monitoring scenarios
- Central decision-making

**MCP Configuration**:
```javascript
mcp__claude-flow__swarm_init {
  topology: "star",
  maxAgents: 12
}
```

---

### HIERARCHICAL (Tree Structure)

```
        Queen
       /  |  \
      /   |   \
   Lead1 Lead2 Lead3
   / \    |    / \
  W1 W2   W3  W4 W5
```

**Characteristics**:
- Queen coordinator at top
- Team leads for coordination
- Workers at leaf nodes
- Clear delegation structure
- Raft consensus for leader election

**Best for**:
- Clear task delegation
- Large teams
- Structured workflows
- Lower communication overhead

**MCP Configuration**:
```javascript
mcp__claude-flow__swarm_init {
  topology: "hierarchical",
  maxAgents: 20
}

mcp__claude-flow__coordination_topology {
  action: "set",
  type: "hierarchical",
  consensusAlgorithm: "raft"
}
```

---

### ADAPTIVE (Self-Optimizing)

**Characteristics**:
- Dynamically adjusts topology based on workload
- Self-optimizing communication patterns
- Balances overhead vs. collaboration
- Learns from usage patterns

**Best for**:
- Variable workloads
- Unknown communication patterns
- Long-running projects
- Projects with changing requirements

**MCP Configuration**:
```javascript
mcp__claude-flow__swarm_init {
  topology: "adaptive",
  maxAgents: 15
}
```

---

### HIERARCHICAL-MESH (Hybrid)

```
      Queen
     /     \
  Lead1   Lead2
  / | \   / | \
 A--B--C D--E--F
  \ | /   \ | /
   mesh     mesh
```

**Characteristics**:
- Hierarchical control structure
- Mesh collaboration within teams
- Cross-team coordination via leads
- Best of both patterns

**Best for**:
- Multiple teams working together
- Complex systems with subsystems
- Projects needing both control and collaboration

**MCP Configuration**:
```javascript
mcp__claude-flow__coordination_topology {
  action: "set",
  type: "hierarchical-mesh",
  consensusAlgorithm: "raft",
  maxNodes: 20,
  redundancy: 2
}
```

---

## Consensus Algorithms

### Raft (Leader Election)

```javascript
mcp__claude-flow__coordination_topology {
  action: "set",
  consensusAlgorithm: "raft"
}
```

**Best for**: Hierarchical topologies, leader-based coordination
**Features**: Leader election, log replication, fault tolerance

### Byzantine (Fault Tolerant)

```javascript
mcp__claude-flow__coordination_topology {
  action: "set",
  consensusAlgorithm: "byzantine"
}
```

**Best for**: High-security, untrusted environments
**Features**: Handles malicious nodes, f < n/3 fault tolerance

### Gossip (Eventually Consistent)

```javascript
mcp__claude-flow__coordination_topology {
  action: "set",
  consensusAlgorithm: "gossip"
}
```

**Best for**: Large-scale, loose coordination
**Features**: Epidemic protocol, eventual consistency, scalable

### CRDT (Conflict-Free Replication)

```javascript
mcp__claude-flow__coordination_topology {
  action: "set",
  consensusAlgorithm: "crdt"
}
```

**Best for**: Concurrent updates, automatic merge
**Features**: Conflict-free, automatic resolution, high availability

---

## Decision Trees

### Mode Selection

```
Is your task...
|
|-- Quick/focused (< 1 hour)?
|   +-- Use SWARM
|       npx claude-flow@alpha swarm init && swarm start -o "task"
|
|-- Complex/multi-day?
|   +-- Use HIVE-MIND
|       npx claude-flow@alpha hive-mind init -t hierarchical-mesh
|
|-- Need to resume later?
|   +-- Use HIVE-MIND (persistent sessions)
|
|-- Experimental/research?
|   +-- Use SWARM (lightweight)
|
|-- Self-learning required?
|   +-- Use DAA (autonomous agents)
|       mcp__claude-flow__daa_agent_create
|
+-- Distributed/fault-tolerant?
    +-- Use HIVE-MIND with byzantine consensus
        --consensus byzantine
```

### Topology Selection

```
How do your agents need to communicate?
|
|-- Everyone talks to everyone? -------> MESH
|
|-- Clear boss/worker structure? ------> HIERARCHICAL
|
|-- Work flows in stages? -------------> RING
|
|-- Central coordinator needed? -------> STAR
|
|-- Multiple teams + cross-team? ------> HIERARCHICAL-MESH
|
+-- Dynamic/changing requirements? ----> ADAPTIVE
```

### Consensus Selection

```
What consistency do you need?
|
|-- Strong consistency, leader-based?
|   +-- Raft
|
|-- Need to handle malicious agents?
|   +-- Byzantine
|
|-- Large scale, eventual consistency OK?
|   +-- Gossip
|
+-- Concurrent updates, auto-merge?
    +-- CRDT
```

---

## Load Balancing

```javascript
// Round-robin
mcp__claude-flow__coordination_load_balance {
  action: "set",
  algorithm: "round-robin"
}

// Least connections
mcp__claude-flow__coordination_load_balance {
  action: "set",
  algorithm: "least-connections"
}

// Weighted
mcp__claude-flow__coordination_load_balance {
  action: "set",
  algorithm: "weighted",
  weights: { "agent-1": 2, "agent-2": 1 }
}

// Adaptive (recommended)
mcp__claude-flow__coordination_load_balance {
  action: "set",
  algorithm: "adaptive"
}
```

---

## Topology Recommendations by Project Type

| Project Type | Recommended Topology | Consensus |
|--------------|---------------------|-----------|
| Single feature | Swarm (no topology) | N/A |
| API development | Mesh (3-5 agents) | Raft |
| Full-stack app | Hierarchical (5-10) | Raft |
| Microservices | Hierarchical-mesh | CRDT |
| Research project | Mesh | Gossip |
| Pipeline processing | Ring (sequential) | Raft |
| Data transformation | Ring (chain) | Raft |
| Central monitoring | Star (hub) | Raft |
| Aggregation tasks | Star (spoke) | Raft |
| Enterprise system | Hierarchical-mesh | Byzantine |
| Learning system | DAA + Adaptive | Adaptive |
