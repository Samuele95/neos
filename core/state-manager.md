# Multi-Field State Manager

Specification for managing state across multiple neural fields in an orchestrated system.

---

## 1. Overview

The State Manager maintains the complete system state Σ = ({Fᵢ}, C, O), coordinating:
- Individual field states
- Inter-field couplings
- Routes and information flow
- Orchestration protocols
- System-wide versioning

---

## 2. System State Structure

### 2.1 Complete System State

```json
{
  "system": {
    "id": "sys_uuid",
    "name": "analysis_system",
    "created": "2024-01-15T10:30:00Z",
    "modified": "2024-01-15T14:22:00Z"
  },

  "fields": {
    "main": { /* field state */ },
    "memory": { /* field state */ },
    "creative": { /* field state */ },
    "meta": { /* field state */ }
  },

  "active_field": "main",

  "couplings": {
    "main:memory": {
      "gamma_forward": 0.6,
      "gamma_reverse": 0.6,
      "type": "excitatory"
    },
    "main:creative": {
      "gamma_forward": 0.3,
      "gamma_reverse": 0.3,
      "type": "excitatory"
    },
    "meta:main": {
      "gamma_forward": 0.7,
      "gamma_reverse": 0.2,
      "type": "asymmetric"
    }
  },

  "routes": [
    {
      "id": "r_001",
      "source": "memory",
      "target": "main",
      "strength": 0.8,
      "filter": null,
      "transform": null
    },
    {
      "id": "r_002",
      "source": "main",
      "target": "evaluation",
      "strength": 1.0,
      "filter": "activation > 0.5",
      "transform": null
    }
  ],

  "orchestration": {
    "mode": "checkpoint",
    "active_protocol": null,
    "checkpoints": ["coherence > 0.8", "attractor_emerged"]
  },

  "meta": {
    "total_cycles": 147,
    "total_operations": 523,
    "version": "1.0"
  }
}
```

### 2.2 Individual Field State

Each field within `fields` follows this structure:

```json
{
  "field_id": "f_uuid",
  "name": "main",
  "role": "reasoning",
  "status": "active",
  "priority": "normal",

  "parameters": {
    "lambda": 0.05,
    "alpha": 0.3,
    "tau": 0.4,
    "sigma": 0.5
  },

  "patterns": {
    "p_001": {
      "name": "security",
      "activation": 0.85,
      "position": [0.42, 0.78, 0.33],
      "tags": ["critical"],
      "created": "2024-01-15T10:32:00Z",
      "modified": "2024-01-15T14:20:00Z"
    }
  },

  "resonance_cache": {
    "p_001:p_002": 0.78,
    "p_001:p_003": 0.65
  },

  "attractors": [
    {
      "id": "a_001",
      "name": "security_focus",
      "coherence": 0.82,
      "core_patterns": ["p_001", "p_002", "p_003"],
      "peripheral_patterns": ["p_004"],
      "basin_size": 0.65
    }
  ],

  "metrics": {
    "coherence": 0.74,
    "energy": 0.45,
    "entropy": 0.32,
    "stability": 0.88
  },

  "history": {
    "cycles": 47,
    "last_cycle": "2024-01-15T14:20:00Z"
  }
}
```

---

## 3. State Manager Components

### 3.1 Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     State Manager                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│  │   Field     │  │  Coupling   │  │   Route     │        │
│  │  Registry   │  │  Manager    │  │  Manager    │        │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘        │
│         │                │                │                │
│         └────────────────┼────────────────┘                │
│                          │                                 │
│                  ┌───────┴───────┐                        │
│                  │ Orchestration │                        │
│                  │  Controller   │                        │
│                  └───────┬───────┘                        │
│                          │                                 │
│                  ┌───────┴───────┐                        │
│                  │   Version     │                        │
│                  │   Manager     │                        │
│                  └───────────────┘                        │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 Component Responsibilities

| Component | Responsibility |
|-----------|----------------|
| Field Registry | Track all fields, active field selection |
| Coupling Manager | Maintain coupling matrix, stability checks |
| Route Manager | Information flow paths, transfers |
| Orchestration Controller | Protocols, sync, pipeline execution |
| Version Manager | System-wide commits, branches, history |

---

## 4. Field Registry

### 4.1 Operations

```
CLASS FieldRegistry:

  FUNCTION create_field(name, params, role):
    IF name IN fields:
      RAISE FieldExistsError

    field = {
      "field_id": generate_uuid(),
      "name": name,
      "role": role,
      "status": "idle",
      "parameters": apply_role_defaults(params, role),
      "patterns": {},
      "resonance_cache": {},
      "attractors": [],
      "metrics": initial_metrics(),
      "history": {"cycles": 0}
    }

    fields[name] = field
    RETURN field

  FUNCTION delete_field(name, force=False):
    IF name NOT IN fields:
      RAISE FieldNotFoundError

    // Check couplings
    couplings = coupling_manager.get_couplings(name)
    IF couplings AND NOT force:
      RAISE HasCouplingsError(couplings)

    // Remove couplings
    FOR coupling IN couplings:
      coupling_manager.remove(coupling)

    // Remove routes
    routes = route_manager.get_routes(name)
    FOR route IN routes:
      route_manager.remove(route)

    DELETE fields[name]

  FUNCTION select_field(name):
    IF name NOT IN fields:
      RAISE FieldNotFoundError
    active_field = name
    RETURN fields[name]

  FUNCTION get_field(name):
    IF name IS None:
      RETURN fields[active_field]
    RETURN fields.get(name)

  FUNCTION list_fields(include_metrics=False):
    result = []
    FOR name, field IN fields:
      entry = {
        "name": name,
        "role": field.role,
        "status": field.status,
        "pattern_count": len(field.patterns)
      }
      IF include_metrics:
        entry["metrics"] = field.metrics
      result.append(entry)
    RETURN result
```

### 4.2 Role Defaults

```
FUNCTION apply_role_defaults(params, role):
  defaults = {
    "reasoning":   {"lambda": 0.03, "alpha": 0.35, "tau": 0.5, "sigma": 0.4},
    "knowledge":   {"lambda": 0.01, "alpha": 0.25, "tau": 0.3, "sigma": 0.6},
    "planning":    {"lambda": 0.04, "alpha": 0.30, "tau": 0.45, "sigma": 0.5},
    "evaluation":  {"lambda": 0.05, "alpha": 0.35, "tau": 0.5, "sigma": 0.4},
    "creative":    {"lambda": 0.08, "alpha": 0.25, "tau": 0.3, "sigma": 0.7},
    "meta":        {"lambda": 0.02, "alpha": 0.20, "tau": 0.6, "sigma": 0.3}
  }

  base = defaults.get(role, DEFAULT_PARAMS)
  RETURN merge(base, params)  // params override defaults
```

---

## 5. Coupling Manager

### 5.1 Coupling Matrix

```
CLASS CouplingManager:

  couplings = {}  // {field1:field2 -> coupling_data}

  FUNCTION set_coupling(field1, field2, gamma, type="excitatory", asymmetric=False, gamma_reverse=None):
    key = canonical_key(field1, field2)

    IF asymmetric:
      gamma_fwd = gamma
      gamma_rev = gamma_reverse OR gamma
    ELSE:
      gamma_fwd = gamma_rev = gamma

    // Apply sign based on type
    IF type == "inhibitory":
      gamma_fwd = -abs(gamma_fwd)
      gamma_rev = -abs(gamma_rev)

    couplings[key] = {
      "field1": field1,
      "field2": field2,
      "gamma_forward": gamma_fwd,
      "gamma_reverse": gamma_rev,
      "type": type
    }

    // Check stability
    check_stability()

  FUNCTION get_coupling(field1, field2):
    key = canonical_key(field1, field2)
    RETURN couplings.get(key)

  FUNCTION get_coupling_strength(source, target):
    key = canonical_key(source, target)
    coupling = couplings.get(key)
    IF coupling IS None:
      RETURN 0.0

    IF coupling.field1 == source:
      RETURN coupling.gamma_forward
    ELSE:
      RETURN coupling.gamma_reverse

  FUNCTION get_matrix():
    field_names = list(field_registry.fields.keys())
    n = len(field_names)
    matrix = zeros(n, n)

    FOR i, f1 IN enumerate(field_names):
      FOR j, f2 IN enumerate(field_names):
        IF i != j:
          matrix[i][j] = get_coupling_strength(f1, f2)

    RETURN matrix, field_names
```

### 5.2 Stability Check

```
FUNCTION check_stability():
  matrix, _ = get_matrix()

  // Compute spectral radius
  eigenvalues = compute_eigenvalues(matrix)
  spectral_radius = max(abs(eigenvalues))

  // Get parameter bounds
  lambda_min = min(f.parameters.lambda FOR f IN fields)
  alpha_max = max(f.parameters.alpha FOR f IN fields)

  // Stability condition: ρ(Γ) < λ_min / α_max
  stability_bound = lambda_min / alpha_max

  IF spectral_radius >= stability_bound:
    WARN "Coupling matrix may be unstable"
    WARN f"  Spectral radius: {spectral_radius}"
    WARN f"  Stability bound: {stability_bound}"

  RETURN {
    "spectral_radius": spectral_radius,
    "stability_bound": stability_bound,
    "is_stable": spectral_radius < stability_bound
  }
```

---

## 6. Route Manager

### 6.1 Route Operations

```
CLASS RouteManager:

  routes = []

  FUNCTION create_route(source, target, strength=1.0, filter=None, transform=None, bidirectional=False):
    route = {
      "id": generate_route_id(),
      "source": source,
      "target": target,
      "strength": strength,
      "filter": filter,
      "transform": transform,
      "enabled": True
    }

    routes.append(route)

    IF bidirectional:
      reverse = {
        "id": generate_route_id(),
        "source": target,
        "target": source,
        "strength": strength,
        "filter": filter,
        "transform": transform,
        "enabled": True
      }
      routes.append(reverse)

    RETURN route

  FUNCTION transfer(source, target, patterns=None):
    // Get route
    route = find_route(source, target)
    IF route IS None:
      RAISE NoRouteError(source, target)

    // Get source field
    source_field = field_registry.get_field(source)
    target_field = field_registry.get_field(target)

    // Select patterns
    IF patterns IS None:
      patterns = select_patterns(source_field, route.filter)

    // Apply transform
    IF route.transform:
      patterns = apply_transform(patterns, route.transform)

    // Inject into target
    FOR pattern IN patterns:
      adjusted_activation = pattern.activation * route.strength
      field_engine.inject(
        target_field,
        pattern.name,
        adjusted_activation,
        pattern.tags
      )

    RETURN {
      "transferred": len(patterns),
      "source": source,
      "target": target,
      "strength": route.strength
    }

  FUNCTION select_patterns(field, filter_expr):
    IF filter_expr IS None:
      // Return all patterns above threshold
      RETURN [p FOR p IN field.patterns IF p.activation > field.parameters.tau]

    // Parse and evaluate filter
    RETURN evaluate_filter(field.patterns, filter_expr)
```

### 6.2 Filter Expressions

```
FUNCTION evaluate_filter(patterns, expr):
  // Supported expressions:
  // "activation > 0.5"
  // "activation > 0.5 AND tags CONTAINS 'critical'"
  // "name LIKE 'security%'"

  ast = parse_filter(expr)
  RETURN [p FOR p IN patterns IF evaluate_ast(ast, p)]

FILTER_GRAMMAR:
  expr     := comparison (AND|OR comparison)*
  comparison := field OPERATOR value
  field    := "activation" | "name" | "tags" | "created"
  OPERATOR := ">" | "<" | ">=" | "<=" | "==" | "!=" | "CONTAINS" | "LIKE"
  value    := NUMBER | STRING | LIST
```

---

## 7. Orchestration Controller

### 7.1 Synchronized Execution

```
CLASS OrchestrationController:

  FUNCTION sync(fields=None, cycles=1):
    IF fields IS None:
      fields = [f FOR f IN field_registry.fields IF f.status == "active"]

    results = []

    FOR cycle IN range(cycles):
      cycle_result = execute_coupled_cycle(fields)
      results.append(cycle_result)

      // Check for orchestration checkpoints
      IF should_pause(cycle_result):
        BREAK

    RETURN {
      "cycles_executed": len(results),
      "final_state": summarize_fields(fields),
      "cross_field_resonance": compute_cross_field_resonance(fields)
    }

  FUNCTION execute_coupled_cycle(fields):
    // Phase 1: Decay
    FOR field IN fields:
      field_engine.apply_decay(field)

    // Phase 2: Internal resonance
    FOR field IN fields:
      field_engine.compute_resonance(field)

    // Phase 3: Cross-field coupling
    apply_coupling_effects(fields)

    // Phase 4: Amplification/Attenuation
    FOR field IN fields:
      field_engine.apply_dynamics(field)

    // Phase 5: Attractor detection
    FOR field IN fields:
      field_engine.detect_attractors(field)

    RETURN {
      "field_states": {f.name: f.metrics FOR f IN fields}
    }

  FUNCTION apply_coupling_effects(fields):
    FOR f1 IN fields:
      FOR f2 IN fields:
        IF f1 != f2:
          gamma = coupling_manager.get_coupling_strength(f1.name, f2.name)
          IF gamma != 0:
            // Compute cross-field resonance gradient
            gradient = compute_cross_resonance_gradient(f1, f2)

            // Apply coupling influence
            FOR pattern IN f1.patterns:
              pattern.activation += gamma * gradient[pattern.id]
```

### 7.2 Pipeline Execution

```
FUNCTION pipeline(field_sequence, input_pattern):
  current_patterns = [input_pattern]

  FOR i, field_name IN enumerate(field_sequence):
    field = field_registry.get_field(field_name)

    // Inject patterns from previous stage
    FOR pattern IN current_patterns:
      field_engine.inject(field, pattern.name, pattern.activation, pattern.tags)

    // Run until attractor or max cycles
    cycles = 0
    WHILE cycles < MAX_PIPELINE_CYCLES:
      field_engine.execute_cycle(field)
      cycles += 1

      IF field.attractors:
        BREAK

    // Collapse to get output
    IF i < len(field_sequence) - 1:  // Not last stage
      current_patterns = field_engine.collapse(field, return_patterns=True)

  // Final collapse
  RETURN field_engine.collapse(field_sequence[-1])

FUNCTION parallel(fields, input_pattern, integrate_method="resonance"):
  results = {}

  // Parallel execution (simulated)
  FOR field_name IN fields:
    field = field_registry.get_field(field_name)
    field_engine.inject(field, input_pattern.name, input_pattern.activation)

    // Run cycles
    FOR _ IN range(PARALLEL_CYCLES):
      field_engine.execute_cycle(field)

    results[field_name] = field_engine.collapse(field, return_patterns=True)

  // Integration
  RETURN integrate_results(results, integrate_method)

FUNCTION integrate_results(results, method):
  IF method == "average":
    RETURN weighted_average(results)
  ELSE IF method == "resonance":
    RETURN resonance_fusion(results)
  ELSE IF method == "competition":
    RETURN highest_activation(results)
  ELSE IF method == "union":
    RETURN union_patterns(results)
```

---

## 8. Version Management

### 8.1 System-Wide Commits

```
FUNCTION commit_system(message):
  // Capture complete system state
  state = {
    "system": system_metadata,
    "fields": {name: serialize_field(f) FOR name, f IN fields},
    "couplings": serialize_couplings(),
    "routes": serialize_routes(),
    "orchestration": orchestration_state
  }

  // Compute hash
  content_hash = compute_hash(state)

  // Create commit object
  commit = {
    "id": content_hash[:12],
    "hash": content_hash,
    "parent": current_commit,
    "message": message,
    "timestamp": now(),
    "state_hash": compute_hash(state)
  }

  // Store state and commit
  storage.put_object(state)
  storage.put_commit(commit)

  // Update ref
  refs[current_branch] = commit.id
  current_commit = commit.id

  RETURN commit
```

### 8.2 Checkout

```
FUNCTION checkout_system(ref):
  // Resolve ref to commit
  commit = resolve_ref(ref)

  // Load state
  state = storage.get_object(commit.state_hash)

  // Restore fields
  field_registry.fields = {}
  FOR name, field_data IN state.fields:
    field_registry.fields[name] = deserialize_field(field_data)

  // Restore couplings
  coupling_manager.couplings = deserialize_couplings(state.couplings)

  // Restore routes
  route_manager.routes = state.routes

  // Update current
  current_commit = commit.id

  RETURN commit
```

---

## 9. Cross-Field Operations

### 9.1 Cross-Field Resonance

```
FUNCTION compute_cross_field_resonance(field1, field2):
  total_resonance = 0.0
  count = 0

  FOR p1 IN field1.patterns:
    FOR p2 IN field2.patterns:
      // Semantic similarity between patterns in different fields
      similarity = compute_semantic_similarity(p1.position, p2.position)

      // Weight by activations
      weighted = similarity * p1.activation * p2.activation
      total_resonance += weighted
      count += 1

  IF count == 0:
    RETURN 0.0

  RETURN total_resonance / count

FUNCTION compute_cross_resonance_gradient(field1, field2):
  gradient = {}

  FOR p1 IN field1.patterns:
    grad = 0.0
    FOR p2 IN field2.patterns:
      similarity = compute_semantic_similarity(p1.position, p2.position)
      grad += similarity * p2.activation
    gradient[p1.id] = grad / len(field2.patterns)

  RETURN gradient
```

### 9.2 Collective Attractors

```
FUNCTION detect_collective_attractors(fields):
  // Find attractors that span multiple fields
  collective = []

  // Get all attractors
  all_attractors = []
  FOR field IN fields:
    FOR attractor IN field.attractors:
      all_attractors.append((field.name, attractor))

  // Check for cross-field resonance
  FOR i, (f1, a1) IN enumerate(all_attractors):
    FOR f2, a2 IN all_attractors[i+1:]:
      IF f1 != f2:  // Different fields
        // Compute resonance between attractor cores
        resonance = compute_attractor_resonance(a1, a2)

        IF resonance > COLLECTIVE_THRESHOLD:
          collective.append({
            "fields": [f1, f2],
            "attractors": [a1.name, a2.name],
            "resonance": resonance,
            "combined_coherence": (a1.coherence + a2.coherence) / 2
          })

  RETURN collective
```

---

## 10. State Serialization

### 10.1 Export

```
FUNCTION export_system(format="json"):
  state = get_complete_state()

  IF format == "json":
    RETURN json.dumps(state, indent=2)
  ELSE IF format == "yaml":
    RETURN yaml.dump(state)
  ELSE IF format == "binary":
    RETURN msgpack.pack(state)

FUNCTION import_system(data, format="json"):
  IF format == "json":
    state = json.loads(data)
  ELSE IF format == "yaml":
    state = yaml.load(data)
  ELSE IF format == "binary":
    state = msgpack.unpack(data)

  restore_system(state)
```

### 10.2 State Validation

```
FUNCTION validate_system_state(state):
  errors = []

  // Validate fields
  FOR name, field IN state.fields:
    field_errors = validate_field_state(field)
    errors.extend(field_errors)

  // Validate couplings reference valid fields
  FOR key, coupling IN state.couplings:
    f1, f2 = key.split(":")
    IF f1 NOT IN state.fields:
      errors.append(f"Coupling references unknown field: {f1}")
    IF f2 NOT IN state.fields:
      errors.append(f"Coupling references unknown field: {f2}")

  // Validate routes
  FOR route IN state.routes:
    IF route.source NOT IN state.fields:
      errors.append(f"Route source unknown: {route.source}")
    IF route.target NOT IN state.fields:
      errors.append(f"Route target unknown: {route.target}")

  // Validate stability
  stability = check_stability_from_state(state)
  IF NOT stability.is_stable:
    errors.append(f"Coupling matrix unstable: ρ={stability.spectral_radius}")

  RETURN errors
```

---

## 11. API Reference

```python
class StateManager:
    """Central state management for multi-field systems."""

    def __init__(self):
        self.field_registry = FieldRegistry()
        self.coupling_manager = CouplingManager()
        self.route_manager = RouteManager()
        self.orchestration = OrchestrationController()
        self.version_manager = VersionManager()

    # Field operations
    def create_field(self, name, params=None, role=None) -> Field
    def delete_field(self, name, force=False) -> None
    def get_field(self, name=None) -> Field
    def select_field(self, name) -> Field
    def list_fields(self, verbose=False) -> List[FieldInfo]

    # Coupling operations
    def set_coupling(self, f1, f2, gamma, type="excitatory") -> Coupling
    def get_coupling(self, f1, f2) -> Optional[Coupling]
    def remove_coupling(self, f1, f2) -> None
    def get_coupling_matrix(self) -> Tuple[Matrix, List[str]]
    def check_stability(self) -> StabilityResult

    # Route operations
    def create_route(self, src, dst, strength=1.0, filter=None) -> Route
    def delete_route(self, src, dst) -> None
    def transfer(self, src, dst, patterns=None) -> TransferResult

    # Orchestration
    def sync(self, fields=None, cycles=1) -> SyncResult
    def pipeline(self, fields, input) -> CollapseResult
    def parallel(self, fields, input, integrate="resonance") -> IntegrationResult

    # Versioning
    def commit(self, message) -> Commit
    def checkout(self, ref) -> Commit
    def branch(self, name) -> Branch

    # Serialization
    def export(self, format="json") -> str
    def import_(self, data, format="json") -> None
    def validate(self) -> List[str]
```

---

## Related Documents

- `./field-engine.md` - Single field operations
- `../commands/field-mgmt.md` - Management commands
- `../persistence/versioning.md` - Version control
- `../../foundations/07-field-orchestration.md` - Theoretical foundation
