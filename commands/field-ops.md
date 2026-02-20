# Field Operations Commands

Detailed specification for the five core field operations: inject, amplify, attenuate, tune, and collapse.

---

## 1. Inject

Introduces a new pattern into the active field.

### Syntax

```
nf inject "<pattern_name>" [strength] [--flags]
```

### Arguments

| Argument | Type | Default | Description |
|----------|------|---------|-------------|
| `pattern_name` | string | required | Name/identifier for the pattern |
| `strength` | float | 0.8 | Initial activation (0.0-1.0) |

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--into $field` | field_ref | active | Target field |
| `--tags t1,t2` | string list | none | Semantic tags |
| `--position x,y,z` | float tuple | auto | Semantic coordinates |
| `--quiet` | bool | false | Suppress output |

### Behavior

1. Create pattern entry with given name and strength
2. Assign unique pattern ID
3. Position in semantic space (auto-place if not specified)
4. Trigger resonance computation with existing patterns (optional, immediate)
5. Update field state

### Mathematical Foundation

From the master equation, injection corresponds to the ι(x,t) term:

```
A'(x) = A(x) + ι(x,t)
```

Where ι is a localized activation bump at the injected position.

### Output Format

```
[INJECT] <pattern_name> (s: <strength>)
  Field: <field_name>
  ID: <pattern_id>
  Position: (<x>, <y>, <z>)
  Tags: [<tags>]
  Patterns active: <count>

  [IMMEDIATE RESONANCE] (if significant)
    ↔ @existing_pattern: R = <value>
```

### Examples

```bash
# Basic injection
> nf inject "security_concern" 0.9
[INJECT] security_concern (s: 0.90)
  Field: main
  ID: p_001
  Position: (0.42, 0.78, 0.33)
  Patterns active: 1

# With tags
> nf inject "sql_injection" 0.85 --tags vulnerability,input
[INJECT] sql_injection (s: 0.85)
  Field: main
  ID: p_002
  Tags: [vulnerability, input]
  Patterns active: 2

  [IMMEDIATE RESONANCE]
    ↔ @security_concern: R = 0.78

# Into specific field
> nf inject "hypothesis" 0.7 --into $reasoning
[INJECT] hypothesis (s: 0.70)
  Field: reasoning
  Patterns active: 5
```

### Errors

```
[ERROR] Pattern already exists: @security_concern
  Use: nf amplify @security_concern to strengthen
  Or:  nf remove @security_concern first

[ERROR] Invalid strength: 1.5
  Strength must be in range [0.0, 1.0]

[WARNING] Field approaching saturation
  Active patterns: 11 (recommended max: 12)
  Consider consolidating patterns
```

---

## 2. Amplify

Increases the activation of an existing pattern, typically based on resonance.

### Syntax

```
nf amplify @pattern [factor] [--flags]
```

### Arguments

| Argument | Type | Default | Description |
|----------|------|---------|-------------|
| `@pattern` | pattern_ref | required | Pattern to amplify |
| `factor` | float | 1.5 | Amplification multiplier |

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--max <value>` | float | 1.0 | Maximum activation cap |
| `--resonance` | bool | false | Scale by total resonance |
| `--with @pattern` | pattern_ref | none | Amplify by resonance with specific pattern |

### Behavior

1. Locate pattern in field
2. Compute new activation: A' = min(A × factor, max)
3. If `--resonance`: factor = 1 + Σ R(p, q) × A(q)
4. If `--with @q`: factor = 1 + R(p, q) × A(q)
5. Update pattern activation

### Mathematical Foundation

Amplification implements the resonance integral term:

```
A'(x) = A(x) + α∫K(x,y)A(y)dy
```

With factor serving as the effective α for the operation.

### Output Format

```
[AMPLIFY] @<pattern_name>
  Activation: <old> → <new>
  Factor: <factor>
  Resonance contribution: <value> (if --resonance)
```

### Examples

```bash
# Basic amplification
> nf amplify @security_concern 1.3
[AMPLIFY] @security_concern
  Activation: 0.85 → 1.00 (capped)
  Factor: 1.3

# Resonance-based
> nf amplify @security_concern --resonance
[AMPLIFY] @security_concern
  Activation: 0.72 → 0.89
  Factor: 1.24 (from resonance)
  Contributing patterns:
    @sql_injection: R=0.78, contrib=0.12
    @user_input: R=0.65, contrib=0.08

# With specific pattern
> nf amplify @security_concern --with @evidence
[AMPLIFY] @security_concern
  Activation: 0.72 → 0.84
  Factor: 1.17 (R=0.82 with @evidence)
```

### Errors

```
[ERROR] Pattern not found: @nonexistent
  Active patterns: @security, @input, @validation

[WARNING] Pattern at maximum activation
  @security_concern: 1.00 (cannot increase further)
```

---

## 3. Attenuate

Decreases the activation of a pattern, implementing decay or suppression.

### Syntax

```
nf attenuate @pattern [factor] [--flags]
```

### Arguments

| Argument | Type | Default | Description |
|----------|------|---------|-------------|
| `@pattern` | pattern_ref | required | Pattern to attenuate |
| `factor` | float | 0.5 | Attenuation multiplier (0-1) |

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--to <value>` | float | none | Set to specific value |
| `--weak` | bool | false | Attenuate all patterns below threshold |
| `--threshold <t>` | float | 0.3 | Threshold for --weak |

### Behavior

1. Locate pattern in field
2. Compute new activation:
   - Default: A' = A × factor
   - With `--to`: A' = value
3. If A' < τ (field threshold), pattern may be marked dormant
4. Update pattern activation

### Mathematical Foundation

Attenuation implements the decay term:

```
A'(x) = A(x) × (1 - λ)
```

Where factor = (1 - λ) for decay, or arbitrary for suppression.

### Output Format

```
[ATTENUATE] @<pattern_name>
  Activation: <old> → <new>
  Factor: <factor>
  [DORMANT] (if below threshold τ)
```

### Examples

```bash
# Basic attenuation
> nf attenuate @noise_pattern 0.5
[ATTENUATE] @noise_pattern
  Activation: 0.60 → 0.30
  Factor: 0.5

# Set to specific value
> nf attenuate @distraction --to 0.2
[ATTENUATE] @distraction
  Activation: 0.75 → 0.20
  Set to: 0.2

# Attenuate all weak patterns
> nf attenuate --weak --threshold 0.4
[ATTENUATE] Weak patterns (A < 0.4)
  @noise_a: 0.35 → 0.18 [DORMANT]
  @noise_b: 0.28 → 0.14 [DORMANT]
  Patterns attenuated: 2
  Patterns now dormant: 2
```

### Errors

```
[ERROR] Pattern not found: @nonexistent

[WARNING] Pattern already dormant
  @old_pattern: 0.05 (below threshold τ=0.40)
```

---

## 4. Tune

Adjusts field parameters that govern dynamics.

### Syntax

```
nf tune <param>=<value> [<param>=<value>...] [--flags]
```

### Parameters

| Parameter | Symbol | Range | Default | Effect |
|-----------|--------|-------|---------|--------|
| `lambda` | λ | 0.0-1.0 | 0.05 | Decay rate |
| `alpha` | α | 0.0-1.0 | 0.30 | Amplification strength |
| `tau` | τ | 0.0-1.0 | 0.40 | Activation threshold |
| `sigma` | σ | 0.0-∞ | 0.50 | Resonance bandwidth |

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--field $f` | field_ref | active | Target field |
| `--preset <name>` | string | none | Apply parameter preset |

### Presets

| Preset | λ | α | τ | σ | Use Case |
|--------|---|---|---|---|----------|
| `default` | 0.05 | 0.30 | 0.40 | 0.50 | Balanced |
| `persistent` | 0.02 | 0.35 | 0.30 | 0.50 | Long memory |
| `volatile` | 0.10 | 0.25 | 0.50 | 0.40 | Fast decay |
| `broad` | 0.05 | 0.30 | 0.35 | 0.80 | Wide associations |
| `focused` | 0.05 | 0.40 | 0.50 | 0.30 | Tight clusters |

### Behavior

1. Parse parameter assignments
2. Validate ranges
3. Update field parameters
4. Optionally re-evaluate thresholds (patterns may become dormant)

### Output Format

```
[TUNE] Field: <field_name>
  lambda (λ): <old> → <new>  # decay rate
  alpha (α): <old> → <new>   # amplification
  tau (τ): <old> → <new>     # threshold
  sigma (σ): <old> → <new>   # bandwidth

  [THRESHOLD EFFECT] (if τ changed)
    Patterns now dormant: <count>
```

### Examples

```bash
# Adjust single parameter
> nf tune lambda=0.03
[TUNE] Field: main
  lambda (λ): 0.05 → 0.03

# Multiple parameters
> nf tune lambda=0.02 alpha=0.35 sigma=0.7
[TUNE] Field: main
  lambda (λ): 0.05 → 0.02
  alpha (α): 0.30 → 0.35
  sigma (σ): 0.50 → 0.70

# Apply preset
> nf tune --preset focused
[TUNE] Field: main (preset: focused)
  lambda (λ): 0.05 → 0.05
  alpha (α): 0.30 → 0.40
  tau (τ): 0.40 → 0.50
  sigma (σ): 0.50 → 0.30

  [THRESHOLD EFFECT]
    Patterns now dormant: 2
    @weak_pattern_a, @weak_pattern_b
```

### Errors

```
[ERROR] Invalid parameter: lambdaa
  Valid parameters: lambda, alpha, tau, sigma

[ERROR] Value out of range: lambda=2.0
  lambda must be in [0.0, 1.0]

[WARNING] High threshold may prune patterns
  tau=0.6 would make 4 patterns dormant
  Proceed with 'nf tune tau=0.6 --force'
```

---

## 5. Collapse

Resolves the distributed field state into a concrete output.

### Syntax

```
nf collapse [--strategy <strategy>] [--flags]
```

### Strategies

| Strategy | Description | Use Case |
|----------|-------------|----------|
| `attractor` | Follow dominant attractor | Clear convergence |
| `threshold` | Include all above threshold | Comprehensive output |
| `weighted` | Weight by activation | Nuanced response |
| `sample` | Probabilistic sampling | Creative variety |

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--strategy <s>` | string | attractor | Collapse strategy |
| `--threshold <t>` | float | 0.5 | Threshold for threshold strategy |
| `--format <f>` | string | text | Output format (text, json, structured) |
| `--include-basin` | bool | false | Include basin patterns |

### Strategy Details

#### Attractor Strategy (default)
```
1. Identify dominant attractor (highest coherence)
2. Extract core patterns
3. Generate output from attractor semantics
4. Confidence = attractor coherence
```

#### Threshold Strategy
```
1. Select all patterns with A > threshold
2. Rank by activation
3. Generate output covering all selected
4. Confidence = mean activation
```

#### Weighted Strategy
```
1. Weight all patterns by activation
2. Synthesize output proportionally
3. Confidence = energy concentration
```

#### Sample Strategy
```
1. Sample pattern based on activation distribution
2. Generate output from sampled perspective
3. Useful for creative/diverse outputs
```

### Output Format

```
[COLLAPSE] Strategy: <strategy>
  Dominant attractor: <name> (<coherence>)

OUTPUT:
  <primary_content>

  Supporting: {<supporting_patterns>}
  Confidence: <HIGH|MEDIUM|LOW> (<value>)
```

### Examples

```bash
# Default attractor collapse
> nf collapse
[COLLAPSE] Strategy: attractor
  Dominant attractor: input_security_concern (0.82)

OUTPUT:
  The analysis converges on input security as the primary concern.
  SQL injection risk through user input handling requires
  parameterized queries and input validation.

  Supporting: {user_input, validation_need}
  Confidence: HIGH (0.82)

# Threshold collapse
> nf collapse --strategy threshold --threshold 0.6
[COLLAPSE] Strategy: threshold (t=0.6)
  Patterns selected: 4

OUTPUT:
  Primary concerns (by activation):
  1. SQL injection risk (0.92)
  2. User input handling (0.88)
  3. Authentication weakness (0.75)
  4. Session management (0.65)

  Confidence: MEDIUM (0.80)

# Weighted synthesis
> nf collapse --strategy weighted
[COLLAPSE] Strategy: weighted
  Energy distribution:
    security_cluster: 72%
    performance_cluster: 18%
    other: 10%

OUTPUT:
  The field suggests primarily security-focused concerns (72%)
  with secondary performance considerations (18%). Security
  aspects dominate the analysis...

  Confidence: HIGH (0.90 concentration)

# Structured output
> nf collapse --format structured
[COLLAPSE] Strategy: attractor

OUTPUT:
{
  "primary": {
    "conclusion": "Input validation required",
    "attractor": "input_security_concern",
    "coherence": 0.82
  },
  "supporting": [
    {"pattern": "sql_injection", "activation": 0.92},
    {"pattern": "user_input", "activation": 0.88}
  ],
  "confidence": "HIGH"
}
```

### Errors

```
[ERROR] No attractor emerged
  Field coherence: 0.35 (below 0.6 threshold)
  Consider: nf cycle --until "coherence > 0.6"
  Or: nf collapse --strategy threshold

[WARNING] Multiple attractors with similar strength
  attractor_a: 0.78
  attractor_b: 0.75
  Consider: nf evolve to resolve competition
  Or: nf collapse --strategy weighted
```

---

## 6. Resonate

Computes resonance between patterns.

### Syntax

```
nf resonate [@p1 @p2] [--flags]
```

### Arguments

| Argument | Type | Default | Description |
|----------|------|---------|-------------|
| `@p1 @p2` | pattern_refs | optional | Specific pair |

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--all` | bool | false | Compute all pairwise |
| `--above <t>` | float | 0.0 | Only show R > t |
| `--matrix` | bool | false | Output as matrix |
| `--explain` | bool | false | Show resonance breakdown |

### Behavior

1. If pair specified: compute R(p1, p2)
2. If `--all`: compute all pairwise resonances
3. Cache results for dynamics
4. Classify: STRONG (>0.7), MODERATE (0.4-0.7), WEAK (<0.4)

### Resonance Formula

```
R(A, B) = (overlap × support × fit)^(1/3)
```

Where:
- overlap: semantic similarity
- support: logical relationship
- fit: contextual appropriateness

### Output Format

```
[RESONANCE] @<p1> ↔ @<p2>
  Semantic overlap: <description>
  Logical support: <description>
  Contextual fit: <description>
  R = <value> [<classification>]
```

### Examples

```bash
# Specific pair
> nf resonate @security @injection
[RESONANCE] @security ↔ @injection
  Semantic overlap: shared concern for system protection
  Logical support: injection IS a security issue
  Contextual fit: naturally paired in security analysis
  R = 0.88 [STRONG]

# All pairwise
> nf resonate --all --above 0.5
[RESONANCE] All pairs (R > 0.5)
  @security ↔ @injection: 0.88 [STRONG]
  @security ↔ @user_input: 0.72 [STRONG]
  @injection ↔ @user_input: 0.65 [MODERATE]

  Mean resonance: 0.75
  Max: 0.88 (@security ↔ @injection)

# Matrix format
> nf resonate --all --matrix
[RESONANCE MATRIX]
              security  injection  user_input  logging
security      1.00      0.88       0.72        0.25
injection     0.88      1.00       0.65        0.18
user_input    0.72      0.65       1.00        0.30
logging       0.25      0.18       0.30        1.00
```

---

## Related Documents

- `../core/field-engine.md` - Operation implementation details
- `dynamics.md` - Dynamics commands
- `measurement.md` - Measurement commands
- `../foundations/05-operations.md` - Mathematical foundations
