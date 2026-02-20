# Autonomy Commands

Commands for controlling execution modes, checkpoints, and autonomous tasks.

---

## Overview

Autonomy commands control how NFOS executes operations—from step-by-step manual control to fully autonomous task execution.

---

## 1. Mode Command

Controls the execution autonomy level.

### 1.1 Set Mode

```
nf mode <mode> [--config <options>]
```

**Modes:**
| Mode | Description |
|------|-------------|
| `step` | Pause after every atomic operation |
| `checkpoint` | Pause at defined conditions |
| `auto` | Run to completion |

**Examples:**
```
> nf mode step
[MODE] Switched to: step
  Behavior: Pause after every operation
  Resume with: nf proceed

> nf mode checkpoint
[MODE] Switched to: checkpoint
  Behavior: Pause at checkpoint conditions
  Active checkpoints: 3

> nf mode auto
[MODE] Switched to: auto
  Behavior: Run to completion
  Checkpoints logged but won't pause
```

### 1.2 Mode Status

```
> nf mode
[MODE] Current: checkpoint

  Execution state: paused
  Pause reason: attractor_emerged
  Pending operations: 7 cycles

  Active checkpoints:
    - attractor_emerged (triggered)
    - coherence > 0.8
    - stability_warning
```

### 1.3 Mode Configuration

```
> nf mode step --config show_values=true,group_by=operation
[MODE] Step mode configured
  show_values: true
  group_by: operation

> nf mode checkpoint --config pause_behavior=interactive
[MODE] Checkpoint mode configured
  pause_behavior: interactive

> nf mode auto --config progress=bar,log_events=true
[MODE] Auto mode configured
  progress: bar
  log_events: true
```

---

## 2. Proceed Command

Continues execution after a pause.

### 2.1 Basic Proceed

```
nf proceed [count] [--flags]
```

**Parameters:**
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `count` | int | 1 | Steps/checkpoints to proceed through |
| `--skip-checkpoints` | bool | false | Skip remaining checkpoints |
| `--abort` | bool | false | Abort current operation |
| `--to` | string | none | Proceed to specific checkpoint |

**Examples:**
```
# Continue one step/checkpoint
> nf proceed
[PROCEED] Continuing execution...

# Continue 5 steps (step mode)
> nf proceed 5
[PROCEED] Executing 5 steps...
  Step 1: decay @security... done
  Step 2: decay @input... done
  Step 3: resonance (@security, @input)... done
  Step 4: resonance (@security, @logging)... done
  Step 5: amplify @security... done
[PAUSED] 5 steps completed

# Skip all checkpoints and run to completion
> nf proceed --skip-checkpoints
[PROCEED] Skipping checkpoints, running to completion...
  Cycle 5: C=0.78
  Cycle 6: C=0.82 [checkpoint logged: coherence > 0.8]
  Cycle 7: C=0.84
[COMPLETE] Execution finished

# Proceed to specific checkpoint
> nf proceed --to "coherence > 0.9"
[PROCEED] Running until: coherence > 0.9
  Cycle 5: C=0.78
  Cycle 6: C=0.82 [attractor_emerged - skipped]
  Cycle 7: C=0.86
  Cycle 8: C=0.89
  Cycle 9: C=0.91
[CHECKPOINT] coherence > 0.9 reached
```

### 2.2 Abort

```
> nf proceed --abort
[ABORT] Aborting current operation
  Keeping current field state
  Operation cancelled: cycle (3/10 completed)
```

---

## 3. Checkpoint Command

Manages checkpoint conditions.

### 3.1 Add Checkpoint

```
nf checkpoint add <condition> [--name <n>] [--priority <p>] [--action <a>]
```

**Parameters:**
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `condition` | string | required | Checkpoint expression |
| `--name` | string | auto | Checkpoint name |
| `--priority` | string | normal | high, normal, low |
| `--action` | string | pause | Action(s) on trigger |

**Actions:**
| Action | Description |
|--------|-------------|
| `pause` | Pause execution (default) |
| `log` | Log event only |
| `notify` | Send notification |
| `snapshot` | Auto-commit state |
| `branch` | Create branch |

**Examples:**
```
> nf checkpoint add "coherence > 0.85"
[CHECKPOINT] Added: coherence > 0.85
  ID: cp_001
  Priority: normal
  Action: pause

> nf checkpoint add "attractor_emerged" --priority high
[CHECKPOINT] Added: attractor_emerged
  ID: cp_002
  Priority: high
  Action: pause

> nf checkpoint add "@critical.activation < 0.3" --name "critical_dying" --action "notify,pause"
[CHECKPOINT] Added: @critical.activation < 0.3
  ID: cp_003
  Name: critical_dying
  Priority: normal
  Actions: notify, pause

> nf checkpoint add "cycle_complete" --action log
[CHECKPOINT] Added: cycle_complete
  ID: cp_004
  Action: log (no pause)
```

### 3.2 List Checkpoints

```
nf checkpoint list [--verbose]
```

**Output:**
```
> nf checkpoint list
[CHECKPOINTS] 5 active

  ID       NAME              CONDITION                  PRIORITY  ENABLED
  cp_001   -                 coherence > 0.85           normal    yes
  cp_002   -                 attractor_emerged          high      yes
  cp_003   critical_dying    @critical.activation<0.3   normal    yes
  cp_004   -                 cycle_complete             low       yes
  cp_005   -                 stability_warning          high      yes

> nf checkpoint list --verbose
[CHECKPOINTS] Detailed view

  cp_002: attractor_emerged [HIGH] [ENABLED]
    Action: pause
    Triggered: 2 times
    Last: Cycle 3 - "security_focus" emerged

  cp_003: critical_dying [NORMAL] [ENABLED]
    Condition: @critical.activation < 0.3
    Action: notify, pause
    Triggered: 0 times
```

### 3.3 Remove Checkpoint

```
nf checkpoint remove <id|name>
```

**Examples:**
```
> nf checkpoint remove cp_001
[CHECKPOINT] Removed: cp_001

> nf checkpoint remove --name critical_dying
[CHECKPOINT] Removed: critical_dying

> nf checkpoint clear
[CHECKPOINT] Cleared all custom checkpoints
  Built-in checkpoints preserved
```

### 3.4 Enable/Disable Checkpoint

```
nf checkpoint enable <id|name>
nf checkpoint disable <id|name>
```

**Examples:**
```
> nf checkpoint disable cp_002
[CHECKPOINT] Disabled: attractor_emerged

> nf checkpoint enable cp_002
[CHECKPOINT] Enabled: attractor_emerged

> nf checkpoint disable --all
[CHECKPOINT] Disabled all checkpoints
```

### 3.5 Checkpoint History

```
nf checkpoint history [--checkpoint <id>] [--limit <n>]
```

**Output:**
```
> nf checkpoint history
[CHECKPOINT HISTORY] Last 10 events

  CYCLE  TIME      CHECKPOINT           ACTION  RESULT
  3      14:22:00  attractor_emerged    pause   proceeded
  5      14:22:15  coherence > 0.8      pause   proceeded
  8      14:22:45  cycle_complete       log     -
  12     14:23:10  attractor_emerged    pause   proceeded

> nf checkpoint history --checkpoint attractor_emerged
[HISTORY] attractor_emerged (2 triggers)

  Trigger 1: Cycle 3 at 14:22:00
    Attractor: security_focus
    Coherence: 0.71
    User: proceeded after 12s

  Trigger 2: Cycle 12 at 14:23:10
    Attractor: secondary_pattern
    Coherence: 0.65
    User: proceeded immediately
```

---

## 4. Task Command

Manages and executes autonomous tasks.

### 4.1 List Tasks

```
nf task list [--type <t>]
```

**Output:**
```
> nf task list
[TASKS] Available tasks

  BUILT-IN:
    quick_analysis      Fast single-field analysis
    deep_analysis       Thorough multi-cycle analysis
    multi_perspective   Multi-field synthesis
    comparison          Option comparison

  USER-DEFINED:
    security_analysis   Security vulnerability scan (v1.2)
    code_review         Code quality review (v1.0)
```

### 4.2 Task Info

```
nf task info <name>
```

**Output:**
```
> nf task info security_analysis
[TASK] security_analysis v1.2

  Description: Analyze input for security vulnerabilities
  Author: user
  Created: 2024-01-10

  INPUT:
    content    text      required   Content to analyze
    context    text      optional   Analysis context (default: general)

  OUTPUT:
    vulnerabilities   list    Detected vulnerabilities
    risk_level        enum    low|medium|high|critical
    recommendations   text    Mitigation recommendations

  CONFIGURATION:
    max_cycles: 20
    target_coherence: 0.85
    timeout: 60s

  CHECKPOINTS:
    - attractor_emerged
    - coherence > 0.8
    - @vulnerability.activation > 0.7

  STEPS: 4
    1. inject_input
    2. initial_cycles
    3. evaluate_attractors
    4. generate_output
```

### 4.3 Run Task

```
nf task run <name> [--input.<param>=<value>...] [--flags]
```

**Flags:**
| Flag | Description |
|------|-------------|
| `--background` | Run in background |
| `--no-checkpoints` | Disable checkpoints |
| `--dry-run` | Show what would execute |
| `--verbose` | Detailed output |

**Examples:**
```
# Basic task execution
> nf task run security_analysis --input.content "SELECT * FROM users"
[TASK] Starting: security_analysis

  Input: content="SELECT * FROM users", context="general"

  Step 1/4: inject_input ✓
  Step 2/4: initial_cycles
    Cycle 1: C=0.45
    Cycle 2: C=0.58
    [CHECKPOINT] attractor_emerged
  [PAUSED] Review and proceed...

> nf proceed
  Step 2/4: initial_cycles ✓ (C=0.74)
  Step 3/4: evaluate_attractors ✓
  Step 4/4: generate_output ✓

[COMPLETE] Task finished in 8.2s

  OUTPUT:
    vulnerabilities: [SQL Injection]
    risk_level: HIGH
    recommendations: Use parameterized queries...

# Background execution
> nf task run deep_analysis --input.data "complex" --background
[TASK] Started in background: t_abc123
  Monitor: nf task status t_abc123
  Logs: nf task logs t_abc123

# Dry run
> nf task run security_analysis --input.content "test" --dry-run
[DRY RUN] security_analysis

  Would execute:
    1. inject_input: 2 patterns
    2. initial_cycles: evolve to C>0.7 (max 10 cycles)
    3. evaluate_attractors: conditional logic
    4. generate_output: collapse with coherence strategy

  Checkpoints would trigger on:
    - attractor_emerged
    - coherence > 0.8
```

### 4.4 Task Status

```
nf task status [task_id]
```

**Output:**
```
> nf task status
[TASKS] Running tasks

  ID         NAME               STATE     PROGRESS  ELAPSED
  t_abc123   deep_analysis      running   60%       15.2s
  t_def456   comparison         paused    40%       8.5s

> nf task status t_abc123
[TASK] t_abc123 (deep_analysis)

  State: running
  Progress: 60% (step 3/5: evaluate_patterns)
  Elapsed: 15.2s
  Field: analysis (C=0.72)

  Recent events:
    14:22:15 - Step 2 complete
    14:22:18 - Attractor emerged: primary_concern
    14:22:20 - Step 3 started

  Next checkpoint: coherence > 0.8
```

### 4.5 Task Control

```
# Pause running task
> nf task pause t_abc123
[TASK] Paused: t_abc123
  Current step: evaluate_patterns (45% complete)

# Resume paused task
> nf task resume t_abc123
[TASK] Resumed: t_abc123

# Abort task
> nf task abort t_abc123
[TASK] Aborted: t_abc123
  State saved, can be reviewed

# Wait for task completion
> nf task wait t_abc123
[TASK] Waiting for t_abc123...
  [████████████████████] 100%
  Complete! (23.5s)
```

### 4.6 Task Logs

```
nf task logs <task_id> [--follow] [--level <l>]
```

**Output:**
```
> nf task logs t_abc123
[LOGS] t_abc123 (deep_analysis)

  14:22:10 [INFO]  Task started
  14:22:10 [INFO]  Step 1: inject_input
  14:22:11 [DEBUG] Injected pattern: input_data (0.9)
  14:22:11 [DEBUG] Injected pattern: analysis_lens (0.8)
  14:22:11 [INFO]  Step 1 complete
  14:22:11 [INFO]  Step 2: initial_evolution
  14:22:12 [DEBUG] Cycle 1: coherence=0.45
  14:22:13 [DEBUG] Cycle 2: coherence=0.58
  14:22:14 [INFO]  Attractor emerged: primary_concern
  14:22:15 [DEBUG] Cycle 3: coherence=0.68

> nf task logs t_abc123 --follow
[LOGS] Following t_abc123... (Ctrl+C to stop)
  14:22:20 [DEBUG] Cycle 4: coherence=0.72
  14:22:21 [DEBUG] Cycle 5: coherence=0.75
  ...
```

### 4.7 Task Results

```
nf task result <task_id> [--format <f>]
```

**Output:**
```
> nf task result t_abc123
[RESULT] t_abc123 (deep_analysis)

  Status: completed
  Duration: 23.5s
  Final coherence: 0.86

  OUTPUT:
    findings:
      - Primary concern identified (confidence: 0.88)
      - Secondary pattern detected (confidence: 0.72)
    summary: "Analysis reveals..."
    risk_assessment: medium

> nf task result t_abc123 --format json
{
  "task_id": "t_abc123",
  "status": "completed",
  "duration_seconds": 23.5,
  "output": {
    "findings": [...],
    "summary": "...",
    "risk_assessment": "medium"
  }
}
```

### 4.8 Register Custom Task

```
nf task register <file.yaml>
nf task unregister <name>
nf task validate <file.yaml>
```

**Examples:**
```
> nf task validate my_task.yaml
[VALIDATE] my_task.yaml
  ✓ Schema valid
  ✓ Steps valid
  ✓ Variables resolvable
  ✓ Actions available
  Result: VALID

> nf task register my_task.yaml
[TASK] Registered: my_task v1.0
  Location: ~/.nfos/tasks/my_task.yaml

> nf task unregister my_task
[TASK] Unregistered: my_task
```

---

## 5. Watch Command

Monitors field state continuously.

```
nf watch [--interval <ms>] [--metrics <list>]
```

**Output:**
```
> nf watch --metrics coherence,patterns.count,attractors.count
[WATCH] Monitoring field: main (Ctrl+C to stop)

  TIME      COHERENCE  PATTERNS  ATTRACTORS  EVENTS
  14:22:00  0.72       8         1           -
  14:22:01  0.74       8         1           -
  14:22:02  0.76       8         1           resonance spike
  14:22:03  0.78       8         1           -
  14:22:04  0.80       8         2           attractor_emerged
  14:22:05  0.81       7         2           pattern_died: logging

^C
[WATCH] Stopped
  Duration: 5s
  Events captured: 3
```

---

## 6. Quick Reference

```
MODE COMMANDS
────────────────────────────────────────────────────────
nf mode step|checkpoint|auto   Set execution mode
nf mode                        Show current mode
nf mode <m> --config k=v       Configure mode

PROCEED COMMANDS
────────────────────────────────────────────────────────
nf proceed [n]                 Continue n steps/checkpoints
nf proceed --skip-checkpoints  Run to completion
nf proceed --to "condition"    Run until condition
nf proceed --abort             Abort current operation

CHECKPOINT COMMANDS
────────────────────────────────────────────────────────
nf checkpoint add "expr"       Add checkpoint condition
nf checkpoint list [--verbose] List checkpoints
nf checkpoint remove <id>      Remove checkpoint
nf checkpoint enable|disable   Toggle checkpoint
nf checkpoint history          View trigger history
nf checkpoint clear            Clear custom checkpoints

TASK COMMANDS
────────────────────────────────────────────────────────
nf task list                   List available tasks
nf task info <name>            Task details
nf task run <name> --input...  Execute task
nf task status [id]            Execution status
nf task pause|resume|abort     Control execution
nf task logs <id> [--follow]   View task logs
nf task result <id>            Get task output
nf task register <file>        Register custom task
nf task validate <file>        Validate task definition

MONITORING
────────────────────────────────────────────────────────
nf watch [--metrics list]      Monitor field continuously
```

---

## Related Documents

- `../autonomy/modes.md` - Mode specifications
- `../autonomy/checkpoints.md` - Checkpoint language
- `../autonomy/tasks.md` - Task definitions
- `../examples/05-autonomous.md` - Usage examples
