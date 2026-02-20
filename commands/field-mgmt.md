# Field Management Commands

Commands for creating, managing, and orchestrating multiple neural fields.

---

## Overview

Multi-field orchestration enables specialized fields that work together. These commands manage the field system Σ = ({Fᵢ}, C, O).

---

## 1. Field Command

Creates, lists, and manages individual fields.

### 1.1 Create Field

```
nf field create <name> [--params] [--role <role>]
```

**Parameters:**
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `name` | string | required | Unique field identifier |
| `--lambda` | float | 0.05 | Decay rate |
| `--alpha` | float | 0.3 | Amplification |
| `--tau` | float | 0.4 | Attractor threshold |
| `--sigma` | float | 0.5 | Resonance bandwidth |
| `--role` | string | none | Specialized role |

**Roles:**
| Role | Configuration | Purpose |
|------|---------------|---------|
| `reasoning` | High τ, structured | Logical inference |
| `knowledge` | Low λ, persistent | Factual storage |
| `planning` | Hierarchical attractors | Goal decomposition |
| `evaluation` | Comparative resonance | Quality assessment |
| `creative` | Low τ, wide σ | Novel generation |
| `meta` | Monitors others | Orchestration |

**Examples:**
```
> nf field create perception --role reasoning
[FIELD] Created 'perception'
  Role: reasoning
  Parameters: λ=0.03, α=0.35, τ=0.5, σ=0.4

> nf field create memory --lambda 0.01 --role knowledge
[FIELD] Created 'memory'
  Role: knowledge
  Parameters: λ=0.01, α=0.3, τ=0.4, σ=0.5
```

**Output Format:**
```
[FIELD] Created '<name>'
  Role: <role>
  Parameters: λ=<lambda>, α=<alpha>, τ=<tau>, σ=<sigma>
```

---

### 1.2 List Fields

```
nf field list [--verbose]
```

**Output:**
```
> nf field list
[FIELDS] 4 active fields

  NAME         ROLE        PATTERNS  COHERENCE  STATUS
  main         reasoning   8         0.72       active
  memory       knowledge   15        0.85       active
  creative     creative    5         0.45       idle
  meta         meta        3         0.68       active

> nf field list --verbose
[FIELDS] Detailed view

  main (reasoning)
    Parameters: λ=0.05, α=0.3, τ=0.4, σ=0.5
    Patterns: 8 active, 3 attractors
    Coherence: 0.72
    Couplings: → memory (0.6), → creative (0.3)
    Status: active (last cycle: 2.3s ago)
```

---

### 1.3 Select Field

```
nf field select <name>
```

Sets the active field for subsequent commands.

**Example:**
```
> nf field select memory
[FIELD] Active field: memory
  Patterns: 15
  Coherence: 0.85
```

**Note:** Most commands operate on the active field. Use `$field:` prefix to target specific fields:
```
> nf inject "concept" 0.8 --field memory
```

---

### 1.4 Delete Field

```
nf field delete <name> [--force]
```

**Example:**
```
> nf field delete creative
[WARNING] Field 'creative' has 3 couplings. Delete anyway? [y/N]

> nf field delete creative --force
[FIELD] Deleted 'creative'
  Removed 3 couplings
```

---

### 1.5 Clone Field

```
nf field clone <source> <target> [--include-patterns]
```

Creates a copy of a field with its parameters.

**Example:**
```
> nf field clone main analysis --include-patterns
[FIELD] Cloned 'main' → 'analysis'
  Parameters: copied
  Patterns: 8 copied
  Couplings: not copied
```

---

### 1.6 Field Info

```
nf field info [name]
```

**Output:**
```
> nf field info main
[FIELD] main

  Role: reasoning
  Created: 2024-01-15 10:30:22

  Parameters:
    λ (decay):        0.05
    α (amplification): 0.30
    τ (threshold):    0.40
    σ (bandwidth):    0.50

  State:
    Patterns:    8
    Attractors:  2
    Coherence:   0.72
    Energy:      0.45

  Couplings:
    → memory     (γ=0.60, excitatory)
    ← meta       (γ=0.40, excitatory)
    ↔ creative   (γ=0.30, bidirectional)

  History:
    Cycles: 47
    Commits: 5
    Last modified: 2 minutes ago
```

---

## 2. Route Command

Manages information flow between fields.

### 2.1 Create Route

```
nf route <source> <target> [--strength <s>] [--filter <expr>]
```

**Parameters:**
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `source` | field | required | Source field |
| `target` | field | required | Target field |
| `--strength` | float | 1.0 | Transfer strength (0-1) |
| `--filter` | expr | none | Pattern filter expression |
| `--transform` | string | none | Transform type |
| `--bidirectional` | bool | false | Create two-way route |

**Transforms:**
| Transform | Description |
|-----------|-------------|
| `none` | Direct transfer |
| `abstract` | Increase abstraction level |
| `concrete` | Decrease abstraction level |
| `invert` | Flip activation polarity |

**Examples:**
```
> nf route memory main --strength 0.8
[ROUTE] Created: memory → main
  Strength: 0.80
  Filter: none
  Transform: none

> nf route main evaluation --filter "activation > 0.5"
[ROUTE] Created: main → evaluation
  Strength: 1.00
  Filter: activation > 0.5
  Transform: none

> nf route main creative --bidirectional --strength 0.4
[ROUTE] Created bidirectional:
  main ↔ creative (strength: 0.40)
```

---

### 2.2 List Routes

```
nf route list [--field <name>]
```

**Output:**
```
> nf route list
[ROUTES] 5 active routes

  SOURCE      TARGET      STRENGTH  FILTER           TRANSFORM
  memory      main        0.80      none             none
  main        evaluation  1.00      activation>0.5   none
  main        creative    0.40      none             none
  creative    main        0.40      none             none
  meta        *           1.00      none             none

> nf route list --field main
[ROUTES] Routes involving 'main'

  Incoming:
    memory → main (0.80)
    creative → main (0.40)

  Outgoing:
    main → evaluation (1.00, filtered)
    main → creative (0.40)
```

---

### 2.3 Delete Route

```
nf route delete <source> <target>
```

**Example:**
```
> nf route delete main creative
[ROUTE] Deleted: main → creative
[WARNING] Bidirectional route partially removed. Delete reverse? [y/N]
```

---

### 2.4 Transfer

Manually transfer patterns between fields.

```
nf route transfer <source> <target> [--patterns <list>]
```

**Examples:**
```
> nf route transfer memory main
[TRANSFER] memory → main
  Patterns transferred: 5 (above coherence threshold)
  Injection strength: 0.80 (route strength)

> nf route transfer memory main --patterns @concept1,@concept2
[TRANSFER] memory → main
  Patterns transferred: 2 (specified)
  Injection strength: 0.80
```

---

## 3. Couple Command

Manages resonance coupling between fields.

### 3.1 Set Coupling

```
nf couple <field1> <field2> [--gamma <g>] [--type <t>]
```

**Parameters:**
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `field1` | field | required | First field |
| `field2` | field | required | Second field |
| `--gamma` | float | 0.3 | Coupling strength |
| `--type` | string | excitatory | Coupling type |
| `--asymmetric` | bool | false | Different γ each direction |

**Types:**
| Type | γ | Effect |
|------|---|--------|
| `excitatory` | > 0 | Fields reinforce each other |
| `inhibitory` | < 0 | Fields suppress each other |
| `competitive` | < 0, asymmetric | Winner-take-all |

**Examples:**
```
> nf couple main memory --gamma 0.6
[COUPLE] Set: main ↔ memory
  Coupling strength: γ = 0.60
  Type: excitatory (bidirectional)

> nf couple main creative --gamma 0.3 --type excitatory
[COUPLE] Set: main ↔ creative
  Coupling strength: γ = 0.30
  Type: excitatory

> nf couple reasoning evaluation --gamma -0.2 --type inhibitory
[COUPLE] Set: reasoning ↔ evaluation
  Coupling strength: γ = -0.20
  Type: inhibitory (mutual suppression)
```

---

### 3.2 Asymmetric Coupling

```
nf couple <field1> <field2> --asymmetric --gamma1 <g1> --gamma2 <g2>
```

**Example:**
```
> nf couple meta main --asymmetric --gamma1 0.7 --gamma2 0.2
[COUPLE] Set asymmetric:
  meta → main: γ = 0.70
  main → meta: γ = 0.20
```

---

### 3.3 List Couplings

```
nf couple list [--matrix]
```

**Output:**
```
> nf couple list
[COUPLINGS] 4 active couplings

  FIELD1    FIELD2      GAMMA    TYPE
  main      memory      0.60     excitatory
  main      creative    0.30     excitatory
  main      meta        0.70/0.20 asymmetric
  reasoning evaluation  -0.20    inhibitory

> nf couple list --matrix
[COUPLING MATRIX]

              main    memory  creative  meta    eval
main           -      0.60    0.30      0.20     -
memory        0.60     -       -         -       -
creative      0.30     -       -         -       -
meta          0.70     -       -         -       -
evaluation     -       -       -         -       -

  Spectral radius ρ(Γ) = 0.82
  Stability: ρ(Γ) < λ_min/α_max = 0.17 [WARNING: Near limit]
```

---

### 3.4 Remove Coupling

```
nf couple remove <field1> <field2>
```

---

## 4. Activate/Deactivate Commands

### 4.1 Activate Field

```
nf activate <field> [--priority <p>]
```

Brings a field online for processing.

**Example:**
```
> nf activate creative --priority high
[ACTIVATE] Field 'creative' now active
  Priority: high
  Couplings engaged: 2
```

---

### 4.2 Deactivate Field

```
nf deactivate <field> [--preserve]
```

Takes a field offline.

**Example:**
```
> nf deactivate creative --preserve
[DEACTIVATE] Field 'creative' now idle
  State preserved
  Couplings disengaged: 2
```

---

## 5. Orchestration Commands

### 5.1 Sync Fields

```
nf sync [--fields <list>] [--cycles <n>]
```

Runs coupled dynamics across multiple fields.

**Example:**
```
> nf sync --cycles 3
[SYNC] Running 3 synchronized cycles across 4 fields

  Cycle 1:
    main:     C=0.68 → 0.72
    memory:   C=0.85 → 0.86
    creative: C=0.45 → 0.52
    meta:     C=0.68 → 0.70
    Cross-field resonance: 0.58

  Cycle 2:
    main:     C=0.72 → 0.75
    memory:   C=0.86 → 0.87
    creative: C=0.52 → 0.58
    meta:     C=0.70 → 0.72
    Cross-field resonance: 0.65

  Cycle 3:
    main:     C=0.75 → 0.78
    memory:   C=0.87 → 0.88
    creative: C=0.58 → 0.62
    meta:     C=0.72 → 0.74
    Cross-field resonance: 0.71

[COMPLETE] System coherence: 0.76
```

---

### 5.2 Pipeline

```
nf pipeline <f1> <f2> [<f3>...] [--input <pattern>]
```

Executes sequential orchestration.

**Example:**
```
> nf pipeline memory main evaluation --input "security_concern"
[PIPELINE] memory → main → evaluation

  Stage 1: memory
    Input: security_concern
    Cycles: 2
    Output: 3 patterns (security, vulnerability, mitigation)

  Stage 2: main
    Input: 3 patterns from memory
    Cycles: 4
    Output: 2 patterns (sql_injection_risk, input_validation)
    Attractor emerged: security_analysis

  Stage 3: evaluation
    Input: 2 patterns from main
    Cycles: 2
    Output: confidence=0.82, recommendation="high priority"

[COMPLETE] Pipeline finished
  Total cycles: 8
  Final output: security_analysis (confidence: 0.82)
```

---

### 5.3 Parallel

```
nf parallel <f1>,<f2>[,<f3>...] [--input <pattern>] [--integrate <method>]
```

Executes parallel orchestration.

**Integration Methods:**
| Method | Description |
|--------|-------------|
| `average` | Weighted average of outputs |
| `resonance` | Resonance-based fusion |
| `competition` | Highest activation wins |
| `union` | All unique patterns |

**Example:**
```
> nf parallel technical,user,business --input "feature_request" --integrate resonance
[PARALLEL] Running 3 fields simultaneously

  technical:  analyzing... C=0.72
  user:       analyzing... C=0.68
  business:   analyzing... C=0.75

[INTEGRATION] Resonance-based fusion
  Cross-field resonance matrix computed
  High resonance pairs: 4
  Conflicts detected: 2

  Integrated result:
    priority: 0.78 (consensus)
    complexity: 0.65 (technical)
    user_value: 0.82 (user)
    roi: 0.71 (business)

  Conflicts:
    timeline: technical(long) vs business(short) - R=0.25
    scope: user(broad) vs technical(narrow) - R=0.32
```

---

### 5.4 Arbitrate

```
nf arbitrate <field1> <field2> [--strategy <s>]
```

Resolves conflicts between fields.

**Strategies:**
| Strategy | Description |
|----------|-------------|
| `resonance` | Higher cross-field resonance wins |
| `coherence` | Higher coherence field wins |
| `priority` | Field priority determines winner |
| `merge` | Attempt to merge perspectives |

**Example:**
```
> nf arbitrate technical business --strategy merge
[ARBITRATE] Resolving: technical ↔ business

  Conflict: timeline
    technical: "6 months minimum" (0.78)
    business: "2 months target" (0.72)

  Resolution (merge):
    "phased delivery: MVP in 2 months, full in 5"
    Resonance with technical: 0.68
    Resonance with business: 0.71

  Injecting resolution into both fields...
[COMPLETE] Conflict resolved
```

---

## 6. Quick Reference

```
FIELD MANAGEMENT
────────────────────────────────────────────────────────
nf field create <name> [--role <r>] [--params...]
nf field list [--verbose]
nf field select <name>
nf field delete <name> [--force]
nf field clone <src> <dst> [--include-patterns]
nf field info [name]

ROUTING
────────────────────────────────────────────────────────
nf route <src> <dst> [--strength s] [--filter expr]
nf route list [--field <name>]
nf route delete <src> <dst>
nf route transfer <src> <dst> [--patterns list]

COUPLING
────────────────────────────────────────────────────────
nf couple <f1> <f2> [--gamma g] [--type t]
nf couple list [--matrix]
nf couple remove <f1> <f2>

ACTIVATION
────────────────────────────────────────────────────────
nf activate <field> [--priority p]
nf deactivate <field> [--preserve]

ORCHESTRATION
────────────────────────────────────────────────────────
nf sync [--fields list] [--cycles n]
nf pipeline <f1> <f2> ... [--input pattern]
nf parallel <f1,f2,...> [--integrate method]
nf arbitrate <f1> <f2> [--strategy s]
```

---

## Related Documents

- `../core/state-manager.md` - Multi-field state management
- `../../foundations/07-field-orchestration.md` - Theoretical foundation
- `../examples/04-multi-field.md` - Orchestration walkthrough
