# Autonomy Modes

Specification for NFOS execution modes that control the level of autonomous operation.

---

## 1. Overview

Autonomy modes determine how much user interaction is required during field operations. Three modes provide a spectrum from fully manual to fully autonomous execution.

| Mode | Behavior | Use Case |
|------|----------|----------|
| `step` | Pause after every operation | Learning, debugging, exploration |
| `checkpoint` | Pause at defined conditions | Interactive analysis, monitoring |
| `auto` | Run to completion | Batch processing, defined tasks |

---

## 2. Mode Definitions

### 2.1 Step Mode

**Behavior**: Pauses after every atomic operation, requiring explicit `proceed` to continue.

```
> nf mode step
[MODE] Switched to: step
  Behavior: Pause after every operation
  Resume with: nf proceed

> nf cycle 3
[CYCLE] Step mode - executing incrementally

  [STEP 1/12] Applying decay to 'security'
    Before: 0.85
    After: 0.81
  [PAUSED] Press Enter or 'nf proceed' to continue...

> nf proceed
  [STEP 2/12] Applying decay to 'input_validation'
    Before: 0.78
    After: 0.74
  [PAUSED] Press Enter or 'nf proceed' to continue...

> nf proceed 5
  [STEP 3/12] Applying decay to 'logging'... done
  [STEP 4/12] Computing resonance (security, input_validation)... done
  [STEP 5/12] Computing resonance (security, logging)... done
  [STEP 6/12] Computing resonance (input_validation, logging)... done
  [STEP 7/12] Amplifying 'security' via resonance... done
  [PAUSED] 5 steps completed. Press Enter to continue...
```

**Atomic Operations in Step Mode:**
| Operation Type | Steps |
|----------------|-------|
| Decay | 1 per pattern |
| Resonance | 1 per pair |
| Amplification | 1 per pattern |
| Attenuation | 1 per pattern |
| Attractor check | 1 per field |

**Configuration:**
```json
{
  "mode": "step",
  "step_config": {
    "show_before_after": true,
    "show_intermediate_values": true,
    "auto_proceed_delay": null,
    "group_by": "operation"
  }
}
```

---

### 2.2 Checkpoint Mode

**Behavior**: Runs continuously until a checkpoint condition is met, then pauses.

```
> nf mode checkpoint
[MODE] Switched to: checkpoint
  Behavior: Pause at checkpoint conditions
  Default checkpoints:
    - attractor_emerged
    - coherence > 0.8
    - cycle_complete

> nf cycle 10
[CYCLE] Checkpoint mode - running until condition met

  Cycle 1: C=0.52
  Cycle 2: C=0.62
  Cycle 3: C=0.71
  [CHECKPOINT] attractor_emerged
    Attractor 'security_focus' detected
    Coherence: 0.71
    Core patterns: security, input_validation
  [PAUSED] Review attractor and 'nf proceed' to continue...

> nf proceed
  Cycle 4: C=0.76
  Cycle 5: C=0.82
  [CHECKPOINT] coherence > 0.8
    Field coherence reached 0.82
    Remaining cycles: 5
  [PAUSED] Target coherence reached. Continue? 'nf proceed'...

> nf proceed --skip-checkpoints
  Cycle 6: C=0.84
  Cycle 7: C=0.85
  Cycle 8: C=0.86
  Cycle 9: C=0.86
  Cycle 10: C=0.86
[COMPLETE] 10 cycles executed
```

**Built-in Checkpoints:**
| Checkpoint | Trigger |
|------------|---------|
| `attractor_emerged` | New attractor detected |
| `attractor_collapsed` | Attractor disappeared |
| `coherence > N` | Coherence exceeds threshold |
| `coherence < N` | Coherence drops below threshold |
| `cycle_complete` | After each full cycle |
| `pattern_died` | Pattern activation → 0 |
| `conflict_detected` | Cross-field resonance conflict |
| `stability_warning` | System approaching instability |

**Configuration:**
```json
{
  "mode": "checkpoint",
  "checkpoint_config": {
    "default_checkpoints": ["attractor_emerged", "coherence > 0.8"],
    "custom_checkpoints": [],
    "pause_behavior": "interactive",
    "log_checkpoints": true
  }
}
```

---

### 2.3 Auto Mode

**Behavior**: Runs to completion without pausing, unless critical error.

```
> nf mode auto
[MODE] Switched to: auto
  Behavior: Run to completion
  Will pause only on: critical errors, explicit stop

> nf cycle 10
[CYCLE] Auto mode - running 10 cycles

  Progress: [████████████████████] 100%

  Cycle 1: C=0.52
  Cycle 2: C=0.62
  Cycle 3: C=0.71 [attractor: security_focus]
  Cycle 4: C=0.76
  Cycle 5: C=0.82
  Cycle 6: C=0.84
  Cycle 7: C=0.85
  Cycle 8: C=0.86
  Cycle 9: C=0.86
  Cycle 10: C=0.86

[COMPLETE] 10 cycles executed
  Final coherence: 0.86
  Attractors: 1 (security_focus)
  Events logged: 2 (attractor_emerged, coherence_threshold)
```

**Configuration:**
```json
{
  "mode": "auto",
  "auto_config": {
    "log_events": true,
    "progress_display": "bar",
    "stop_on_error": true,
    "max_cycles_safety": 1000
  }
}
```

---

## 3. Mode Transitions

### 3.1 Transition Rules

```
┌──────────┐     nf mode checkpoint     ┌────────────┐
│          │ ────────────────────────►  │            │
│   step   │                            │ checkpoint │
│          │ ◄────────────────────────  │            │
└──────────┘     nf mode step           └────────────┘
      │                                       │
      │                                       │
      │  nf mode auto                         │  nf mode auto
      │                                       │
      ▼                                       ▼
┌─────────────────────────────────────────────────────┐
│                        auto                          │
└─────────────────────────────────────────────────────┘
                          │
                          │  nf mode step / nf mode checkpoint
                          ▼
                  (respective mode)
```

### 3.2 Mid-Operation Transitions

Transitions can occur during operations:

```
> nf mode auto
> nf cycle 100
[CYCLE] Auto mode - running 100 cycles
  Progress: [████████░░░░░░░░░░░░] 40%
  Cycle 40: C=0.75

> nf mode checkpoint    # User interrupts
[MODE] Switching to checkpoint...
  Current operation will complete
  Next checkpoint: cycle_complete

  Cycle 40 completing...
  [CHECKPOINT] cycle_complete
    Switched to checkpoint mode
    Remaining: 60 cycles
  [PAUSED] Continue with 'nf proceed'
```

---

## 4. Mode State

### 4.1 State Structure

```json
{
  "autonomy": {
    "current_mode": "checkpoint",
    "previous_mode": "step",
    "mode_config": { /* mode-specific config */ },

    "execution_state": {
      "is_running": true,
      "is_paused": true,
      "pause_reason": "checkpoint:attractor_emerged",
      "pending_operations": 60,
      "completed_operations": 40
    },

    "checkpoints": {
      "active": ["attractor_emerged", "coherence > 0.8"],
      "triggered": [
        {
          "condition": "attractor_emerged",
          "cycle": 3,
          "timestamp": "2024-01-15T14:22:00Z",
          "data": {"attractor": "security_focus", "coherence": 0.71}
        }
      ]
    }
  }
}
```

### 4.2 State Persistence

Mode state persists across:
- Session interruptions
- System commits
- Field switches

```
> nf mode checkpoint
> nf cycle 10
  Cycle 3: [CHECKPOINT] attractor_emerged
  [PAUSED]

> nf commit "paused at checkpoint"
[COMMIT] Saved with pause state

# Later...
> nf checkout abc123
[CHECKOUT] Restored state
  Mode: checkpoint
  Status: paused at attractor_emerged
  Pending: 7 cycles
```

---

## 5. Mode Behaviors by Command

### 5.1 Cycle Command

| Mode | Behavior |
|------|----------|
| step | Pause after each atomic step |
| checkpoint | Pause at conditions |
| auto | Run all cycles continuously |

### 5.2 Evolve Command

| Mode | Behavior |
|------|----------|
| step | Pause after each step toward target |
| checkpoint | Pause at conditions or target |
| auto | Run until target reached |

### 5.3 Pipeline Command

| Mode | Behavior |
|------|----------|
| step | Pause after each operation in each stage |
| checkpoint | Pause at stage transitions |
| auto | Run full pipeline |

### 5.4 Sync Command

| Mode | Behavior |
|------|----------|
| step | Pause after each field's step |
| checkpoint | Pause at cross-field events |
| auto | Run all fields continuously |

---

## 6. Interruption Handling

### 6.1 User Interruption

```
> nf mode auto
> nf evolve --target 0.9
[EVOLVE] Auto mode - evolving toward C=0.9
  Progress: [████████░░░░░░░░░░░░] 42%

^C  # User presses Ctrl+C

[INTERRUPT] User requested pause
  Current cycle completing...
  Cycle 15: C=0.76

[PAUSED] Options:
  nf proceed        - Continue evolution
  nf proceed --abort - Abort and keep current state
  nf mode step      - Switch to step mode
```

### 6.2 Error Handling

```
> nf mode auto
> nf sync --cycles 20
[SYNC] Auto mode - syncing 4 fields

  Cycle 5:
    [ERROR] Coupling instability detected
      Spectral radius exceeded safety bound
      Field: creative (oscillating)

[AUTO-PAUSE] Critical error requires attention
  Suggestion: Reduce coupling strength for 'creative'

> nf couple creative main --gamma 0.2
[COUPLE] Reduced coupling
  Stability restored

> nf proceed
[SYNC] Resuming...
```

---

## 7. Mode Selection Guidelines

### 7.1 When to Use Step Mode

- **Learning**: Understanding how dynamics work
- **Debugging**: Tracing unexpected behavior
- **Precision**: Carefully controlled experiments
- **Teaching**: Demonstrating concepts

```
# Debug why pattern is dying
> nf mode step
> nf cycle 1
  [STEP] Decay: security 0.85 → 0.81
  [STEP] Resonance: (security, input) = 0.72
  [STEP] Resonance: (security, logging) = 0.15
  # Aha! Weak resonance with logging
```

### 7.2 When to Use Checkpoint Mode

- **Interactive analysis**: Want to observe key events
- **Exploration**: Don't know what will happen
- **Quality control**: Need to approve decisions
- **Incremental work**: Building understanding

```
# Explore unknown data
> nf mode checkpoint
> nf checkpoint add "energy < 0.3"  # Alert on low energy
> nf evolve --target 0.9
  # Will pause at attractors, coherence thresholds, energy drops
```

### 7.3 When to Use Auto Mode

- **Batch processing**: Known good parameters
- **Production**: Trusted configurations
- **Speed**: Need results quickly
- **Repeated tasks**: Established workflows

```
# Production analysis
> nf mode auto
> nf task run security_analysis --input $data
  # Runs full task without interruption
```

---

## 8. API

```python
class AutonomyController:
    """Controls execution autonomy levels."""

    def __init__(self):
        self.current_mode = "checkpoint"
        self.execution_state = ExecutionState()
        self.checkpoint_manager = CheckpointManager()

    def set_mode(self, mode: str, config: dict = None) -> None:
        """Switch autonomy mode."""

    def get_mode(self) -> str:
        """Get current mode."""

    def proceed(self, steps: int = None, skip_checkpoints: bool = False) -> None:
        """Continue execution."""

    def pause(self, reason: str = "user") -> None:
        """Pause execution."""

    def abort(self) -> None:
        """Abort current operation."""

    def is_paused(self) -> bool:
        """Check if execution is paused."""

    def get_pause_reason(self) -> Optional[str]:
        """Get reason for current pause."""

    def should_pause(self, event: str, data: dict) -> bool:
        """Check if event should trigger pause in current mode."""
```

---

## Related Documents

- `./checkpoints.md` - Checkpoint condition language
- `./tasks.md` - Task definition system
- `../commands/autonomy.md` - Autonomy commands
