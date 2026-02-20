# NFOS Versioning System Design

Git-like version control for neural field states, enabling branching, history tracking, and exploratory analysis.

---

## 1. Overview

NFOS versioning provides:

- **Commits**: Immutable snapshots of field state
- **Branches**: Named pointers to commit chains
- **History**: Full audit trail of field evolution
- **Diff**: Compare states across commits
- **Merge**: Combine branches with conflict resolution

---

## 2. Core Concepts

### 2.1 Commit

A commit is an immutable snapshot of the complete field state.

```
Commit {
  id: string           // Unique identifier (short hash)
  message: string      // Human description
  timestamp: datetime  // When created
  parent: string?      // Previous commit (null for root)
  parents: string[]    // For merge commits
  state_hash: string   // Reference to stored state
  metadata: {
    coherence: float   // Field coherence at commit
    patterns: int      // Pattern count
    attractors: int    // Attractor count
    author: string?    // Optional author
  }
}
```

### 2.2 Branch

A branch is a named, movable pointer to a commit.

```
Branch {
  name: string         // Branch identifier
  head: string         // Current commit ID
  created_at: datetime
  description: string?
}
```

### 2.3 Reference Types

| Type | Format | Example | Description |
|------|--------|---------|-------------|
| Branch | `name` | `main`, `experiment` | Branch name |
| Commit | `#hash` | `#a1b2c3d` | Commit short hash |
| Relative | `~n` | `~1`, `~3` | N commits before HEAD |
| HEAD | `HEAD` | `HEAD` | Current position |

### 2.4 HEAD

HEAD indicates the current position in the version graph.

```
HEAD → branch_name → commit_id
  or
HEAD → commit_id (detached HEAD)
```

---

## 3. Operations

### 3.1 Commit

Creates a new snapshot of the current field state.

```
COMMIT(message):
  // Serialize current state
  state = serialize_all_fields()
  state_hash = sha256(state)

  // Check for changes
  IF state_hash == current_commit.state_hash:
    RETURN ERROR("No changes to commit")

  // Store state if new
  IF state_hash NOT IN storage:
    storage[state_hash] = compress(state)

  // Create commit object
  commit = {
    id: generate_short_hash(),
    message: message,
    timestamp: now(),
    parent: HEAD.commit_id,
    state_hash: state_hash,
    metadata: compute_metadata(state)
  }

  // Store commit
  commits[commit.id] = commit

  // Update branch
  branches[HEAD.branch].head = commit.id

  RETURN commit
```

### 3.2 Branch Create

Creates a new branch at the current position.

```
BRANCH_CREATE(name):
  IF name IN branches:
    RETURN ERROR("Branch already exists")

  branch = {
    name: name,
    head: HEAD.commit_id,
    created_at: now()
  }

  branches[name] = branch
  RETURN branch
```

### 3.3 Checkout

Restores state from a reference (branch or commit).

```
CHECKOUT(ref):
  // Resolve reference
  IF ref IN branches:
    commit_id = branches[ref].head
    new_head = {type: "branch", name: ref}
  ELSE IF ref matches commit_pattern:
    commit_id = resolve_commit(ref)
    new_head = {type: "detached", commit: commit_id}
  ELSE:
    RETURN ERROR("Invalid reference")

  // Check for uncommitted changes
  IF has_uncommitted_changes():
    RETURN ERROR("Uncommitted changes. Commit or discard first.")

  // Load state
  commit = commits[commit_id]
  state = decompress(storage[commit.state_hash])

  // Restore fields
  restore_all_fields(state)

  // Update HEAD
  HEAD = new_head

  RETURN commit
```

### 3.4 Diff

Compares two commits or states.

```
DIFF(ref1, ref2):
  state1 = get_state(ref1 OR HEAD~1)
  state2 = get_state(ref2 OR HEAD)

  diff = {
    patterns: {
      added: [],
      removed: [],
      changed: []
    },
    attractors: {
      added: [],
      removed: [],
      changed: []
    },
    parameters: {
      changed: []
    },
    metrics: {
      coherence: [state1.coherence, state2.coherence],
      energy: [state1.energy, state2.energy]
    }
  }

  // Compare patterns
  FOR id IN union(state1.patterns.keys, state2.patterns.keys):
    IF id NOT IN state1.patterns:
      diff.patterns.added.append(state2.patterns[id])
    ELSE IF id NOT IN state2.patterns:
      diff.patterns.removed.append(state1.patterns[id])
    ELSE IF state1.patterns[id] != state2.patterns[id]:
      diff.patterns.changed.append({
        id: id,
        before: state1.patterns[id],
        after: state2.patterns[id]
      })

  // Similar for attractors, parameters...

  RETURN diff
```

### 3.5 Merge

Combines two branches.

```
MERGE(source_branch, strategy="resonance"):
  target = HEAD.branch
  source_commit = branches[source_branch].head
  target_commit = branches[target].head

  // Find common ancestor
  ancestor = find_common_ancestor(source_commit, target_commit)

  // Get states
  ancestor_state = get_state(ancestor)
  source_state = get_state(source_commit)
  target_state = get_state(target_commit)

  // Compute diffs
  source_diff = diff(ancestor_state, source_state)
  target_diff = diff(ancestor_state, target_state)

  // Merge based on strategy
  MATCH strategy:
    "resonance":
      merged = merge_by_resonance(target_state, source_state, source_diff, target_diff)
    "ours":
      merged = target_state  // Keep target, apply non-conflicting source changes
    "theirs":
      merged = source_state  // Keep source
    "manual":
      conflicts = detect_conflicts(source_diff, target_diff)
      IF conflicts:
        RETURN CONFLICT(conflicts)
      merged = auto_merge(target_state, source_diff)

  // Apply merged state
  restore_all_fields(merged)

  // Create merge commit
  commit = {
    id: generate_short_hash(),
    message: "Merge " + source_branch + " into " + target,
    timestamp: now(),
    parents: [target_commit, source_commit],
    state_hash: sha256(serialize(merged)),
    metadata: compute_metadata(merged)
  }

  commits[commit.id] = commit
  branches[target].head = commit.id

  RETURN commit
```

---

## 4. Merge Strategies

### 4.1 Resonance-Weighted Merge (Default)

Combines patterns based on their resonance with the resulting field.

```
MERGE_BY_RESONANCE(target, source, source_diff, target_diff):
  merged = copy(target)

  // Handle added patterns from source
  FOR pattern IN source_diff.patterns.added:
    // Compute resonance with target patterns
    resonance = mean([R(pattern, p) for p in merged.patterns])

    IF resonance > 0.5:
      // High resonance: add with full strength
      merged.patterns[pattern.id] = pattern
    ELSE IF resonance > 0.3:
      // Moderate resonance: add with reduced strength
      pattern.activation *= 0.7
      merged.patterns[pattern.id] = pattern
    // Low resonance: don't add

  // Handle conflicts (same pattern modified in both)
  FOR conflict IN detect_conflicts(source_diff, target_diff):
    source_version = source.patterns[conflict.id]
    target_version = target.patterns[conflict.id]

    // Weight by coherence contribution
    source_coherence = coherence_contribution(source_version, source)
    target_coherence = coherence_contribution(target_version, target)

    IF source_coherence > target_coherence:
      merged.patterns[conflict.id] = source_version
    ELSE:
      merged.patterns[conflict.id] = target_version

  // Handle attractors
  merged.attractors = merge_attractors(target.attractors, source.attractors)

  RETURN merged
```

### 4.2 Ours Strategy

Keeps target state, only applies non-conflicting additions from source.

```
MERGE_OURS(target, source, source_diff):
  merged = copy(target)

  // Only add patterns that don't conflict
  FOR pattern IN source_diff.patterns.added:
    IF pattern.id NOT IN merged.patterns:
      merged.patterns[pattern.id] = pattern

  RETURN merged
```

### 4.3 Theirs Strategy

Takes source state entirely.

```
MERGE_THEIRS(source):
  RETURN copy(source)
```

### 4.4 Manual Strategy

Stops on conflicts for user resolution.

```
MERGE_MANUAL(target, source, source_diff, target_diff):
  conflicts = detect_conflicts(source_diff, target_diff)

  IF conflicts.length > 0:
    RETURN {
      status: "conflict",
      conflicts: conflicts,
      partial_merge: auto_merge_non_conflicting(target, source_diff)
    }

  RETURN auto_merge(target, source_diff)
```

---

## 5. Conflict Detection

### 5.1 Pattern Conflicts

A conflict occurs when the same pattern was modified differently in both branches.

```
DETECT_PATTERN_CONFLICTS(source_diff, target_diff):
  conflicts = []

  source_modified = set(source_diff.patterns.changed.map(c → c.id))
  target_modified = set(target_diff.patterns.changed.map(c → c.id))

  FOR id IN intersection(source_modified, target_modified):
    source_change = source_diff.patterns.changed.find(c → c.id == id)
    target_change = target_diff.patterns.changed.find(c → c.id == id)

    // Check if changes are different
    IF source_change.after != target_change.after:
      conflicts.append({
        type: "pattern_modification",
        pattern_id: id,
        source_value: source_change.after,
        target_value: target_change.after
      })

  RETURN conflicts
```

### 5.2 Attractor Conflicts

```
DETECT_ATTRACTOR_CONFLICTS(source_diff, target_diff):
  conflicts = []

  // Conflicting attractors (same patterns, different interpretation)
  FOR s_attr IN source_diff.attractors.added:
    FOR t_attr IN target_diff.attractors.added:
      overlap = intersection(s_attr.core_patterns, t_attr.core_patterns)
      IF len(overlap) > len(s_attr.core_patterns) * 0.5:
        IF s_attr.name != t_attr.name:
          conflicts.append({
            type: "attractor_naming",
            patterns: overlap,
            source_name: s_attr.name,
            target_name: t_attr.name
          })

  RETURN conflicts
```

---

## 6. History Traversal

### 6.1 Linear History

```
GET_HISTORY(ref, limit=10):
  history = []
  current = resolve_commit(ref)

  WHILE current AND len(history) < limit:
    history.append(commits[current])
    current = commits[current].parent

  RETURN history
```

### 6.2 Graph History

```
GET_HISTORY_GRAPH(limit=20):
  // BFS from all branch heads
  visited = set()
  queue = [branches[b].head for b in branches]
  graph = {nodes: [], edges: []}

  WHILE queue AND len(graph.nodes) < limit:
    commit_id = queue.pop(0)
    IF commit_id IN visited:
      CONTINUE
    visited.add(commit_id)

    commit = commits[commit_id]
    graph.nodes.append(commit)

    // Add edges to parents
    FOR parent IN [commit.parent] + (commit.parents OR []):
      IF parent:
        graph.edges.append({from: commit_id, to: parent})
        queue.append(parent)

  RETURN graph
```

### 6.3 Reference Resolution

```
RESOLVE_REF(ref):
  // Branch name
  IF ref IN branches:
    RETURN branches[ref].head

  // Commit hash
  IF ref.startswith("#"):
    hash = ref[1:]
    matches = [c for c in commits if c.startswith(hash)]
    IF len(matches) == 1:
      RETURN matches[0]
    ELSE IF len(matches) > 1:
      ERROR("Ambiguous reference")
    ELSE:
      ERROR("Commit not found")

  // Relative reference
  IF ref.startswith("~"):
    n = int(ref[1:])
    current = HEAD.commit_id
    FOR i IN range(n):
      IF commits[current].parent:
        current = commits[current].parent
      ELSE:
        ERROR("Not enough history")
    RETURN current

  // HEAD
  IF ref == "HEAD":
    RETURN HEAD.commit_id

  ERROR("Invalid reference: " + ref)
```

---

## 7. Storage Optimization

### 7.1 Content-Addressable Storage

States are stored by their content hash:

```
state_hash = sha256(canonical_serialize(state))
storage[state_hash] = state
```

Benefits:
- Automatic deduplication
- Integrity verification
- Efficient comparison

### 7.2 Delta Storage

For similar states, store deltas instead of full copies:

```
IF similarity(new_state, recent_state) > 0.8:
  delta = compute_delta(recent_state, new_state)
  storage[new_hash] = {
    type: "delta",
    base: recent_hash,
    operations: delta
  }
```

### 7.3 Garbage Collection

Remove unreferenced states:

```
GARBAGE_COLLECT():
  referenced = set()

  // Mark all referenced states
  FOR commit IN commits.values():
    referenced.add(commit.state_hash)

  // Sweep unreferenced
  FOR hash IN storage.keys():
    IF hash NOT IN referenced:
      DELETE storage[hash]
```

---

## 8. Use Cases

### 8.1 Exploratory Analysis

```
# Start analysis
nf inject "hypothesis_a" 0.8
nf cycle 5
nf commit "Initial hypothesis"

# Try alternative
nf branch create alternative
nf inject "hypothesis_b" 0.9
nf cycle 5
nf commit "Alternative approach"

# Compare results
nf checkout main
nf diff alternative

# Merge if beneficial
nf merge alternative --strategy resonance
```

### 8.2 Checkpoint and Rollback

```
# Save checkpoint
nf commit "Before risky operation"

# Try something
nf inject "experimental" 0.9
nf cycle 10

# Didn't work - rollback
nf checkout ~1
```

### 8.3 Parallel Exploration

```
# Create parallel branches
nf branch create security_focus
nf branch create performance_focus

# Develop each
nf checkout security_focus
nf inject "security_patterns"...
nf commit "Security analysis"

nf checkout performance_focus
nf inject "performance_patterns"...
nf commit "Performance analysis"

# Merge insights
nf checkout main
nf merge security_focus
nf merge performance_focus
```

---

## 9. Visualization

### 9.1 History Graph

```
nf history --graph

* c_005 (mitigation_analysis) Mitigation patterns added
|
| * c_004 (main) Additional evidence
| |
|/
* c_003 SQL injection identified
|
* c_002 Patterns injected
|
* c_001 Initial session
```

### 9.2 Diff Visualization

```
nf diff main mitigation_analysis

[DIFF] main → mitigation_analysis

  Patterns:
    + @parameterized_queries (0.88) [NEW]
    + @input_validation (0.85) [NEW]
    Δ @sql_query_construction: 0.95 → 0.65
    Δ @string_concatenation: 0.87 → 0.45
    - @no_input_validation (removed)

  Attractors:
    - sql_injection_vulnerability (0.82) [REMOVED]
    + secure_pattern (0.75) [NEW]

  Metrics:
    Coherence: 0.82 → 0.75
    Energy: 3.14 → 2.89
```

---

## Related Documents

- `format-spec.md` - File format specifications
- `storage-engine.md` - Storage implementation
- `../commands/versioning.md` - Command reference
