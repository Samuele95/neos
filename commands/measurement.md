# Measurement Commands

Commands for measuring and inspecting field state: measure, attractor, basin, and state.

---

## 1. Measure

Computes field metrics.

### Syntax

```
nf measure <metric> [--flags]
```

### Metrics

| Metric | Description |
|--------|-------------|
| `coherence` | Field organization quality |
| `energy` | Total and distributed activation |
| `entropy` | Activation distribution disorder |
| `stability` | Resistance to perturbation |

---

### 1.1 Measure Coherence

Computes the coherence score indicating how well-organized the field is.

```
nf measure coherence [--flags]
```

**Formula:**
```
Coherence(F) = μ_R / (1 + σ²_R)
```
Where:
- μ_R = mean of all pairwise resonances
- σ²_R = variance of pairwise resonances

**Classification:**
| Range | Class | Meaning |
|-------|-------|---------|
| > 0.7 | HIGH | Strong organization, attractor likely |
| 0.5-0.7 | MEDIUM | Partial organization |
| < 0.5 | LOW | Fragmented, conflicting |

**Flags:**
| Flag | Description |
|------|-------------|
| `--breakdown` | Show cluster-level coherence |
| `--history` | Show coherence over recent cycles |

**Output:**
```
[COHERENCE] <value> [<class>]
  Mean resonance (μ_R): <value>
  Resonance variance (σ²_R): <value>
  Active patterns: <count>
```

**Example:**
```bash
> nf measure coherence
[COHERENCE] 0.78 [HIGH]
  Mean resonance (μ_R): 0.72
  Resonance variance (σ²_R): 0.08
  Active patterns: 4

> nf measure coherence --breakdown
[COHERENCE] 0.78 [HIGH]
  Overall:
    μ_R: 0.72, σ²_R: 0.08

  Cluster 1 (security): 0.85
    @sql_injection, @user_input, @validation
  Cluster 2 (logging): 0.45
    @audit_log, @error_handling
```

---

### 1.2 Measure Energy

Computes the total activation energy and its distribution.

```
nf measure energy [--flags]
```

**Formula:**
```
E(F) = Σ_p A(p)²
```

**Flags:**
| Flag | Description |
|------|-------------|
| `--breakdown` | Show per-pattern energy |
| `--clusters` | Show per-cluster distribution |

**Output:**
```
[ENERGY] Total: <value>
  Distribution:
    <cluster_name>: <percent>%
    ...
```

**Example:**
```bash
> nf measure energy --breakdown
[ENERGY] Total: 2.45
  Distribution:
    @sql_injection: 0.85 (35%)
    @user_input: 0.77 (31%)
    @validation: 0.49 (20%)
    @logging: 0.21 (9%)
    @other: 0.13 (5%)

  Concentration: 86% in top 3
  Entropy: 1.45 bits

> nf measure energy --clusters
[ENERGY] Total: 2.45
  Clusters:
    security_focus: 2.11 (86%)
    auxiliary: 0.34 (14%)
```

---

### 1.3 Measure Entropy

Computes the entropy (disorder) of the activation distribution.

```
nf measure entropy
```

**Formula:**
```
H(F) = -Σ_p p(p) × log₂(p(p))
```
Where p(p) = A(p) / Σ A

**Interpretation:**
| Entropy | Meaning |
|---------|---------|
| Low (<1.5) | Concentrated on few patterns |
| Medium (1.5-2.5) | Moderate distribution |
| High (>2.5) | Diffuse, unfocused |

**Output:**
```
[ENTROPY] <value> bits
  Interpretation: <low/medium/high>
  Effective patterns: <2^H>
```

**Example:**
```bash
> nf measure entropy
[ENTROPY] 1.82 bits
  Interpretation: Medium (moderately focused)
  Effective patterns: 3.5

  Most concentrated: @security_concern (45%)
  Least concentrated: @logging (5%)
```

---

### 1.4 Measure Stability

Computes resistance to perturbation.

```
nf measure stability [--flags]
```

**Method:**
- For each pattern, compute: would removal cause permanent change?
- Aggregate stability score

**Flags:**
| Flag | Description |
|------|-------------|
| `--pattern @p` | Test specific pattern |
| `--attractor <name>` | Test attractor stability |

**Output:**
```
[STABILITY] <value> [<class>]
  Stable patterns: <count>
  Unstable patterns: <count>
  Bottleneck: <pattern> (if any)
```

**Example:**
```bash
> nf measure stability
[STABILITY] 0.85 [HIGH]
  Stable patterns: 3
    @security: would recover (R_support=0.82)
    @injection: would recover (R_support=0.78)
    @input: would recover (R_support=0.75)
  Unstable patterns: 1
    @logging: would not recover (R_support=0.15)

> nf measure stability --attractor security_focus
[STABILITY] attractor: security_focus
  Core stability: 0.92 [VERY HIGH]
  Each core pattern sustained by others:
    @security: sustained by @injection (R=0.88)
    @injection: sustained by @security (R=0.88), @input (R=0.65)
  Perturbation test: PASSED
```

---

## 2. Attractor

Commands for working with emerged attractors.

### Syntax

```
nf attractor <subcommand> [arguments] [--flags]
```

### Subcommands

| Subcommand | Description |
|------------|-------------|
| `list` | List all attractors |
| `info <name>` | Detailed attractor info |
| `compare <a1> <a2>` | Compare two attractors |

---

### 2.1 Attractor List

Lists all emerged attractors.

```
nf attractor list [--flags]
```

**Flags:**
| Flag | Description |
|------|-------------|
| `--all` | Include weak/potential attractors |
| `--history` | Show emergence timeline |

**Output:**
```
[ATTRACTORS] <count> emerged

  1. <name> (coherence: <value>)
     Core: {@p1, @p2, ...}
     Emerged: cycle <n>

  2. ...
```

**Example:**
```bash
> nf attractor list
[ATTRACTORS] 2 emerged

  1. security_focus (coherence: 0.85)
     Core: {@sql_injection, @user_input, @validation}
     Emerged: cycle 4
     Energy: 72% of field

  2. logging_concern (coherence: 0.62)
     Core: {@audit_log, @error_handling}
     Emerged: cycle 6
     Energy: 18% of field

> nf attractor list --all
[ATTRACTORS] 2 emerged, 1 potential

  Emerged:
    1. security_focus (0.85)
    2. logging_concern (0.62)

  Potential (coherence 0.4-0.6):
    3. performance_cluster (0.52)
       Core: {@query_speed, @caching}
       May emerge with more cycles
```

---

### 2.2 Attractor Info

Detailed information about a specific attractor.

```
nf attractor info <name> [--flags]
```

**Flags:**
| Flag | Description |
|------|-------------|
| `--full` | Include all metrics |
| `--history` | Show evolution |

**Output:**
```
[ATTRACTOR] <name>
  Coherence: <value>
  Stability: <value>

  Core Patterns:
    @pattern_1: <activation>
    @pattern_2: <activation>

  Resonance Structure:
    @p1 ↔ @p2: <R>
    ...

  Basin:
    Patterns that would flow here: {...}

  Semantic Summary:
    <interpretation>
```

**Example:**
```bash
> nf attractor info security_focus --full
[ATTRACTOR] security_focus
  Coherence: 0.85 [HIGH]
  Stability: 0.92 [VERY HIGH]
  Emerged: cycle 4
  Age: 6 cycles

  Core Patterns:
    @sql_injection: 0.92 (dominant)
    @user_input: 0.88
    @validation: 0.75

  Resonance Structure:
    @sql_injection ↔ @user_input: 0.88 [STRONG]
    @sql_injection ↔ @validation: 0.82 [STRONG]
    @user_input ↔ @validation: 0.78 [STRONG]
    Mean core resonance: 0.83

  Basin:
    Patterns that would flow here:
      @input_sanitization (likely)
      @parameterized_queries (likely)
      @error_messages (possible)

  Energy: 2.05 (72% of field)

  Semantic Summary:
    This attractor represents concern about SQL injection
    vulnerabilities through user input. The stable configuration
    indicates this is a well-founded and coherent interpretation.
```

---

### 2.3 Attractor Compare

Compares two attractors.

```
nf attractor compare <a1> <a2>
```

**Output:**
```
[COMPARE] <a1> vs <a2>

                    <a1>        <a2>
  Coherence:        <val>       <val>
  Stability:        <val>       <val>
  Energy:           <val>       <val>

  Core overlap: <patterns in both>
  Exclusive to <a1>: <patterns>
  Exclusive to <a2>: <patterns>

  Inter-attractor resonance: <value>
  Relationship: <competing/complementary/orthogonal>
```

**Example:**
```bash
> nf attractor compare security_focus logging_concern
[COMPARE] security_focus vs logging_concern

                    security_focus  logging_concern
  Coherence:        0.85            0.62
  Stability:        0.92            0.68
  Energy:           72%             18%

  Core overlap: none
  Exclusive to security_focus: @sql_injection, @user_input, @validation
  Exclusive to logging_concern: @audit_log, @error_handling

  Inter-attractor resonance: 0.35
  Relationship: Complementary (low overlap, moderate resonance)

  Interpretation:
    These attractors address different aspects of the analysis.
    security_focus is dominant; logging_concern is secondary.
```

---

## 3. Basin

Analyzes the basin of attraction.

### Syntax

```
nf basin @attractor [--flags]
```

### Flags

| Flag | Description |
|------|-------------|
| `--map` | Generate basin visualization |
| `--boundary` | Show basin boundaries |
| `--test @pattern` | Test if pattern in basin |

### Output

```
[BASIN] <attractor_name>
  Size: <fraction> of semantic space
  Depth: <value> (attractor strength)

  Core (always in basin):
    @pattern_1, @pattern_2, ...

  Likely to flow here:
    @pattern_a (distance: <d>)
    @pattern_b (distance: <d>)

  Boundary patterns (could go either way):
    @pattern_x (51% this basin, 49% other)
```

### Example

```bash
> nf basin @security_focus
[BASIN] security_focus
  Size: ~65% of active semantic space
  Depth: 0.85 (strong attractor)

  Core (always in basin):
    @sql_injection, @user_input, @validation

  Likely to flow here:
    @input_sanitization (distance: 0.15)
    @parameterized_queries (distance: 0.22)
    @xss_prevention (distance: 0.28)

  Boundary patterns:
    @error_messages (55% security, 45% logging)
    @monitoring (48% security, 52% logging)

> nf basin @security_focus --test @authentication
[BASIN TEST] @authentication → security_focus
  Result: LIKELY IN BASIN
  Estimated flow time: 3 cycles
  Resonance with core: 0.72
```

---

## 4. State

Dumps full field state.

### Syntax

```
nf state [--flags]
```

### Flags

| Flag | Description |
|------|-------------|
| `--json` | JSON format |
| `--algebraic` | Algebraic notation |
| `--compact` | Brief summary |
| `--field $f` | Specific field |

### Output Formats

#### Default
```
[FIELD STATE] <field_name>
  Parameters: λ=<v>, α=<v>, τ=<v>, σ=<v>
  Cycle: <n>
  Coherence: <value> [<class>]

  Patterns (<count> active):
    @pattern_1: <activation> [tags]
    @pattern_2: <activation> [tags]
    ...

  Attractors (<count>):
    <name>: <coherence>

  Resonance Cache (<count> pairs):
    @p1 ↔ @p2: <R>
    ...
```

#### JSON
```json
{
  "field": "main",
  "parameters": {
    "lambda": 0.05,
    "alpha": 0.30,
    "tau": 0.40,
    "sigma": 0.50
  },
  "cycle": 10,
  "coherence": 0.78,
  "patterns": {...},
  "attractors": [...],
  "resonance_cache": {...}
}
```

#### Algebraic
```
Field State: |F⟩

  Parameters: λ=0.05, α=0.30, τ=0.40, σ=0.50

  State Vector: |ψ⟩ = Σᵢ aᵢ|pᵢ⟩
    |security⟩: a = 0.92
    |injection⟩: a = 0.88
    |input⟩: a = 0.75

  Resonance Operator R̂:
           sec   inj   inp
    sec  [ 1.00  0.88  0.72 ]
    inj  [ 0.88  1.00  0.65 ]
    inp  [ 0.72  0.65  1.00 ]

  Eigenstructure:
    λ₁ = 2.45, |e₁⟩ = dominant_mode
    λ₂ = 0.38, |e₂⟩ = secondary_mode
```

### Examples

```bash
> nf state --compact
[FIELD] main
  Cycle: 10, Coherence: 0.78 [HIGH]
  Patterns: 4 active
  Attractors: 1 (security_focus)
  Energy: 2.45

> nf state
[FIELD STATE] main
  Parameters: λ=0.05, α=0.30, τ=0.40, σ=0.50
  Cycle: 10
  Coherence: 0.78 [HIGH]

  Patterns (4 active):
    @sql_injection: 0.92 [vulnerability, input]
    @user_input: 0.88 [boundary, validation]
    @validation: 0.75 [security]
    @logging: 0.35 [auxiliary]

  Attractors (1):
    security_focus: 0.85 (core: sql_injection, user_input, validation)

  Resonance Cache (6 pairs):
    @sql_injection ↔ @user_input: 0.88
    @sql_injection ↔ @validation: 0.82
    @user_input ↔ @validation: 0.78
    @sql_injection ↔ @logging: 0.25
    @user_input ↔ @logging: 0.30
    @validation ↔ @logging: 0.22
```

---

## 5. Quick Reference

```
COHERENCE                       ENERGY
─────────────────────────────── ───────────────────────────────
nf measure coherence            nf measure energy
  > 0.7: HIGH                     Shows total and distribution
  0.5-0.7: MEDIUM                 --breakdown: per-pattern
  < 0.5: LOW                      --clusters: per-cluster

ATTRACTORS                      BASIN
─────────────────────────────── ───────────────────────────────
nf attractor list               nf basin @attractor
nf attractor info <name>          Shows size, depth, boundaries
nf attractor compare <a> <b>      --map: visualization
                                  --test @p: membership test

STATE
───────────────────────────────
nf state                        Full dump
nf state --compact              Brief summary
nf state --json                 JSON format
nf state --algebraic            Mathematical notation
```

---

## Related Documents

- `field-ops.md` - Core operations
- `dynamics.md` - Dynamics control
- `../visualization/` - Visual representations
- `../../foundations/04-attractors.md` - Attractor theory
