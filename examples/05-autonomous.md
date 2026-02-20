# Example 5: Autonomous Task Execution

A complete walkthrough demonstrating autonomy modes, checkpoints, and task-based execution.

---

## Scenario: Security Code Review Pipeline

We're building an automated security review workflow that analyzes code for vulnerabilities, pausing at key decision points for human review.

---

## Part 1: Exploring Autonomy Modes

### 1.1 Step Mode - Learning the Dynamics

```
> nf init security_review
[NFOS] Initialized field: security_review

> nf mode step
[MODE] Switched to: step
  Behavior: Pause after every operation

> nf inject "sql_query" 0.85 --tags input,analyze
[INJECT] Pattern 'sql_query' added
  [STEP] Injection complete
  [PAUSED] Press Enter or 'nf proceed'...

> nf proceed
> nf inject "user_input" 0.80 --tags input
[INJECT] Pattern 'user_input' added
  [STEP] Injection complete
  [PAUSED]

> nf proceed
> nf inject "validation" 0.70 --tags security
[INJECT] Pattern 'validation' added
  [STEP] Injection complete
  [PAUSED]

> nf proceed
> nf cycle 1
[CYCLE] Step mode - executing incrementally

  [STEP 1/9] Applying decay to 'sql_query'
    Before: 0.85
    After: 0.81
  [PAUSED]

> nf proceed
  [STEP 2/9] Applying decay to 'user_input'
    Before: 0.80
    After: 0.76
  [PAUSED]

> nf proceed 3
  [STEP 3/9] Applying decay to 'validation'... 0.70 → 0.67
  [STEP 4/9] Computing resonance (sql_query, user_input)... R=0.78
  [STEP 5/9] Computing resonance (sql_query, validation)... R=0.65
  [PAUSED] 3 steps completed

> nf proceed --skip-checkpoints
  [STEP 6/9] Computing resonance (user_input, validation)... R=0.58
  [STEP 7/9] Amplifying 'sql_query'... 0.81 → 0.86
  [STEP 8/9] Amplifying 'user_input'... 0.76 → 0.82
  [STEP 9/9] Attractor check... none yet
[COMPLETE] Cycle 1 finished
  Coherence: 0.58
```

**Observation**: Step mode lets us see exactly how each operation affects the field.

---

### 1.2 Checkpoint Mode - Monitoring Key Events

```
> nf mode checkpoint
[MODE] Switched to: checkpoint
  Default checkpoints: attractor_emerged, coherence > 0.8

> nf cycle 10
[CYCLE] Checkpoint mode - running until condition met

  Cycle 2: C=0.62
  Cycle 3: C=0.68
  Cycle 4: C=0.72
  [CHECKPOINT] attractor_emerged
    Attractor: 'input_processing'
    Patterns: sql_query (0.88), user_input (0.84)
    Coherence: 0.72

  [PAUSED] Review attractor. Options:
    nf proceed          - Continue cycles
    nf state            - Inspect field state
    nf plot network     - Visualize relationships
    nf proceed --abort  - Stop here

> nf state
[STATE] Field: security_review

  Patterns:
    sql_query    0.88  [input,analyze]  ★ in attractor
    user_input   0.84  [input]          ★ in attractor
    validation   0.65  [security]

  Attractors:
    input_processing (C=0.72)
      Core: sql_query, user_input
      Basin: 45%

  Coherence: 0.72
  Energy: 0.52

> nf proceed
  Cycle 5: C=0.76
  Cycle 6: C=0.79
  Cycle 7: C=0.81
  [CHECKPOINT] coherence > 0.8
    Field coherence: 0.81
    Remaining cycles: 3

  [PAUSED] Target coherence reached. Continue?

> nf proceed
  Cycle 8: C=0.82
  Cycle 9: C=0.83
  Cycle 10: C=0.83
[COMPLETE] 10 cycles executed
```

---

### 1.3 Auto Mode - Fast Execution

```
> nf mode auto
[MODE] Switched to: auto
  Behavior: Run to completion
  Events will be logged but won't pause

> nf reset --preserve @sql_query
[RESET] Field cleared, preserved: sql_query

> nf inject "untrusted_source" 0.75
> nf inject "concatenation" 0.70
> nf inject "execute_query" 0.80

> nf evolve --target 0.85
[EVOLVE] Auto mode - evolving toward C=0.85

  Progress: [████████████████████] 100%

  Cycle 1: C=0.48
  Cycle 2: C=0.58
  Cycle 3: C=0.66
  Cycle 4: C=0.72 [logged: attractor_emerged - vulnerability_pattern]
  Cycle 5: C=0.77
  Cycle 6: C=0.81 [logged: coherence > 0.8]
  Cycle 7: C=0.84
  Cycle 8: C=0.86

[COMPLETE] Target reached in 8 cycles
  Final coherence: 0.86
  Events logged: 2
```

---

## Part 2: Custom Checkpoints

### 2.1 Adding Security-Specific Checkpoints

```
> nf checkpoint add "attractor_emerged" --priority high --name "pattern_found"
[CHECKPOINT] Added: attractor_emerged
  Name: pattern_found
  Priority: high

> nf checkpoint add "@vulnerability.activation > 0.7" --name "vuln_detected"
[CHECKPOINT] Added: @vulnerability.activation > 0.7
  Name: vuln_detected
  Priority: normal

> nf checkpoint add "coherence > 0.75 AND attractors.count >= 1" --name "analysis_ready"
[CHECKPOINT] Added: coherence > 0.75 AND attractors.count >= 1
  Name: analysis_ready
  Priority: normal

> nf checkpoint add "stability < 0.4" --priority high --action "notify,pause" --name "instability"
[CHECKPOINT] Added: stability < 0.4
  Name: instability
  Priority: high
  Actions: notify, pause

> nf checkpoint list
[CHECKPOINTS] 4 active

  ID       NAME            CONDITION                           PRIORITY
  cp_001   pattern_found   attractor_emerged                   high
  cp_002   vuln_detected   @vulnerability.activation > 0.7     normal
  cp_003   analysis_ready  coherence>0.75 AND attractors>=1   normal
  cp_004   instability     stability < 0.4                     high
```

### 2.2 Checkpoint-Driven Analysis

```
> nf mode checkpoint
> nf reset
> nf inject "sql_injection_attempt" 0.90 --tags vulnerability,critical
> nf inject "string_concatenation" 0.75 --tags pattern
> nf inject "user_controlled_input" 0.80 --tags input
> nf inject "database_query" 0.85 --tags target

> nf evolve --target 0.9
[EVOLVE] Checkpoint mode - evolving toward C=0.9

  Cycle 1: C=0.52
  Cycle 2: C=0.64
  Cycle 3: C=0.71
  [CHECKPOINT] pattern_found (HIGH)
    Attractor: sql_injection_risk
    Core patterns: sql_injection_attempt, user_controlled_input
    Coherence: 0.71

  [PAUSED] High-priority checkpoint. Review required.

> nf plot network
[NETWORK] sql_injection_risk attractor

    ┌─────────────────┐         ┌──────────────────┐
    │ sql_injection   │═════════│ user_controlled  │
    │      0.92       │ R=0.85  │      0.84        │
    └────────┬────────┘         └────────┬─────────┘
             │                           │
             │ R=0.72                     │ R=0.68
             │                           │
    ┌────────┴────────┐         ┌────────┴─────────┐
    │ database_query  │─────────│string_concatenat │
    │      0.82       │ R=0.58  │      0.70        │
    └─────────────────┘         └──────────────────┘

> nf proceed
  Cycle 4: C=0.76
  [CHECKPOINT] analysis_ready
    Coherence: 0.76
    Attractors: 1 (sql_injection_risk)
    Ready for output generation

  [PAUSED] Analysis checkpoint reached.

> nf collapse --strategy coherence
[COLLAPSE] Generating output...

  SQL INJECTION VULNERABILITY DETECTED
  Confidence: 0.92
  Risk Level: CRITICAL

  Pattern Analysis:
    - User input flows to database query
    - String concatenation used (not parameterized)
    - No visible input validation

  Recommendation:
    Use parameterized queries or prepared statements
```

---

## Part 3: Creating a Security Review Task

### 3.1 Task Definition

Create `~/.nfos/tasks/security_review.yaml`:

```yaml
task:
  name: security_review
  version: "1.0"
  description: "Automated security code review with human checkpoints"

  input:
    - name: code
      type: text
      required: true
      description: "Code snippet to analyze"
    - name: language
      type: text
      required: false
      default: "auto"
    - name: sensitivity
      type: enum[low, medium, high]
      default: medium

  output:
    - name: vulnerabilities
      type: list
    - name: risk_level
      type: enum[none, low, medium, high, critical]
    - name: recommendations
      type: list
    - name: confidence
      type: float

  parameters:
    max_cycles: 25
    target_coherence: 0.85
    vulnerability_threshold: 0.7

  checkpoints:
    - name: vulnerability_detected
      condition: "any_pattern_above(${parameters.vulnerability_threshold}, tag='vulnerability')"
      priority: high
    - name: analysis_complete
      condition: "coherence > ${parameters.target_coherence}"
    - name: multiple_issues
      condition: "attractors.count > 1"

  steps:
    - name: setup
      action: inject
      patterns:
        - name: "code_analysis"
          activation: 0.8
          tags: [context]
        - name: "${input.language}_patterns"
          activation: 0.7
          tags: [language]

    - name: inject_security_lenses
      action: inject
      patterns:
        - name: "sql_injection_lens"
          activation: 0.75
          tags: [security, lens]
        - name: "xss_lens"
          activation: 0.75
          tags: [security, lens]
        - name: "auth_bypass_lens"
          activation: 0.70
          tags: [security, lens]
        - name: "input_validation_lens"
          activation: 0.70
          tags: [security, lens]

    - name: inject_code
      action: inject
      patterns:
        - name: "${input.code}"
          activation: 0.9
          tags: [input, analyze]

    - name: initial_analysis
      action: evolve
      target: "coherence > 0.6"
      max_cycles: 10
      on_checkpoint:
        vulnerability_detected: pause

    - name: deep_analysis
      action: evolve
      target: "coherence > ${parameters.target_coherence}"
      max_cycles: 15
      on_checkpoint:
        vulnerability_detected: continue
        analysis_complete: proceed
        multiple_issues: pause

    - name: assess_severity
      action: conditional
      condition: "attractors.count > 0"
      then:
        - action: for_each
          items: "attractors"
          do:
            - action: measure
              metrics: [coherence, stability]
            - action: compute
              expression: "severity_score(${item})"
              store: "severity.${item.name}"
      else:
        - action: set
          variable: risk_level
          value: none

    - name: generate_report
      action: collapse
      strategy: coherence
      map_to_output:
        vulnerabilities: "attractors[*].name WHERE tag='vulnerability'"
        risk_level: "max(severity.*)"
        recommendations: "generate_recommendations(attractors)"
        confidence: "field.coherence"
```

### 3.2 Register and Validate

```
> nf task validate ~/.nfos/tasks/security_review.yaml
[VALIDATE] security_review.yaml

  Checking schema... ✓
  Checking steps... ✓
  Checking conditions... ✓
  Checking variables... ✓
  Checking actions... ✓

  Result: VALID

> nf task register ~/.nfos/tasks/security_review.yaml
[TASK] Registered: security_review v1.0

> nf task info security_review
[TASK] security_review v1.0

  Automated security code review with human checkpoints

  INPUT:
    code        text    required   Code snippet to analyze
    language    text    optional   Language (default: auto)
    sensitivity enum    optional   low|medium|high (default: medium)

  OUTPUT:
    vulnerabilities   list    Detected security issues
    risk_level        enum    none|low|medium|high|critical
    recommendations   list    Mitigation suggestions
    confidence        float   Analysis confidence

  CHECKPOINTS: 3
    vulnerability_detected (high)
    analysis_complete (normal)
    multiple_issues (normal)

  STEPS: 7
```

---

## Part 4: Running the Task

### 4.1 Interactive Execution

```
> nf task run security_review --input.code "query = 'SELECT * FROM users WHERE id=' + user_id"
[TASK] Starting: security_review v1.0

  Input:
    code: "query = 'SELECT * FROM users WHERE id=' + user_id"
    language: auto (default)
    sensitivity: medium (default)

  Step 1/7: setup
    Injecting analysis context...
    ✓ code_analysis (0.80)
    ✓ auto_patterns (0.70)

  Step 2/7: inject_security_lenses
    Injecting security analysis patterns...
    ✓ sql_injection_lens (0.75)
    ✓ xss_lens (0.75)
    ✓ auth_bypass_lens (0.70)
    ✓ input_validation_lens (0.70)

  Step 3/7: inject_code
    Injecting code for analysis...
    ✓ Code pattern injected (0.90)

  Step 4/7: initial_analysis
    Evolving toward coherence > 0.6
    Cycle 1: C=0.42
    Cycle 2: C=0.51
    Cycle 3: C=0.58
    [CHECKPOINT] vulnerability_detected (HIGH)
      Pattern: sql_injection_lens resonating strongly
      Activation: 0.82
      Resonance with code: 0.78

  [PAUSED] Potential vulnerability detected. Review?

    Current state:
      Coherence: 0.58
      Strong resonance: sql_injection_lens ↔ input_code (R=0.78)
      Emerging pattern: string concatenation with user input

    Options:
      nf proceed            Continue analysis
      nf task status        Detailed status
      nf plot network       Visualize patterns
      nf proceed --abort    Stop and report current findings

> nf plot network --threshold 0.5
[NETWORK] Emerging vulnerability pattern

         ┌───────────────────────────────────────────────┐
         │           (Forming Attractor)                 │
         │                                               │
         │  ┌────────────────┐     ┌─────────────────┐  │
         │  │sql_inject_lens │═════│   input_code    │  │
         │  │      0.82      │0.78 │      0.88       │  │
         │  └────────┬───────┘     └────────┬────────┘  │
         │           │                      │           │
         │           │ 0.65                 │ 0.58      │
         │           │                      │           │
         │  ┌────────┴───────┐     ┌────────┴────────┐  │
         │  │ input_valid    │─────│ code_analysis   │  │
         │  │     0.72       │0.52 │      0.75       │  │
         │  └────────────────┘     └─────────────────┘  │
         └───────────────────────────────────────────────┘

> nf proceed
  Step 4/7: initial_analysis (continuing)
    Cycle 4: C=0.65
    Target reached ✓

  Step 5/7: deep_analysis
    Evolving toward coherence > 0.85
    Cycle 5: C=0.68
    Cycle 6: C=0.72
    Cycle 7: C=0.76
    Cycle 8: C=0.80
    Cycle 9: C=0.83
    Cycle 10: C=0.86
    [CHECKPOINT] analysis_complete
      Coherence: 0.86
      Attractors: 1 (sql_injection_vulnerability)

  Step 6/7: assess_severity
    Evaluating attractor severity...
    sql_injection_vulnerability:
      Coherence: 0.86
      Stability: 0.82
      Severity score: 0.91 (CRITICAL)

  Step 7/7: generate_report
    Collapsing to output...

[COMPLETE] Task finished in 12.3s

┌──────────────────────────────────────────────────────────────────┐
│                    SECURITY REVIEW REPORT                        │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  VULNERABILITIES DETECTED: 1                                     │
│                                                                  │
│  1. SQL Injection (CRITICAL)                                     │
│     Confidence: 0.91                                             │
│     Pattern: String concatenation with user input                │
│     Location: query construction                                 │
│                                                                  │
│  RISK LEVEL: CRITICAL                                            │
│                                                                  │
│  RECOMMENDATIONS:                                                │
│  • Use parameterized queries or prepared statements              │
│  • Never concatenate user input directly into queries            │
│  • Implement input validation and sanitization                   │
│  • Use an ORM with built-in SQL injection protection             │
│                                                                  │
│  ANALYSIS CONFIDENCE: 0.86                                       │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

### 4.2 Background Execution

```
> nf task run security_review --background \
    --input.code "document.write(userInput)" \
    --input.language "javascript"
[TASK] Started in background: t_js_review
  Monitor: nf task status t_js_review
  Logs: nf task logs t_js_review --follow

> nf task status t_js_review
[TASK] t_js_review (security_review)

  State: running
  Progress: 57% (step 5/7: deep_analysis)
  Elapsed: 5.2s

  Current:
    Coherence: 0.72
    Cycles: 6
    Attractors: 1 (xss_vulnerability)

> nf task logs t_js_review --follow
[LOGS] t_js_review (following...)

  10:15:23 [INFO]  Cycle 7: coherence=0.76
  10:15:24 [INFO]  Cycle 8: coherence=0.80
  10:15:25 [INFO]  Checkpoint: analysis_complete
  10:15:25 [INFO]  Step 6: assess_severity
  10:15:26 [INFO]  Severity: xss_vulnerability = 0.85 (HIGH)
  10:15:26 [INFO]  Step 7: generate_report
  10:15:27 [INFO]  Task complete

^C

> nf task result t_js_review
[RESULT] t_js_review

  Status: completed
  Duration: 8.1s

  VULNERABILITIES: [XSS - Cross-Site Scripting]
  RISK LEVEL: HIGH
  RECOMMENDATIONS:
    • Use textContent instead of innerHTML/document.write
    • Sanitize user input before rendering
    • Implement Content Security Policy
  CONFIDENCE: 0.84
```

---

## Part 5: Batch Processing

### 5.1 Auto Mode for Multiple Files

```
> nf mode auto

> for file in code_samples/*.py; do
    echo "Analyzing: $file"
    nf task run security_review --input.code "$(cat $file)" --input.language python
  done

Analyzing: code_samples/auth.py
[TASK] security_review: COMPLETED
  Risk: LOW (weak password hashing)

Analyzing: code_samples/database.py
[TASK] security_review: COMPLETED
  Risk: CRITICAL (SQL injection)

Analyzing: code_samples/api.py
[TASK] security_review: COMPLETED
  Risk: MEDIUM (insufficient input validation)

Analyzing: code_samples/utils.py
[TASK] security_review: COMPLETED
  Risk: NONE
```

### 5.2 Parallel Task Execution

```
> nf task run security_review --background --input.code "$CODE1" &
> nf task run security_review --background --input.code "$CODE2" &
> nf task run security_review --background --input.code "$CODE3" &

[TASK] Started: t_001
[TASK] Started: t_002
[TASK] Started: t_003

> nf task status
[TASKS] 3 running

  ID      NAME              STATE    PROGRESS  ELAPSED
  t_001   security_review   running  45%       3.2s
  t_002   security_review   running  60%       3.2s
  t_003   security_review   running  30%       3.2s

> nf task wait t_001 t_002 t_003
[WAIT] Waiting for 3 tasks...
  t_002 completed (8.1s) - Risk: HIGH
  t_001 completed (9.2s) - Risk: CRITICAL
  t_003 completed (10.5s) - Risk: LOW

All tasks complete.
```

---

## Part 6: Task History and Audit

```
> nf task history --limit 5
[TASK HISTORY] Last 5 executions

  ID       TASK              STATUS     DURATION  RISK      TIME
  t_003    security_review   completed  10.5s     LOW       10:20:15
  t_002    security_review   completed  8.1s      HIGH      10:20:12
  t_001    security_review   completed  9.2s      CRITICAL  10:20:10
  t_js     security_review   completed  8.1s      HIGH      10:15:20
  t_sql    security_review   completed  12.3s     CRITICAL  10:10:05

> nf task history t_001 --verbose
[TASK] t_001 execution details

  Task: security_review v1.0
  Started: 2024-01-15 10:20:10
  Completed: 2024-01-15 10:20:19
  Duration: 9.2s

  Input:
    code: "execute(user_query)"
    language: python
    sensitivity: medium

  Execution:
    Cycles: 12
    Checkpoints triggered: 2
      - vulnerability_detected (cycle 4)
      - analysis_complete (cycle 10)

  Output:
    Risk: CRITICAL
    Vulnerabilities: [Command Injection]
    Confidence: 0.88

  Field state saved: commit_abc123
```

---

## Summary

This walkthrough demonstrated:

1. **Step Mode**: Fine-grained control for learning and debugging
2. **Checkpoint Mode**: Balanced automation with human oversight
3. **Auto Mode**: Full automation for batch processing
4. **Custom Checkpoints**: Domain-specific pause conditions
5. **Task Definitions**: Reusable automated workflows
6. **Interactive Tasks**: Human-in-the-loop execution
7. **Background Tasks**: Parallel asynchronous processing
8. **Task History**: Audit trail and reproducibility

### Quick Reference

```
AUTONOMY QUICK START
────────────────────────────────────────────────────────

# Learning/Debugging
nf mode step
nf proceed [n]

# Interactive Analysis
nf mode checkpoint
nf checkpoint add "condition"
nf proceed

# Batch Processing
nf mode auto
nf task run <task> --input...

# Background Execution
nf task run <task> --background
nf task status [id]
nf task wait [id...]
```

---

## Related Documents

- `../autonomy/modes.md` - Mode specifications
- `../autonomy/checkpoints.md` - Checkpoint language
- `../autonomy/tasks.md` - Task definition format
- `../commands/autonomy.md` - Command reference
