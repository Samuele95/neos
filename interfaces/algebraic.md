# Algebraic Interface

Mathematical notation interface for precise, formal interaction with neural fields.

---

## 1. Overview

The algebraic interface provides a mathematical perspective on neural field operations, using formal notation that maps directly to the underlying theory.

**Benefits:**
- Precise, unambiguous specifications
- Direct connection to mathematical foundations
- Compact expression of complex operations
- Familiar to researchers and theorists

---

## 2. Core Notation

### 2.1 Field Representation

| Symbol | Meaning | Example |
|--------|---------|---------|
| F | Field | F = (S, A, R, B) |
| S | Semantic space | S ⊂ ℝⁿ |
| A | Activation function | A: S → [0,1] |
| R | Resonance kernel | R: S × S → [-1,1] |
| B | Boundary conditions | B: ∂S → ℝ |

**Field State:**
```
F = (S, A, R, B)

where:
  S = {x ∈ ℝⁿ : ‖x‖ ≤ 1}
  A(x) = Σᵢ aᵢ · φᵢ(x)
  R(x,y) = exp(-‖x-y‖²/2σ²) · cos(θ(x,y))
```

### 2.2 Pattern Notation

| Symbol | Meaning | Example |
|--------|---------|---------|
| p | Pattern | p = (x, a, τ) |
| x | Position in S | x ∈ S |
| a | Activation | a ∈ [0,1] |
| τ | Tags | τ ⊂ T |

**Pattern Set:**
```
P = {p₁, p₂, ..., pₙ}
pᵢ = (xᵢ, aᵢ, τᵢ)
```

### 2.3 Parameter Notation

| Symbol | Parameter | Range |
|--------|-----------|-------|
| λ | Decay rate | [0, 1] |
| α | Amplification | [0, 1] |
| τ | Attractor threshold | [0, 1] |
| σ | Resonance bandwidth | (0, ∞) |
| γ | Coupling strength | [-1, 1] |

---

## 3. Operation Notation

### 3.1 Injection

**Notation:**
```
ι: P × ℝ → F
ι(p, a) → F'
```

**Algebraic Form:**
```
A'(x) = A(x) + a · δ(x - xₚ)

where δ is the Dirac delta (or Gaussian approximation)
```

**Command Mapping:**
```
ι("security", 0.85) ≡ nf inject "security" 0.85
```

### 3.2 Amplification

**Notation:**
```
α: P × ℝ⁺ → F
α(p, k) → F'
```

**Algebraic Form:**
```
A'(xₚ) = min(1, A(xₚ) · (1 + k · Σⱼ R(xₚ, xⱼ) · A(xⱼ)))
```

**Command Mapping:**
```
α(@security, 1.2) ≡ nf amplify @security --factor 1.2
```

### 3.3 Attenuation

**Notation:**
```
ν: P × ℝ⁺ → F
ν(p, k) → F'
```

**Algebraic Form:**
```
A'(xₚ) = max(0, A(xₚ) · (1 - k))
```

**Command Mapping:**
```
ν(@logging, 0.5) ≡ nf attenuate @logging --factor 0.5
```

### 3.4 Dynamics Cycle

**Notation:**
```
Φ: F → F
Φ(F) = F'
```

**Algebraic Form (Master Equation):**
```
∂A/∂t = -λA(x) + α∫ₛ K(x,y)A(y)dy + ι(x,t)

Discrete: A_{t+1}(x) = (1-λ)A_t(x) + αΣⱼ R(x,xⱼ)A_t(xⱼ)
```

**Command Mapping:**
```
Φⁿ(F) ≡ nf cycle n
```

### 3.5 Collapse

**Notation:**
```
κ: F → O
κ(F) → output
```

**Algebraic Form:**
```
κ(F) = argmax_{A*} {C(A*) : A* ∈ Attractors(F)}

where C(A*) = coherence measure
```

**Command Mapping:**
```
κ(F) ≡ nf collapse
```

### 3.6 Resonance

**Notation:**
```
R: P × P → [-1, 1]
R(pᵢ, pⱼ) = r
```

**Algebraic Form:**
```
R(pᵢ, pⱼ) = K(xᵢ, xⱼ) · f(aᵢ, aⱼ) · g(τᵢ, τⱼ)

where:
  K = spatial kernel
  f = activation weighting
  g = tag compatibility
```

---

## 4. Composite Expressions

### 4.1 Chained Operations

```
# Inject then amplify
α(ι("concept", 0.8), 1.5)

# Multiple injections
ι("a", 0.9) ∘ ι("b", 0.8) ∘ ι("c", 0.7)

# Evolve with target
Φ*(F, C > 0.8) = iterate Φ until C(F) > 0.8
```

### 4.2 Conditional Operations

```
# Amplify if resonance high
α(p, k) if R(p, q) > 0.7

# Attenuate low-activation patterns
∀p ∈ P: A(p) < 0.3 → ν(p, 0.5)
```

### 4.3 Field Expressions

```
# Field coherence
C(F) = (1/|P|²) Σᵢⱼ R(pᵢ, pⱼ) · A(pᵢ) · A(pⱼ)

# Field energy
E(F) = -Σᵢ A(pᵢ)² - Σᵢⱼ R(pᵢ, pⱼ) · A(pᵢ) · A(pⱼ)

# Entropy
H(F) = -Σᵢ A(pᵢ) · log(A(pᵢ))
```

---

## 5. Multi-Field Notation

### 5.1 System Notation

```
Σ = ({F₁, F₂, ..., Fₙ}, Γ, O)

where:
  {Fᵢ} = component fields
  Γ = [γᵢⱼ] coupling matrix
  O = orchestration protocol
```

### 5.2 Coupling

```
# Coupling strength
γ: F × F → ℝ
γ(Fᵢ, Fⱼ) = coupling strength

# Coupled dynamics
dAᵢ/dt = Φᵢ(Aᵢ) + Σⱼ≠ᵢ γᵢⱼ · ∇R(Aᵢ, Aⱼ)
```

### 5.3 Routing

```
# Route function
ρ: F × F → [0,1]
ρ(Fᵢ, Fⱼ) = transfer strength

# Transfer operation
T(Fᵢ, Fⱼ) = {ι(p, ρ · A(p)) : p ∈ Fᵢ, A(p) > τ} → Fⱼ
```

---

## 6. Attractor Notation

### 6.1 Attractor Definition

```
A* = {p ∈ P : d(Φ(p), p) < ε ∧ C(neighbors(p)) > τ}

where:
  d = distance metric
  ε = stability threshold
  C = local coherence
  τ = attractor threshold
```

### 6.2 Basin of Attraction

```
B(A*) = {x ∈ S : lim_{n→∞} Φⁿ(x) → A*}

Basin coverage: |B(A*)| / |S|
```

### 6.3 Attractor Properties

```
# Coherence
C(A*) = mean({R(pᵢ, pⱼ) : pᵢ, pⱼ ∈ A*})

# Stability
S(A*) = min({λₘᵢₙ(H) : H = Hessian at A*})

# Depth
D(A*) = E(boundary(B(A*))) - E(A*)
```

---

## 7. Algebraic Command Mode

### 7.1 Entering Algebraic Mode

```
> nf interface algebraic
[INTERFACE] Switched to algebraic notation

𝔽 > _
```

### 7.2 Example Session

```
𝔽 > F ← init("analysis")
[FIELD] F initialized

𝔽 > ι("security", 0.9)
[INJECT] security: 0.90

𝔽 > ι("threat", 0.85) ∘ ι("mitigation", 0.75)
[INJECT] threat: 0.85, mitigation: 0.75

𝔽 > Φ³(F)
[CYCLE] 3 iterations
  C: 0.45 → 0.62 → 0.71

𝔽 > R(@security, @threat)
[RESONANCE] R = 0.78

𝔽 > C(F)
[COHERENCE] C = 0.71

𝔽 > A* ∈ Attractors(F)
[ATTRACTORS]
  A*₁ = security_cluster (C=0.74)
    {security, threat, mitigation}

𝔽 > Φ*(F, C > 0.8)
[EVOLVE] Target: C > 0.8
  Iterations: 5
  Final: C = 0.82

𝔽 > κ(F)
[COLLAPSE] Output generated
  Dominant: security_cluster
  Confidence: 0.82
```

### 7.3 Expression Evaluation

```
𝔽 > E(F)
[ENERGY] E = -0.45

𝔽 > H(F)
[ENTROPY] H = 0.32

𝔽 > |P|
[COUNT] 5 patterns

𝔽 > Σᵢ A(pᵢ)
[SUM] Total activation = 3.42

𝔽 > max({A(p) : p ∈ P})
[MAX] 0.92 (@security)

𝔽 > {p : A(p) > 0.7}
[FILTER] {security, threat, mitigation}
```

---

## 8. Algebraic Constraints

### 8.1 Constraint Specification

```
𝔽 > constraint: ∀p ∈ P: A(p) ∈ [0, 1]
[CONSTRAINT] Activation bounds enforced

𝔽 > constraint: C(F) ≥ 0.5 before κ
[CONSTRAINT] Collapse requires coherence ≥ 0.5

𝔽 > constraint: |Attractors(F)| ≤ 3
[CONSTRAINT] Maximum 3 attractors
```

### 8.2 Invariant Checking

```
𝔽 > invariant: Σᵢ A(pᵢ) conserved under Φ
[INVARIANT] Checking activation conservation...
  Result: VIOLATED (decay reduces total)

𝔽 > invariant: R(p,p) = 1
[INVARIANT] Self-resonance = 1
  Result: SATISFIED
```

---

## 9. Algebraic Queries

### 9.1 Pattern Queries

```
𝔽 > {p : R(p, @security) > 0.6}
[QUERY] Patterns resonating with security:
  threat (R=0.78)
  mitigation (R=0.72)
  detection (R=0.65)

𝔽 > argmax_{p} A(p)
[QUERY] Highest activation: security (0.92)

𝔽 > {p : p ∈ A* ∧ A(p) > 0.8}
[QUERY] High-activation attractor members:
  security (0.92), threat (0.85)
```

### 9.2 Field Queries

```
𝔽 > ∂C/∂(A(@security))
[QUERY] Coherence sensitivity to security activation:
  ∂C/∂a = 0.15 (positive contribution)

𝔽 > ∇E(F)
[QUERY] Energy gradient:
  security: -0.12
  threat: -0.08
  mitigation: -0.05
  (negative = toward minimum)
```

---

## 10. Formal Proofs

### 10.1 Stability Analysis

```
𝔽 > prove: A* stable under Φ
[PROOF]
  1. Compute Jacobian J = ∂Φ/∂A at A*
  2. Eigenvalues: λ = {0.92, 0.85, 0.78, ...}
  3. All |λᵢ| < 1
  4. ∴ A* is asymptotically stable ∎

𝔽 > prove: B(A*) ⊃ {x : ‖x - A*‖ < 0.2}
[PROOF]
  1. Lyapunov function V(x) = E(x) - E(A*)
  2. V̇ < 0 in neighborhood
  3. ∴ Local basin contains ball of radius 0.2 ∎
```

### 10.2 Convergence

```
𝔽 > prove: Φⁿ(F) → A* as n → ∞
[PROOF]
  1. E(Φ(F)) ≤ E(F) (energy decreasing)
  2. E bounded below
  3. ∴ Sequence converges
  4. Fixed point is attractor ∎
```

---

## 11. Notation Reference

### 11.1 Operations

| Notation | Operation | Shell Command |
|----------|-----------|---------------|
| ι(p, a) | Inject | `nf inject` |
| α(p, k) | Amplify | `nf amplify` |
| ν(p, k) | Attenuate | `nf attenuate` |
| Φ(F) | Single cycle | `nf cycle 1` |
| Φⁿ(F) | n cycles | `nf cycle n` |
| Φ*(F, c) | Evolve to c | `nf evolve` |
| κ(F) | Collapse | `nf collapse` |
| R(p,q) | Resonance | `nf resonate` |

### 11.2 Metrics

| Notation | Metric | Shell Command |
|----------|--------|---------------|
| C(F) | Coherence | `nf measure coherence` |
| E(F) | Energy | `nf measure energy` |
| H(F) | Entropy | `nf measure entropy` |
| S(A*) | Stability | `nf measure stability` |

### 11.3 Sets

| Notation | Meaning |
|----------|---------|
| P | All patterns |
| A* | Attractor |
| B(A*) | Basin of attraction |
| S | Semantic space |

---

## Related Documents

- `./semantic.md` - Natural language interface
- `./translation.md` - Interface translation
- `../../foundations/` - Mathematical foundations
