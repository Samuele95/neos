# Versioning Walkthrough

A step-by-step example demonstrating NFOS version control with branching, exploration, and merging.

---

## Scenario

We're analyzing a software architecture decision. We want to explore multiple approaches and compare their outcomes before committing to a direction.

---

## Session Transcript

### 1. Start Session and Create Baseline

```
> nf session new "Architecture Decision"
╔═══════════════════════════════════════════════════════════════════════════════╗
║                    NEURAL FIELD OPERATING SYSTEM v1.0                          ║
╚═══════════════════════════════════════════════════════════════════════════════╝

[SESSION] Created: Architecture Decision
  Field: main
  Mode: step

> nf inject "user_requirements" 0.9
[INJECT] user_requirements (s: 0.90)
  Patterns active: 1

> nf inject "scalability_need" 0.85
[INJECT] scalability_need (s: 0.85)
  Patterns active: 2

> nf inject "team_expertise" 0.8
[INJECT] team_expertise (s: 0.80)
  Patterns active: 3

> nf inject "budget_constraints" 0.75
[INJECT] budget_constraints (s: 0.75)
  Patterns active: 4

> nf cycle 3 --compact
[CYCLES 1-3]
  1: C=0.48, 4 patterns
  2: C=0.62, 4 patterns
  3: C=0.71, 4 patterns [HIGH]

> nf commit "Baseline requirements analysis"
[COMMIT] a1b2c3d: "Baseline requirements analysis"
  Patterns: 4
  Attractors: 0
  Coherence: 0.71
```

### 2. Create Exploration Branches

Now we'll explore two architectural approaches in parallel.

```
> nf branch create microservices
[BRANCH] Created: microservices
  Based on: a1b2c3d
  From: main

> nf branch create monolith
[BRANCH] Created: monolith
  Based on: a1b2c3d
  From: main

> nf branch list
[BRANCHES]
  * main (current)
    microservices
    monolith
```

### 3. Explore Microservices Approach

```
> nf checkout microservices
[CHECKOUT] microservices (a1b2c3d)
  Restored: Baseline requirements analysis

> nf inject "service_decomposition" 0.85
[INJECT] service_decomposition (s: 0.85)
  Patterns active: 5

  [IMMEDIATE RESONANCE]
    ↔ @scalability_need: R = 0.82 [STRONG]

> nf inject "container_orchestration" 0.8
[INJECT] container_orchestration (s: 0.80)
  Patterns active: 6

> nf inject "api_gateway" 0.75
[INJECT] api_gateway (s: 0.75)
  Patterns active: 7

> nf inject "distributed_complexity" 0.7
[INJECT] distributed_complexity (s: 0.70)
  Patterns active: 8

  [IMMEDIATE RESONANCE]
    ↔ @team_expertise: R = -0.45 [TENSION]
    ↔ @budget_constraints: R = -0.38 [TENSION]

> nf cycle 5 --trace
[CYCLE 1]
  DECAY: λ=0.05
    (all patterns decay slightly)

  RESONANCE:
    @service_decomposition ↔ @scalability_need: R = 0.82 [STRONG]
      Both address growing system demands
    @container_orchestration ↔ @service_decomposition: R = 0.78 [STRONG]
      Orchestration enables decomposition
    @distributed_complexity ↔ @team_expertise: R = -0.45 [TENSION]
      Team may lack distributed systems experience
    @distributed_complexity ↔ @budget_constraints: R = -0.38 [TENSION]
      Complexity increases operational costs

  AMPLIFY:
    @service_decomposition: 0.81 → 0.89
    @scalability_need: 0.81 → 0.88
    @container_orchestration: 0.76 → 0.82

  ATTENUATE:
    @distributed_complexity: 0.67 → 0.58 (tension with core concerns)

  COHERENCE: 0.52 [MEDIUM]

[CYCLE 2-5]
  ...evolving...

[CYCLE 5]
  COHERENCE: 0.68 [MEDIUM]

  [ATTRACTOR CHECK]
    ✓ Coherence > 0.6
    ✓ Energy concentrated: 78%
    ✓ Stability verified

  [ATTRACTOR-EMERGED] "scalable_microservices"
    Core: {@service_decomposition, @scalability_need, @container_orchestration}
    Coherence: 0.72
    Note: @distributed_complexity is peripheral (R=0.35)

> nf attractor info scalable_microservices
[ATTRACTOR] scalable_microservices
  Coherence: 0.72 [HIGH]
  Stability: 0.78 [HIGH]

  Core Patterns:
    @service_decomposition: 0.89
    @scalability_need: 0.88
    @container_orchestration: 0.82

  Peripheral (weakly connected):
    @distributed_complexity: 0.45 (R=0.35)
    @api_gateway: 0.58 (R=0.48)

  Tensions:
    @team_expertise conflicts with @distributed_complexity
    @budget_constraints conflicts with operational overhead

  Semantic Summary:
    The microservices approach aligns well with scalability needs
    but introduces complexity tensions with team expertise and budget.

> nf commit "Microservices analysis complete"
[COMMIT] e5f6g7h: "Microservices analysis complete"
  Patterns: 8
  Attractors: 1
  Coherence: 0.68
```

### 4. Explore Monolith Approach

```
> nf checkout monolith
[CHECKOUT] monolith (a1b2c3d)
  Restored: Baseline requirements analysis
  Note: Returned to baseline state

> nf inject "modular_monolith" 0.85
[INJECT] modular_monolith (s: 0.85)
  Patterns active: 5

  [IMMEDIATE RESONANCE]
    ↔ @team_expertise: R = 0.75 [STRONG]
    ↔ @budget_constraints: R = 0.72 [STRONG]

> nf inject "single_deployment" 0.8
[INJECT] single_deployment (s: 0.80)
  Patterns active: 6

> nf inject "shared_database" 0.75
[INJECT] shared_database (s: 0.75)
  Patterns active: 7

> nf inject "future_scaling_concern" 0.6
[INJECT] future_scaling_concern (s: 0.60)
  Patterns active: 8

  [IMMEDIATE RESONANCE]
    ↔ @scalability_need: R = -0.55 [TENSION]

> nf cycle 5 --compact
[CYCLES 1-5]
  1: C=0.55, 8 patterns
  2: C=0.68, 8 patterns
  3: C=0.75, 7 patterns (future_scaling_concern weakened)
  4: C=0.78, 7 patterns [HIGH]
  5: C=0.80, 7 patterns [HIGH]

  [ATTRACTOR-EMERGED] "pragmatic_monolith"
    Core: {@modular_monolith, @team_expertise, @budget_constraints, @single_deployment}
    Coherence: 0.82

> nf attractor info pragmatic_monolith
[ATTRACTOR] pragmatic_monolith
  Coherence: 0.82 [HIGH]
  Stability: 0.85 [VERY HIGH]

  Core Patterns:
    @modular_monolith: 0.88
    @team_expertise: 0.85
    @budget_constraints: 0.82
    @single_deployment: 0.78

  Peripheral:
    @shared_database: 0.65 (R=0.55)
    @user_requirements: 0.72 (R=0.62)

  Tensions:
    @scalability_need partially satisfied (R=0.48)
    @future_scaling_concern acknowledged but deprioritized

  Semantic Summary:
    The monolith approach strongly aligns with team expertise and
    budget constraints. Higher overall coherence than microservices.

> nf commit "Monolith analysis complete"
[COMMIT] i9j0k1l: "Monolith analysis complete"
  Patterns: 7
  Attractors: 1
  Coherence: 0.80
```

### 5. Compare Branches

```
> nf checkout main
[CHECKOUT] main (a1b2c3d)
  Restored: Baseline requirements analysis

> nf history --graph --all
[HISTORY] All branches

* a1b2c3d (HEAD -> main) Baseline requirements analysis
|
|\
| * e5f6g7h (microservices) Microservices analysis complete
|
 \
  * i9j0k1l (monolith) Monolith analysis complete

> nf diff main microservices
[DIFF] main..microservices

  Patterns:
    + @service_decomposition (0.89) [ADDED]
    + @container_orchestration (0.82) [ADDED]
    + @api_gateway (0.58) [ADDED]
    + @distributed_complexity (0.45) [ADDED]
    Δ @scalability_need: 0.71 → 0.88 (+0.17) [AMPLIFIED]
    Δ @team_expertise: 0.68 → 0.52 (-0.16) [ATTENUATED]

  Attractors:
    + scalable_microservices (0.72) [EMERGED]

  Metrics:
    Coherence: 0.71 → 0.68 (-0.03)
    Patterns: 4 → 8 (+4)
    Energy: 2.45 → 4.12 (+1.67)

> nf diff main monolith
[DIFF] main..monolith

  Patterns:
    + @modular_monolith (0.88) [ADDED]
    + @single_deployment (0.78) [ADDED]
    + @shared_database (0.65) [ADDED]
    + @future_scaling_concern (0.35) [ADDED, WEAK]
    Δ @team_expertise: 0.68 → 0.85 (+0.17) [AMPLIFIED]
    Δ @budget_constraints: 0.63 → 0.82 (+0.19) [AMPLIFIED]
    Δ @scalability_need: 0.71 → 0.55 (-0.16) [ATTENUATED]

  Attractors:
    + pragmatic_monolith (0.82) [EMERGED]

  Metrics:
    Coherence: 0.71 → 0.80 (+0.09)
    Patterns: 4 → 7 (+3)
    Energy: 2.45 → 3.85 (+1.40)

> nf diff microservices monolith
[DIFF] microservices..monolith

  Summary:
    Microservices:
      - Higher scalability alignment
      - Lower coherence (0.68)
      - Team/budget tensions

    Monolith:
      - Higher overall coherence (0.80)
      - Better team/budget alignment
      - Scalability concern acknowledged

  Key Differences:
    @scalability_need: 0.88 → 0.55
    @team_expertise: 0.52 → 0.85
    @budget_constraints: 0.48 → 0.82

  Recommendation: monolith has stronger attractor (0.82 vs 0.72)
```

### 6. Merge Insights

Let's merge the monolith analysis back to main, but also incorporate the scalability awareness from microservices.

```
> nf merge monolith
[MERGE] monolith into main
  Strategy: resonance

  Changes from monolith:
    + @modular_monolith (included, R=0.75)
    + @single_deployment (included, R=0.68)
    + @shared_database (included, R=0.55)
    + @future_scaling_concern (excluded, R=0.28) [LOW RESONANCE]

  Result:
    Patterns: 7
    Coherence: 0.78

[COMMIT] m1n2o3p: "Merge monolith into main"

> nf state --compact
[FIELD] main
  Cycle: 3, Coherence: 0.78 [HIGH]
  Patterns: 7 active
  Attractors: 1 (pragmatic_monolith)

> nf merge microservices --strategy manual
[MERGE] microservices into main
  Strategy: manual

  Auto-mergeable:
    + @api_gateway (R=0.52) - included with reduced strength

  CONFLICT: Scalability approach
    main: @modular_monolith (monolith scaling)
    microservices: @service_decomposition (service scaling)

    These represent different scaling philosophies.

  Options:
    1. Keep main (monolith approach)
    2. Keep microservices (service approach)
    3. Include both (acknowledge tension)

> nf merge --resolve scalability --option 3
[RESOLVED] Including both scaling approaches
  Added: @service_decomposition (0.45, reduced from 0.89)
  Note: Represents "future option" for service extraction

> nf merge --continue
[MERGE] Completed
  Patterns: 9
  Coherence: 0.72 (slightly reduced due to added complexity)

[COMMIT] q4r5s6t: "Merge microservices into main (selective)"
```

### 7. Final Analysis

```
> nf cycle 3 --trace
[CYCLE 1]
  ...dynamics with merged patterns...

[CYCLE 3]
  COHERENCE: 0.75 [HIGH]

  [ATTRACTOR] "pragmatic_monolith_with_options"
    Core: {@modular_monolith, @team_expertise, @budget_constraints}
    Options: {@service_decomposition, @api_gateway} (peripheral)
    Coherence: 0.78

> nf collapse --strategy attractor
[COLLAPSE] Strategy: attractor
  Dominant: pragmatic_monolith_with_options (0.78)

╔══════════════════════════════════════════════════════════════════════╗
║                           RECOMMENDATION                              ║
╚══════════════════════════════════════════════════════════════════════╝

DECISION: Start with Modular Monolith

The field analysis recommends a modular monolith architecture that:

PRIMARY DRIVERS:
  • Aligns with team expertise (0.85)
  • Respects budget constraints (0.82)
  • Maintains high internal coherence (0.78)

ARCHITECTURE:
  • Single deployable unit with modular internal structure
  • Shared database with clear module boundaries
  • Well-defined internal APIs between modules

FUTURE OPTIONS (preserved as peripheral patterns):
  • Service decomposition path available when needed
  • API gateway pattern ready for implementation
  • Scalability addressed through module extraction

TRADE-OFFS ACKNOWLEDGED:
  • Scalability need (0.55) is partially addressed
  • Future scaling concern exists but is deprioritized
  • Team can evolve toward services as expertise grows

CONFIDENCE: HIGH (0.78)

╚══════════════════════════════════════════════════════════════════════╝

> nf commit "Final recommendation: Modular Monolith"
[COMMIT] s7t8u9v: "Final recommendation: Modular Monolith"

> nf history --graph
[HISTORY]

* s7t8u9v (HEAD -> main) Final recommendation: Modular Monolith
|
*   q4r5s6t Merge microservices into main (selective)
|\
| * e5f6g7h (microservices) Microservices analysis complete
|
*   m1n2o3p Merge monolith into main
|\
| * i9j0k1l (monolith) Monolith analysis complete
|/
* a1b2c3d Baseline requirements analysis
```

### 8. Save Session

```
> nf session save architecture-decision.nfsession
[SESSION] Saved: architecture-decision.nfsession
  Size: 45 KB
  Commits: 6
  Branches: 3

  Contents:
    - Full version history
    - All field states
    - Command history

> nf session info
[SESSION] Architecture Decision
  Duration: 25 minutes
  Commits: 6
  Branches: 3 (main, microservices, monolith)

  Key Decisions:
    1. Baseline requirements established
    2. Explored microservices (C=0.68)
    3. Explored monolith (C=0.80)
    4. Merged monolith as primary approach
    5. Incorporated microservices as future options
    6. Final recommendation: Modular Monolith
```

---

## Key Takeaways

### 1. Branch for Exploration
Create branches to explore alternatives without affecting the main analysis.

### 2. Compare Before Merging
Use `nf diff` to understand how approaches differ in:
- Pattern composition
- Attractor strength
- Overall coherence

### 3. Strategic Merging
The resonance-based merge strategy automatically:
- Includes high-resonance patterns
- Excludes low-resonance noise
- Resolves conflicts by coherence contribution

### 4. Preserve Options
Merging with reduced strength keeps future options alive without overwhelming the current analysis.

### 5. Track the Journey
The full history shows how the decision evolved, providing audit trail and learning opportunity.

---

## Command Summary

```bash
# Branching
nf branch create <name>     # Create exploration branch
nf checkout <branch>        # Switch branches
nf branch list              # See all branches

# Comparison
nf diff <a> <b>            # Compare branches/commits
nf history --graph         # Visualize history

# Merging
nf merge <branch>                     # Auto-merge
nf merge <branch> --strategy manual   # Handle conflicts
nf merge --resolve <conflict> --option <n>
nf merge --continue

# Persistence
nf commit "message"        # Save state
nf session save <file>     # Export full session
```

---

## Next Steps

- See `03-visualization.md` for visualizing version differences
- See `../persistence/versioning.md` for merge strategy details
- See `../commands/versioning.md` for full command reference
