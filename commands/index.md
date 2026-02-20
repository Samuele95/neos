# NFOS Command Language Specification

Complete reference for the NFOS command interface.

---

## 1. Command Syntax

### General Format

```
nf <command> [subcommand] [arguments...] [--flags...]
```

### Components

| Component | Description | Example |
|-----------|-------------|---------|
| `nf` | Command prefix (required) | `nf` |
| `command` | Primary command | `inject`, `cycle`, `plot` |
| `subcommand` | Secondary action | `field create`, `attractor list` |
| `arguments` | Positional values | `"pattern_name" 0.8` |
| `--flags` | Named options | `--trace`, `--strategy attractor` |

### Quoting Rules

- Pattern names with spaces: `"sql injection risk"`
- Simple names: `sql_risk` (no quotes needed)
- Strings with special chars: `"user's input"`

### Reference Syntax

| Syntax | Meaning | Example |
|--------|---------|---------|
| `@name` | Pattern reference | `@security_concern` |
| `$name` | Field reference | `$reasoning` |
| `#hash` | Commit reference | `#abc123` |
| `~n` | Relative commit | `~1` (previous), `~3` (3 commits ago) |

---

## 2. Command Categories

### Session Management

| Command | Description |
|---------|-------------|
| `nf session new "<name>"` | Create new named session |
| `nf session save [file]` | Save session to file |
| `nf session load <file>` | Load session from file |
| `nf session info` | Display session information |
| `nf session export <format>` | Export session (json, yaml) |

### Field Operations

| Command | Description |
|---------|-------------|
| `nf inject "<pattern>" [s]` | Add pattern with strength s |
| `nf amplify @pattern [factor]` | Increase pattern activation |
| `nf attenuate @pattern [factor]` | Decrease pattern activation |
| `nf remove @pattern` | Remove pattern from field |
| `nf tune <param>=<value>` | Adjust field parameters |
| `nf collapse [--strategy]` | Generate output from field |
| `nf resonate [@p1 @p2]` | Compute resonance |

### Dynamics Control

| Command | Description |
|---------|-------------|
| `nf cycle [n] [--trace]` | Run n dynamics cycles |
| `nf evolve [--target <c>]` | Evolve to target coherence |
| `nf step` | Single decay step |
| `nf reset [--preserve]` | Clear field state |

### Measurement

| Command | Description |
|---------|-------------|
| `nf measure coherence` | Field coherence value |
| `nf measure energy` | Energy distribution |
| `nf attractor list` | List emerged attractors |
| `nf attractor info <name>` | Attractor details |
| `nf basin @attractor` | Basin analysis |
| `nf state` | Full state dump |

### Visualization

| Command | Description |
|---------|-------------|
| `nf plot field` | Field state visualization |
| `nf plot network` | Resonance network graph |
| `nf plot topology` | Attractor landscape |
| `nf animate dynamics` | Animated evolution |
| `nf export <format>` | Export visualization |

### Versioning

| Command | Description |
|---------|-------------|
| `nf commit [message]` | Save state snapshot |
| `nf branch create <name>` | Create branch |
| `nf branch list` | List branches |
| `nf checkout <ref>` | Restore state |
| `nf history [--graph]` | Show history |
| `nf diff [ref1] [ref2]` | Compare states |
| `nf merge <branch>` | Merge branch |

### Field Management

| Command | Description |
|---------|-------------|
| `nf field create <name>` | Create new field |
| `nf field list` | List all fields |
| `nf field activate $field` | Switch active field |
| `nf field delete $field` | Delete field |
| `nf route $src $dest` | Connect fields |
| `nf couple $f1 $f2` | Set coupling strength |

### Autonomy

| Command | Description |
|---------|-------------|
| `nf mode step` | Pause after each op |
| `nf mode checkpoint` | Pause at conditions |
| `nf mode auto` | Autonomous execution |
| `nf checkpoint "<cond>"` | Add pause condition |
| `nf checkpoint list` | List checkpoints |
| `nf proceed [n]` | Continue execution |
| `nf task "<desc>"` | Define autonomous task |

### Interface

| Command | Description |
|---------|-------------|
| `nf config interface <mode>` | Set interface mode |
| `nf config set <key> <val>` | Set configuration |
| `nf ask "<question>"` | Natural language query |
| `nf compute <expr>` | Algebraic computation |
| `nf help [command]` | Show help |

---

## 3. Argument Types

### Numeric Values

| Type | Format | Range | Example |
|------|--------|-------|---------|
| strength | float | 0.0-1.0 | `0.85` |
| factor | float | 0.0-∞ | `1.5`, `0.5` |
| coherence | float | 0.0-1.0 | `0.8` |
| count | int | 1-∞ | `5` |
| gamma | float | 0.0-1.0 | `0.3` |

### String Values

| Type | Format | Example |
|------|--------|---------|
| pattern_name | quoted or simple | `"sql injection"`, `sql_risk` |
| field_name | simple identifier | `reasoning`, `analysis` |
| message | quoted string | `"Initial analysis complete"` |
| condition | quoted expression | `"coherence > 0.8"` |

### Parameter Assignments

```
nf tune lambda=0.03 alpha=0.4 tau=0.35
```

| Parameter | Symbol | Range | Default |
|-----------|--------|-------|---------|
| lambda | λ | 0.0-1.0 | 0.05 |
| alpha | α | 0.0-1.0 | 0.30 |
| tau | τ | 0.0-1.0 | 0.40 |
| sigma | σ | 0.0-∞ | 0.50 |

---

## 4. Flag Reference

### Global Flags

| Flag | Description | Example |
|------|-------------|---------|
| `--help` | Show command help | `nf inject --help` |
| `--quiet` | Suppress output | `nf cycle 5 --quiet` |
| `--verbose` | Extended output | `nf cycle --verbose` |
| `--json` | JSON output format | `nf state --json` |

### Operation-Specific Flags

#### inject
| Flag | Description | Default |
|------|-------------|---------|
| `--into $field` | Target field | active field |
| `--tags t1,t2` | Semantic tags | none |
| `--position x,y` | Semantic coordinates | auto |

#### cycle
| Flag | Description | Default |
|------|-------------|---------|
| `--trace` | Show detailed trace | false |
| `--compact` | Minimal output | false |
| `--until <cond>` | Stop condition | none |

#### collapse
| Flag | Description | Default |
|------|-------------|---------|
| `--strategy <s>` | Collapse strategy | attractor |

Strategies: `attractor`, `threshold`, `weighted`, `sample`

#### plot
| Flag | Description | Default |
|------|-------------|---------|
| `--style <s>` | Visualization style | default |
| `--threshold <t>` | Edge threshold | 0.4 |
| `--3d` | 3D visualization | false |
| `--output <file>` | Save to file | display |

#### history
| Flag | Description | Default |
|------|-------------|---------|
| `--graph` | Show branch graph | false |
| `--limit <n>` | Max entries | 10 |
| `--all` | Include all branches | false |

---

## 5. Condition Language

For checkpoints and conditional execution:

### Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `>` | Greater than | `coherence > 0.8` |
| `<` | Less than | `patterns < 5` |
| `>=` | Greater or equal | `cycles >= 10` |
| `<=` | Less or equal | `energy <= 0.5` |
| `==` | Equal | `mode == "auto"` |
| `!=` | Not equal | `attractor != null` |

### Variables

| Variable | Type | Description |
|----------|------|-------------|
| `coherence` | float | Current field coherence |
| `patterns` | int | Active pattern count |
| `cycles` | int | Cycles executed |
| `energy` | float | Total field energy |
| `attractor` | string/null | Emerged attractor name |
| `mode` | string | Current autonomy mode |

### Events

| Event | Trigger |
|-------|---------|
| `attractor_emerged` | New attractor detected |
| `coherence_peak` | Coherence stops increasing |
| `saturation` | Too many patterns active |
| `equilibrium` | Field state stabilized |

### Compound Conditions

```
nf checkpoint "coherence > 0.8 && patterns < 10"
nf checkpoint "attractor_emerged || cycles >= 20"
```

---

## 6. Output Formats

### Standard Output

```
[COMMAND] brief description
  Detail: value
  Detail: value
  [STATUS] result
```

### Trace Output

```
[CYCLE n]
  DECAY: Applied λ=0.05
    @pattern: old → new
  RESONANCE:
    @p1 ↔ @p2: R = 0.78 [STRONG]
  COHERENCE: 0.72 [HIGH]
```

### JSON Output (--json flag)

```json
{
  "command": "cycle",
  "cycles": 3,
  "final_state": {
    "coherence": 0.78,
    "patterns": [...],
    "attractors": [...]
  }
}
```

### Error Output

```
[ERROR] <error_type>: <message>
  Context: <relevant_info>
  Suggestion: <how_to_fix>
```

---

## 7. Command Composition

### Piping (Conceptual)

While not true pipes, commands can reference results:

```
nf inject "concept" 0.8
nf cycle 3
nf collapse
```

### Batch Execution

Multiple commands in sequence:

```
nf session new "Analysis"
nf inject "hypothesis_a" 0.9
nf inject "hypothesis_b" 0.85
nf inject "evidence" 0.7
nf cycle 5 --trace
```

### Conditional Execution

```
nf checkpoint "coherence > 0.7"
nf cycle 10
# Pauses when coherence exceeds 0.7
nf proceed
# Continues until next checkpoint or completion
```

---

## 8. Examples

### Basic Analysis Session

```bash
> nf session new "Security Review"
[SESSION] Created: Security Review
  Field: main (default)
  Mode: step

> nf inject "sql_injection_risk" 0.9
[INJECT] sql_injection_risk (s: 0.90)
  Field: main
  Patterns: 1

> nf inject "user_input" 0.85
[INJECT] user_input (s: 0.85)
  Field: main
  Patterns: 2

> nf cycle 3 --trace
[CYCLE 1]
  DECAY: λ=0.05
    @sql_injection_risk: 0.90 → 0.86
    @user_input: 0.85 → 0.81

  RESONANCE:
    @sql_injection_risk ↔ @user_input: R = 0.78 [STRONG]
      Semantic: both concern input security
      Logical: sql risk requires input vector
      Contextual: security analysis domain

  AMPLIFY:
    @sql_injection_risk: 0.86 → 0.92
    @user_input: 0.81 → 0.88

  COHERENCE: 0.78 [HIGH]

  [ATTRACTOR CHECK]
    ✓ Coherence > 0.6
    ✓ Energy concentrated: 100%
    ✓ Stability verified

[CYCLE 2]
  ...

[ATTRACTOR-EMERGED] "input_security_concern"
  Core: {@sql_injection_risk, @user_input}
  Coherence: 0.82

> nf collapse --strategy attractor
[COLLAPSE] Strategy: attractor
  Dominant: input_security_concern (0.82)

OUTPUT:
  The analysis has converged on input security as the primary
  concern. SQL injection risk through user input handling
  requires validation and parameterized queries.

  Confidence: HIGH (coherence 0.82)
```

### Versioned Exploration

```bash
> nf commit "Baseline analysis"
[COMMIT] a1b2c3: "Baseline analysis"

> nf branch create hypothesis_b
[BRANCH] Created: hypothesis_b

> nf inject "alternative_theory" 0.9
> nf cycle 5
> nf commit "Alternative explored"
[COMMIT] d4e5f6: "Alternative explored"

> nf checkout main
[CHECKOUT] main (a1b2c3)

> nf diff hypothesis_b
[DIFF] main..hypothesis_b
  + @alternative_theory (0.87)
  - @original_pattern (0.65)
  Coherence: 0.78 → 0.72
```

### Multi-Field Orchestration

```bash
> nf field create perception --params "lambda=0.03"
[FIELD] Created: perception
  Parameters: λ=0.03, α=0.30, τ=0.40, σ=0.50

> nf field create reasoning --params "lambda=0.05"
[FIELD] Created: reasoning

> nf inject "visual_input" 0.9 --into $perception
> nf inject "logical_rule" 0.85 --into $reasoning

> nf couple $perception $reasoning --gamma 0.4
[COUPLE] perception ↔ reasoning (γ=0.4)

> nf cycle 5
[CYCLE 1] (multi-field)
  perception:
    @visual_input: 0.90 → 0.87
  reasoning:
    @logical_rule: 0.85 → 0.81
  Cross-field transfer: perception → reasoning (0.35)
```

---

## 9. Quick Reference Card

```
CORE                          DYNAMICS
────────────────────────────  ────────────────────────────
nf inject "p" [s]             nf cycle [n] [--trace]
nf amplify @p [f]             nf evolve [--target c]
nf attenuate @p [f]           nf step
nf tune λ=v α=v               nf reset [--preserve @p]
nf collapse [--strategy s]

MEASUREMENT                   VISUALIZATION
────────────────────────────  ────────────────────────────
nf measure coherence          nf plot field [--3d]
nf measure energy             nf plot network
nf attractor list             nf plot topology
nf basin @a [--map]           nf animate dynamics
nf state                      nf export <fmt>

VERSIONING                    AUTONOMY
────────────────────────────  ────────────────────────────
nf commit [msg]               nf mode step|checkpoint|auto
nf branch create <n>          nf checkpoint "<cond>"
nf checkout <ref>             nf proceed [n]
nf history [--graph]          nf task "<desc>"
nf diff [r1] [r2]

FIELDS                        INTERFACE
────────────────────────────  ────────────────────────────
nf field create <n>           nf config interface <m>
nf field list                 nf ask "<question>"
nf route $a $b                nf compute <expr>
nf couple $a $b [--γ]         nf help [cmd]
```

---

## Related Documents

- `field-ops.md` - Detailed field operation specs
- `dynamics.md` - Dynamics control specs
- `measurement.md` - Measurement command specs
- `versioning.md` - Version control specs
- `visualization.md` - Visualization specs
- `autonomy.md` - Autonomy control specs
