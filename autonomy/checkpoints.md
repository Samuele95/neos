# Checkpoint System

Specification for defining conditions that pause execution in checkpoint mode.

---

## 1. Overview

Checkpoints define conditions that trigger execution pauses, allowing user review and intervention at meaningful moments during field dynamics.

---

## 2. Checkpoint Types

### 2.1 Built-in Checkpoints

| Checkpoint | Description | Default |
|------------|-------------|---------|
| `attractor_emerged` | New attractor detected | enabled |
| `attractor_collapsed` | Attractor disappeared | enabled |
| `attractor_merged` | Two attractors combined | enabled |
| `coherence_threshold` | Coherence crosses threshold | 0.8 |
| `cycle_complete` | After each dynamics cycle | disabled |
| `stage_complete` | Pipeline stage finished | enabled |
| `pattern_created` | New pattern injected | disabled |
| `pattern_died` | Pattern activation → 0 | enabled |
| `conflict_detected` | Cross-field conflict | enabled |
| `stability_warning` | Approaching instability | enabled |
| `energy_minimum` | Local energy minimum found | disabled |
| `resonance_spike` | Sudden resonance increase | disabled |

### 2.2 Custom Checkpoints

User-defined conditions using the checkpoint expression language.

```
> nf checkpoint add "coherence > 0.75"
[CHECKPOINT] Added: coherence > 0.75

> nf checkpoint add "patterns.count > 10"
[CHECKPOINT] Added: patterns.count > 10

> nf checkpoint add "@security.activation < 0.3"
[CHECKPOINT] Added: @security.activation < 0.3
```

---

## 3. Condition Language

### 3.1 Grammar

```
checkpoint_expr := condition (AND|OR condition)*
condition       := metric COMPARATOR value
                 | metric COMPARATOR metric
                 | function_call
                 | NOT condition
                 | '(' checkpoint_expr ')'

metric          := field_metric | pattern_metric | system_metric
field_metric    := 'coherence' | 'energy' | 'entropy' | 'stability'
                 | 'patterns.count' | 'attractors.count'
pattern_metric  := '@' pattern_name '.' property
system_metric   := '$' field_name '.' field_metric
                 | 'cross_resonance' '(' field1 ',' field2 ')'

property        := 'activation' | 'resonance_sum' | 'in_attractor'
                 | 'tag' '(' string ')'

COMPARATOR      := '>' | '<' | '>=' | '<=' | '==' | '!='
value           := NUMBER | STRING | BOOLEAN
function_call   := function_name '(' args ')'
```

### 3.2 Available Metrics

**Field Metrics:**
| Metric | Type | Description |
|--------|------|-------------|
| `coherence` | float | Field coherence (0-1) |
| `energy` | float | Total field energy |
| `entropy` | float | Pattern distribution entropy |
| `stability` | float | Attractor stability measure |
| `patterns.count` | int | Number of active patterns |
| `patterns.above(n)` | int | Patterns with activation > n |
| `attractors.count` | int | Number of attractors |
| `attractors.max_coherence` | float | Highest attractor coherence |
| `cycle` | int | Current cycle number |

**Pattern Metrics (prefix with @):**
| Metric | Type | Description |
|--------|------|-------------|
| `@name.activation` | float | Pattern activation level |
| `@name.resonance_sum` | float | Sum of resonances with others |
| `@name.in_attractor` | bool | Is pattern in an attractor |
| `@name.tag(t)` | bool | Pattern has tag t |
| `@name.age` | int | Cycles since creation |

**System Metrics (prefix with $):**
| Metric | Type | Description |
|--------|------|-------------|
| `$field.coherence` | float | Specific field's coherence |
| `$field.patterns.count` | int | Specific field's pattern count |
| `cross_resonance(f1, f2)` | float | Cross-field resonance |
| `coupling_stability` | float | Coupling matrix stability |
| `fields.active.count` | int | Number of active fields |

---

### 3.3 Operators

**Comparison:**
| Operator | Meaning |
|----------|---------|
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater or equal |
| `<=` | Less or equal |
| `==` | Equal |
| `!=` | Not equal |

**Logical:**
| Operator | Meaning |
|----------|---------|
| `AND` | Both conditions |
| `OR` | Either condition |
| `NOT` | Negation |

**Special:**
| Operator | Meaning |
|----------|---------|
| `CHANGED` | Value changed since last check |
| `INCREASED` | Value increased |
| `DECREASED` | Value decreased |
| `CROSSES` | Value crosses threshold |

---

## 4. Expression Examples

### 4.1 Simple Conditions

```
# Coherence threshold
coherence > 0.8

# Pattern count
patterns.count >= 5

# Specific pattern
@security.activation < 0.5

# Attractor presence
attractors.count > 0
```

### 4.2 Compound Conditions

```
# High coherence with many patterns
coherence > 0.75 AND patterns.count > 8

# Either attractor emerged or coherence threshold
attractor_emerged OR coherence > 0.9

# Pattern dying while field coherent
@logging.activation < 0.1 AND coherence > 0.6

# Cross-field condition
$technical.coherence > 0.7 AND $user.coherence > 0.7
```

### 4.3 Change Detection

```
# Coherence increased significantly
coherence INCREASED BY 0.1

# Pattern activation dropped
@security.activation DECREASED

# Coherence crossed threshold (either direction)
coherence CROSSES 0.7

# Attractor count changed
attractors.count CHANGED
```

### 4.4 Function Conditions

```
# Any pattern below threshold
any_pattern_below(0.2)

# All core patterns above threshold
all_patterns_above(0.5, tag="core")

# Resonance between specific patterns
resonance(@security, @input) > 0.7

# Basin coverage
basin_coverage(@security_focus) > 0.6
```

---

## 5. Checkpoint Management

### 5.1 Adding Checkpoints

```
> nf checkpoint add "coherence > 0.85"
[CHECKPOINT] Added: coherence > 0.85
  ID: cp_001
  Priority: normal

> nf checkpoint add "attractor_emerged" --priority high
[CHECKPOINT] Added: attractor_emerged
  ID: cp_002
  Priority: high

> nf checkpoint add "@critical_pattern.activation < 0.3" --name "critical_dying"
[CHECKPOINT] Added: @critical_pattern.activation < 0.3
  ID: cp_003
  Name: critical_dying
  Priority: normal
```

### 5.2 Listing Checkpoints

```
> nf checkpoint list
[CHECKPOINTS] 5 active

  ID       NAME              CONDITION                    PRIORITY  TRIGGERS
  cp_001   -                 coherence > 0.85             normal    0
  cp_002   -                 attractor_emerged            high      2
  cp_003   critical_dying    @critical.activation < 0.3  normal    0
  cp_004   -                 cycle_complete               low       15
  cp_005   -                 stability_warning            high      0

> nf checkpoint list --verbose
[CHECKPOINTS] Detailed view

  cp_002: attractor_emerged [HIGH]
    Triggered: 2 times
    Last trigger: Cycle 3 - attractor 'security_focus'
    Action: pause

  cp_003: critical_dying [NORMAL]
    Condition: @critical_pattern.activation < 0.3
    Triggered: 0 times
    Action: pause
```

### 5.3 Removing Checkpoints

```
> nf checkpoint remove cp_001
[CHECKPOINT] Removed: cp_001

> nf checkpoint remove --name critical_dying
[CHECKPOINT] Removed: critical_dying

> nf checkpoint clear
[CHECKPOINT] Cleared all custom checkpoints
  Built-in checkpoints preserved
```

### 5.4 Enabling/Disabling

```
> nf checkpoint disable cp_002
[CHECKPOINT] Disabled: attractor_emerged

> nf checkpoint enable cp_002
[CHECKPOINT] Enabled: attractor_emerged

> nf checkpoint disable --all
[CHECKPOINT] Disabled all checkpoints
  Mode effectively becomes 'auto'
```

---

## 6. Checkpoint Actions

### 6.1 Default Action: Pause

```
[CHECKPOINT] coherence > 0.85
  Condition met: coherence = 0.87
[PAUSED] Review and 'nf proceed' to continue
```

### 6.2 Custom Actions

```
> nf checkpoint add "coherence > 0.9" --action log
[CHECKPOINT] Added with action: log (no pause)

> nf checkpoint add "stability < 0.3" --action "notify,pause"
[CHECKPOINT] Added with actions: notify, pause

> nf checkpoint add "attractor_emerged" --action "snapshot,pause"
[CHECKPOINT] Added with actions: snapshot, pause
```

**Available Actions:**
| Action | Behavior |
|--------|----------|
| `pause` | Pause execution (default) |
| `log` | Log event, continue |
| `notify` | Send notification |
| `snapshot` | Auto-commit current state |
| `branch` | Create branch at this point |
| `callback` | Call user-defined handler |

### 6.3 Action Chains

```
> nf checkpoint add "attractor_emerged" --action "snapshot,log,pause"
```

Execution order: snapshot → log → pause

---

## 7. Checkpoint Events

### 7.1 Event Data

When a checkpoint triggers, event data is captured:

```json
{
  "checkpoint_id": "cp_002",
  "condition": "attractor_emerged",
  "triggered_at": "2024-01-15T14:22:00Z",
  "cycle": 3,
  "field": "main",

  "context": {
    "coherence_before": 0.68,
    "coherence_after": 0.71,
    "patterns_changed": ["security", "input_validation"],
    "event_specific": {
      "attractor_name": "security_focus",
      "attractor_coherence": 0.75,
      "core_patterns": ["security", "input_validation", "logging"]
    }
  },

  "action_taken": "pause",
  "user_response": null
}
```

### 7.2 Event History

```
> nf checkpoint history
[CHECKPOINT HISTORY] Last 10 events

  CYCLE  TIME      CHECKPOINT         CONDITION MET                ACTION
  3      14:22:00  attractor_emerged  security_focus emerged      pause
  5      14:22:15  coherence > 0.8    coherence = 0.82            pause
  8      14:22:45  cycle_complete     cycle 8 finished            log
  8      14:22:45  stability_warning  stability = 0.35            pause

> nf checkpoint history --checkpoint attractor_emerged
[HISTORY] attractor_emerged

  Trigger 1: Cycle 3
    Attractor: security_focus (C=0.75)
    User action: proceeded after review

  Trigger 2: Cycle 12
    Attractor: secondary_concern (C=0.62)
    User action: proceeded, added to watch list
```

---

## 8. Conditional Checkpoint Groups

### 8.1 Checkpoint Sets

```
> nf checkpoint set create "security_watch"
[CHECKPOINT SET] Created: security_watch

> nf checkpoint set add security_watch "@security.activation < 0.5"
> nf checkpoint set add security_watch "@vulnerability.activation > 0.7"
> nf checkpoint set add security_watch "resonance(@security, @exploit) > 0.6"

> nf checkpoint set enable security_watch
[CHECKPOINT SET] Enabled: security_watch (3 conditions)
```

### 8.2 Conditional Sets

```
# Only active when analyzing security
> nf checkpoint set create "security_analysis" --when "task.type == 'security'"

# Only in multi-field mode
> nf checkpoint set create "multi_field" --when "fields.active.count > 1"
```

---

## 9. Checkpoint Evaluation

### 9.1 Evaluation Order

1. High priority checkpoints
2. Normal priority checkpoints
3. Low priority checkpoints

Within same priority: order added

### 9.2 Evaluation Timing

| Event | When Evaluated |
|-------|----------------|
| `cycle_complete` | End of each cycle |
| `attractor_*` | After attractor detection phase |
| `pattern_*` | After pattern state changes |
| `coherence_*` | After coherence calculation |
| `stability_*` | After stability check |
| Custom metrics | After relevant computation |

### 9.3 Short-Circuit Evaluation

```
# First matching checkpoint triggers pause
# Remaining checkpoints not evaluated until proceed

Checkpoints: [cp_001, cp_002, cp_003]

Cycle 5:
  Evaluate cp_001 (coherence > 0.8): FALSE
  Evaluate cp_002 (attractor_emerged): TRUE → PAUSE
  # cp_003 not evaluated

After proceed:
  Evaluate cp_003 (patterns.count > 10): checked
```

---

## 10. API

```python
class CheckpointManager:
    """Manages checkpoint conditions and triggering."""

    def add(self, condition: str, name: str = None,
            priority: str = "normal", action: str = "pause") -> str:
        """Add a checkpoint condition. Returns checkpoint ID."""

    def remove(self, checkpoint_id: str) -> None:
        """Remove a checkpoint."""

    def enable(self, checkpoint_id: str) -> None:
        """Enable a checkpoint."""

    def disable(self, checkpoint_id: str) -> None:
        """Disable a checkpoint."""

    def list(self, verbose: bool = False) -> List[Checkpoint]:
        """List all checkpoints."""

    def evaluate(self, context: FieldContext) -> Optional[CheckpointEvent]:
        """Evaluate all checkpoints against current context."""

    def history(self, checkpoint_id: str = None) -> List[CheckpointEvent]:
        """Get checkpoint trigger history."""

    def parse_condition(self, expr: str) -> CheckpointCondition:
        """Parse condition expression into evaluable form."""


class CheckpointCondition:
    """Parsed checkpoint condition."""

    def evaluate(self, context: FieldContext) -> bool:
        """Evaluate condition against context."""

    def get_metrics_needed(self) -> List[str]:
        """Return metrics required for evaluation."""
```

---

## 11. Best Practices

### 11.1 Effective Checkpoints

**Good:**
```
# Meaningful thresholds based on task
coherence > 0.85 AND attractors.count >= 1

# Specific pattern monitoring
@critical_insight.activation DECREASED BY 0.2

# Cross-field awareness
cross_resonance($analysis, $synthesis) > 0.7
```

**Avoid:**
```
# Too frequent (every cycle)
cycle_complete

# Too vague
coherence CHANGED

# Redundant with built-in
patterns.count CHANGED  # Use pattern_created/pattern_died
```

### 11.2 Checkpoint Strategy

| Goal | Recommended Checkpoints |
|------|------------------------|
| Learning | `cycle_complete`, `attractor_emerged` |
| Monitoring | `coherence > X`, `stability_warning` |
| Quality | `attractor_emerged`, `coherence > 0.8` |
| Debugging | `@pattern.activation CHANGED`, verbose |

---

## Related Documents

- `./modes.md` - Autonomy modes
- `./tasks.md` - Task definitions
- `../commands/autonomy.md` - Autonomy commands
