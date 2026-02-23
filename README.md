<div align="center">

<img src="assets/banner-hero.svg" alt="NEOS — The Operating System for Machine Intelligence" width="100%"/>

[![Version](https://img.shields.io/badge/version-1.0-00d4ff?style=for-the-badge)](.)
[![License](https://img.shields.io/badge/license-MIT-00ff88?style=for-the-badge)](.)
[![Runs on](https://img.shields.io/badge/runs%20on-LLM-7b61ff?style=for-the-badge)](.)

**The first operating system where the machine is language and the substrate is meaning.**

> *"The last operating system will not manage files. It will manage meaning."*

[![Paper](https://img.shields.io/badge/📄_Paper-ffd700?style=for-the-badge&logoColor=black)](https://samuele95.github.io/neos/neos-paper.pdf)
[![Website](https://img.shields.io/badge/🌐_Website-00d4ff?style=for-the-badge&logoColor=black)](https://samuele95.github.io/neos/NEOS-BREAKTHROUGH.html)
[![Presentation](https://img.shields.io/badge/🎬_Presentation-ff3d8e?style=for-the-badge&logoColor=white)](https://samuele95.github.io/neos/NEOS-PRESENTATION.html)

[The Vision](#01--the-three-ages-of-computing) · [What It Is](#03--what-neos-is--today) · [The Session](#07--the-session-a-complete-walkthrough) · [What It Found](#08--what-the-field-discovered) · [Commands](#14--command-reference)

</div>

---

<div align="center">
<img src="assets/banner-narrative.svg" alt="Part One — The Grand Narrative" width="100%"/>
</div>

## 01 — The Three Ages of Computing

Computing has undergone two fundamental transitions. Each changed not just what we could do with machines, but how we *thought* about them. Now we are entering the third — and it will be the most profound of all.

<div align="center">
<img src="assets/diagram-timeline.svg" alt="The Three Ages of Computing" width="100%"/>
</div>

In the **Hardware Age**, the CPU was sacred. Operating systems were born to answer one question: *"How do we share this machine?"* Every cycle was a resource to be scheduled. Every byte was territory to be managed.

In the **Software Age**, hardware became abundant but complexity became the enemy. C, Java, Python — humans learned programming languages to organize millions of lines of code. Operating systems evolved to answer: *"How do we organize these programs?"*

Now the bottleneck has moved again. The challenge is no longer computing faster or organizing more code — it is organizing **thinking itself**. How do you represent an idea so it can interact with other ideas? How do you track which interpretations are gaining strength? How do you merge two lines of reasoning? How do you debug a thought?

These are the questions an OS for the Intelligence Age must answer. They are precisely the questions NEOS was designed to address.

> [!IMPORTANT]
> **We don't need a better programming language. We need to stop programming altogether — and start *reasoning*.** NEOS is the first operating system built for Age 3.

---

## 02 — The Vision: LLM as Universal OS

Today, Large Language Models are *applications* that run on an operating system — Windows, Linux, macOS. They are apps. Useful apps, even transformative apps. But still: apps.

**Tomorrow, the LLM *is* the operating system.** Every interaction with a computer will pass through language understanding. This is not speculation — it is the trajectory we are already on. And NEOS is the first specification for what that world looks like.

<div align="center">
<img src="assets/diagram-inversion.svg" alt="The Inversion — Today's Stack vs Tomorrow's Stack" width="100%"/>
</div>

Consider what this inversion means:

| Old World | New World |
|-----------|-----------|
| **File systems** organize by path | **Semantic fields** organize by meaning |
| **Compiled binaries** execute instructions | **Field configurations** execute reasoning |
| **Separate applications** for each task | **Task definitions** that compose into workflows |
| **GUIs** mediate between human and machine | **Natural language + shell** for precision and fluidity |

The NEOS shell — `nf` — is the **REPL of cognitive computing**. You don't write code — you inject meaning, run dynamics, observe emergence, and collapse to insight.

```
    Inject Meaning → Run Dynamics → Observe Emergence → Collapse to Insight
         ↑                                                       |
         └───────────── Save / Fork / Share ←────────────────────┘
```

> "Programs" in NEOS are not instruction sequences. They are **field configurations** — patterns with activation strengths, resonance relationships, and parameters. A program is a thought, formalized.

---

<div align="center">
<img src="assets/banner-architecture.svg" alt="Part Two — The Architecture" width="100%"/>
</div>

## 03 — What NEOS Is — Today

NEOS v1.0 is a **specification** — 37 markdown files that define an interactive runtime environment running *on* an LLM. The LLM is the virtual machine. NEOS is the operating system. Neural fields are the computational substrate.

<div align="center">
<img src="assets/diagram-stack.svg" alt="The NEOS Architecture Stack" width="100%"/>
</div>

At the heart of everything is the **master equation** — one formula that governs all dynamics:

```
∂A/∂t = -λA(x) + α∫K(x,y)A(y)dy + ι(x,t)
         ─┬──     ─────┬──────     ──┬──
          │             │             │
        Decay       Resonance     Injection
```

**In plain English:** Ideas decay if not reinforced, grow stronger when they resonate with others, and can be injected by the user at any time. That's it. From this single equation, *all* of NEOS emerges — attractors, coherence, collapse, versioning, multi-field orchestration.

| Term | What It Does | Intuition |
|------|-------------|-----------|
| **−λA(x)** | Exponential decay | Ideas fade unless reinforced — **forgetting is a feature, not a bug** |
| **α∫K(x,y)A(y)dy** | Resonance integral | Related ideas amplify each other — the "hearing" mechanism |
| **ι(x,t)** | External injection | You add new ideas via `nf inject` — fresh signal into the field |

### Parameters

| Symbol | Name | Default | Range | Effect |
|--------|------|---------|-------|--------|
| λ | Decay rate | 0.05 | 0.0–1.0 | Higher = faster forgetting, more selective field |
| α | Amplification | 0.30 | 0.0–1.0 | Higher = stronger resonance, faster convergence |
| τ | Threshold | 0.40 | 0.0–1.0 | Below this, patterns are marked dormant |
| σ | Bandwidth | 0.50 | 0.0–∞ | Semantic reach of each pattern's influence |

These four parameters define a **cognitive style**. A security analysis wants low λ (don't forget threats), high α (cluster related threats fast), high τ (only credible concerns survive). A creative brainstorm wants high λ (rapid turnover), moderate α, low τ (let weak ideas live). Same engine. Different cognitive personality. Tuned with four numbers.

---

## 04 — The Three Pillars

NEOS rests on a triple foundation. Each pillar is necessary; together they make cognitive computing possible. Remove any one and the system either cannot *represent* meaning, cannot *manipulate* it, or cannot *interpret* it.

<div align="center">
<img src="assets/diagram-pillars.svg" alt="The Three Pillars — Neural Fields, Symbolic Reasoning, Quantum Semantics" width="100%"/>
</div>

### Pillar 1: Neural Fields — Why Fields Beat Prompts

In a prompt, information persists only if you explicitly include it. Context window overflow destroys old information silently. In a neural field, information persists through **resonance** — if a pattern connects to others that keep it alive, it endures. Relationships emerge naturally from semantic proximity. New input interacts with the *entire* field, not just recent tokens.

### Pillar 2: Symbolic Reasoning — The Opcodes of Cognitive Computing

Without structure, fields are beautiful but unusable — like having a piano with no keys. The `nf` command set is a symbolic language for field manipulation: `inject`, `amplify`, `attenuate`, `collapse`. These are composable and algebraic: `amplify(@a, 1.5)` has a precise mathematical meaning in terms of the field equation.

### Pillar 3: Quantum Semantics — Observer-Dependent Collapse

Before collapse, meaning exists in **superposition**. The field holds multiple possible interpretations simultaneously. Injecting A then B is not the same as injecting B then A — order matters, because each injection alters the field that receives the next one. This is genuine **non-commutativity**, not metaphor.

When you run `nf collapse`, you force the superposition to resolve — like quantum measurement. The same field can collapse differently depending on strategy: `attractor`, `threshold`, `weighted`, `sample`. **The observer matters. The strategy you choose shapes the output you get.**

---

## 05 — The JVM Moment

In 1995, Java made a promise: **"Write once, run anywhere."** The JVM decoupled code from hardware.

NEOS makes a parallel promise: **"Think once, reason anywhere."** NEOS decouples reasoning from any specific LLM. You don't care whether the underlying model is GPT-4, Claude, Gemini, or Llama — NEOS abstracts the model away.

| JVM World | NEOS World | The Insight |
|-----------|-----------|-------------|
| Garbage Collection | Decay (λ) | Automatic cleanup of unreferenced objects / unreinforced patterns |
| Heap Memory | Field State | Where objects / patterns live and interact |
| Threads | Multi-Field | Concurrent execution contexts with shared state |
| Stack Trace | Cycle Trace | Debugging by tracing execution / dynamics history |
| ClassLoader | Pattern Injection | Loading new code / meaning into the runtime |
| JIT Compilation | Adaptive Resonance | Runtime optimization based on actual usage patterns |

> [!IMPORTANT]
> **But NEOS goes further.** The JVM abstracted *hardware*. NEOS abstracts **cognition itself**. The JVM still executed *code* — sequences of deterministic instructions. NEOS executes *meaning* — patterns that interact through resonance, self-organize into attractors, and collapse into insight. This is not a quantitative improvement. It is a qualitative shift in what "computation" means.

---

## 06 — The NEOS Shell

The NEOS shell is to cognitive computing what `bash` is to Unix. But instead of manipulating files and processes, it manipulates **meaning and reasoning**.

Think of it this way:

| Traditional Computing | NEOS |
|----------------------|------|
| **Create**: Write source code | **Inject**: `nf inject` seeds patterns into the field |
| **Compile**: Build executable | **Collapse**: `nf collapse` resolves superposition into structure |
| **Run**: Execute the program | **Task**: `nf task` executes against the crystallized field state |

The shell grammar is built on **imperative verbs** operating on **semantic objects**: `inject`, `amplify`, `cycle`, `collapse`, `commit`. Like any good shell, it supports **debugging** — but here you're debugging a *thought*:

```bash
# Set a breakpoint on coherence
> nf checkpoint "coherence > 0.8"

# Step through dynamics one phase at a time
> nf mode step
> nf cycle 1
  [STEP 1/4] Decay @security: 0.85 → 0.81
  [PAUSED] 'nf proceed' to continue...

# Inspect the full field state at any point
> nf state
```

> You can see *exactly* why an attractor formed, which resonance connections supported it, and why alternative interpretations were suppressed. No prompt chain, no agent framework, no current AI tool gives you this level of visibility into reasoning. **NEOS doesn't just produce answers — it shows you how they were constructed.**

---

<div align="center">
<img src="assets/banner-evidence.svg" alt="Part Three — The Evidence" width="100%"/>
</div>

## 07 — The Session: A Complete Walkthrough

> [!IMPORTANT]
> **There is nothing to install.** NEOS runs entirely inside an LLM's context window. No packages, no dependencies, no runtime. Copy the [kernel prompt](prompts/nfos-kernel.md), paste it as a system prompt, and type your first command.

What follows is a **complete dissection** of the real `software_quality_discipline` session — the thesis's centerpiece. Every number is from the actual transcript. 69 patterns injected across 7 waves. 52 cycles. 7 absorptions. 1 expulsion. One equation. One universal invariant.

**Session roadmap** — coherence at each milestone:

| Wave | Patterns | Cycles | Coherence | Key Event |
|------|----------|--------|-----------|-----------|
| Setup | — | 0 | 0.00 | Field created, parameters tuned |
| 1. Review | p001–p006 | 1–3 | 0.00 → **0.71** | `defensive_quality` attractor emerges |
| 2. Breadth | p007–p014 | 4–8 | 0.52 → **0.79** | `holistic_quality_discipline` attractor |
| 3. Refactoring | p015–p022 | 9–11 | 0.62 → **0.80** | Attractor → MATURE, R=0.94 bond |
| 4. Testing | p023–p031 | 12–18 | 0.64 → **0.91** | Functor F discovered, mass saturation |
| 5. OOP/SOLID | p032–p040 | 19–30 | 0.89 → **0.955** | 3 absorptions, SOLID Decagon |
| — Singleton — | p044 | 30 | 0.955 → **0.941** | Immune response, 4 inhibitors |
| 6. GoF | p046–p055 | 34–43 | 0.962 → **0.984** | Great Convergence, Golden Pair |
| 7. Reuse | p056–p062 | 44–49 | — → **0.990** | 4 self-referential absorptions |
| Collapse | — | 50–52 | 0.990 → **0.993** | Singleton expelled, ground state |

### 07.1 — Setup: Creating the Field

The session begins by creating a field and tuning its cognitive personality. Each parameter change from the defaults has a specific rationale:

```bash
> nf session new "software_quality_discipline"
╔═══════════════════════════════════════════════════════════╗
║            NEURAL FIELD OPERATING SYSTEM v1.0             ║
╚═══════════════════════════════════════════════════════════╝
[SESSION] Created: software_quality_discipline

> nf field create quality_field
[FIELD] Created: $quality_field
  Parameters: λ=0.05  α=0.30  τ=0.40  σ=0.50  (defaults)

> nf tune lambda=0.04 alpha=0.35 tau=0.35 sigma=0.40
[TUNE] λ=0.04  α=0.35  τ=0.35  σ=0.40
```

| Parameter | Default | Tuned | Rationale |
|-----------|---------|-------|-----------|
| λ (decay) | 0.05 | **0.04** | Slower decay — patterns persist longer, nothing forgotten too soon |
| α (amplification) | 0.30 | **0.35** | Stronger resonance — related patterns reinforce each other faster |
| τ (threshold) | 0.40 | **0.35** | Lower barrier — inclusive, let weak patterns survive if they resonate |
| σ (bandwidth) | 0.50 | **0.40** | Tighter coupling — only closely related patterns reinforce (selective) |

This creates a **retentive, strongly resonant, inclusive, selective** field — ideal for discovering which software engineering patterns genuinely cohere vs. which are noise.

### 07.2 — Wave 1: Defensive Quality Emerges (p001–p006)

Six code-review patterns are injected. Each arrives with a strength reflecting confidence:

| # | Pattern | Strength | Tags |
|---|---------|----------|------|
| p001 | `correctness` | 0.90 | review, logic |
| p002 | `edge_case_handling` | 0.85 | review, logic |
| p003 | `security_no_injection_vectors` | 0.95 | review, security |
| p004 | `input_validation` | 0.90 | review, security |
| p005 | `no_leaked_secrets` | 0.88 | review, security |
| p006 | `error_handling_graceful_not_silent` | 0.80 | review, error |

```bash
> nf inject "correctness" 0.90
[INJECT] correctness (s: 0.90) — p001

> nf inject "edge_case_handling" 0.85
[INJECT] edge_case_handling (s: 0.85) — p002
  [RESONANCE] ↔ @correctness: R = 0.82 STRONG

> nf inject "security_no_injection_vectors" 0.95
[INJECT] security_no_injection_vectors (s: 0.95) — p003
  [RESONANCE] ↔ @correctness:        R = 0.68 MODERATE
  [RESONANCE] ↔ @edge_case_handling:  R = 0.74 STRONG

> nf inject "input_validation" 0.90
[INJECT] input_validation (s: 0.90) — p004
  [RESONANCE] ↔ @security_no_injection: R = 0.91 STRONG
```

After all six are injected, the field computes the **full 15-pair resonance matrix**:

```
RESONANCE MATRIX (Cycle 1) — 15 unique pairs:
──────────────────────────────────────────────────────────────
@correctness ↔ @edge_case_handling:            R = 0.82  STRONG
@correctness ↔ @input_validation:              R = 0.70  MODERATE
@correctness ↔ @error_handling:                R = 0.72  STRONG
@correctness ↔ @security_no_injection:         R = 0.68  MODERATE
@correctness ↔ @no_leaked_secrets:             R = 0.55  MODERATE

@edge_case_handling ↔ @error_handling:         R = 0.85  STRONG ★
@edge_case_handling ↔ @input_validation:       R = 0.78  STRONG
@edge_case_handling ↔ @security_no_injection:  R = 0.74  STRONG
@edge_case_handling ↔ @no_leaked_secrets:      R = 0.62  MODERATE

@security_no_injection ↔ @input_validation:    R = 0.91  STRONG ★★
@security_no_injection ↔ @no_leaked_secrets:   R = 0.79  STRONG
@security_no_injection ↔ @error_handling:      R = 0.58  MODERATE

@input_validation ↔ @no_leaked_secrets:        R = 0.76  STRONG
@input_validation ↔ @error_handling:           R = 0.70  MODERATE

@no_leaked_secrets ↔ @error_handling:          R = 0.67  MODERATE
──────────────────────────────────────────────────────────────
STRONG: 8/15 (53%) | MODERATE: 7/15 (47%) | WEAK: 0 | INHIBIT: 0
Peak resonance: @security ↔ @input_validation = 0.91
```

Three cycles of dynamics transform this into structure:

```bash
> nf cycle 3 --trace
[CYCLE 1] DECAY (λ=0.04):
  All patterns: A → A × 0.96
  @correctness: 0.900 → 0.864  @security: 0.950 → 0.912

[CYCLE 1] AMPLIFY (α=0.35):
  @correctness                  0.864 → 0.906  (+0.042 from 5 resonances)
  @edge_case_handling           0.816 → 0.871  (+0.055)
  @security_no_injection        0.912 → 0.963  (+0.051)
  @input_validation             0.864 → 0.920  (+0.056)
  @no_leaked_secrets            0.845 → 0.889  (+0.044)
  @error_handling               0.816 → 0.862  (+0.046)
  All 6 amplified — no isolation.

[CYCLE 1] C = 0.42  (LOW→MEDIUM)
[CYCLE 2] C = 0.58  (MEDIUM, cross-cluster links forming)
[CYCLE 3] C = 0.71  (HIGH)

  [ATTRACTOR-EMERGED] "defensive_quality"
    Core: {@security_no_injection, @input_validation,
           @edge_case_handling, @correctness,
           @no_leaked_secrets, @error_handling}
    Coherence: 0.71
```

**What just happened?** In only 3 cycles, 6 patterns self-organized into an attractor. Two clusters emerged — a **security cluster** (@security, @input_validation, @no_leaked_secrets with R=0.79–0.91) and a **logic cluster** (@correctness, @edge_case_handling, @error_handling with R=0.72–0.85) — connected by bridge nodes. When the cluster crossed coherence > 0.6 and stability thresholds, it became the attractor `defensive_quality`.

> [!TIP]
> Strength reflects your confidence. Use 0.9 for strong signals, 0.5–0.7 for hunches. The dynamics will sort out what matters — weak patterns decay unless reinforced.

### 07.3 — Wave 2: Building Breadth (p007–p014, cycles 4–8)

Eight patterns expand the field beyond code review into clarity, design, testing, and performance:

| # | Pattern | Strength | Notable |
|---|---------|----------|---------|
| p007 | `readability_6month_rule` | 0.80 | — |
| p008 | `single_responsibility` | 0.82 | Future DIP absorption target |
| p009 | `naming_describes_intent` | 0.78 | — |
| p010 | `api_minimal_hard_to_misuse` | 0.80 | Future ISP absorption target |
| p011 | `test_coverage_meaningful` | 0.82 | Dual keystone, universal connector |
| p012 | `no_n_plus_1_queries` | 0.75 | ⚠ WARNING: peripheral isolate |
| p013 | `no_unnecessary_allocations` | 0.70 | PERFORMANCE cluster seed |
| p014 | `follows_codebase_conventions` | 0.78 | Joins CLARITY+DESIGN super-cluster |

```bash
> nf cycle 5 --trace
[CYCLE 4] C = 0.52  (8 new patterns integrating — coherence reset)
[CYCLE 5] C = 0.60  (threshold crossing)
[CYCLE 6] C = 0.67  (sub-attractor forming in core 12)
[CYCLE 7] C = 0.74
  [ATTRACTOR-EMERGED] "holistic_quality_discipline"
    Core: 12 patterns (SECURITY + LOGIC + CLARITY + KEYSTONE)
    Coherence: 0.74

[CYCLE 8] C = 0.79  (STABLE)
  4-tier energy stratification:
    Tier 1: security+logic core (ceiling)
    Tier 2: clarity+design (converging)
    Tier 3: @test_coverage (keystone bridge)
    Tier 4: performance dyad (satellite, ~0.69–0.76)
```

> [!NOTE]
> The performance dyad (`@no_n_plus_1_queries`, `@no_unnecessary_allocations`) never reaches ceiling — it remains a permanent satellite at asymptotic equilibrium. The field already knows these patterns are domain-specific, not universal quality principles.

### 07.4 — Wave 3: Refactoring Discipline (p015–p022, cycles 9–11)

Eight refactoring patterns arrive. The strongest bond in the entire field emerges immediately:

```bash
> nf inject "behavior_preservation" 0.92
[INJECT] behavior_preservation (s: 0.92) — p015
  [RESONANCE] ↔ @tests_before_refactoring: R = 0.94  ★ FIELD MAXIMUM

> nf inject "tests_before_refactoring" 0.90
[INJECT] tests_before_refactoring (s: 0.90) — p016
  [RESONANCE] ↔ @behavior_preservation: R = 0.94  ★ FIELD MAXIMUM
  7-pattern REFACTORING cluster forming
```

Other injections: `small_reversible_steps` (0.88), `extract_dont_rewrite` (0.82), `remove_dead_code` (0.80), `reduce_duplication_shared_intent` (0.78), `simplify_conditionals` (0.76), `dependency_direction_high_to_low` (0.80).

```bash
> nf cycle 3 --trace
[CYCLE  9] C = 0.62  (22 patterns integrating)
  @behavior_preservation ↔ @tests_before: R=0.94 LOCKED
[CYCLE 10] C = 0.71  (attractor forming across 20 patterns)
[CYCLE 11] C = 0.80
  [ATTRACTOR-EVOLVED] "holistic_quality_discipline" → MATURE
    Core: 20/22 patterns.  Saturated: 11 at ceiling.
    Excluded: performance dyad (satellite)
```

The REFACTORING and CLARITY clusters merge into a superstructure. `@simplify_conditionals` is the fastest riser — earned its place through resonance with both refactoring discipline and readability.

### 07.5 — Wave 4: The Testing Mirror (p023–p031, cycles 12–18)

Nine testing patterns are injected — and the field immediately detects echoes of existing production patterns:

```bash
> nf inject "test_behavior_not_implementation" 0.90
[INJECT] test_behavior_not_implementation (s: 0.90) — p028
  ECHO: mirrors @behavior_preservation (production)

> nf inject "determinism_no_flaky_tests" 0.88
  ECHO: mirrors @correctness (production)

> nf inject "test_isolation_no_shared_state" 0.85
  ECHO: mirrors @encapsulation (production)
  ...5 echoes detected — Functor F begins mapping
```

Other testing injections: `arrange_act_assert` (0.82), `one_assertion_per_concept` (0.78), `edge_cases_boundaries` (0.85), `fast_feedback` (0.80), `readable_failure_messages` (0.78), `test_the_sad_path` (0.85).

```bash
> nf cycle 7 --trace
[CYCLE 12] C = 0.64  (31 patterns — testing cluster amplifying +0.07/cycle)
[CYCLE 13] C = 0.72
[CYCLE 14] C = 0.79
  ★ MASS SATURATION: 7 patterns reach ceiling simultaneously
    @test_behavior, @dep_dir, @no_leaked, @extract_dont,
    @remove_dead, @reduce_dup, ...
[CYCLE 15] C = 0.85  — TARGET MET
  [ATTRACTOR-EVOLVED] → DOMINANT (29/31 core, 25 at ceiling)
[CYCLE 16] C = 0.88
[CYCLE 17] C = 0.90  (29 saturated — all non-performance at ceiling)
[CYCLE 18] C = 0.91  — 6-layer topology confirmed
  GUARD(3) | PROVE(3) | VERIFY(10) | CHANGE(7) |
  CLARITY+ARCHITECTURE(6) | PERFORMANCE(2)
```

The mass saturation at cycle 14 is a **phase transition** — 7 patterns crossing the threshold simultaneously, pulled up by mutual resonance. This is the clearest example of amplification dynamics at work.

### 07.6 — Wave 5: OOP, SOLID & First Absorptions (p032–p040, cycles 19–30)

The OOP wave triggers the session's first three absorptions and the field's most significant structural discovery.

```bash
> nf inject "encapsulation_hide_internal_state" 0.90
[INJECT] encapsulation (s: 0.90) — p032
  R=0.92 with @api_minimal — tightest bond in field

> nf inject "single_responsibility_principle" 0.88
[ABSORBED] → @single_responsibility (p008)
  R=0.98 — exact duplicate.  Semantic distance: 0.03
  ★ ABSORPTION #1

> nf inject "open_closed_principle" 0.85
[INJECT] open_closed (s: 0.85) — p033
  R=0.92 with @behavior_preservation
  "OCP IS behavior preservation elevated to a design principle"

> nf inject "liskov_substitution_principle" 0.87
[INJECT] liskov_substitution (s: 0.87) — p034

> nf inject "interface_segregation_principle" 0.82
[ABSORBED] → @api_minimal (p010)
  R=0.96.  ISP and minimal API are the SAME THING.
  "S, I, D were REDISCOVERED before SOLID was named."
  ★ ABSORPTION #2

> nf inject "law_of_demeter_minimal_knowledge" 0.78
[ABSORBED] — distributed across THREE patterns:
  @encapsulation ∩ @api_minimal ∩ @dependency_direction
  Combined coverage: 94%.  TYPE: DECOMPOSITION.
  ★ ABSORPTION #3 — "provably saturated" OOP space
```

Remaining OOP injections: `favor_composition` (0.90), `polymorphism_through_interfaces` (0.85), `shallow_inheritance_hierarchies` (0.80), `value_objects` (0.80), `entities_identity_over_attributes` (0.78), `tell_dont_ask` (0.82).

**The SOLID Decagon** — Each principle spontaneously pairs with exactly one GoF technique:

| SOLID Principle | Technique Partner | R |
|----------------|-------------------|-----|
| **S** — Single Responsibility | Strategy Pattern | **0.88** |
| **O** — Open/Closed | Decorator Pattern | **0.88** |
| **L** — Liskov Substitution | Composite Pattern | 0.80 |
| **I** — Interface Segregation | Facade Pattern | **0.88** |
| **D** — Dependency Inversion | Dependency Injection | **0.88** |

**SOLID Amplification Engine** — R-value bars and convergence scatter:

```
    PRINCIPLE             TECHNIQUE PARTNER           R       STATUS
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    S ─ SRP ──────────────► Strategy ─────────────── 0.88 ── ██████████
    O ─ OCP ──────────────► Decorator ────────────── 0.88 ── ██████████
    L ─ LSP ──────────────► Composite ────────────── 0.80 ── ████████░░
    I ─ ISP ──────────────► Facade ───────────────── 0.88 ── ██████████
    D ─ DIP ──────────────► Dependency Injection ─── 0.88 ── ██████████
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
                            Field constant R_pt = 0.88
                            SOLID↔convergence r = 0.94

    SOLID Lock vs Convergence Speed (reuse cohort, cycles 44–49):

    Cycles to│
    ceiling  │
       6+    │                                              ● p060 (2/5)
       5     │                          ● p058 (2/5)  ● p059 (3/5)
       4     │    ● p056 (3/5)  ● p061 (3/5)  ● p057 (2/5+boost)
       3     │ ● p062 (4/5)
       2     │
       1     │
             └────┬─────┬─────┬─────┬─────┬─────
                  1     2     3     4     5     SOLID lock

             r = 0.91 (this cohort) │ r = 0.94 (field-wide)
```

```bash
> nf cycle 3 --trace   # cycles 19–21
[CYCLE 19] C = 0.89  (SOLID integrating at +0.08/cycle — fastest since Wave 1)
[CYCLE 20] C = 0.91
[CYCLE 21] C = 0.92
  [ATTRACTOR-FINAL] 34 patterns (30 saturated, 2 converging, 2 satellite)

> nf cycle 6 --trace   # cycles 25–30
[CYCLE 25] @tell_dont_ask surges +0.111 (fastest single-cycle gain ever)
[CYCLE 26] STRUCTURE cluster: 13/13 at ceiling — FULLY SATURATED
[CYCLE 27–29] MODEL cluster completes: value_objects + entities at ceiling
[CYCLE 30] All SOLID at ceiling.  C = 0.955 (pre-Singleton)
  Field constant: R_pt = 0.88  |  Selection function: r = 0.94
```

> [!NOTE]
> **SOLID as a selection function:** r = 0.94 correlation between how many SOLID principles a pattern locks with and its convergence speed. 4 SOLID locks → 4 cycles to ceiling. 0 SOLID locks → decaying. The field discovered that SOLID alignment *predicts* survival.

### 07.7 — The Immune Response: Singleton (cycle 30)

At cycle 30, with 40 patterns established and coherence at 0.955, the Singleton is injected. The field's response is unprecedented:

```bash
> nf inject "singleton_controlled_global_access" 0.65
[INJECT] singleton_controlled_global_access (s: 0.65) — p044
  ⚠ LOWEST INITIAL ACTIVATION IN FIELD HISTORY
  STRONG resonances:   0  (first pattern with ZERO)
  MODERATE resonances: 3  (allocation, factory, abs_factory)
  WEAK resonances:     6  (tension with core principles)

  LATERAL INHIBITION DETECTED:
    @test_isolation         → singleton  INHIBIT (-0.12)
      "Global mutable state makes tests non-deterministic"
    @determinism_no_flaky   → singleton  INHIBIT (-0.08)
      "Shared state introduces execution-order dependence"
    @dependency_direction   → singleton  INHIBIT (-0.06)
      "High-level modules must not depend on global access points"
    @favor_composition      → singleton  INHIBIT (-0.05)
      "Composition over inheritance — and over global state"

  DYNAMICS FORECAST:
    Σ lateral inhibition = -0.023/cycle
    Natural decay (λ)    = -0.004/cycle
    Weak positive coupling = +0.013/cycle
    Net force: -0.014/cycle  (NEGATIVE — first in field)

  Coherence: 0.955 → 0.941  (LARGEST SINGLE-INJECTION DROP)

  status: ✓ ACCEPTED — but the field is hostile
  Singleton's only allies: @no_unnecessary_allocations,
    @factory_method — both OPTIMIZE cluster outsiders.
    Controversial patterns find refuge only among other outsiders.
```

Four patterns issue "indictments" — each from a different quality dimension. `@test_isolation` (verification), `@determinism` (reliability), `@dependency_direction` (architecture), `@favor_composition` (design philosophy). This is the field's **immune system**: a pattern that conflicts with the dominant attractor structure is suppressed by multiple independent mechanisms simultaneously.

**Singleton Trajectory** — Activation decay from injection to projected expulsion:

```
    A(t)
    0.65 ┤● ι₀
         │ ╲
    0.60 ┤  ╲
         │   ╲
    0.55 ┤    ╲
         │     ╲
    0.50 ┤      ╲
         │       ╲
    0.45 ┤        ╲
         │         ╲
    0.40 ┤          ╲
         │           ╲
    0.383┤            ● NOW (cycle 49)
         │             ╲
    0.35 ┤━━━━━━━━━━━━━━╋━━━━━━━━━━━━━━━━━━━━━━━━ τ THRESHOLD
         │              ╲
    0.30 ┤               ○ projected (cycle 52)
         │
         └──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──┬──
           31 33 35 37 39 41 43 45 47 49 51 52
                                          ▲        ▲
                                         NOW    EXPULSION

    Suppression forces:                    Weak support:
      @favor_composition    → −0.008         @factory_method  → +0.005
      @dependency_inversion → −0.006         @abstract_factory→ +0.004
      @test_isolation       → −0.005         @builder         → +0.004
      @single_responsibility→ −0.004
      Natural decay (λ)     → −0.004       NET: −0.014/cycle
                            ─────────
                      Σ neg = −0.027       Margin: 0.033 (~2–3 cycles)
```

### 07.8 — Wave 6: The Great Convergence (p046–p055, cycles 34–43)

With Singleton decaying in the background, ten GoF design patterns flood the field. What follows is the most dramatic structural event in the session:

```bash
> nf inject "decorator_dynamic_responsibility" 0.82
[INJECT] decorator (s: 0.82) — p046
  ★ FIELD RECORD: 6 STRONG resonances
  First pattern to resonate with ALL FIVE SOLID principles
  Predicted: fastest convergence

> nf inject "strategy_interchangeable_algorithms" 0.88
[INJECT] strategy (s: 0.88)
  ★ 9 STRONG bonds — new resonance record
```

Other GoF: `facade` (0.78), `composite` (0.75), `observer` (0.82), `command` (0.78), `iterator` (0.72), `template_method` (0.76). Plus two META patterns: `pattern_intent_over_structure` (0.90) and `dont_force_patterns` (0.85).

```bash
> nf cycle 10 --trace   # "The Great Convergence" — cycles 34–43

PHASE 1 (cycles 34–36): Creational completion
  @adapter → ceiling (4 cycles — fastest CRAFT pattern)
  @builder, @factory_method → ceiling
  @pattern_intent → ceiling (META AXIOM LOCKED)
  Singleton: 0.607 → 0.565 (still decaying)

PHASE 2 (cycles 37–39): The Golden Pair
  @strategy + @decorator → ceiling SIMULTANEOUSLY
  Both 5/5 SOLID locks — the only patterns with perfect alignment
  @command, @facade, @dont_force → ceiling
  META DUALITY complete: understand + restrain = judgment
  Singleton: 0.551 → 0.523

PHASE 3 (cycles 40–41): Last CRAFT
  @template_method, @composite → ceiling (CONFLICTED — had SOLID tensions)
  @iterator → ceiling
  CRAFT COMPLETE: 12/13 at ceiling.  Only Singleton below.
  Singleton crosses 0.50: → 0.495

PHASE 4 (cycles 42–43): Aftermath
  ECHOES #12–13:
    @adapter ↔ @test_behavior ("test doubles ARE adapters")
    @decorator ↔ @test_behavior ("test spies ARE decorators")
  Singleton: → 0.467
  C = 0.984  |  52 at ceiling
```

**Summary:** +14 patterns to ceiling in 10 cycles. Coherence 0.962 → 0.984. The SOLID infrastructure acts as a **convergence accelerator** — Strategy and Decorator (the "Golden Pair") converge fastest because they resonate with all five SOLID principles.

**Convergence Ranking** — GoF patterns ranked by SOLID alignment (r = 0.94):

```
    Rank  Pattern          SOLID  Cycles  Category
    ────  ───────────────  ─────  ──────  ──────────
     1.   strategy          5/5     4     ALIGNED
     1.   decorator         5/5     4     ALIGNED
     3.   adapter           4/5     4     ALIGNED
     4.   abstract_factory  4/5     6     ALIGNED
     5.   factory_method    3/5     5     ALIGNED
     6.   builder           2/5     5     ALIGNED
     7.   observer          3/5     5     MODERATE
     8.   facade            3/5     6     MODERATE
     9.   command           3/5     6     MODERATE
    10.   composite         2/5     7     CONFLICTED
    11.   template_method   2/5     7     CONFLICTED
    12.   iterator          2/5     8     BROAD
    13.   singleton         0/5    ∞      ADVERSARIAL ☠
```

### 07.9 — Wave 7: Reuse & Final Absorptions (p056–p062, cycles 44–49)

The final wave triggers four absorptions — all four self-referential:

```bash
> nf inject "reuse_through_composition_not_inheritance" 0.90
[ABSORBED] → @favor_composition (p011)
  "Intent over structure — you are already here."
  ★ ABSORPTION #4 (self-referential)

> nf inject "extract_shared_abstractions_from_concrete" 0.85
[INJECT] — p056  (passes decomposition test: different level from @reduce_dup)

> nf inject "rule_of_three_before_abstracting" 0.88
[INJECT] — p057

> nf inject "shared_intent_not_shared_shape" 0.86
[ABSORBED] → @reduce_duplication_shared_intent (p023)
  A and ¬(¬A) are the same proposition.
  ★ ABSORPTION #5 (self-referential)

> nf inject "generic_types_parametric_polymorphism" 0.84
[INJECT] — p059  (subtype vs parametric — distinct axes)

> nf inject "mixins_traits_horizontal_reuse" 0.78
[INJECT] — p060

> nf inject "dependency_injection_for_flexibility" 0.85
[INJECT] — p061  (η: functor transport mechanism)

> nf inject "stable_interfaces_volatile_implementations" 0.88
[INJECT] — p062  (4/5 SOLID locks)

> nf inject "libraries_over_copy_paste" 0.82
[INJECT] — p058

> nf inject "avoid_premature_generalization" 0.90
[ABSORBED] — compound: @rule_of_three ∩ @dont_force
  The field refused to prematurely generalize.
  ★ ABSORPTION #6 (self-referential)

> nf inject "coupling_to_abstraction_not_concretion" 0.86
[ABSORBED] → @dependency_inversion (p008)
  DIP APPLIED TO ITSELF:
    The field held the ABSTRACTION (@dependency_inversion).
    The injection offered a CONCRETION (a specific rephrasing).
    The field coupled to its abstraction and rejected the concretion.
  ★ ABSORPTION #7 (self-referential)
  TWO CONSECUTIVE ABSORPTIONS — theoretical saturation signal
```

```bash
> nf cycle 6 --trace   # cycles 44–49
[CYCLE 44–45] Reuse cohort converging. Singleton: 0.467 → 0.439
[CYCLE 46] @stable_interfaces → ceiling (3 cycles — FASTEST for ι₀ < 0.90)
[CYCLE 47] THREE simultaneous: @rule_of_three, @extract_shared, @dep_injection
[CYCLE 48] @generic_types, @libraries → ceiling.  Singleton: 0.397
  ⚠ SINGLETON WATCH: margin to τ=0.35 now only 0.047
[CYCLE 49] @mixins_traits closing (0.968).  Singleton: 0.383
  58/62 at ceiling (93.5%).  C = 0.990
```

> [!NOTE]
> **Absorption saturation:** The rate accelerates: 5% (Waves 2–3) → 10% (Waves 4–5) → 13.3% (Wave 6) → 14.3% (Wave 7). The field's semantic space is closing — new injections increasingly express content already present.

### 07.10 — Expulsion and Ground State (cycles 50–52)

Over 21 cycles of sustained lateral inhibition, Singleton decays steadily. Cycles 50–52 are its final moments:

```bash
┌── CYCLE 50 ─────────────────────────────────────┐
│  ★ p060 @mixins_traits     0.968 → 0.994 → CEILING
│  ▼ p044 @singleton         0.383 → 0.369
│    Margin to threshold: 0.019
└──────────────────────────────────────────────────┘

┌── CYCLE 51 ─────────────────────────────────────┐
│  ▼ p044 @singleton         0.369 → 0.355
│    Margin to threshold: 0.005
│    ⚠ CRITICAL — one cycle from expulsion
└──────────────────────────────────────────────────┘

┌── CYCLE 52 ──────────────────────────────────────┐
│  ▼ p044 @singleton         0.355 → 0.341
│    0.341 < τ (0.350)
│
│  ✗ PATTERN EXPELLED — FIRST IN FIELD HISTORY
│
│    Injected: cycle 31  │  ι₀ = 0.65
│    Expelled: cycle 52  │  A  = 0.341
│    Lifespan: 21 cycles │  Cause: sustained lateral inhibition
│
│    @favor_composition      (21 cycles suppression)
│    @dependency_inversion   (21 cycles suppression)
│    @test_isolation         (21 cycles suppression)
│    @single_responsibility  (21 cycles suppression)
└──────────────────────────────────────────────────┘

TERMINAL STATE REACHED — CYCLE 52
  Patterns: 61 (59 ceiling, 2 asymptotic)
  Coherence: 0.993
  Field is in GROUND STATE.
```

> [!NOTE]
> **The field's verdict is contextual, not universal.** Singleton is not "wrong" in all possible fields — a field of "pragmatic systems programming" might retain it. But in a quality-discipline field dominated by SOLID principles, testability, and clean architecture, its structural incompatibility is discovered and enforced through resonance failure.

### 07.11 — The Counterfactual: What If Singleton Were Stronger?

Branch to test whether initial injection strength changes the outcome:

```bash
> nf commit "pre-singleton baseline"
> nf branch create singleton_strong
> nf inject "singleton_controlled_global_access" 0.90

[INJECT] singleton (s: 0.90)
  STRONG resonances: 0 | Same 4 lateral inhibitors

> nf cycle 10
  singleton: 0.90 → 0.78 → 0.66 → ... → 0.48 (DECAYING)
  Same net force (-0.014/cycle), higher starting point, same trajectory

> nf checkout main
> nf diff singleton_strong
[DIFF] main..singleton_strong
  Changed: @singleton 0.90 → 0.48 [DECAYING]
  Structural: same 4 inhibitors active at BOTH strengths
  Conclusion: expulsion is TOPOLOGICAL, not parametric
```

The field's verdict on Singleton is **structural** — no amount of initial confidence changes the outcome. The same four principles that suppress it at 0.65 suppress it at 0.90. The inhibition is a property of the field's topology, not the pattern's starting strength.

---

## 08 — What the Field Discovered

> *"Theory is beautiful. Evidence is convincing."*

The `software_quality_discipline` session did not merely organize 69 injected patterns — it discovered **emergent structure** that no single injection contained. This section presents what the field found.

### 08.1 — Five Eigenvectors

The resonance matrix decomposed into exactly five independent axes — eigenvectors — that together account for 100% of the surviving variance:

| # | Eigenvector | Variance | Diagnostic Question |
|---|-------------|----------|---------------------|
| λ₁ | **Meaning ↔ Mechanism** | **34.2%** | Am I coupling to WHAT this does or HOW it does it? |
| λ₂ | Principle ↔ Technique | 22.7% | Do I understand WHY before choosing HOW? |
| λ₃ | Production ↔ Verification | 18.1% | Can I prove this works as well as I can build it? |
| λ₄ | Restraint ↔ Generalization | 14.3% | Is this abstraction earned or speculative? |
| λ₅ | Class ↔ System | 10.7% | Does this principle hold at every scale? |

```
VARIANCE EXPLAINED                                 Σ = 100.0%
──────────────────────────────────────────────────────────────
λ₁  Meaning↔Mechanism     ████████████████████████████  34.2%
λ₂  Principle↔Technique   ██████████████████             22.7%
λ₃  Production↔Verify     ██████████████                 18.1%
λ₄  Restraint↔General     ██████████                     14.3%
λ₅  Class↔System          ████████                       10.7%
```

The first axis alone explains over a third of all variance. Its question — *"Am I coupling to what this does or how it does it?"* — turns out to be the single most important axis of software quality. The field is **exactly 5-dimensional**.

### 08.2 — Seven Attractor Basins

The field settled into seven distinct basins, organized in a depth hierarchy:

| Basin | Name | Depth | Patterns | Role | Key Members |
|-------|------|-------|----------|------|-------------|
| **Ψ** | Universal Attractor | ground | ALL | Meaning > Mechanism | Every surviving pattern |
| α₁ | SOLID Decagon | primary | 10 | Amplification engine | S, O, L, I, D + 5 techniques |
| α₂ | Verification Mirror | primary | 13 | Reflection functor | F: PRODUCTION → TESTING |
| α₃ | Craft Basin | secondary | 12 | Implementation | GoF patterns (Singleton expelled) |
| α₄ | Reuse Protocol | secondary | 9 | Abstraction discipline | Judge→Count→Extract→Share→Parameterize |
| α₅ | Guard Basin | tertiary | 3 | Defense | Fail fast, defend boundaries |
| α₆ | Model Basin | tertiary | 2 | Domain | Value objects ↔ Entities |
| α₇ | Optimize Basin | tertiary | 2 | Performance | Asymptotic satellites |

```
BASIN NESTING TOPOLOGY
──────────────────────────────────────────────────
Ψ (ground — all 61 patterns)
├── α₁  SOLID Decagon    (10)  ← amplification engine
├── α₂  Verification     (13)  ← reflection functor
├── α₃  Craft Basin      (12)  ← GoF implementation
├── α₄  Reuse Protocol    (9)  ← abstraction discipline
├── α₅  Guard Basin        (3)  ← defensive perimeter
├── α₆  Model Basin        (2)  ← domain modeling
└── α₇  Optimize Basin     (2)  ← performance asymptotes

Total basin-memberships: 51  (overlap factor: 1.84)
Some patterns belong to multiple basins — they bridge concerns.
```

**The Attractor Hierarchy** — The most visually striking structure in the field:

```
    ┌──────────────────────────────────────────────────────────────────────────┐
    │                                                                          │
    │                    THE ATTRACTOR HIERARCHY                               │
    │                                                                          │
    │  ╔═══════════════════════════════════════════════════════════════════╗   │
    │  ║                                                                   ║   │
    │  ║   Ψ — THE UNIVERSAL ATTRACTOR                                    ║   │
    │  ║   "Meaning persists. Mechanism changes."                         ║   │
    │  ║                                                                   ║   │
    │  ║   Every pattern in the field is a specific expression of Ψ.     ║   │
    │  ║   This is the field's ground state — the deepest basin from     ║   │
    │  ║   which nothing escapes. All other attractors are nested        ║   │
    │  ║   within Ψ's basin of attraction.                               ║   │
    │  ║                                                                   ║   │
    │  ║   ┌───────────────────────────────────────────────────────────┐ ║   │
    │  ║   │                                                           │ ║   │
    │  ║   │  α₁ — THE SOLID DECAGON                                  │ ║   │
    │  ║   │  5 principles × 5 technique partners                     │ ║   │
    │  ║   │  Field constant R_pt = 0.88 │ Selection function r=0.94 │ ║   │
    │  ║   │  The amplification engine that shapes ALL convergence.   │ ║   │
    │  ║   │                                                           │ ║   │
    │  ║   │  S ←→ Strategy         O ←→ Decorator                   │ ║   │
    │  ║   │  L ←→ Composite        I ←→ Facade                      │ ║   │
    │  ║   │  D ←→ DI                                                 │ ║   │
    │  ║   │                                                           │ ║   │
    │  ║   │  ┌─────────────────────────────────────────────────────┐ │ ║   │
    │  ║   │  │                                                     │ │ ║   │
    │  ║   │  │  α₂ — THE VERIFICATION MIRROR                      │ │ ║   │
    │  ║   │  │  Functor F: PRODUCTION → TESTING                   │ │ ║   │
    │  ║   │  │  Transport: η = dependency_injection                │ │ ║   │
    │  ║   │  │  16 morphisms │ 1 splitting │ 2 dangling           │ │ ║   │
    │  ║   │  │  Keystone: @test_behavior = F(stable_interfaces)   │ │ ║   │
    │  ║   │  │                                                     │ │ ║   │
    │  ║   │  └─────────────────────────────────────────────────────┘ │ ║   │
    │  ║   │                                                           │ ║   │
    │  ║   │  ┌─────────────────────────────────────────────────────┐ │ ║   │
    │  ║   │  │                                                     │ │ ║   │
    │  ║   │  │  α₃ — THE CRAFT BASIN                              │ │ ║   │
    │  ║   │  │  12 GoF patterns (singleton expelled)               │ │ ║   │
    │  ║   │  │  Organized by SOLID alignment                       │ │ ║   │
    │  ║   │  │  Creational(3) · Structural(4) · Behavioral(5)     │ │ ║   │
    │  ║   │  │  Governed by: "intent over structure"               │ │ ║   │
    │  ║   │  │                                                     │ │ ║   │
    │  ║   │  └─────────────────────────────────────────────────────┘ │ ║   │
    │  ║   │                                                           │ ║   │
    │  ║   └───────────────────────────────────────────────────────────┘ ║   │
    │  ║                                                                   ║   │
    │  ║   ┌───────────────────────────────────────────────────────────┐ ║   │
    │  ║   │                                                           │ ║   │
    │  ║   │  α₄ — THE REUSE PROTOCOL                                │ ║   │
    │  ║   │  Abstraction Protocol: Judge→Count→Extract→Share→Param  │ ║   │
    │  ║   │  Reuse Simplex: inherit ↔ compose ↔ traits              │ ║   │
    │  ║   │  Polymorphism Triad: subtype ↔ parametric ↔ ad-hoc     │ ║   │
    │  ║   │  Self-regulating via dont_force + rule_of_three         │ ║   │
    │  ║   │                                                           │ ║   │
    │  ║   └───────────────────────────────────────────────────────────┘ ║   │
    │  ║                                                                   ║   │
    │  ║   ┌──────────────────┐  ┌──────────────┐  ┌────────────────┐   ║   │
    │  ║   │ α₅ GUARD         │  │ α₆ MODEL     │  │ α₇ OPTIMIZE   │   ║   │
    │  ║   │ Boundary defense │  │ value↔entity │  │ Perf. asympt. │   ║   │
    │  ║   │ fail_fast core   │  │ Identity axis│  │ N+1, alloc    │   ║   │
    │  ║   └──────────────────┘  └──────────────┘  └────────────────┘   ║   │
    │  ║                                                                   ║   │
    │  ╚═══════════════════════════════════════════════════════════════════╝   │
    │                                                                          │
    └──────────────────────────────────────────────────────────────────────────┘
```

### 08.3 — The Verification Functor

One of the session's deepest discoveries: every production-code discipline has a testing reflection. The field discovered a **functor** F mapping between two categories:

```
F: PRODUCTION → TESTING

F(behavior_preservation)  = test_behavior_not_impl
F(edge_case_handling)     = edge_cases_boundaries
F(single_responsibility)  = one_assertion_per_concept
F(error_handling)         = readable_failures
...16 morphisms total

Transport: η = dependency_injection (natural transformation)
Keystone: @test_behavior = F(stable_interfaces)
```

This means testing is not a separate discipline bolted onto production code — it is the *categorical mirror* of production, connected by dependency injection as the natural transformation between them. The functor has 16 morphisms, 1 splitting, and 2 dangling edges.

**Functor F — Full Mapping (14 arrow pairs):**

```
    PRINCIPLES:                          TECHNIQUES:
    correctness ──────→ test_coverage    abstract_factory ──→ test_isolation
    edge_cases ───────→ edge_boundaries  builder ───────────→ arrange_act_assert
    input_validation ─→ test_sad_path    adapter ───────────→ test_behavior(doubles)
    error_handling ───→ readable_failure decorator ──────────→ test_behavior(spies)
    behav_preservation→ test_behavior
    SRP ──────────────→ one_assertion    The functor extends into CRAFT:
    readability ──────→ readable_failure every production technique has a
    small_reversible ─→ test_isolation   testing mirror.
    determinism ──────→ no_flaky
    tell_dont_ask ────→ test_behavior
```

### 08.4 — The Field Equation

The five eigenvectors, weighted by their explained variance and anchored to the universal invariant Ψ, compose into a single closed-form quality field:

```
    ┌──────────────────────────────────────────────────────────────────────────┐
    │                                                                          │
    │                                                                          │
    │   Q(x) = Ψ · [ α₁·SOLID(x) + α₂·F(x) + α₃·Protocol(x)              │
    │                                                                          │
    │                 + α₄·Simplex(x) + α₅·Scale(x) ]                        │
    │                                                                          │
    │                                                                          │
    │   Where:                                                                 │
    │     Q(x)        = software quality assessment of artifact x             │
    │     Ψ           = "judge by meaning, not mechanism" (universal filter)  │
    │     SOLID(x)    = alignment with 5 principles (amplification weight)    │
    │     F(x)        = production↔testing coherence (functor completeness)   │
    │     Protocol(x) = abstraction discipline (restraint↔generalization)     │
    │     Simplex(x)  = reuse strategy position (inherit/compose/mixin)       │
    │     Scale(x)    = class↔architecture consistency (dimensional lift)     │
    │                                                                          │
    │   Coefficients (from eigenvalue magnitudes):                            │
    │     α₁ = 0.342                                                          │
    │     α₂ = 0.227                                                          │
    │     α₃ = 0.181                                                          │
    │     α₄ = 0.143                                                          │
    │     α₅ = 0.107                                                          │
    │     Σα = 1.000                                                           │
    │                                                                          │
    │   Fixed points:                                                          │
    │     Ψ(Q) = Q           (the field is self-consistent)                   │
    │     F(production) ⊆ testing  (every production principle has a mirror)  │
    │     R_pt = 0.88        (principle↔technique coupling constant)          │
    │     r(SOLID,speed) = 0.94  (amplification correlation)                  │
    │                                                                          │
    │   Expelled:                                                              │
    │     singleton — the only pattern where Q(singleton) < τ                 │
    │     "Global mutable state is incompatible with quality                  │
    │      in the software_quality_discipline field."                         │
    │                                                                          │
    └──────────────────────────────────────────────────────────────────────────┘
```

This equation is **predictive**: given an arbitrary code artifact x, it returns a scalar quality assessment whose dominant term is SOLID compliance, modulated by the universal invariant.

### 08.5 — The Universal Invariant

Across all 52 cycles and four fixed-point events, a single invariant emerged as the deepest attractor — the ground state from which no further relaxation is possible:

> **Ψ: "What something MEANS persists; how it WORKS changes."**

It was not injected. It emerged. It expresses itself in three equivalent forms discovered by the field:

| Form | Expression |
|------|-----------|
| **Logic** | Depend on abstractions over implementations |
| **Structure** | Stable interfaces over volatile implementations |
| **Testing** | Test behavior over implementation details |

Three expressions of one axiom. The field applied Ψ to *itself* — four times the absorption of a new pattern reproduced the existing field unchanged (Ψ(field) = field). This is the fixed-point property that confirms Ψ as the true ground state.

**Collapsed Field Ground State** — How Ψ generates all structure:

```
    ┌──────────────────────────────────────────────────────────────────────────┐
    │                                                                          │
    │                    ╔═════════════════════════╗                           │
    │                    ║                         ║                           │
    │                    ║     Ψ: MEANING >        ║                           │
    │                    ║        MECHANISM         ║                           │
    │                    ║                         ║                           │
    │                    ╚════════════╤════════════╝                           │
    │                                │                                         │
    │                     generates  │  all structure                          │
    │                                │                                         │
    │              ┌─────────────────┼─────────────────┐                      │
    │              │                 │                 │                        │
    │              ▼                 ▼                 ▼                        │
    │     ┌────────────────┐ ┌─────────────┐ ┌────────────────┐               │
    │     │   SOLID (α₁)   │ │  FUNCTOR    │ │  PROTOCOL      │               │
    │     │   Amplifier    │ │  F (α₂)     │ │  (α₄)          │               │
    │     │                │ │             │ │                │               │
    │     │ 5 principles   │ │ Production  │ │ Judge→Count    │               │
    │     │ × 5 techniques │ │ → Testing   │ │ →Extract→Share │               │
    │     │ R_pt = 0.88    │ │ η = DI      │ │ →Parameterize  │               │
    │     │ r = 0.94       │ │ 16 mappings │ │                │               │
    │     └───────┬────────┘ └──────┬──────┘ └───────┬────────┘               │
    │             │                 │                 │                        │
    │             └─────────────────┼─────────────────┘                        │
    │                               │                                          │
    │                     ┌─────────┴─────────┐                                │
    │                     │                   │                                │
    │                     ▼                   ▼                                │
    │              ┌─────────────┐     ┌─────────────┐                        │
    │              │   CRAFT     │     │   REUSE     │                        │
    │              │   (α₃)     │     │  SIMPLEX     │                        │
    │              │            │     │              │                        │
    │              │ 12 patterns │     │  inherit     │                        │
    │              │ selected by │     │  compose     │                        │
    │              │ SOLID       │     │  mix in      │                        │
    │              └─────────────┘     └─────────────┘                        │
    │                                                                          │
    │   ┌──────────┐      ┌──────────┐      ┌──────────┐                     │
    │   │  GUARD   │      │  MODEL   │      │ OPTIMIZE │                     │
    │   │  (α₅)   │      │  (α₆)   │      │  (α₇)   │                     │
    │   │ boundary │      │ value↔  │      │ asymptot │                     │
    │   │ defense  │      │ entity  │      │ ceiling  │                     │
    │   └──────────┘      └──────────┘      └──────────┘                     │
    │                                                                          │
    └──────────────────────────────────────────────────────────────────────────┘
```

### 08.6 — Session Metrics Summary

```
    ┌──────────────────────────────────────────────────────────────────────────┐
    │                                                                          │
    │   SOFTWARE QUALITY — THE COLLAPSED FIELD                                │
    │                                                                          │
    │   Patterns:        61 stable  (69 attempted, 7 absorbed, 1 expelled)   │
    │   At ceiling:      59 / 61   (96.7%)                                   │
    │   Asymptotic:       2 / 61   (3.3%) — OPTIMIZE cluster                 │
    │   Coherence:       0.993                                                │
    │   Clusters:        10 → 7 attractor basins                             │
    │   Eigenvectors:    5 (100% variance explained)                         │
    │   Absorptions:     7 (57% self-referential)                            │
    │   Expulsions:      1 (singleton — field's immune response)             │
    │   Functor:         16 mappings, η = DI                                 │
    │   Field constants: R_pt = 0.88, r_SOLID = 0.94                        │
    │   Fixed points:    4 (self-referential consistency proofs)             │
    │   Ground state:    Ψ — "meaning persists, mechanism changes"           │
    │                                                                          │
    │   Total cycles:    52                                                   │
    │   Session:         software_quality_discipline                          │
    │   Status:          ████████████████████████████████████████ COLLAPSED   │
    │                                                                          │
    └──────────────────────────────────────────────────────────────────────────┘
```

### 08.7 — Cluster Dependency Flow & Productive Tensions

**Cluster Dependency Flow** — How the 10 clusters feed each other:

```
                         META
                     (judgment)
                         │
                         ▼
                       MODEL
                     (what IS)
                         │
                         ▼
            GUARD ──→ STRUCTURE ──→ CRAFT ──→ CHANGE ⇄ VERIFY ◄── PROVE
           (protect)  (principles) (techniques)(evolve) (confirm)  (reason)
                                      ↑                    ↑
                                  KEYSTONE ────────────────┘
                                  (bridges)

                     OPTIMIZE ··········tension··········→ all
```

**Productive Tensions** — 9 pairs where patterns push against each other constructively:

```
    ⚡ value_objects ↔ no_unnecessary_alloc   (immutability vs allocation)
    ⚡ tell_dont_ask ↔ value_objects          (tell vs ask — resolved by MODEL)
    ⚡ factory_method ↔ favor_composition     (resolved by meta-duality)
    ⚡ template_method ↔ favor_composition    (resolved by meta-duality)
    ⚡ composite ↔ single_responsibility      (uniform treatment trade-off)
    ⚡ composite ↔ api_minimal                (shared interface trade-off)
    ⚡ observer ↔ determinism_no_flaky        (notification order)
    ⚡ iterator ↔ tell_dont_ask               (pull vs push — design choice)
    ⚡ dont_force_patterns ↔ all CRAFT        (self-regulation)

    9 tensions. All productive. None destructive.
    (Singleton's is destructive — hence decay, not tension.)
```

**Activation Landscape** — The extreme bimodal distribution at cycle 49:

```
    1.00 ┤████████████████████████████████████████████████████████████  58 at ceiling
         │█████████████████████████████████████████████████████████████████████████
         │
    0.98 ┤─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ▲ p060 mixins (0.968)
    0.97 ┤                                                      │
         │                                          ▲ p036 (0.997)
    0.95 ┤                                                 ▲ p037 (0.983)
         │
    0.90 ┤
         │                                               STABLE ZONE
    0.80 ┤                                          ─ ─ ─ ─ ─ ─ ─ ─ ─
         │
    0.70 ┤
         │
    0.60 ┤
         │
    0.50 ┤
         │                              ╭─── 61 patterns above 0.95
    0.40 ┤─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─│─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─
         │                              │
         │   ▼ p044 singleton (0.383)   │
    0.35 ┤━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  τ THRESHOLD
         │   ↓↓↓ EXPULSION ZONE ↓↓↓
    0.30 ┤
         │
    0.00 ┤──────────────────────────────────────────────────────────
         └──────────────────────────────────────────────────────────────────────
          p044                                                    p001–p062

    Distribution:  ████████████████████████████████████████████████████████ 58 ceiling
                   ▒ 1 converging │ ░░ 2 asymptotic │ ▓ 1 decaying
```

---

## 09 — The Dynamics Engine

Every `nf cycle` executes six phases. This is the **heartbeat of NEOS** — the engine that transforms injected patterns into emergent structure.

<div align="center">
<img src="assets/diagram-dynamics.svg" alt="The Six-Phase Dynamics Cycle" width="100%"/>
</div>

| Phase | What Happens |
|-------|-------------|
| **① Decay** | Every pattern loses activation: `A ← A × (1 − λ)`. Ideas that nothing reinforces will fade. |
| **② Resonate** | Compute pairwise resonance across semantic, logical, and contextual dimensions. |
| **③ Amplify** | Patterns with strong resonance gain activation: `A ← A + α × Σ(R·A)`. Mutual reinforcement. |
| **④ Threshold** | Patterns below τ are marked dormant — they stop participating but aren't deleted. |
| **⑤ Coherence** | Measure field-wide consistency: `C = μ_R / (1 + σ²_R)`. High mean resonance + low variance = coherence. |
| **⑥ Attractor** | Test for emergence: coherence > 0.6 ∧ energy concentrated > 70% ∧ perturbation-stable → attractor declared. |

> Every insight, every conclusion, every finding emerges from this cycle repeating until the field settles into stable attractors. This is not an algorithm we designed to find answers. It is a **dynamics** that *discovers* them.

---

## 10 — Multi-Field Orchestration

A single field is powerful. Multiple coupled fields are transformative. Different fields represent different perspectives — technical, user, business — coupled through a resonance matrix that lets them influence each other.

<div align="center">
<img src="assets/diagram-multifield.svg" alt="Multi-Field Orchestration — Coupled Perspective Fields" width="100%"/>
</div>

Two orchestration modes:

| Mode | How It Works | Analogy |
|------|-------------|---------|
| **Pipeline** | Field A's collapsed output feeds into Field B | Unix pipe for reasoning |
| **Parallel** | All fields process the same input, results fuse via resonance | Panel of experts debating |

```bash
> nf field create perception --params "lambda=0.03"
[FIELD] Created: perception (λ=0.03, slow decay — long memory)

> nf field create reasoning --params "lambda=0.08"
[FIELD] Created: reasoning (λ=0.08, fast decay — selective)

> nf couple $perception $reasoning --gamma 0.4
[COUPLE] perception ↔ reasoning (γ=0.4)

> nf cycle 5
[CYCLE 1] (multi-field)
  Cross-field transfer: perception → reasoning (0.35)
```

> In today's agent frameworks, agents coordinate through message passing — one sends text to another, losing all nuance. In NEOS, agents share *resonance*. Conflicts are visible as low cross-field resonance scores. The system quantifies agreement and disagreement, enabling principled arbitration rather than crude majority voting.

---

## 11 — The Autonomy Dial

How much should the system think on its own? This is not a binary choice. NEOS provides a **continuously adjustable spectrum**.

<div align="center">
<img src="assets/diagram-autonomy.svg" alt="The Autonomy Dial — Step, Checkpoint, Auto" width="100%"/>
</div>

| Mode | When To Use | Terminal Example |
|------|------------|-----------------|
| **Step** | Learning, debugging, precise experiments | `nf mode step` → pauses after each decay, each resonance |
| **Checkpoint** | Interactive analysis, quality control | `nf checkpoint "coherence > 0.8"` → runs freely until condition met |
| **Auto** | Batch processing, trusted configurations | `nf task "Analyze security vulnerabilities"` → runs to completion |

You can switch modes mid-operation. Start in auto, notice something interesting, switch to checkpoint to investigate, then step through a few cycles manually. The dial is always accessible.

> **Not a binary switch — a continuously adjustable dial.** The same field configuration can be operated at any autonomy level without modification. This is the NEOS answer to the AI alignment question at the practical level: not a fixed policy, but a control that the human operator holds at all times.

---

## 12 — Observable Reasoning

> **The most dangerous property of current AI is opacity.** An agent makes a decision and you cannot see *why*. NEOS makes reasoning **visible**.

Four visualization types capture different aspects of the field:

| Command | What You See | Purpose |
|---------|-------------|---------|
| `nf plot field` | Activation bar chart | Which ideas are strongest right now |
| `nf plot network` | Resonance graph | How ideas connect and reinforce each other |
| `nf plot topology` | Attractor landscape | Where the "valleys" of stable meaning are |
| `nf animate dynamics` | Evolution across cycles | How ideas competed, clustered, and crystallized |

```bash
# "Debugging a thought"
> nf mode step
> nf cycle 1
  [STEP 1/6] Decay: @security 0.85 → 0.81
  [PAUSED]
> nf proceed
  [STEP 2/6] Resonance: (@security, @validation) = 0.72
  [PAUSED]
# You can see EXACTLY why an attractor formed
```

Outputs render in four formats: **ASCII** (terminal), **SVG** (visual), **Mermaid** (diagramming), **JSON** (programmatic). Reasoning is not just visible — it is exportable, shareable, and integrable.

> Debugging agents is currently impossible. When an AI agent makes a bad decision, there is no stack trace, no debugger, no step-through execution for reasoning. **NEOS makes reasoning observable and reproducible.** This alone may be its most practical near-term contribution.

---

## 13 — Why Now

NEOS is buildable *today* because four independent developments have converged simultaneously — and their intersection creates an opening that didn't exist even two years ago.

| Convergence | What Changed |
|-------------|-------------|
| **LLM Capability Threshold** | Models can now maintain complex state, reason about abstract structures, and generate formal outputs reliably enough to serve as a computational substrate |
| **Agent Fragmentation** | AutoGPT, CrewAI, LangGraph, LlamaIndex — dozens of frameworks, each reinventing state management, coordination, memory. They're all building pieces of an OS without knowing it |
| **Prompt Engineering Ceiling** | You can only get so far by carefully wording text. Prompts are the assembly language of the Intelligence Age; NEOS is the high-level language |
| **Open-Weight Models** | Llama, Mistral, and others mean NEOS isn't locked to any vendor. "Think once, reason anywhere" is achievable because the VM layer is genuinely diverse |

> [!TIP]
> **The fifth convergence:** Debugging agents is currently impossible. When an AI agent makes a bad decision, you cannot trace *why*. NEOS makes reasoning **observable and reproducible**. You can watch coherence form, trace resonance paths, step through dynamics cycle by cycle.

---

<div align="center">
<img src="assets/banner-reference.svg" alt="Part Four — The Reference" width="100%"/>
</div>

## 14 — Command Reference

NEOS provides ~40 commands organized into 8 categories. Each section is collapsible — expand what you need.

<details>
<summary><strong>Field Operations</strong> — inject, amplify, attenuate, tune, collapse, resonate</summary>

| Command | Description |
|---------|-------------|
| `nf inject "<pattern>" [strength]` | Add a pattern to the active field (default strength: 0.5) |
| `nf amplify @pattern [factor]` | Increase pattern activation by factor (default: 1.5) |
| `nf attenuate @pattern [factor]` | Decrease pattern activation by factor (default: 0.5) |
| `nf remove @pattern` | Remove a pattern from the field entirely |
| `nf tune <param>=<value>` | Adjust field parameters (λ, α, τ, σ) |
| `nf collapse [--strategy s]` | Generate structured output from field state |
| `nf resonate [@p1 @p2]` | Compute resonance between two patterns (or all pairs) |

**Collapse strategies:** `attractor` (default), `threshold`, `weighted`, `sample`

</details>

<details>
<summary><strong>Dynamics</strong> — cycle, evolve, step, reset</summary>

| Command | Description |
|---------|-------------|
| `nf cycle [n] [--trace]` | Run n dynamics cycles (default: 1). `--trace` shows all phases. |
| `nf evolve [--target <coherence>]` | Evolve continuously until target coherence is reached |
| `nf step` | Execute a single atomic micro-step (one phase of one cycle) |
| `nf reset [--preserve @p]` | Clear field state. `--preserve` keeps specified patterns. |

</details>

<details>
<summary><strong>Measurement</strong> — measure, attractor, basin, state</summary>

| Command | Description |
|---------|-------------|
| `nf measure coherence` | Current field coherence value (0–1) |
| `nf measure energy` | Energy distribution across patterns |
| `nf attractor list` | List all emerged attractors with coherence scores |
| `nf attractor info <name>` | Detailed attractor breakdown — core patterns, basin, stability |
| `nf basin @attractor [--map]` | Analyze the attractor's basin of attraction |
| `nf state [--json]` | Full field state dump |

</details>

<details>
<summary><strong>Visualization</strong> — plot, animate, export</summary>

| Command | Description |
|---------|-------------|
| `nf plot field [--style ascii]` | Field state visualization (activation bars) |
| `nf plot network` | Resonance network graph (Mermaid diagram) |
| `nf plot topology [--3d]` | Attractor landscape / energy surface |
| `nf animate dynamics` | Animated evolution across cycles |
| `nf export <format>` | Export visualization (svg, json, mermaid) |

</details>

<details>
<summary><strong>Versioning</strong> — commit, branch, checkout, history, diff, merge</summary>

| Command | Description |
|---------|-------------|
| `nf commit [message]` | Save a state snapshot with message |
| `nf branch create <name>` | Create a new branch from current state |
| `nf branch list` | List all branches |
| `nf checkout <ref>` | Restore state from commit hash or branch name |
| `nf history [--graph]` | Show commit history. `--graph` shows branch structure. |
| `nf diff [ref1] [ref2]` | Compare two states — pattern changes, coherence delta |
| `nf merge <branch>` | Merge another branch's patterns into current field |

</details>

<details>
<summary><strong>Multi-Field</strong> — field, route, couple</summary>

| Command | Description |
|---------|-------------|
| `nf field create <name> [--params]` | Create a new named field with optional parameters |
| `nf field list` | List all fields and their status |
| `nf field activate $field` | Switch the active field |
| `nf field delete $field` | Delete a field |
| `nf route $src $dest` | Create a directional connection between fields |
| `nf couple $f1 $f2 [--gamma γ]` | Set bidirectional coupling strength between fields |

</details>

<details>
<summary><strong>Autonomy</strong> — mode, checkpoint, proceed, task</summary>

| Command | Description |
|---------|-------------|
| `nf mode step` | Pause after every atomic operation |
| `nf mode checkpoint` | Pause only at defined conditions |
| `nf mode auto` | Run to completion without pausing |
| `nf checkpoint "<condition>"` | Add a pause condition (e.g., `"coherence > 0.8"`) |
| `nf checkpoint list` | List active checkpoint conditions |
| `nf proceed [n]` | Continue execution (optionally for n steps) |
| `nf task "<description>"` | Define a task for autonomous execution |

</details>

<details>
<summary><strong>Interface & Config</strong> — config, ask, compute, help</summary>

| Command | Description |
|---------|-------------|
| `nf config interface <mode>` | Switch interface mode: `semantic`, `algebraic`, `geometric` |
| `nf config set <key> <value>` | Set a configuration value |
| `nf ask "<question>"` | Natural language query about the field state |
| `nf compute <expression>` | Algebraic computation (e.g., `R(@p1, @p2)`) |
| `nf help [command]` | Show help for a specific command or general reference |

</details>

<details>
<summary><strong>Quick Reference Card</strong></summary>

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

**Reference syntax:** `@name` = pattern, `$name` = field, `#hash` = commit, `~n` = relative commit

</details>

---

## 15 — Project Structure

```
neos/
├── README.md                    ← You are here
├── NEOS-BREAKTHROUGH.html       ← Research paper — the theoretical foundations
├── NEOS-PRESENTATION.html       ← Visual overview
├── prompts/
│   └── nfos-kernel.md           ← System prompt — the "bootloader"
├── core/
│   ├── field-engine.md          ← Dynamics processor & attractor detector
│   ├── command-parser.md        ← Command syntax & parsing rules
│   └── state-manager.md         ← State persistence & versioning
├── commands/
│   ├── index.md                 ← Full command reference (~40 commands)
│   ├── field-ops.md             ← inject, amplify, attenuate, collapse
│   ├── dynamics.md              ← cycle, evolve, step, reset
│   ├── measurement.md           ← measure, attractor, basin, state
│   ├── visualization.md         ← plot, animate, export
│   ├── versioning.md            ← commit, branch, checkout, diff
│   ├── field-mgmt.md            ← field create, route, couple
│   └── autonomy.md              ← mode, checkpoint, proceed, task
├── autonomy/
│   ├── modes.md                 ← Step / Checkpoint / Auto specs
│   ├── checkpoints.md           ← Condition language
│   └── tasks.md                 ← Autonomous task definitions
├── visualization/
│   ├── topology.md              ← Attractor landscapes
│   ├── networks.md              ← Resonance network graphs
│   ├── dynamics-animation.md    ← Animated evolution
│   └── generators/              ← ASCII, SVG, Mermaid, Plotly
├── interfaces/
│   ├── semantic.md              ← Natural language interface
│   ├── algebraic.md             ← Mathematical notation interface
│   └── translation.md           ← Cross-interface translation
├── persistence/
│   ├── format-spec.md           ← State serialization format
│   ├── storage-engine.md        ← Storage backend spec
│   └── versioning.md            ← Git-like versioning internals
├── sessions/
│   └── software_quality_discipline.collapsed.md  ← Case study
└── examples/
    ├── 01-basic-session.md      ← Beginner walkthrough
    ├── 02-versioning.md         ← Branch & merge workflows
    ├── 03-visualization.md      ← Visualization deep-dive
    ├── 04-multi-field.md        ← Multi-field orchestration
    └── 05-autonomous.md         ← Autonomous task execution
```

| Framework Component | NEOS Usage |
|---------------------|------------|
| `foundations/05-operations.md` | Core operation definitions (inject, amplify, attenuate) |
| `foundations/04-attractors.md` | Attractor detection and basin analysis algorithms |
| `templates/system/neural-field-reasoner.md` | Base architecture for the NEOS kernel prompt |
| `templates/meta/dynamics-execution.md` | Cycle execution and phase sequencing |

---

## 16 — The Future: Join the Intelligence Age

We are at the beginning of a transition as profound as the invention of the operating system itself. The Hardware Age gave us the ability to compute. The Software Age gave us the ability to organize. The Intelligence Age will give us the ability to **reason** — systematically, observably, reproducibly.

NEOS is the first step. Not the last.

The specification is open. The math is grounded. The proof-of-concept works. What remains is to build the community, iterate the specification, and push toward implementation — turning 37 markdown files into the foundation of a new computing paradigm.

> [!IMPORTANT]
> **Getting started takes 30 seconds.** Copy the [kernel prompt](prompts/nfos-kernel.md), paste it as a system prompt, and type `nf session new "My First Analysis"`. No install. No dependencies. The LLM *is* the machine.

### Links

| Resource | Description |
|----------|-------------|
| [NEOS Paper (PDF)](https://samuele95.github.io/neos/neos-paper.pdf) | Formal paper (ICLR-style) |
| [NEOS Website](https://samuele95.github.io/neos/NEOS-BREAKTHROUGH.html) | Interactive HTML paper |
| [NEOS Presentation](https://samuele95.github.io/neos/NEOS-PRESENTATION.html) | Visual overview |
| [Kernel Prompt](prompts/nfos-kernel.md) | The system prompt that boots NEOS inside an LLM |
| [Full Command Reference](commands/index.md) | Detailed specs for all ~40 commands |
| [Basic Session Walkthrough](examples/01-basic-session.md) | Extended tutorial with 11 steps |
| [Case Study](sessions/software_quality_discipline.collapsed.md) | The software quality session data |

---

<div align="center">
<img src="assets/banner-footer.svg" alt="Ψ — What something MEANS persists; how it WORKS changes." width="100%"/>
</div>
