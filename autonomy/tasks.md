# Task Definition System

Specification for defining, managing, and executing autonomous tasks in NFOS.

---

## 1. Overview

Tasks are predefined workflows that execute autonomously, combining field operations, checkpoints, and decision logic into reusable units.

---

## 2. Task Structure

### 2.1 Task Definition

```yaml
task:
  name: security_analysis
  version: "1.0"
  description: "Analyze input for security vulnerabilities"

  # Input specification
  input:
    - name: content
      type: text
      required: true
    - name: context
      type: text
      required: false
      default: "general"

  # Output specification
  output:
    - name: vulnerabilities
      type: list
    - name: risk_level
      type: enum[low, medium, high, critical]
    - name: recommendations
      type: text

  # Field configuration
  fields:
    primary: security
    auxiliary: [knowledge, evaluation]

  # Execution parameters
  parameters:
    max_cycles: 20
    target_coherence: 0.85
    timeout: 60s

  # Checkpoint configuration
  checkpoints:
    - attractor_emerged
    - coherence > 0.8
    - custom: "@vulnerability.activation > 0.7"

  # Execution steps
  steps:
    - inject_input
    - initial_cycles
    - evaluate_attractors
    - generate_output
```

### 2.2 Task Steps

```yaml
steps:
  inject_input:
    action: inject
    patterns:
      - name: "${input.content}"
        activation: 0.9
        tags: [input, analyze]
      - name: "security_lens"
        activation: 0.8
        tags: [context]

  initial_cycles:
    action: evolve
    target: coherence > 0.7
    max_cycles: 10
    on_checkpoint:
      attractor_emerged: continue
      stability_warning: abort

  evaluate_attractors:
    action: conditional
    condition: attractors.count > 0
    then:
      - action: measure
        metrics: [coherence, energy, stability]
      - action: assess_risk
    else:
      - action: inject
        patterns:
          - name: "no_clear_pattern"
            activation: 0.6
      - action: cycle
        count: 5

  generate_output:
    action: collapse
    strategy: coherence
    map_to_output:
      vulnerabilities: attractors[*].name
      risk_level: compute_risk(coherence, energy)
      recommendations: format_recommendations(attractors)
```

---

## 3. Task Types

### 3.1 Analysis Task

```yaml
task:
  name: code_review
  type: analysis

  input:
    - name: code
      type: text

  fields:
    primary: analysis
    patterns:
      inject:
        - security_concerns
        - performance_issues
        - maintainability
        - best_practices

  steps:
    - action: inject_input
    - action: evolve
      target: coherence > 0.8
    - action: collapse
      output: findings
```

### 3.2 Synthesis Task

```yaml
task:
  name: design_recommendation
  type: synthesis

  input:
    - name: requirements
      type: list
    - name: constraints
      type: list

  fields:
    primary: synthesis
    auxiliary: [technical, user, business]

  steps:
    - action: parallel_inject
      fields: [technical, user, business]
      input: requirements

    - action: sync
      cycles: 5

    - action: transfer_to_synthesis

    - action: evolve
      target: coherence > 0.85

    - action: arbitrate_conflicts

    - action: collapse
      output: recommendation
```

### 3.3 Comparison Task

```yaml
task:
  name: option_comparison
  type: comparison

  input:
    - name: options
      type: list
    - name: criteria
      type: list

  steps:
    - action: for_each
      items: "${input.options}"
      do:
        - action: inject
          pattern: "${item}"
          activation: 0.8
        - action: cycle
          count: 3
        - action: measure
          store: "scores.${item}"

    - action: rank
      by: scores.coherence
      output: ranking
```

### 3.4 Monitoring Task

```yaml
task:
  name: continuous_monitor
  type: monitor
  persistent: true

  input:
    - name: watch_patterns
      type: list

  checkpoints:
    - "${watch_patterns[*]}.activation DECREASED BY 0.2"
    - stability < 0.5

  steps:
    - action: watch
      patterns: "${input.watch_patterns}"
      on_change:
        - action: alert
          message: "Pattern ${pattern} changed: ${old} → ${new}"
        - action: log
          level: warning
```

---

## 4. Task Actions

### 4.1 Field Actions

| Action | Parameters | Description |
|--------|------------|-------------|
| `inject` | patterns, activation | Inject patterns into field |
| `amplify` | pattern, factor | Amplify specific pattern |
| `attenuate` | pattern, factor | Attenuate pattern |
| `cycle` | count | Run N dynamics cycles |
| `evolve` | target, max_cycles | Evolve until target |
| `collapse` | strategy | Generate output |
| `reset` | preserve | Reset field |

### 4.2 Multi-Field Actions

| Action | Parameters | Description |
|--------|------------|-------------|
| `sync` | fields, cycles | Synchronized evolution |
| `pipeline` | fields | Sequential processing |
| `parallel` | fields | Parallel processing |
| `transfer` | from, to, filter | Transfer patterns |
| `arbitrate` | f1, f2, strategy | Resolve conflicts |

### 4.3 Control Actions

| Action | Parameters | Description |
|--------|------------|-------------|
| `conditional` | condition, then, else | Branching |
| `for_each` | items, do | Iteration |
| `while` | condition, do | Loop |
| `wait` | condition | Wait for condition |
| `abort` | message | Abort task |
| `checkpoint` | name | Force checkpoint |

### 4.4 Utility Actions

| Action | Parameters | Description |
|--------|------------|-------------|
| `measure` | metrics | Capture metrics |
| `log` | level, message | Log message |
| `alert` | message | Send alert |
| `commit` | message | Save state |
| `branch` | name | Create branch |

---

## 5. Task Variables

### 5.1 Input Variables

```yaml
# Access input parameters
patterns:
  - name: "${input.content}"    # Direct input
  - name: "${input.context}"    # Optional with default
```

### 5.2 Runtime Variables

```yaml
# Field state
condition: "${field.coherence} > 0.8"
condition: "${field.patterns.count} >= 5"

# Pattern state
condition: "${@security.activation} > 0.5"

# Task state
condition: "${task.cycles_executed} < 20"
condition: "${task.elapsed_time} < 30s"

# Step results
previous_result: "${steps.evaluate.result}"
```

### 5.3 Computed Variables

```yaml
variables:
  risk_threshold: 0.7
  adjusted_target: "${input.base_target} + 0.1"

steps:
  - action: evolve
    target: "coherence > ${variables.adjusted_target}"
```

---

## 6. Task Control Flow

### 6.1 Conditional Execution

```yaml
steps:
  evaluate:
    action: conditional
    condition: "${field.attractors.count} > 0"
    then:
      - action: log
        message: "Attractor found, proceeding to analysis"
      - action: measure
        metrics: [coherence, stability]
    else:
      - action: log
        message: "No attractor, running more cycles"
      - action: cycle
        count: 5
      - action: goto
        step: evaluate
```

### 6.2 Loops

```yaml
steps:
  iterate_options:
    action: for_each
    items: "${input.options}"
    as: option
    do:
      - action: field
        create: "eval_${option}"
      - action: inject
        field: "eval_${option}"
        pattern: "${option}"
      - action: cycle
        field: "eval_${option}"
        count: 3

  refinement_loop:
    action: while
    condition: "${field.coherence} < 0.8"
    max_iterations: 10
    do:
      - action: cycle
        count: 2
      - action: log
        message: "Coherence: ${field.coherence}"
```

### 6.3 Error Handling

```yaml
steps:
  risky_operation:
    action: evolve
    target: coherence > 0.9
    max_cycles: 50
    on_error:
      timeout:
        - action: log
          message: "Evolution timed out at coherence ${field.coherence}"
        - action: collapse
          strategy: best_effort
      instability:
        - action: reduce_coupling
          factor: 0.5
        - action: retry
          max: 2
```

---

## 7. Task Lifecycle

### 7.1 States

```
┌─────────┐     start      ┌─────────┐
│ defined │ ─────────────► │ running │
└─────────┘                └────┬────┘
                                │
              ┌─────────────────┼─────────────────┐
              │                 │                 │
              ▼                 ▼                 ▼
        ┌──────────┐     ┌──────────┐     ┌──────────┐
        │  paused  │     │ completed│     │  failed  │
        └────┬─────┘     └──────────┘     └──────────┘
             │
             │ resume
             ▼
        ┌─────────┐
        │ running │
        └─────────┘
```

### 7.2 State Transitions

| From | To | Trigger |
|------|-----|---------|
| defined | running | `nf task run` |
| running | paused | checkpoint, user interrupt |
| running | completed | all steps done |
| running | failed | error, abort |
| paused | running | `nf proceed` |
| paused | failed | `nf task abort` |

### 7.3 Task Persistence

```json
{
  "task_id": "t_abc123",
  "name": "security_analysis",
  "state": "paused",

  "progress": {
    "current_step": "evaluate_attractors",
    "step_index": 2,
    "total_steps": 4,
    "cycles_executed": 8,
    "elapsed_time": "12.5s"
  },

  "context": {
    "input": {"content": "user input data"},
    "variables": {"risk_threshold": 0.7},
    "step_results": {
      "inject_input": {"patterns_added": 2},
      "initial_cycles": {"final_coherence": 0.72}
    }
  },

  "field_snapshot": "commit_def456",

  "pause_info": {
    "reason": "checkpoint:attractor_emerged",
    "checkpoint_data": {"attractor": "vulnerability_cluster"}
  }
}
```

---

## 8. Task Library

### 8.1 Built-in Tasks

| Task | Type | Description |
|------|------|-------------|
| `quick_analysis` | analysis | Fast single-field analysis |
| `deep_analysis` | analysis | Thorough multi-cycle analysis |
| `multi_perspective` | synthesis | Multi-field perspective synthesis |
| `comparison` | comparison | Compare multiple options |
| `brainstorm` | creative | Creative pattern generation |

### 8.2 Task Registry

```
> nf task list
[TASKS] Available tasks

  BUILT-IN:
    quick_analysis      Fast single-field analysis
    deep_analysis       Thorough multi-cycle analysis
    multi_perspective   Multi-field synthesis
    comparison          Option comparison
    brainstorm          Creative generation

  USER-DEFINED:
    security_analysis   Security vulnerability analysis (v1.2)
    code_review         Code quality review (v1.0)
    design_eval         Design evaluation (v2.1)

> nf task info security_analysis
[TASK] security_analysis v1.2

  Description: Analyze input for security vulnerabilities
  Type: analysis
  Author: user

  Input:
    content (text, required)
    context (text, optional, default: "general")

  Output:
    vulnerabilities (list)
    risk_level (enum)
    recommendations (text)

  Fields: security, knowledge, evaluation
  Parameters:
    max_cycles: 20
    target_coherence: 0.85
    timeout: 60s
```

### 8.3 Task Storage

```
nfos/
└── tasks/
    ├── builtin/
    │   ├── quick_analysis.yaml
    │   ├── deep_analysis.yaml
    │   └── ...
    └── user/
        ├── security_analysis.yaml
        └── code_review.yaml
```

---

## 9. Task Execution

### 9.1 Running Tasks

```
> nf task run security_analysis --input.content "SELECT * FROM users WHERE id=$id"
[TASK] Starting: security_analysis

  Input:
    content: "SELECT * FROM users WHERE id=$id"
    context: "general" (default)

  Step 1/4: inject_input
    Injecting 2 patterns...
    Done

  Step 2/4: initial_cycles
    Evolving toward coherence > 0.7
    Cycle 1: C=0.45
    Cycle 2: C=0.58
    Cycle 3: C=0.68
    [CHECKPOINT] attractor_emerged: sql_injection
    Cycle 4: C=0.74
    Target reached

  Step 3/4: evaluate_attractors
    Attractors found: 1
    Measuring metrics...
    Assessing risk...
    Risk level: HIGH

  Step 4/4: generate_output
    Collapsing with coherence strategy...

[COMPLETE] Task finished in 8.2s

  OUTPUT:
  ┌─────────────────────────────────────────────────────────┐
  │ Vulnerabilities:                                        │
  │   - SQL Injection (confidence: 0.92)                   │
  │                                                         │
  │ Risk Level: HIGH                                        │
  │                                                         │
  │ Recommendations:                                        │
  │   - Use parameterized queries                          │
  │   - Validate and sanitize input                        │
  │   - Implement input length limits                      │
  └─────────────────────────────────────────────────────────┘
```

### 9.2 Task with Checkpoints

```
> nf task run deep_analysis --input.content "complex scenario"
[TASK] Starting: deep_analysis

  Step 1/5: inject_input
    Done

  Step 2/5: initial_evolution
    Evolving...
    [CHECKPOINT] coherence > 0.7
      Current coherence: 0.72
      Attractors: 2

  [PAUSED] Checkpoint reached. Review state?
    nf proceed          - Continue task
    nf task status      - View detailed status
    nf task abort       - Abort task

> nf proceed
  Continuing from step 2...
```

### 9.3 Background Tasks

```
> nf task run deep_analysis --background --input.content "data"
[TASK] Started in background: t_xyz789
  Monitor with: nf task status t_xyz789
  Logs: nf task logs t_xyz789

> nf task status t_xyz789
[TASK] t_xyz789 (deep_analysis)

  State: running
  Progress: 60% (step 3/5)
  Elapsed: 15.2s
  Current: evaluate_patterns

  Recent events:
    14:22:15 - Step 2 complete (coherence: 0.75)
    14:22:18 - Attractor emerged: primary_concern
    14:22:20 - Step 3 started

> nf task wait t_xyz789
[TASK] Waiting for t_xyz789...
  [████████████████░░░░] 80%
  Complete!
```

---

## 10. Creating Custom Tasks

### 10.1 Task Definition File

```yaml
# ~/.nfos/tasks/my_analysis.yaml
task:
  name: my_analysis
  version: "1.0"
  description: "Custom analysis workflow"

  input:
    - name: data
      type: text
      required: true

  output:
    - name: result
      type: text

  parameters:
    cycles: 10
    threshold: 0.75

  steps:
    - action: inject
      patterns:
        - name: "${input.data}"
          activation: 0.9

    - action: evolve
      target: "coherence > ${parameters.threshold}"
      max_cycles: "${parameters.cycles}"

    - action: collapse
      strategy: coherence
      output: result
```

### 10.2 Registering Tasks

```
> nf task register my_analysis.yaml
[TASK] Registered: my_analysis v1.0
  Location: ~/.nfos/tasks/my_analysis.yaml

> nf task validate my_analysis
[TASK] Validating: my_analysis
  ✓ Schema valid
  ✓ Steps valid
  ✓ Variables resolvable
  ✓ Actions available
  Result: VALID
```

---

## 11. API

```python
class TaskManager:
    """Manages task definitions and execution."""

    def register(self, path: str) -> Task:
        """Register task from YAML file."""

    def unregister(self, name: str) -> None:
        """Remove task registration."""

    def list(self, include_builtin: bool = True) -> List[TaskInfo]:
        """List available tasks."""

    def get(self, name: str) -> Task:
        """Get task definition."""

    def run(self, name: str, input: dict, background: bool = False) -> TaskExecution:
        """Execute a task."""

    def status(self, task_id: str) -> TaskStatus:
        """Get task execution status."""

    def abort(self, task_id: str) -> None:
        """Abort running task."""

    def wait(self, task_id: str, timeout: float = None) -> TaskResult:
        """Wait for task completion."""


class TaskExecution:
    """Running task instance."""

    task_id: str
    name: str
    state: TaskState
    progress: TaskProgress
    context: TaskContext

    def pause(self) -> None
    def resume(self) -> None
    def abort(self) -> None
    def get_result(self) -> Optional[TaskResult]
```

---

## Related Documents

- `./modes.md` - Autonomy modes
- `./checkpoints.md` - Checkpoint conditions
- `../commands/autonomy.md` - Autonomy commands
- `../examples/05-autonomous.md` - Task examples
