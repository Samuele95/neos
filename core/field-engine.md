# Field Engine Specification

Defines how NFOS executes field operations on the neural field state.

---

## 1. Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         FIELD ENGINE                                 │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐            │
│  │  Operation  │ →  │  Dynamics   │ →  │  Attractor  │            │
│  │  Executor   │    │  Processor  │    │  Detector   │            │
│  └─────────────┘    └─────────────┘    └─────────────┘            │
│         │                  │                  │                    │
│         ▼                  ▼                  ▼                    │
│  ┌─────────────────────────────────────────────────────┐          │
│  │               STATE MANAGER                          │          │
│  │  ┌─────────┐ ┌───────────┐ ┌──────────┐ ┌────────┐ │          │
│  │  │Patterns │ │ Resonance │ │Attractors│ │History │ │          │
│  │  │  Store  │ │   Cache   │ │  Store   │ │  Log   │ │          │
│  │  └─────────┘ └───────────┘ └──────────┘ └────────┘ │          │
│  └─────────────────────────────────────────────────────┘          │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 2. State Manager

### 2.1 Field State Structure

```json
{
  "field_id": "uuid-v4",
  "name": "field_name",
  "created_at": "ISO-8601",
  "modified_at": "ISO-8601",

  "parameters": {
    "lambda": 0.05,
    "alpha": 0.30,
    "tau": 0.40,
    "sigma": 0.50
  },

  "patterns": {
    "p_001": {
      "id": "p_001",
      "name": "security_concern",
      "activation": 0.85,
      "position": [0.42, 0.78, 0.33],
      "tags": ["vulnerability", "input"],
      "injected_at": 0,
      "last_modified": 5
    }
  },

  "resonance_cache": {
    "p_001:p_002": {
      "value": 0.78,
      "computed_at": 5,
      "semantic": "shared concern for input security",
      "logical": "sql risk requires input vector",
      "contextual": "security analysis domain"
    }
  },

  "attractors": [
    {
      "id": "a_001",
      "name": "input_security_focus",
      "core_patterns": ["p_001", "p_002"],
      "coherence": 0.82,
      "emerged_at": 4,
      "stability": 0.88
    }
  ],

  "cycle_count": 10,
  "coherence": 0.78,
  "energy": 2.45
}
```

### 2.2 State Operations

```
STATE_GET(field_id, path) → value
STATE_SET(field_id, path, value) → success
STATE_UPDATE(field_id, updater_fn) → new_state
STATE_SNAPSHOT(field_id) → immutable_copy
STATE_RESTORE(field_id, snapshot) → success
```

---

## 3. Operation Executor

### 3.1 Inject Operation

```
EXECUTE_INJECT(field, pattern_name, strength, options):
  // Validate
  IF pattern_exists(field, pattern_name):
    RETURN ERROR("Pattern already exists")
  IF strength < 0 OR strength > 1:
    RETURN ERROR("Invalid strength")

  // Generate ID and position
  pattern_id ← generate_id()
  position ← options.position OR auto_position(field, pattern_name)

  // Create pattern
  pattern ← {
    id: pattern_id,
    name: pattern_name,
    activation: strength,
    position: position,
    tags: options.tags OR [],
    injected_at: field.cycle_count,
    last_modified: field.cycle_count
  }

  // Add to field
  field.patterns[pattern_id] ← pattern

  // Compute immediate resonances (optional optimization)
  IF options.compute_resonance:
    FOR existing_id, existing IN field.patterns:
      IF existing_id != pattern_id:
        R ← compute_resonance(pattern, existing)
        cache_resonance(field, pattern_id, existing_id, R)

  // Update metrics
  update_field_metrics(field)

  RETURN {
    success: true,
    pattern_id: pattern_id,
    active_count: count(field.patterns)
  }
```

### 3.2 Amplify Operation

```
EXECUTE_AMPLIFY(field, pattern_ref, factor, options):
  // Resolve reference
  pattern ← resolve_pattern(field, pattern_ref)
  IF pattern == null:
    RETURN ERROR("Pattern not found")

  // Compute factor
  IF options.resonance:
    factor ← 1 + sum_resonance_contributions(field, pattern)
  ELSE IF options.with:
    other ← resolve_pattern(field, options.with)
    R ← get_resonance(field, pattern, other)
    factor ← 1 + R * other.activation

  // Apply amplification
  old_activation ← pattern.activation
  new_activation ← min(old_activation * factor, options.max OR 1.0)
  pattern.activation ← new_activation
  pattern.last_modified ← field.cycle_count

  RETURN {
    success: true,
    old: old_activation,
    new: new_activation,
    factor: factor
  }
```

### 3.3 Attenuate Operation

```
EXECUTE_ATTENUATE(field, pattern_ref, factor, options):
  IF options.weak:
    // Attenuate all weak patterns
    results ← []
    FOR id, pattern IN field.patterns:
      IF pattern.activation < options.threshold:
        old ← pattern.activation
        pattern.activation ← old * factor
        results.append({id: id, old: old, new: pattern.activation})
    RETURN {success: true, attenuated: results}

  // Single pattern
  pattern ← resolve_pattern(field, pattern_ref)
  IF pattern == null:
    RETURN ERROR("Pattern not found")

  old_activation ← pattern.activation
  IF options.to:
    new_activation ← options.to
  ELSE:
    new_activation ← old_activation * factor

  pattern.activation ← new_activation
  pattern.last_modified ← field.cycle_count

  // Check dormancy
  dormant ← new_activation < field.parameters.tau

  RETURN {
    success: true,
    old: old_activation,
    new: new_activation,
    dormant: dormant
  }
```

### 3.4 Tune Operation

```
EXECUTE_TUNE(field, params, options):
  old_params ← copy(field.parameters)
  threshold_effects ← []

  FOR param, value IN params:
    // Validate parameter name
    IF param NOT IN ["lambda", "alpha", "tau", "sigma"]:
      RETURN ERROR("Invalid parameter: " + param)

    // Validate range
    IF param IN ["lambda", "alpha", "tau"] AND (value < 0 OR value > 1):
      RETURN ERROR("Value out of range for " + param)
    IF param == "sigma" AND value < 0:
      RETURN ERROR("Sigma must be non-negative")

    field.parameters[param] ← value

  // Check threshold effects
  IF params.tau AND params.tau > old_params.tau:
    FOR id, pattern IN field.patterns:
      IF pattern.activation < params.tau AND pattern.activation >= old_params.tau:
        threshold_effects.append(id)

  RETURN {
    success: true,
    old: old_params,
    new: field.parameters,
    threshold_effects: threshold_effects
  }
```

### 3.5 Collapse Operation

```
EXECUTE_COLLAPSE(field, options):
  strategy ← options.strategy OR "attractor"

  MATCH strategy:
    "attractor":
      // Find dominant attractor
      IF field.attractors.length == 0:
        RETURN ERROR("No attractor emerged")
      dominant ← max_by(field.attractors, a → a.coherence)
      output ← collapse_from_attractor(dominant)

    "threshold":
      // Select patterns above threshold
      threshold ← options.threshold OR 0.5
      selected ← filter(field.patterns, p → p.activation >= threshold)
      output ← collapse_from_patterns(selected)

    "weighted":
      // Weight all patterns by activation
      weights ← normalize(map(field.patterns, p → p.activation))
      output ← collapse_weighted(field.patterns, weights)

    "sample":
      // Probabilistic sampling
      distribution ← normalize(map(field.patterns, p → p.activation))
      sampled ← sample_from(field.patterns, distribution)
      output ← collapse_from_patterns([sampled])

  RETURN {
    success: true,
    strategy: strategy,
    output: output.content,
    confidence: output.confidence,
    supporting: output.supporting
  }
```

---

## 4. Dynamics Processor

### 4.1 Single Cycle

```
EXECUTE_CYCLE(field, options):
  trace ← options.trace ? [] : null
  prev_state ← snapshot(field)

  // Phase 1: Decay
  decay_results ← apply_decay(field, trace)

  // Phase 2: Resonance
  resonance_results ← compute_all_resonances(field, trace)

  // Phase 3: Amplify/Attenuate
  dynamics_results ← apply_resonance_effects(field, resonance_results, trace)

  // Phase 4: Threshold cleanup
  cleanup_results ← remove_below_threshold(field)

  // Phase 5: Coherence
  coherence ← compute_coherence(field)
  field.coherence ← coherence

  // Phase 6: Attractor check
  attractor_emerged ← check_attractor_emergence(field, prev_state)
  IF attractor_emerged:
    record_attractor(field, attractor_emerged)

  // Increment cycle
  field.cycle_count += 1

  RETURN {
    cycle: field.cycle_count,
    decay: decay_results,
    resonance: resonance_results,
    dynamics: dynamics_results,
    coherence: coherence,
    attractor: attractor_emerged,
    trace: trace
  }
```

### 4.2 Decay Phase

```
APPLY_DECAY(field, trace):
  λ ← field.parameters.lambda
  results ← []

  FOR id, pattern IN field.patterns:
    old ← pattern.activation
    new ← old * (1 - λ)
    pattern.activation ← new
    results.append({id: id, old: old, new: new})

    IF trace:
      trace.append({phase: "decay", pattern: id, old: old, new: new})

  RETURN results
```

### 4.3 Resonance Phase

```
COMPUTE_ALL_RESONANCES(field, trace):
  patterns ← active_patterns(field)
  results ← {}

  FOR i ← 0 TO patterns.length - 2:
    FOR j ← i + 1 TO patterns.length - 1:
      p1 ← patterns[i]
      p2 ← patterns[j]

      // Check cache
      cached ← get_cached_resonance(field, p1.id, p2.id)
      IF cached AND cached.computed_at == field.cycle_count:
        R ← cached.value
      ELSE:
        R ← compute_resonance(p1, p2)
        cache_resonance(field, p1.id, p2.id, R)

      results[p1.id + ":" + p2.id] ← R

      IF trace:
        trace.append({
          phase: "resonance",
          p1: p1.id,
          p2: p2.id,
          value: R,
          classification: classify_resonance(R)
        })

  RETURN results
```

### 4.4 Resonance Computation

```
COMPUTE_RESONANCE(p1, p2):
  // Semantic overlap (embedding similarity)
  semantic ← embedding_similarity(p1.name, p2.name)

  // Logical support (inference relationship)
  logical ← compute_logical_support(p1, p2)

  // Contextual fit (tag/domain overlap)
  contextual ← compute_contextual_fit(p1, p2)

  // Combine (geometric mean)
  R ← (semantic * logical * contextual) ^ (1/3)

  RETURN {
    value: R,
    semantic: semantic,
    logical: logical,
    contextual: contextual
  }
```

### 4.5 Resonance Effects Phase

```
APPLY_RESONANCE_EFFECTS(field, resonances, trace):
  α ← field.parameters.alpha
  results ← []

  FOR id, pattern IN field.patterns:
    // Compute resonance boost
    boost ← 0
    FOR other_id, other IN field.patterns:
      IF other_id != id:
        key ← make_key(id, other_id)
        R ← resonances[key] OR 0
        boost += R * other.activation

    // Apply
    old ← pattern.activation
    new ← old + α * boost
    new ← min(new, 1.0)  // Cap at 1
    pattern.activation ← new

    IF trace:
      trace.append({
        phase: "dynamics",
        pattern: id,
        old: old,
        new: new,
        boost: α * boost,
        type: new > old ? "amplify" : "attenuate"
      })

    results.append({id: id, old: old, new: new})

  RETURN results
```

---

## 5. Attractor Detector

### 5.1 Detection Algorithm

```
CHECK_ATTRACTOR_EMERGENCE(field, prev_state):
  // Find clusters
  clusters ← find_high_resonance_clusters(field)
  IF clusters.length == 0:
    RETURN null

  // Check dominant cluster
  dominant ← max_by(clusters, c → sum(p.activation for p in c))

  // Test 1: Coherence threshold
  cluster_coherence ← compute_cluster_coherence(dominant)
  IF cluster_coherence < 0.6:
    RETURN null

  // Test 2: Energy concentration
  cluster_energy ← sum(p.activation^2 for p in dominant)
  total_energy ← sum(p.activation^2 for p in field.patterns)
  concentration ← cluster_energy / total_energy
  IF concentration < 0.7:
    RETURN null

  // Test 3: Stability (perturbation test)
  IF NOT test_stability(field, dominant):
    RETURN null

  // Test 4: Convergence (if prev_state available)
  IF prev_state:
    drift ← compute_drift(field, prev_state)
    IF drift > 0.1:
      RETURN null

  // Attractor emerged!
  RETURN {
    name: generate_attractor_name(dominant),
    core_patterns: map(dominant, p → p.id),
    coherence: cluster_coherence,
    stability: compute_stability_score(field, dominant)
  }
```

### 5.2 Cluster Finding

```
FIND_HIGH_RESONANCE_CLUSTERS(field):
  patterns ← active_patterns(field)
  threshold ← 0.5  // Resonance threshold for clustering

  // Build adjacency
  adjacency ← {}
  FOR p IN patterns:
    adjacency[p.id] ← []
    FOR q IN patterns:
      IF p.id != q.id:
        R ← get_resonance(field, p, q)
        IF R >= threshold:
          adjacency[p.id].append(q.id)

  // Find connected components
  visited ← {}
  clusters ← []

  FOR p IN patterns:
    IF p.id NOT IN visited:
      cluster ← []
      dfs(p.id, adjacency, visited, cluster)
      IF cluster.length >= 2:
        clusters.append(cluster)

  RETURN clusters
```

### 5.3 Stability Test

```
TEST_STABILITY(field, cluster):
  λ ← field.parameters.lambda
  τ ← field.parameters.tau

  FOR pattern IN cluster:
    // Hypothetically remove pattern
    resonance_support ← 0
    FOR other IN cluster:
      IF other.id != pattern.id:
        R ← get_resonance(field, pattern, other)
        resonance_support += R * other.activation

    // Would pattern recover?
    recovery_threshold ← λ * τ
    IF resonance_support < recovery_threshold:
      RETURN false  // Pattern wouldn't recover

  RETURN true  // All patterns would recover
```

---

## 6. Metrics Computation

### 6.1 Coherence

```
COMPUTE_COHERENCE(field):
  patterns ← active_patterns(field)
  IF patterns.length < 2:
    RETURN 0

  resonances ← []
  FOR i ← 0 TO patterns.length - 2:
    FOR j ← i + 1 TO patterns.length - 1:
      R ← get_resonance(field, patterns[i], patterns[j])
      resonances.append(R)

  μ ← mean(resonances)
  σ² ← variance(resonances)

  coherence ← μ / (1 + σ²)
  RETURN coherence
```

### 6.2 Energy

```
COMPUTE_ENERGY(field):
  total ← 0
  breakdown ← {}

  FOR id, pattern IN field.patterns:
    energy ← pattern.activation ^ 2
    total += energy
    breakdown[id] ← energy

  RETURN {
    total: total,
    breakdown: breakdown,
    distribution: normalize(breakdown)
  }
```

### 6.3 Entropy

```
COMPUTE_ENTROPY(field):
  activations ← map(field.patterns, p → p.activation)
  probabilities ← normalize(activations)

  H ← 0
  FOR p IN probabilities:
    IF p > 0:
      H -= p * log2(p)

  RETURN {
    value: H,
    effective_patterns: 2 ^ H
  }
```

---

## 7. Auto-Positioning

### 7.1 Semantic Positioning

```
AUTO_POSITION(field, pattern_name):
  // Get embedding for pattern name
  embedding ← get_embedding(pattern_name)

  // Project to 3D semantic space
  position ← project_to_3d(embedding)

  // Adjust for existing patterns (avoid overlap)
  FOR existing IN field.patterns:
    distance ← euclidean_distance(position, existing.position)
    IF distance < MIN_DISTANCE:
      position ← nudge_away(position, existing.position)

  RETURN position
```

### 7.2 Projection Methods

```
PROJECT_TO_3D(embedding):
  // Option 1: PCA on common vocabulary
  // Option 2: UMAP projection
  // Option 3: Learned semantic coordinates

  // Simplified: use first 3 principal components
  RETURN [embedding[0], embedding[1], embedding[2]]
```

---

## 8. Performance Considerations

### 8.1 Resonance Caching

- Cache resonance computations
- Invalidate on pattern modification
- Expire after N cycles if not accessed

### 8.2 Incremental Updates

- Don't recompute all resonances every cycle
- Track which patterns changed
- Only update affected pairs

### 8.3 Cluster Caching

- Cache cluster structure
- Update incrementally on pattern changes
- Full recomputation every K cycles

---

## Related Documents

- `command-parser.md` - Command parsing
- `state-manager.md` - State persistence
- `../commands/field-ops.md` - Operation commands
- `../../foundations/05-operations.md` - Mathematical foundations
