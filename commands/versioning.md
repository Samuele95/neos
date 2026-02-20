# Versioning Commands

Commands for Git-like version control: commit, branch, checkout, history, diff, and merge.

---

## 1. Commit

Saves a snapshot of the current field state.

### Syntax

```
nf commit [message] [--flags]
```

### Arguments

| Argument | Type | Default | Description |
|----------|------|---------|-------------|
| `message` | string | auto-generated | Commit description |

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--amend` | bool | false | Modify last commit |
| `--all` | bool | true | Include all fields |
| `--field $f` | field_ref | all | Specific field only |

### Behavior

1. Serialize current field state
2. Compute content hash
3. Check for changes since last commit
4. Store state (deduplicated)
5. Create commit object
6. Update branch HEAD

### Output Format

```
[COMMIT] <short_hash>: "<message>"
  Patterns: <count>
  Attractors: <count>
  Coherence: <value>
  Snapshot saved
```

### Examples

```bash
# Basic commit
> nf commit "Initial security analysis"
[COMMIT] a1b2c3d: "Initial security analysis"
  Patterns: 4
  Attractors: 1
  Coherence: 0.82
  Snapshot saved

# Auto-generated message
> nf commit
[COMMIT] e5f6g7h: "Field state at cycle 10"
  Patterns: 5
  Attractors: 2
  Coherence: 0.75

# Amend last commit
> nf commit "Better description" --amend
[COMMIT] a1b2c3d: "Better description" (amended)
  Previous message: "Initial security analysis"
```

### Errors

```
[ERROR] No changes to commit
  Last commit: a1b2c3d (2 minutes ago)
  Current state matches last commit

[WARNING] Large state snapshot
  Size: 5.2 MB (recommended max: 1 MB)
  Consider: nf attenuate --weak to reduce patterns
```

---

## 2. Branch

Manages named branches.

### Subcommands

```
nf branch create <name>    Create new branch
nf branch list            List all branches
nf branch delete <name>   Delete branch
nf branch rename <old> <new>  Rename branch
```

### 2.1 Branch Create

```
nf branch create <name> [--from <ref>]
```

| Flag | Description |
|------|-------------|
| `--from <ref>` | Create from specific commit/branch |

**Output:**
```
[BRANCH] Created: <name>
  Based on: <commit_hash>
  From: <parent_branch or commit>
```

**Examples:**
```bash
> nf branch create experiment
[BRANCH] Created: experiment
  Based on: a1b2c3d
  From: main

> nf branch create feature --from #abc123
[BRANCH] Created: feature
  Based on: abc123
  From: commit abc123
```

### 2.2 Branch List

```
nf branch list [--all] [--verbose]
```

| Flag | Description |
|------|-------------|
| `--all` | Include remote branches |
| `--verbose` | Show commit info |

**Output:**
```
[BRANCHES]
  * main (current)
    experiment
    mitigation_analysis
```

**Verbose Output:**
```
[BRANCHES]
  * main
      a1b2c3d - SQL injection identified (5 min ago)
      Patterns: 4, Coherence: 0.82

    experiment
      e5f6g7h - Testing alternative (2 min ago)
      Patterns: 6, Coherence: 0.68

    mitigation_analysis
      h8i9j0k - Mitigation patterns added (1 min ago)
      Patterns: 5, Coherence: 0.75
```

### 2.3 Branch Delete

```
nf branch delete <name> [--force]
```

| Flag | Description |
|------|-------------|
| `--force` | Delete even if not merged |

**Output:**
```
[BRANCH] Deleted: experiment
  Commits preserved: 3
  States may be garbage collected
```

**Errors:**
```
[ERROR] Cannot delete current branch
  Checkout another branch first: nf checkout main

[ERROR] Branch not merged
  'experiment' has 3 commits not in 'main'
  Use --force to delete anyway
```

---

## 3. Checkout

Restores state from a branch or commit.

### Syntax

```
nf checkout <ref> [--flags]
```

### Arguments

| Argument | Type | Description |
|----------|------|-------------|
| `ref` | branch/commit/relative | Target reference |

### Reference Types

| Type | Example | Description |
|------|---------|-------------|
| Branch | `main` | Switch to branch |
| Commit | `#a1b2c3d` | Checkout specific commit |
| Relative | `~1`, `~3` | Previous commits |
| HEAD | `HEAD` | Current position |

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--force` | bool | false | Discard uncommitted changes |
| `--create` | bool | false | Create branch if not exists |

### Behavior

1. Resolve reference to commit
2. Check for uncommitted changes
3. Load state from storage
4. Restore all fields
5. Update HEAD

### Output Format

```
[CHECKOUT] <ref> (<commit_hash>)
  Restored: <commit_message>
  Patterns: <count>
  Attractors: <count>
  Coherence: <value>
```

### Examples

```bash
# Checkout branch
> nf checkout main
[CHECKOUT] main (a1b2c3d)
  Restored: SQL injection identified
  Patterns: 4
  Coherence: 0.82

# Checkout commit
> nf checkout #e5f6g7h
[CHECKOUT] #e5f6g7h (detached HEAD)
  Restored: Testing alternative
  Warning: Detached HEAD state

# Checkout previous
> nf checkout ~1
[CHECKOUT] ~1 (abc123)
  Restored: Before risky operation
  Patterns: 3
  Coherence: 0.75

# Create and checkout
> nf checkout -b new_branch
[BRANCH] Created: new_branch
[CHECKOUT] new_branch (a1b2c3d)
```

### Errors

```
[ERROR] Uncommitted changes would be overwritten
  Modified patterns: 3
  Options:
    nf commit "Save changes"    - Save first
    nf checkout --force         - Discard changes

[ERROR] Reference not found: nonexistent
  Branches: main, experiment
  Recent commits: a1b2c3d, e5f6g7h

[WARNING] Detached HEAD
  You are in 'detached HEAD' state.
  Changes will be lost unless you create a branch:
    nf branch create <name>
```

---

## 4. History

Shows version history.

### Syntax

```
nf history [--flags]
```

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--graph` | bool | false | Show branch graph |
| `--limit <n>` | int | 10 | Maximum entries |
| `--all` | bool | false | All branches |
| `--oneline` | bool | false | Compact format |

### Output Formats

**Default:**
```
[HISTORY] main

commit a1b2c3d (HEAD -> main)
  Date: 2024-01-15 10:30:00
  Message: SQL injection identified
  Patterns: 4, Coherence: 0.82

commit e5f6g7h
  Date: 2024-01-15 10:15:00
  Message: Patterns injected
  Patterns: 4, Coherence: 0.45

commit h8i9j0k
  Date: 2024-01-15 10:00:00
  Message: Initial session
  Patterns: 0, Coherence: 0
```

**Graph:**
```
[HISTORY] --graph

* a1b2c3d (HEAD -> main) SQL injection identified
|
| * k2l3m4n (mitigation_analysis) Mitigation patterns added
| |
| * i9j0k1l Alternative approach tested
|/
* e5f6g7h Patterns injected
|
* h8i9j0k Initial session
```

**Oneline:**
```
a1b2c3d (HEAD -> main) SQL injection identified
e5f6g7h Patterns injected
h8i9j0k Initial session
```

### Examples

```bash
> nf history --graph --all
[HISTORY] All branches

* a1b2c3d (HEAD -> main) SQL injection identified
|
| * k2l3m4n (mitigation_analysis) Mitigation patterns added
| |
| | * p5q6r7s (experiment) Experimental approach
| |/
| * i9j0k1l Branched for mitigation
|/
* e5f6g7h Patterns injected
|
* h8i9j0k Initial session
```

---

## 5. Diff

Compares two states.

### Syntax

```
nf diff [ref1] [ref2] [--flags]
```

### Arguments

| Argument | Type | Default | Description |
|----------|------|---------|-------------|
| `ref1` | reference | HEAD~1 | First state |
| `ref2` | reference | HEAD | Second state |

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--stat` | bool | false | Summary only |
| `--patterns` | bool | true | Show pattern changes |
| `--attractors` | bool | true | Show attractor changes |
| `--parameters` | bool | false | Show parameter changes |

### Output Format

```
[DIFF] <ref1>..<ref2>

  Patterns:
    + @new_pattern (0.85) [ADDED]
    - @removed_pattern [REMOVED]
    Δ @changed_pattern: 0.72 → 0.88 [MODIFIED]

  Attractors:
    + new_attractor (0.78) [EMERGED]
    - old_attractor [DISSOLVED]

  Metrics:
    Coherence: 0.65 → 0.82 (+0.17)
    Energy: 2.1 → 3.4 (+1.3)
    Patterns: 3 → 5 (+2)
```

### Examples

```bash
# Compare with previous commit
> nf diff
[DIFF] ~1..HEAD

  Patterns:
    Δ @sql_injection: 0.85 → 0.92 (+0.07)
    Δ @user_input: 0.78 → 0.88 (+0.10)

  Attractors:
    + sql_injection_vulnerability (0.82) [EMERGED]

  Metrics:
    Coherence: 0.65 → 0.82 (+0.17)

# Compare branches
> nf diff main mitigation_analysis
[DIFF] main..mitigation_analysis

  Patterns:
    + @parameterized_queries (0.88) [ADDED]
    + @input_validation (0.85) [ADDED]
    Δ @sql_query_construction: 0.95 → 0.65 (-0.30)
    Δ @string_concatenation: 0.87 → 0.45 (-0.42)
    - @no_input_validation [REMOVED]

  Attractors:
    - sql_injection_vulnerability (0.82) [DISSOLVED]
    + secure_pattern (0.75) [EMERGED]

  Metrics:
    Coherence: 0.82 → 0.75 (-0.07)
    Patterns: 4 → 5 (+1)

# Stats only
> nf diff main experiment --stat
[DIFF STAT] main..experiment
  Patterns: +2, -1, ~3
  Attractors: +1, -1
  Coherence: 0.82 → 0.68
```

---

## 6. Merge

Combines branches.

### Syntax

```
nf merge <branch> [--strategy <s>] [--flags]
```

### Arguments

| Argument | Type | Description |
|----------|------|-------------|
| `branch` | string | Branch to merge in |

### Strategies

| Strategy | Description |
|----------|-------------|
| `resonance` | Weight by resonance (default) |
| `ours` | Prefer current branch |
| `theirs` | Prefer source branch |
| `manual` | Stop on conflicts |

### Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--strategy <s>` | string | resonance | Merge strategy |
| `--no-commit` | bool | false | Merge without committing |
| `--message <m>` | string | auto | Commit message |

### Merge Process

**Resonance Strategy:**
1. Find common ancestor
2. Compute diffs from both branches
3. For added patterns: include if resonance > 0.5
4. For conflicts: keep higher-coherence version
5. Create merge commit

### Output Format

```
[MERGE] <source> into <target>
  Strategy: <strategy>

  Changes from <source>:
    + @pattern_a (included, R=0.72)
    + @pattern_b (excluded, R=0.28)

  Conflicts resolved:
    @conflicting: kept <target> version (higher coherence)

  Result:
    Patterns: <count>
    Coherence: <value>

[COMMIT] m1n2o3p: "Merge <source> into <target>"
```

### Examples

```bash
# Basic merge
> nf merge mitigation_analysis
[MERGE] mitigation_analysis into main
  Strategy: resonance

  Changes from mitigation_analysis:
    + @parameterized_queries (included, R=0.78)
    + @input_validation (included, R=0.72)

  Patterns affected:
    @sql_query_construction: 0.95 → 0.75 (weakened by mitigation)
    @no_input_validation: removed (conflicts with @input_validation)

  Result:
    Patterns: 5
    Coherence: 0.78

[COMMIT] m1n2o3p: "Merge mitigation_analysis into main"

# With specific strategy
> nf merge experiment --strategy ours
[MERGE] experiment into main
  Strategy: ours

  Kept: all patterns from main
  Added: non-conflicting from experiment
    + @experimental_idea (added)

[COMMIT] q4r5s6t: "Merge experiment into main"

# Manual conflict resolution
> nf merge feature --strategy manual
[MERGE] feature into main
  Strategy: manual

  CONFLICT: @hypothesis
    main: activation=0.85, tags=[primary]
    feature: activation=0.72, tags=[alternative]

  Resolve with:
    nf merge --resolve @hypothesis --keep ours
    nf merge --resolve @hypothesis --keep theirs
    nf merge --continue
    nf merge --abort
```

### Conflict Resolution

```bash
> nf merge --resolve @hypothesis --keep ours
[RESOLVED] @hypothesis: kept main version

> nf merge --continue
[MERGE] Completed after conflict resolution
  Patterns: 6
  Coherence: 0.75

[COMMIT] u7v8w9x: "Merge feature into main"
```

---

## 7. Quick Reference

```
COMMIT                        BRANCH
────────────────────────────  ────────────────────────────
nf commit [message]           nf branch create <name>
nf commit --amend             nf branch list
                              nf branch delete <name>

CHECKOUT                      HISTORY
────────────────────────────  ────────────────────────────
nf checkout <branch>          nf history
nf checkout #<commit>         nf history --graph
nf checkout ~1                nf history --all
nf checkout -b <new>

DIFF                          MERGE
────────────────────────────  ────────────────────────────
nf diff                       nf merge <branch>
nf diff <ref1> <ref2>         nf merge --strategy <s>
nf diff --stat                nf merge --abort
```

---

## Related Documents

- `../persistence/versioning.md` - Versioning design
- `../persistence/format-spec.md` - State format
- `../examples/02-versioning.md` - Versioning walkthrough
