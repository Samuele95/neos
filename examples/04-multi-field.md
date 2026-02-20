# Example 4: Multi-Field Orchestration

A complete walkthrough demonstrating multi-field system creation, coupling, routing, and orchestrated processing.

---

## Scenario: Product Feature Analysis

We're analyzing a new feature request from multiple perspectives (technical, user, business) and need to synthesize a balanced recommendation.

---

## 1. System Setup

### Initialize Multi-Field System

```
> nf init product_analysis
[NFOS] Initialized system: product_analysis
  Default field: main
  Status: ready
```

### Create Specialized Fields

```
> nf field create technical --role reasoning
[FIELD] Created 'technical'
  Role: reasoning
  Parameters: λ=0.03, α=0.35, τ=0.5, σ=0.4

> nf field create user --role evaluation
[FIELD] Created 'user'
  Role: evaluation
  Parameters: λ=0.05, α=0.35, τ=0.5, σ=0.4

> nf field create business --role planning
[FIELD] Created 'business'
  Role: planning
  Parameters: λ=0.04, α=0.30, τ=0.45, σ=0.5

> nf field create synthesis --role reasoning
[FIELD] Created 'synthesis'
  Role: reasoning
  Parameters: λ=0.03, α=0.35, τ=0.5, σ=0.4
```

### List Fields

```
> nf field list
[FIELDS] 5 active fields

  NAME        ROLE        PATTERNS  COHERENCE  STATUS
  main        reasoning   0         0.00       active
  technical   reasoning   0         0.00       idle
  user        evaluation  0         0.00       idle
  business    planning    0         0.00       idle
  synthesis   reasoning   0         0.00       idle
```

---

## 2. Configure Couplings

### Set Up Coupling Relationships

```
> nf couple technical user --gamma 0.4
[COUPLE] Set: technical ↔ user
  Coupling strength: γ = 0.40
  Type: excitatory

> nf couple technical business --gamma 0.35
[COUPLE] Set: technical ↔ business
  Coupling strength: γ = 0.35
  Type: excitatory

> nf couple user business --gamma 0.45
[COUPLE] Set: user ↔ business
  Coupling strength: γ = 0.45
  Type: excitatory

> nf couple synthesis technical --gamma 0.6
[COUPLE] Set: synthesis ↔ technical
  Coupling strength: γ = 0.60
  Type: excitatory

> nf couple synthesis user --gamma 0.6
[COUPLE] Set: synthesis ↔ user
  Coupling strength: γ = 0.60
  Type: excitatory

> nf couple synthesis business --gamma 0.6
[COUPLE] Set: synthesis ↔ business
  Coupling strength: γ = 0.60
  Type: excitatory
```

### View Coupling Matrix

```
> nf couple list --matrix
[COUPLING MATRIX]

              main  tech  user  busi  synth
main           -     -     -     -     -
technical      -     -    0.40  0.35  0.60
user           -    0.40   -    0.45  0.60
business       -    0.35  0.45   -    0.60
synthesis      -    0.60  0.60  0.60   -

  Spectral radius ρ(Γ) = 1.02
  Stability bound: λ_min/α_max = 0.086
  Status: STABLE (ρ < bound)
```

---

## 3. Configure Routes

### Set Up Information Flow

```
> nf route technical synthesis --strength 0.8 --filter "activation > 0.5"
[ROUTE] Created: technical → synthesis
  Strength: 0.80
  Filter: activation > 0.5

> nf route user synthesis --strength 0.8 --filter "activation > 0.5"
[ROUTE] Created: user → synthesis
  Strength: 0.80
  Filter: activation > 0.5

> nf route business synthesis --strength 0.8 --filter "activation > 0.5"
[ROUTE] Created: business → synthesis
  Strength: 0.80
  Filter: activation > 0.5
```

### View Routes

```
> nf route list
[ROUTES] 3 active routes

  SOURCE      TARGET      STRENGTH  FILTER           TRANSFORM
  technical   synthesis   0.80      activation>0.5   none
  user        synthesis   0.80      activation>0.5   none
  business    synthesis   0.80      activation>0.5   none
```

---

## 4. Populate Fields

### Technical Perspective

```
> nf field select technical
[FIELD] Active field: technical

> nf inject "api_complexity" 0.75 --tags complexity
[INJECT] Pattern 'api_complexity' added with activation 0.75

> nf inject "database_changes" 0.82 --tags infrastructure
[INJECT] Pattern 'database_changes' added with activation 0.82

> nf inject "testing_effort" 0.70 --tags effort
[INJECT] Pattern 'testing_effort' added with activation 0.70

> nf inject "performance_impact" 0.65 --tags risk
[INJECT] Pattern 'performance_impact' added with activation 0.65

> nf inject "security_review" 0.88 --tags security,critical
[INJECT] Pattern 'security_review' added with activation 0.88

> nf cycle 3
[CYCLE] Running 3 cycles in 'technical'
  Cycle 1: C=0.52
  Cycle 2: C=0.68
  Cycle 3: C=0.74
[COMPLETE] Attractor emerged: 'technical_concerns'
```

### User Perspective

```
> nf field select user
[FIELD] Active field: user

> nf inject "user_value" 0.90 --tags value,high
[INJECT] Pattern 'user_value' added with activation 0.90

> nf inject "learning_curve" 0.55 --tags friction
[INJECT] Pattern 'learning_curve' added with activation 0.55

> nf inject "workflow_improvement" 0.85 --tags value
[INJECT] Pattern 'workflow_improvement' added with activation 0.85

> nf inject "adoption_risk" 0.45 --tags risk
[INJECT] Pattern 'adoption_risk' added with activation 0.45

> nf cycle 3
[CYCLE] Running 3 cycles in 'user'
  Cycle 1: C=0.58
  Cycle 2: C=0.72
  Cycle 3: C=0.78
[COMPLETE] Attractor emerged: 'user_benefits'
```

### Business Perspective

```
> nf field select business
[FIELD] Active field: business

> nf inject "market_opportunity" 0.85 --tags opportunity
[INJECT] Pattern 'market_opportunity' added with activation 0.85

> nf inject "development_cost" 0.72 --tags cost
[INJECT] Pattern 'development_cost' added with activation 0.72

> nf inject "timeline_pressure" 0.78 --tags constraint
[INJECT] Pattern 'timeline_pressure' added with activation 0.78

> nf inject "competitive_advantage" 0.80 --tags strategy
[INJECT] Pattern 'competitive_advantage' added with activation 0.80

> nf inject "revenue_impact" 0.75 --tags revenue
[INJECT] Pattern 'revenue_impact' added with activation 0.75

> nf cycle 3
[CYCLE] Running 3 cycles in 'business'
  Cycle 1: C=0.55
  Cycle 2: C=0.70
  Cycle 3: C=0.76
[COMPLETE] Attractor emerged: 'business_case'
```

---

## 5. Visualize Individual Fields

### Technical Network

```
> nf field select technical
> nf plot network
[NETWORK] Field: technical (5 patterns, 6 edges)

    ┌──────────────┐         ┌─────────────┐
    │security_review│═════════│database_chg │
    │     0.88      │ R=0.72  │    0.82     │
    └──────┬────────┘         └──────┬──────┘
           │                         │
           │ R=0.65                   │ R=0.58
           │                         │
    ┌──────┴────────┐         ┌──────┴──────┐
    │api_complexity │─────────│testing_effort│
    │     0.75      │ R=0.68  │    0.70     │
    └───────────────┘         └─────────────┘
                    \         /
                     \ R=0.45/
                      \     /
               ┌───────────────┐
               │perform_impact │
               │     0.65      │
               └───────────────┘
```

### User Network

```
> nf field select user
> nf plot network
[NETWORK] Field: user (4 patterns, 4 edges)

    ┌───────────────┐         ┌─────────────────┐
    │  user_value   │═════════│workflow_improve │
    │     0.90      │ R=0.82  │      0.85       │
    └───────┬───────┘         └────────┬────────┘
            │                          │
            │ R=0.35                    │ R=0.42
            │                          │
    ┌───────┴───────┐         ┌────────┴────────┐
    │learning_curve │╌╌╌╌╌╌╌╌╌│ adoption_risk   │
    │     0.55      │ R=0.28  │      0.45       │
    └───────────────┘         └─────────────────┘
```

---

## 6. Parallel Processing

### Run All Perspective Fields in Parallel

```
> nf parallel technical,user,business --input "feature_request" --integrate resonance
[PARALLEL] Processing across 3 fields

  Activating fields...
    technical: active
    user: active
    business: active

  Injecting 'feature_request' into each field...

  Running synchronized cycles...
    Cycle 1:
      technical: C=0.74 → 0.76
      user:      C=0.78 → 0.80
      business:  C=0.76 → 0.78
      Cross-field resonance: 0.52

    Cycle 2:
      technical: C=0.76 → 0.78
      user:      C=0.80 → 0.82
      business:  C=0.78 → 0.80
      Cross-field resonance: 0.61

    Cycle 3:
      technical: C=0.78 → 0.80
      user:      C=0.82 → 0.83
      business:  C=0.80 → 0.81
      Cross-field resonance: 0.68

[INTEGRATION] Resonance-based fusion

  High resonance patterns (R > 0.6):
    technical:security_review ↔ business:competitive_advantage  R=0.72
    user:user_value ↔ business:market_opportunity              R=0.78
    technical:database_changes ↔ business:development_cost     R=0.65
    user:workflow_improvement ↔ technical:api_complexity       R=0.62

  Conflicts detected (low cross-field R):
    technical:testing_effort ↔ business:timeline_pressure      R=0.28
    user:adoption_risk ↔ business:market_opportunity           R=0.32

[RESULT] Integrated perspective
  Consensus patterns: 4
  Conflict patterns: 2
  Overall cross-field coherence: 0.68
```

---

## 7. Transfer to Synthesis Field

### Route Patterns to Synthesis

```
> nf route transfer technical synthesis
[TRANSFER] technical → synthesis
  Patterns transferred: 4 (above activation threshold)
    - security_review (0.88 → 0.70)
    - database_changes (0.82 → 0.66)
    - api_complexity (0.75 → 0.60)
    - testing_effort (0.70 → 0.56)

> nf route transfer user synthesis
[TRANSFER] user → synthesis
  Patterns transferred: 3 (above activation threshold)
    - user_value (0.90 → 0.72)
    - workflow_improvement (0.85 → 0.68)
    - learning_curve (0.55 → 0.44)

> nf route transfer business synthesis
[TRANSFER] business → synthesis
  Patterns transferred: 4 (above activation threshold)
    - market_opportunity (0.85 → 0.68)
    - competitive_advantage (0.80 → 0.64)
    - timeline_pressure (0.78 → 0.62)
    - development_cost (0.72 → 0.58)
```

---

## 8. Synthesize Recommendation

### Run Synthesis Dynamics

```
> nf field select synthesis
[FIELD] Active field: synthesis

> nf state
[STATE] Field: synthesis

  Patterns: 11
  ─────────────────────────────────────
  user_value           0.72  [value]
  security_review      0.70  [security,critical]
  market_opportunity   0.68  [opportunity]
  workflow_improvement 0.68  [value]
  database_changes     0.66  [infrastructure]
  competitive_adv      0.64  [strategy]
  timeline_pressure    0.62  [constraint]
  api_complexity       0.60  [complexity]
  development_cost     0.58  [cost]
  testing_effort       0.56  [effort]
  learning_curve       0.44  [friction]

  Coherence: 0.45 (patterns not yet integrated)

> nf cycle 5 --trace
[CYCLE] Running 5 synthesis cycles...

  Cycle 1:
    Resonance computed: 55 pairs
    High resonance clusters forming...
    Coherence: 0.45 → 0.58

  Cycle 2:
    Value-opportunity cluster strengthening
    Security-technical cluster strengthening
    Coherence: 0.58 → 0.68

  Cycle 3:
    Cross-cluster resonance emerging
    Timeline vs quality tension detected
    Coherence: 0.68 → 0.74

  Cycle 4:
    Attractor basin forming...
    Peripheral patterns attenuating
    Coherence: 0.74 → 0.79

  Cycle 5:
    Attractor stabilized
    Coherence: 0.79 → 0.82

[COMPLETE] Attractor emerged: 'balanced_recommendation'
  Core patterns: 5
  Coherence: 0.82
```

### View Synthesis Network

```
> nf plot network
[NETWORK] Field: synthesis (attractor: balanced_recommendation)

         ┌────────────────────────────────────────────────────┐
         │        balanced_recommendation (C=0.82)            │
         │                                                    │
         │  ┌──────────────┐    R=0.78    ┌────────────────┐ │
         │  │  user_value  │══════════════│market_opportun │ │
         │  │     0.82     │              │      0.78      │ │
         │  └──────┬───────┘              └────────┬───────┘ │
         │         │                               │         │
         │         │ R=0.72                R=0.68  │         │
         │         │                               │         │
         │  ┌──────┴───────┐    R=0.70    ┌───────┴───────┐ │
         │  │security_rev  │══════════════│competitive_adv│ │
         │  │     0.75     │              │      0.72     │ │
         │  └──────────────┘              └───────────────┘ │
         │                                                    │
         │              ┌────────────────┐                   │
         │              │workflow_improve│                   │
         │              │      0.70      │                   │
         │              └────────────────┘                   │
         │                                                    │
         └────────────────────────────────────────────────────┘

    Peripheral (below threshold):
      development_cost (0.48), timeline_pressure (0.42),
      testing_effort (0.38), learning_curve (0.32)
```

---

## 9. Arbitrate Conflicts

### Identify and Resolve Conflicts

```
> nf arbitrate technical business --strategy merge
[ARBITRATE] Resolving: technical ↔ business

  Analyzing patterns with low cross-field resonance...

  Conflict 1: Timeline vs Quality
    technical: testing_effort (0.56) + security_review (0.70)
      → "Thorough testing required, security audit essential"

    business: timeline_pressure (0.62)
      → "Market window closing, need quick delivery"

    Cross-field resonance: R=0.28

  Resolution strategy: merge

  Proposed resolution:
    "Phased delivery with security-critical features first"
    - Phase 1: Core features with security focus (2 months)
    - Phase 2: Extended testing, remaining features (1 month)

  Resonance with technical: 0.65
  Resonance with business: 0.68

  Injecting resolution into synthesis field...

> nf inject "phased_delivery" 0.75 --tags resolution,compromise
[INJECT] Pattern 'phased_delivery' added with activation 0.75

> nf cycle 2
[CYCLE] Running 2 cycles...
  Resolution integrating with existing attractor...
  Coherence: 0.82 → 0.85

[COMPLETE] Resolution integrated
```

---

## 10. Generate Final Output

### Collapse Synthesis Field

```
> nf collapse --strategy coherence
[COLLAPSE] Strategy: coherence-weighted

  Extracting from attractor 'balanced_recommendation'...

  Pattern weights (normalized):
    user_value:          0.21 (strong positive)
    market_opportunity:  0.18 (strong positive)
    security_review:     0.17 (important constraint)
    competitive_adv:     0.16 (strategic driver)
    phased_delivery:     0.15 (resolution strategy)
    workflow_improve:    0.13 (user benefit)

[OUTPUT]
┌─────────────────────────────────────────────────────────────────────┐
│                     FEATURE RECOMMENDATION                          │
│                                                                     │
│  Recommendation: PROCEED with phased implementation                 │
│  Confidence: 0.85                                                   │
│                                                                     │
│  Key Drivers:                                                       │
│  • High user value and workflow improvement potential               │
│  • Strong market opportunity with competitive advantage             │
│  • Security requirements can be met with focused Phase 1            │
│                                                                     │
│  Implementation Strategy:                                           │
│  • Phase 1 (2 months): Security-critical features, core workflow   │
│  • Phase 2 (1 month): Extended testing, remaining capabilities     │
│                                                                     │
│  Risks Mitigated:                                                   │
│  • Timeline pressure addressed via phased approach                  │
│  • Security concerns prioritized in Phase 1                         │
│  • Learning curve managed through gradual rollout                   │
│                                                                     │
│  Cross-Perspective Alignment: 0.85 (high consensus)                │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 11. Save System State

### Commit the Analysis

```
> nf commit "Feature analysis: phased delivery recommendation"
[COMMIT] Saved: abc123
  Message: Feature analysis: phased delivery recommendation
  System state:
    Fields: 5
    Total patterns: 26
    Total couplings: 6
    Total routes: 3
```

### Export for Documentation

```
> nf export mermaid system_diagram.md --system
[EXPORT] System diagram saved

Contents:
```

````mermaid
graph TB
    subgraph System["Product Analysis System"]
        subgraph Perspectives["Perspective Fields"]
            tech[("technical<br/>C=0.80")]
            user[("user<br/>C=0.83")]
            biz[("business<br/>C=0.81")]
        end

        synth[("synthesis<br/>C=0.85")]

        tech -->|"γ=0.40"| user
        tech -->|"γ=0.35"| biz
        user -->|"γ=0.45"| biz

        tech -->|"route 0.8"| synth
        user -->|"route 0.8"| synth
        biz -->|"route 0.8"| synth
    end

    style synth fill:#27ae60,stroke:#1e8449
    style tech fill:#3498db,stroke:#2980b9
    style user fill:#e74c3c,stroke:#c0392b
    style biz fill:#f39c12,stroke:#d68910
````

---

## 12. Session Summary

This walkthrough demonstrated:

1. **Field Creation**: Specialized fields for different perspectives
2. **Coupling**: Bidirectional resonance relationships between fields
3. **Routing**: Information flow configuration with filters
4. **Parallel Processing**: Simultaneous analysis across multiple fields
5. **Pattern Transfer**: Moving patterns between fields via routes
6. **Synthesis**: Combining perspectives into integrated attractor
7. **Conflict Resolution**: Arbitrating disagreements with merge strategy
8. **Output Generation**: Coherence-weighted collapse for recommendations

### System Statistics

```
> nf field list --verbose
[FIELDS] Final state

  technical (reasoning)
    Patterns: 5, Attractors: 1
    Coherence: 0.80
    Outgoing routes: 1

  user (evaluation)
    Patterns: 4, Attractors: 1
    Coherence: 0.83
    Outgoing routes: 1

  business (planning)
    Patterns: 5, Attractors: 1
    Coherence: 0.81
    Outgoing routes: 1

  synthesis (reasoning)
    Patterns: 12, Attractors: 1
    Coherence: 0.85
    Incoming routes: 3

  Total system coherence: 0.82
  Cross-field resonance: 0.68
```

---

## Quick Reference

```
MULTI-FIELD WORKFLOW
──────────────────────────────────────────────────────────────

1. Create specialized fields:
   nf field create <name> --role <role>

2. Configure couplings:
   nf couple <f1> <f2> --gamma <g>

3. Set up routes:
   nf route <src> <dst> [--strength s] [--filter expr]

4. Populate each field:
   nf field select <name>
   nf inject <pattern> <activation>
   nf cycle <n>

5. Process in parallel or pipeline:
   nf parallel <f1,f2,...> [--integrate method]
   nf pipeline <f1> <f2> <f3>

6. Transfer to synthesis:
   nf route transfer <src> synthesis

7. Synthesize:
   nf field select synthesis
   nf cycle <n>

8. Resolve conflicts:
   nf arbitrate <f1> <f2> --strategy merge

9. Generate output:
   nf collapse --strategy coherence
```

---

## Related Documents

- `../commands/field-mgmt.md` - Field management commands
- `../core/state-manager.md` - Multi-field state management
- `../../foundations/07-field-orchestration.md` - Theoretical foundation
