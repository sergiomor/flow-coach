# Flow Coach Configuration Examples

Example configuration files for different use cases. Copy these to your project's `.claude/settings.json`.

---

## Available Configurations

### 1. **Default** (`flow-coach-settings-default.json`)
**Balanced settings for most projects**

```json
{
  "thresholds": {
    "intelligence": 50,
    "memory": 60,
    "workers": 40
  }
}
```

**Best for:**
- General development work
- Medium-sized projects
- Balanced resource usage

---

### 2. **Conservative** (`flow-coach-settings-conservative.json`)
**Resource-constrained environments**

```json
{
  "thresholds": {
    "intelligence": 70,
    "memory": 80,
    "workers": 60
  },
  "templates": {
    "hooks": "minimal"
  }
}
```

**Best for:**
- Laptops with limited resources
- Simple projects
- Quick prototypes
- Development on lower-spec machines

**Philosophy:** Only initialize heavy features for truly complex tasks.

---

### 3. **Aggressive** (`flow-coach-settings-aggressive.json`)
**High-performance systems**

```json
{
  "thresholds": {
    "intelligence": 40,
    "memory": 50,
    "workers": 30
  },
  "templates": {
    "hooks": "full",
    "intelligence": "full"
  }
}
```

**Best for:**
- Workstations with ample resources
- Cloud development environments
- Complex, learning-heavy projects
- Research and experimentation

**Philosophy:** Enable advanced features for most tasks to maximize learning and automation.

---

### 4. **Enterprise** (`flow-coach-settings-enterprise.json`)
**Security-focused with compliance**

```json
{
  "thresholds": {
    "intelligence": 60,
    "memory": 70,
    "workers": 50
  },
  "security": {
    "alwaysAudit": true,
    "byzantineConsensus": true
  },
  "compliance": {
    "enableClaimsSystem": true,
    "humanApprovalGates": ["security", "deployment"]
  }
}
```

**Best for:**
- Enterprise environments
- Regulated industries (finance, healthcare, government)
- Security-critical applications
- Projects requiring audit trails

**Philosophy:** Balance capability with security, audit everything, require human oversight.

---

## How to Use

### 1. Copy to Your Project

```bash
# Choose a configuration
cp .claude/skills/flow-coach/resources/examples/configs/flow-coach-settings-default.json .claude/settings.json

# Or create custom
cat > .claude/settings.json << 'EOF'
{
  "skills": {
    "flow-coach": {
      "thresholds": {
        "intelligence": 50,
        "memory": 60
      }
    }
  }
}
EOF
```

### 2. Customize

Edit `.claude/settings.json` to match your needs:

```json
{
  "skills": {
    "flow-coach": {
      "thresholds": {
        "intelligence": 55,  // Slightly higher than default
        "memory": 65,        // Moderate memory usage
        "workers": 45        // More workers
      },
      "templates": {
        "hooks": "standard"
      }
    }
  }
}
```

### 3. Verify

Invoke Flow Coach and check if your settings are applied:

```bash
/flow-coach Build a REST API
```

Look for initialization commands matching your thresholds.

---

## Configuration Reference

### Thresholds (0-100)

| Setting | Default | Conservative | Aggressive | Enterprise |
|---------|---------|--------------|------------|------------|
| `intelligence` | 50 | 70 | 40 | 60 |
| `memory` | 60 | 80 | 50 | 70 |
| `workers` | 40 | 60 | 30 | 50 |

**Lower threshold** = Feature enabled more often
**Higher threshold** = Feature enabled less often (only for complex tasks)

### Templates

**Hooks:**
- `minimal` - Basic hooks only (pre-task, post-task)
- `standard` - Common hooks (pre/post task/edit, session management)
- `full` - All available hooks (includes intelligence, workers, etc.)

**Intelligence:**
- `basic` - SONA only
- `standard` - SONA + MoE
- `full` - SONA + MoE + HNSW + Flash Attention + EWC++

### Auto-initialization

- `autoInit: true` - Automatically run init commands when [E] EXECUTE is chosen
- `autoInit: false` - Only show init commands, user must run manually

---

## Environment-Specific Configurations

### Development
```json
{
  "thresholds": { "intelligence": 45, "memory": 55, "workers": 35 },
  "templates": { "hooks": "standard", "intelligence": "full" }
}
```

### Staging
```json
{
  "thresholds": { "intelligence": 55, "memory": 65, "workers": 45 },
  "templates": { "hooks": "standard", "intelligence": "standard" }
}
```

### Production
```json
{
  "thresholds": { "intelligence": 70, "memory": 75, "workers": 60 },
  "templates": { "hooks": "minimal", "intelligence": "basic" },
  "security": { "alwaysAudit": true }
}
```

---

## Troubleshooting

### "Configuration not being used"

**Check:**
1. File is named `.claude/settings.json` (not `settings.json`)
2. JSON is valid (use `jq . .claude/settings.json`)
3. Location is project root (where `.claude/` directory exists)

### "Too many initializations"

**Lower your thresholds** (make them more restrictive):
```json
{
  "thresholds": {
    "intelligence": 70,  // Higher = less often
    "memory": 80
  }
}
```

### "Features not initializing"

**Lower your thresholds** (make them more aggressive):
```json
{
  "thresholds": {
    "intelligence": 40,  // Lower = more often
    "memory": 50
  }
}
```

---

## See Also

- [INITIALIZATION.md](../../../docs/INITIALIZATION.md) - Complete initialization guide
- [CLI_REFERENCE.md](../../../docs/CLI_REFERENCE.md) - All CLI commands
- [README.md](../../../README.md) - Main documentation
