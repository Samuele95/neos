# Dynamics Control Commands

Commands for controlling field evolution: cycle, evolve, step, and reset.

---

## 1. Cycle

Executes one or more complete dynamics cycles on the field.

### Syntax

```
nf cycle [n] [--flags]
```

### Arguments

| Argument | Type | Default | Description |
|----------|------|---------|-------------|
| `n` | int | 1 | Number of cycles to run |

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--trace` | bool | false | Show detailed execution trace |
| `--compact` | bool | false | Minimal output |
| `--until "<cond>"` | string | none | Stop when condition met |
| `--field $f` | field_ref | active | Target field |

### Cycle Phases

Each cycle executes the master equation in discrete phases:

```
∂A/∂t = -λA(x) + α∫K(x,y)A(y)dy + ι(x,t)
```

**Phase 1: DECAY**
- Apply decay: A(p) → A(p) × (1 - λ)
- All patterns lose activation proportionally

**Phase 2: RESONANCE**
- Compute pairwise resonances: R(p, q) for active patterns
- Cache results for amplification

**Phase 3: AMPLIFY/ATTENUATE**
- High-resonance patterns gain activation
- Isolated patterns lose activation beyond decay
- A'(p) += α × Σ_q R(p,q) × A(q)

**Phase 4: COHERENCE**
- Compute field coherence: C = μ_R / (1 + σ²_R)
- Classify: HIGH (>0.7), MEDIUM (0.5-0.7), LOW (<0.5)

**Phase 5: ATTRACTOR CHECK**
- Test emergence conditions
- If attractor emerges, report and optionally halt

### Output Format

#### Standard Output
```
[CYCLE n]
  Coherence: <value> [<class>]
  Patterns: <count> active
  [ATTRACTOR-EMERGED] <name> (if emerged)
```

#### Trace Output (--trace)
```
[CYCLE n]

  DECAY: Apply λ=<lambda>
    @pattern_a: <old> → <new>
    @pattern_b: <old> → <new>

  RESONANCE:
    @p1 ↔ @p2: R = <value> [<class>]
      Semantic: <overlap_description>
      Logical: <support_description>
      Contextual: <fit_description>

  AMPLIFY:
    @pattern_a: <old> → <new> (resonance boost)
    @pattern_b: <old> → <new> (resonance boost)

  ATTENUATE:
    @pattern_c: <old> → <new> (isolated)

  COHERENCE: <value> [<class>]

  [ATTRACTOR CHECK]
    ✓/✗ Coherence > 0.6
    ✓/✗ Energy concentrated (>70%)
    ✓/✗ Stability verified

  [ATTRACTOR-EMERGED] "<name>" (if emerged)
    Core: {@p1, @p2, ...}
    Coherence: <value>
```

### Examples

```bash
# Single cycle with trace
> nf cycle --trace
[CYCLE 1]

  DECAY: Apply λ=0.05
    @security_concern: 0.90 → 0.86
    @user_input: 0.85 → 0.81

  RESONANCE:
    @security_concern ↔ @user_input: R = 0.78 [STRONG]
      Semantic: both concern input protection
      Logical: user input is attack vector for security
      Contextual: naturally paired in security analysis

  AMPLIFY:
    @security_concern: 0.86 → 0.92 (resonance boost)
    @user_input: 0.81 → 0.88 (resonance boost)

  COHERENCE: 0.78 [HIGH]

  [ATTRACTOR CHECK]
    ✓ Coherence > 0.6
    ✓ Energy concentrated: 100%
    ✓ Stability verified

  [ATTRACTOR-EMERGED] "input_security_concern"
    Core: {@security_concern, @user_input}
    Coherence: 0.82

# Multiple cycles, compact
> nf cycle 5 --compact
[CYCLES 1-5]
  1: C=0.45, 4 active
  2: C=0.58, 4 active
  3: C=0.72, 3 active [HIGH]
  4: C=0.78, 3 active [HIGH]
  5: C=0.82, 3 active [ATTRACTOR: input_security]

# With stop condition
> nf cycle 10 --until "coherence > 0.8"
[CYCLE 1] C=0.45
[CYCLE 2] C=0.58
[CYCLE 3] C=0.72
[CYCLE 4] C=0.81 [STOP: coherence > 0.8]
  Stopped after 4 cycles
  Final coherence: 0.81
```

### Attractor Detection

An attractor emerges when ALL conditions are met:

1. **Coherence**: C > 0.6
2. **Energy Concentration**: >70% in dominant cluster
3. **Mutual Reinforcement**: Each core pattern sustained by others
4. **Stability**: Perturbation would return to same state

Detection algorithm:
```
ATTRACTOR_EMERGED(F):
  clusters ← find_high_resonance_clusters(F)
  dominant ← max_energy_cluster(clusters)

  IF coherence(dominant) < 0.6: RETURN FALSE
  IF energy_fraction(dominant) < 0.7: RETURN FALSE

  FOR p IN dominant:
    resonance_support ← Σ R(p, q) × A(q) for q ≠ p
    IF resonance_support < λ × τ: RETURN FALSE

  RETURN TRUE
```

---

## 2. Evolve

Runs dynamics until a target condition is reached.

### Syntax

```
nf evolve [--flags]
```

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--target <coherence>` | float | 0.8 | Target coherence |
| `--max <cycles>` | int | 20 | Maximum cycles |
| `--trace` | bool | false | Show trace |
| `--until "<cond>"` | string | none | Custom stop condition |

### Behavior

1. Run dynamics cycles
2. Check condition after each cycle
3. Stop when condition met or max reached
4. Report final state

### Output Format

```
[EVOLVE] Target: <condition>
  [CYCLE 1] <brief_state>
  [CYCLE 2] <brief_state>
  ...
  [TARGET MET] at cycle <n>

Final State:
  Coherence: <value>
  Patterns: <count>
  Attractors: <list>
```

### Examples

```bash
# Evolve to coherence target
> nf evolve --target 0.8
[EVOLVE] Target: coherence > 0.8
  [CYCLE 1] C=0.35, 6 patterns
  [CYCLE 2] C=0.48, 5 patterns
  [CYCLE 3] C=0.62, 4 patterns
  [CYCLE 4] C=0.75, 4 patterns
  [CYCLE 5] C=0.83, 3 patterns
  [TARGET MET] at cycle 5

Final State:
  Coherence: 0.83
  Patterns: 3 active
  Attractor: security_focus (0.83)

# Custom condition
> nf evolve --until "attractor_emerged" --max 15
[EVOLVE] Target: attractor_emerged
  [CYCLE 1] C=0.42
  [CYCLE 2] C=0.55
  [CYCLE 3] C=0.68
  [CYCLE 4] C=0.75 [ATTRACTOR EMERGED]
  [TARGET MET] at cycle 4

# With trace
> nf evolve --target 0.7 --trace
[EVOLVE] Target: coherence > 0.7

[CYCLE 1]
  DECAY: λ=0.05
    ...
  RESONANCE:
    ...
  [STATE] C=0.42, 5 patterns

[CYCLE 2]
  ...
  [STATE] C=0.58, 4 patterns

[CYCLE 3]
  ...
  [STATE] C=0.72 [TARGET MET]

Final: Coherence 0.72, 4 patterns active
```

### Errors

```
[WARNING] Max cycles reached without target
  Target: coherence > 0.9
  Achieved: 0.78 after 20 cycles
  Consider: adjusting target or field parameters

[ERROR] Field is saturated
  Too many patterns (15) prevent convergence
  Consider: nf attenuate --weak or nf reset
```

---

## 3. Step

Executes a single micro-step (decay only, no resonance/amplification).

### Syntax

```
nf step [n] [--flags]
```

### Arguments

| Argument | Type | Default | Description |
|----------|------|---------|-------------|
| `n` | int | 1 | Number of steps |

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--phase <p>` | string | decay | Phase to execute |
| `--verbose` | bool | false | Show details |

### Phases

| Phase | Effect |
|-------|--------|
| `decay` | Apply only decay (default) |
| `resonance` | Compute resonances (no activation change) |
| `amplify` | Apply only amplification |

### Output Format

```
[STEP] Phase: <phase>
  @pattern_a: <old> → <new>
  @pattern_b: <old> → <new>
```

### Examples

```bash
# Single decay step
> nf step
[STEP] Phase: decay (λ=0.05)
  @security: 0.90 → 0.86
  @injection: 0.85 → 0.81
  @input: 0.72 → 0.68

# Multiple steps
> nf step 3
[STEP] 3 decay steps (λ=0.05)
  @security: 0.90 → 0.77
  @injection: 0.85 → 0.73
  @input: 0.72 → 0.62

# Resonance-only step
> nf step --phase resonance
[STEP] Phase: resonance
  Computing pairwise resonances...
  @security ↔ @injection: 0.88
  @security ↔ @input: 0.72
  @injection ↔ @input: 0.65
  (no activation changes)
```

### Use Cases

- **Debugging**: Examine individual phases
- **Fine control**: Manual dynamics orchestration
- **Teaching**: Step through algorithm

---

## 4. Reset

Clears the field state, optionally preserving specified patterns.

### Syntax

```
nf reset [--flags]
```

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--preserve @p1 @p2` | pattern_refs | none | Keep these patterns |
| `--preserve-attractors` | bool | false | Keep attractor patterns |
| `--params` | bool | false | Reset parameters to default |
| `--confirm` | bool | false | Skip confirmation |

### Behavior

1. Clear all patterns (except preserved)
2. Clear resonance cache
3. Clear attractors (unless preserving)
4. Optionally reset parameters
5. Reset cycle counter

### Output Format

```
[RESET] Field: <field_name>
  Patterns cleared: <count>
  Patterns preserved: <count>
  Parameters: <reset/unchanged>
  Cycle count: 0
```

### Examples

```bash
# Full reset
> nf reset
[CONFIRM] Reset field 'main'? This will clear:
  - 5 patterns
  - 2 attractors
  - Resonance cache
Type 'yes' to confirm: yes

[RESET] Field: main
  Patterns cleared: 5
  Attractors cleared: 2
  Parameters: unchanged
  Cycle count: 0

# Preserve key patterns
> nf reset --preserve @hypothesis_a @evidence
[RESET] Field: main
  Patterns cleared: 4
  Patterns preserved: 2 (@hypothesis_a, @evidence)
  Parameters: unchanged
  Cycle count: 0

# Preserve attractors
> nf reset --preserve-attractors
[RESET] Field: main
  Patterns cleared: 3 (non-core)
  Attractor patterns preserved: 2
  Attractors preserved: 1 (security_focus)
  Parameters: unchanged

# Reset with parameters
> nf reset --params --confirm
[RESET] Field: main
  Patterns cleared: 5
  Parameters: reset to default
    λ: 0.03 → 0.05
    α: 0.40 → 0.30
    τ: 0.50 → 0.40
    σ: 0.70 → 0.50
  Cycle count: 0
```

### Warnings

```
[WARNING] Preserved patterns may not resonate
  @hypothesis_a resonated strongly with cleared patterns
  Current resonance support: 0.15 (was 0.72)
  Pattern may decay quickly

[WARNING] Attractors lost
  2 attractors will be cleared
  To preserve: use --preserve-attractors
```

---

## 5. Dynamics Summary

### Execution Flow

```
                    ┌─────────────┐
                    │   INJECT    │  (external)
                    └──────┬──────┘
                           │
    ┌──────────────────────▼──────────────────────┐
    │                    CYCLE                     │
    │  ┌─────────┐  ┌───────────┐  ┌───────────┐  │
    │  │  DECAY  │→ │ RESONANCE │→ │  AMPLIFY  │  │
    │  │   -λA   │  │  compute  │  │   +αΣRA   │  │
    │  └─────────┘  └───────────┘  └───────────┘  │
    │                      │                       │
    │              ┌───────▼───────┐              │
    │              │   COHERENCE   │              │
    │              │    C = μ/σ²   │              │
    │              └───────┬───────┘              │
    │                      │                       │
    │              ┌───────▼───────┐              │
    │              │   ATTRACTOR   │              │
    │              │     CHECK     │              │
    │              └───────┬───────┘              │
    └──────────────────────┼──────────────────────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
        ┌─────▼─────┐ ┌────▼────┐ ┌────▼────┐
        │  EMERGED  │ │CONTINUE │ │  STOP   │
        └─────┬─────┘ └────┬────┘ └─────────┘
              │            │
              ▼            └────→ next cycle
        ┌───────────┐
        │ COLLAPSE  │
        └───────────┘
```

### Parameter Effects on Dynamics

| Parameter | Low Value | High Value |
|-----------|-----------|------------|
| λ (decay) | Slow fading, persistent memory | Fast fading, fresh focus |
| α (amplify) | Weak resonance effects | Strong clustering |
| τ (threshold) | More patterns active | Fewer, stronger patterns |
| σ (bandwidth) | Tight clusters | Broad associations |

---

## Related Documents

- `field-ops.md` - Core operations
- `measurement.md` - Measuring field state
- `../core/field-engine.md` - Engine implementation
- `../../templates/meta/dynamics-execution.md` - Base dynamics protocol
