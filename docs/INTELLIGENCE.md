# Intelligence Layer Reference

Complete guide to claude-flow v3 intelligence features.

> **CLI Reference**: For CLI commands including `hooks intelligence`, see [CLI_REFERENCE.md](CLI_REFERENCE.md).
>
> **MCP Reference**: For all intelligence MCP tools, see [MCP_REFERENCE.md](MCP_REFERENCE.md).

---

## RuVector Intelligence Overview

The v3 intelligence layer provides neural-enhanced routing, pattern learning, and adaptive optimization.

### Components

| Component | Purpose | Performance |
|-----------|---------|-------------|
| **SONA** | Trajectory-based reinforcement learning | Continuous improvement |
| **MoE** | Mixture of Experts routing | 8 expert agents |
| **HNSW** | Hierarchical vector indexing | 150x-12,500x faster search |
| **Flash Attention** | Efficient attention mechanism | 2.49x-7.47x speedup |
| **EWC++** | Elastic Weight Consolidation | Prevents forgetting |
| **LoRA** | Low-Rank Adaptation | 128x memory compression |

---

## SONA Optimizer (Reinforcement Learning)

SONA (Self-Organizing Neural Architecture) uses trajectory-based learning to improve from experience.

### SONA MCP Tools

The neural subsystem provides direct access to pattern training and optimization.

#### Train Patterns

```javascript
mcp__claude-flow__neural_train {
  patterns: ["authentication", "caching", "api-design"],
  epochs: 10,
  learningRate: 0.001,
  validation: true
}
// Returns: { trained: 3, loss: 0.023, accuracy: 0.94 }
```

#### Run Predictions

```javascript
mcp__claude-flow__neural_predict {
  input: "Implement user session management",
  model: "routing",
  topK: 3
}
// Returns: { predictions: [{label: "security", confidence: 0.91}, ...] }
```

#### View Learned Patterns

```javascript
mcp__claude-flow__neural_patterns {
  namespace: "routing",
  limit: 20,
  minConfidence: 0.7
}
// Returns: { patterns: [{name: "auth-flow", hits: 45, confidence: 0.92}, ...] }
```

#### Compress Models

```javascript
mcp__claude-flow__neural_compress {
  model: "routing",
  targetSize: 0.5,        // 50% of original size
  method: "quantization"  // quantization, pruning, distillation
}
// Returns: { originalSize: "2.4MB", compressedSize: "1.2MB", accuracy: 0.93 }
```

#### Optimize Performance

```javascript
mcp__claude-flow__neural_optimize {
  model: "routing",
  target: "latency",      // latency, memory, accuracy
  budget: 100             // ms for latency, MB for memory
}
// Returns: { optimized: true, improvement: "2.3x", tradeoff: "accuracy -0.02" }
```

### Trajectory-Based Learning

SONA uses trajectory-based learning to improve from experience.

### Start Trajectory

```javascript
mcp__claude-flow__hooks_intelligence_trajectory-start {
  task: "Implement user authentication",
  agent: "coder"
}
// Returns: { trajectoryId: "traj-xxx", ... }
```

### Record Steps

```javascript
mcp__claude-flow__hooks_intelligence_trajectory-step {
  trajectoryId: "traj-xxx",
  action: "Created auth service",
  result: "success",
  quality: 0.9
}
```

### End Trajectory

```javascript
mcp__claude-flow__hooks_intelligence_trajectory-end {
  trajectoryId: "traj-xxx",
  success: true,
  feedback: "Clean implementation, good test coverage"
}
```

### Force Learning Cycle

```javascript
mcp__claude-flow__hooks_intelligence_learn {
  consolidate: true,
  trajectoryIds: ["traj-xxx", "traj-yyy"]
}
```

---

## MoE (Mixture of Experts) Router

Routes tasks to optimal expert agents based on complexity analysis.

### 8 Expert Agents

| Expert | Domain |
|--------|--------|
| `coder` | Implementation |
| `tester` | Testing |
| `reviewer` | Code review |
| `architect` | System design |
| `security` | Security audit |
| `performance` | Optimization |
| `researcher` | Analysis |
| `coordinator` | Orchestration |

### Route Task

```javascript
mcp__claude-flow__hooks_route {
  task: "Implement OAuth2 authentication",
  context: "REST API project"
}
// Returns: { recommended: "security", confidence: 0.92, ... }
```

### Explain Routing

```javascript
mcp__claude-flow__hooks_explain {
  task: "Implement OAuth2 authentication",
  agent: "security",
  verbose: true
}
```

---

## HNSW Vector Indexing

Hierarchical Navigable Small World graphs for fast similarity search.

### Performance

| Dataset Size | Brute Force | HNSW | Speedup |
|--------------|-------------|------|---------|
| 1,000 | 10ms | 0.1ms | 100x |
| 10,000 | 100ms | 0.2ms | 500x |
| 100,000 | 1,000ms | 0.5ms | 2,000x |
| 1,000,000 | 10,000ms | 0.8ms | 12,500x |

### Store Pattern

```javascript
mcp__claude-flow__hooks_intelligence_pattern-store {
  pattern: "REST API with JWT authentication",
  type: "architecture",
  confidence: 0.95,
  metadata: {
    domain: "backend",
    language: "typescript"
  }
}
```

### Search Patterns

```javascript
mcp__claude-flow__hooks_intelligence_pattern-search {
  query: "authentication flow",
  topK: 10,
  minConfidence: 0.7,
  namespace: "pattern"
}
```

---

## Flash Attention

O(N) memory complexity attention mechanism that dramatically reduces memory usage while improving speed.

### How Flash Attention Works

Flash Attention achieves O(N) memory complexity vs the standard O(N^2) by:

1. **Tiled Computation**: Processes attention in tiles that fit in SRAM
2. **Online Softmax**: Computes softmax incrementally without materializing full attention matrix
3. **Recomputation**: Recomputes attention during backward pass instead of storing
4. **Memory Efficiency**: Never stores the full N x N attention matrix

### Memory Comparison

| Sequence Length | Standard Attention | Flash Attention | Memory Saved |
|-----------------|-------------------|-----------------|--------------|
| 512 | 1 MB | 4 KB | 256x |
| 1024 | 4 MB | 8 KB | 512x |
| 2048 | 16 MB | 16 KB | 1024x |
| 4096 | 64 MB | 32 KB | 2048x |

### Performance

| Sequence Length | Standard | Flash | Speedup |
|-----------------|----------|-------|---------|
| 512 | 1.0x | 2.49x | 2.49x |
| 1024 | 1.0x | 3.67x | 3.67x |
| 2048 | 1.0x | 5.12x | 5.12x |
| 4096 | 1.0x | 7.47x | 7.47x |

### Automatic Fallback

Flash Attention includes automatic fallback to standard attention when:

- Hardware does not support optimized kernels
- Sequence length is below threshold (< 128 tokens)
- Custom attention masks are required
- Debugging mode is enabled

```javascript
// Check current mode
mcp__claude-flow__hooks_intelligence {
  showStatus: true
}
// Returns: { flashAttention: { enabled: true, mode: "flash", fallback: false } }
```

### Compute Attention

```javascript
mcp__claude-flow__hooks_intelligence_attention {
  query: "authentication best practices",
  mode: "flash",       // flash, moe, hyperbolic
  topK: 5
}
```

---

## EWC++ Consolidation (Elastic Weight Consolidation)

Prevents catastrophic forgetting by preserving important learned patterns during continuous learning.

### The Problem: Catastrophic Forgetting

When neural networks learn new tasks sequentially, they tend to "forget" previously learned knowledge. This happens because the weights optimal for the new task overwrite weights that were important for old tasks.

### How EWC++ Solves This

EWC++ uses a quadratic penalty term that discourages changes to weights that were important for previous tasks:

1. **Fisher Information Matrix**: Calculates importance of each weight for past tasks
2. **Quadratic Penalty**: Adds penalty term proportional to (weight change)^2 x importance
3. **Selective Protection**: Only protects truly important weights, allowing others to adapt
4. **Online Updates**: Incrementally updates importance without storing all past data

### EWC++ vs Original EWC

| Feature | EWC | EWC++ |
|---------|-----|-------|
| Memory | Grows with tasks | Constant |
| Fisher updates | Store per task | Online merge |
| Performance | Degrades over time | Stable |
| Computation | O(tasks) | O(1) |

### Weight Importance Visualization

```
Weight Importance Distribution:
|
|  ████                          High importance (protected)
|  ████ ██
|  ████ ██ ████                  Medium importance
|  ████ ██ ████ ██ ██ ████
|  ████ ██ ████ ██ ██ ████ ██   Low importance (can change)
+---------------------------------> Weights
```

### How It Works

1. Identifies important parameters from successful trajectories
2. Adds quadratic penalty to prevent overwriting
3. Maintains Fisher information matrix
4. Automatically consolidates during learning

### Trigger Consolidation

```javascript
mcp__claude-flow__hooks_intelligence_learn {
  consolidate: true
}
```

### Advanced Consolidation Options

```javascript
mcp__claude-flow__hooks_intelligence_learn {
  consolidate: true,
  ewcLambda: 5000,           // Penalty strength (higher = more protection)
  trajectoryIds: ["traj-1", "traj-2"],
  forceOnlineUpdate: true    // Update Fisher matrix incrementally
}
```

### Check Consolidation Status

```javascript
mcp__claude-flow__hooks_intelligence_stats {
  detailed: true
}
// Returns: {
//   ewc: {
//     protectedWeights: 1247,
//     totalWeights: 8192,
//     avgImportance: 0.34,
//     tasksConsolidated: 5
//   }
// }
```

---

## LoRA Adapters

Low-Rank Adaptation for memory-efficient fine-tuning.

### Memory Savings

| Rank | Compression | Memory |
|------|-------------|--------|
| 4 | 256x | Minimal |
| 8 | 128x | Recommended |
| 16 | 64x | Higher quality |
| 32 | 32x | Best quality |

### Configuration

Built into the intelligence layer, activated automatically when memory-constrained.

---

## Model Routing (ADR-026)

Intelligent routing to Claude models based on task complexity.

### Models

| Model | Use Case | Cost |
|-------|----------|------|
| **Haiku** | Simple tasks, formatting, quick responses | Lowest |
| **Sonnet** | Most development tasks (default) | Medium |
| **Opus** | Complex reasoning, architecture, critical code | Highest |

### Route to Model

```javascript
mcp__claude-flow__hooks_model-route {
  task: "Implement distributed cache system",
  preferCost: false,
  preferSpeed: false
}
// Returns: { model: "opus", confidence: 0.89, ... }
```

### Record Outcome

```javascript
mcp__claude-flow__hooks_model-outcome {
  task: "Implement distributed cache system",
  model: "opus",
  outcome: "success"    // success, failure, escalated
}
```

### Get Statistics

```javascript
mcp__claude-flow__hooks_model-stats {}
```

---

## ONNX Embeddings

Local neural embeddings for semantic search.

### Configuration

| Model | Dimensions | Size |
|-------|------------|------|
| `all-MiniLM-L6-v2` | 384 | 23MB |
| `all-mpnet-base-v2` | 768 | 438MB |

### Initialize

```javascript
mcp__claude-flow__embeddings_init {
  model: "all-MiniLM-L6-v2",
  hyperbolic: true,
  curvature: -1,
  cacheSize: 256
}
```

### Generate Embedding

```javascript
mcp__claude-flow__embeddings_generate {
  text: "User authentication with JWT",
  hyperbolic: false,
  normalize: true
}
```

### Compare Texts

```javascript
mcp__claude-flow__embeddings_compare {
  text1: "User authentication",
  text2: "Login system",
  metric: "cosine"      // cosine, euclidean, poincare
}
```

### Semantic Search

```javascript
mcp__claude-flow__embeddings_search {
  query: "authentication patterns",
  topK: 10,
  threshold: 0.7,
  namespace: "patterns"
}
```

### Hyperbolic Embeddings

Hyperbolic space embeddings provide superior representation for hierarchical and tree-like data structures.

#### Why Hyperbolic Space?

| Property | Euclidean Space | Hyperbolic Space |
|----------|-----------------|------------------|
| Growth | Polynomial (r^d) | Exponential (e^r) |
| Tree embedding | Distortion O(n) | Distortion O(1) |
| Hierarchy preservation | Poor | Excellent |
| Memory efficiency | Lower | Higher |

#### Use Cases for Hyperbolic Embeddings

- **Taxonomy hierarchies**: Product categories, biological classifications
- **Organizational structures**: Company hierarchies, team structures
- **Code dependencies**: Import trees, class inheritance
- **Knowledge graphs**: Entity relationships, ontologies
- **File systems**: Directory structures

#### Hyperbolic Operations

```javascript
// Status - check hyperbolic embedding configuration
mcp__claude-flow__embeddings_hyperbolic {
  action: "status"
}
// Returns: { enabled: true, curvature: -1, dimension: 384, model: "poincare" }

// Convert Euclidean to Poincare ball model
mcp__claude-flow__embeddings_hyperbolic {
  action: "convert",
  embedding: [0.1, 0.2, 0.3, ...],
  curvature: -1              // Negative curvature for hyperbolic space
}
// Returns: { hyperbolic: [0.099, 0.197, 0.292, ...], norm: 0.364 }

// Poincare distance (geodesic distance in hyperbolic space)
mcp__claude-flow__embeddings_hyperbolic {
  action: "distance",
  embedding1: [...],
  embedding2: [...]
}
// Returns: { distance: 2.34, euclideanEquivalent: 0.89 }

// Mobius addition (hyperbolic vector addition)
mcp__claude-flow__embeddings_hyperbolic {
  action: "add",
  embedding1: [...],
  embedding2: [...],
  curvature: -1
}

// Parallel transport (move vectors between tangent spaces)
mcp__claude-flow__embeddings_hyperbolic {
  action: "transport",
  vector: [...],
  fromPoint: [...],
  toPoint: [...]
}
```

#### Hierarchical Search with Hyperbolic Embeddings

```javascript
// Search with hierarchy-aware ranking
mcp__claude-flow__embeddings_search {
  query: "authentication service",
  topK: 10,
  threshold: 0.7,
  namespace: "code",
  hyperbolic: true,          // Enable hyperbolic distance
  hierarchyWeight: 0.3       // Balance semantic vs hierarchical similarity
}
```

### Neural Substrate

```javascript
mcp__claude-flow__embeddings_neural {
  action: "status",     // status, init, drift, consolidate, adapt
  driftThreshold: 0.3,
  decayRate: 0.01
}
```

---

## Intelligence Status

### Get Full Status

```javascript
mcp__claude-flow__hooks_intelligence {
  showStatus: true
}
```

### Enable All Features

```javascript
mcp__claude-flow__hooks_intelligence {
  enableSona: true,
  enableMoe: true,
  enableHnsw: true,
  forceTraining: false
}
```

### Get Statistics

```javascript
mcp__claude-flow__hooks_intelligence_stats {
  detailed: true
}
```

### Reset Intelligence

```javascript
mcp__claude-flow__hooks_intelligence-reset {}
```

---

## Decision Tree: Intelligence Selection

```
What level of learning do you need?
|
|-- Just routing tasks efficiently?
|   +-- Enable MoE only
|       { enableMoe: true }
|
|-- Need fast pattern search?
|   +-- Enable HNSW
|       { enableHnsw: true }
|
|-- Want to learn from outcomes?
|   +-- Enable SONA trajectories
|       { enableSona: true }
|
|-- Need to preserve learned patterns?
|   +-- Enable EWC++ consolidation
|       hooks_intelligence_learn { consolidate: true }
|
+-- Full intelligence suite?
    +-- Enable ALL components
        { enableSona: true, enableMoe: true, enableHnsw: true }
```

---

## Intelligence-Driven Development Pattern

```
1. Enable SONA at project start
   mcp__claude-flow__hooks_intelligence { enableSona: true }

2. Start trajectory for each major task
   hooks_intelligence_trajectory-start { task: "..." }

3. Record steps during development
   hooks_intelligence_trajectory-step { action: "...", quality: 0.9 }

4. End trajectory with outcome
   hooks_intelligence_trajectory-end { success: true }

5. Search for similar past solutions
   hooks_intelligence_pattern-search { query: "..." }

6. Let EWC++ consolidate periodically
   hooks_intelligence_learn { consolidate: true }

7. Transfer patterns to new projects
   hooks_transfer { sourcePath: "/old-project" }
```
