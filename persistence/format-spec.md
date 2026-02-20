# NFOS Persistence Format Specification

Defines the file formats for persisting NFOS state: `.nfstate` for field snapshots and `.nfsession` for complete sessions.

---

## 1. Overview

NFOS uses two primary formats:

| Format | Extension | Purpose |
|--------|-----------|---------|
| Field State | `.nfstate` | Single field snapshot |
| Session | `.nfsession` | Complete session with history |

Both formats use JSON internally with optional compression.

---

## 2. Field State Format (.nfstate)

A `.nfstate` file captures a complete snapshot of a single field at a point in time.

### 2.1 Schema

```json
{
  "$schema": "https://nfos.dev/schemas/nfstate-v1.json",
  "version": "1.0",
  "format": "nfstate",

  "metadata": {
    "created_at": "2024-01-15T10:30:00Z",
    "created_by": "nfos-v1.0",
    "description": "SQL injection analysis - initial finding",
    "tags": ["security", "sql", "vulnerability"]
  },

  "field": {
    "id": "f_001",
    "name": "main",

    "parameters": {
      "lambda": 0.05,
      "alpha": 0.30,
      "tau": 0.40,
      "sigma": 0.50
    },

    "patterns": {
      "p_001": {
        "id": "p_001",
        "name": "sql_query_construction",
        "activation": 0.95,
        "position": [0.42, 0.78, 0.33],
        "tags": ["vulnerability", "query"],
        "metadata": {
          "injected_at": 0,
          "last_modified": 3,
          "source": "user"
        }
      },
      "p_002": {
        "id": "p_002",
        "name": "user_input_handling",
        "activation": 0.91,
        "position": [0.38, 0.72, 0.41],
        "tags": ["input", "boundary"],
        "metadata": {
          "injected_at": 0,
          "last_modified": 3,
          "source": "user"
        }
      }
    },

    "resonance_cache": {
      "p_001:p_002": {
        "value": 0.72,
        "computed_at": 3,
        "components": {
          "semantic": 0.75,
          "logical": 0.78,
          "contextual": 0.65
        }
      }
    },

    "attractors": [
      {
        "id": "a_001",
        "name": "sql_injection_vulnerability",
        "core_patterns": ["p_001", "p_002"],
        "coherence": 0.82,
        "stability": 0.91,
        "emerged_at": 2,
        "basin_estimate": {
          "size": 0.65,
          "depth": 0.85
        }
      }
    ],

    "metrics": {
      "cycle_count": 3,
      "coherence": 0.82,
      "energy": 3.14,
      "entropy": 1.45,
      "pattern_count": 4,
      "attractor_count": 1
    }
  },

  "checksum": "sha256:a1b2c3d4..."
}
```

### 2.2 Field Descriptions

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `version` | string | yes | Format version |
| `format` | string | yes | Always "nfstate" |
| `metadata.created_at` | ISO-8601 | yes | Creation timestamp |
| `metadata.description` | string | no | Human description |
| `metadata.tags` | string[] | no | Categorization tags |
| `field.id` | string | yes | Unique field identifier |
| `field.name` | string | yes | Human-readable name |
| `field.parameters` | object | yes | Field dynamics parameters |
| `field.patterns` | object | yes | Pattern ID → Pattern map |
| `field.resonance_cache` | object | no | Cached resonance values |
| `field.attractors` | array | no | Emerged attractors |
| `field.metrics` | object | yes | Computed metrics |
| `checksum` | string | yes | Integrity verification |

### 2.3 Pattern Object

```json
{
  "id": "p_001",
  "name": "pattern_name",
  "activation": 0.85,
  "position": [x, y, z],
  "tags": ["tag1", "tag2"],
  "metadata": {
    "injected_at": 0,
    "last_modified": 5,
    "source": "user|derived|imported"
  }
}
```

### 2.4 Attractor Object

```json
{
  "id": "a_001",
  "name": "attractor_name",
  "core_patterns": ["p_001", "p_002"],
  "coherence": 0.82,
  "stability": 0.91,
  "emerged_at": 4,
  "basin_estimate": {
    "size": 0.65,
    "depth": 0.85
  }
}
```

---

## 3. Session Format (.nfsession)

A `.nfsession` file captures a complete NFOS session including all fields, version history, and configuration.

### 3.1 Schema

```json
{
  "$schema": "https://nfos.dev/schemas/nfsession-v1.json",
  "version": "1.0",
  "format": "nfsession",

  "session": {
    "id": "sess_001",
    "name": "Code Security Analysis",
    "created_at": "2024-01-15T10:00:00Z",
    "modified_at": "2024-01-15T11:30:00Z",
    "description": "Analyzing web application for security vulnerabilities"
  },

  "config": {
    "mode": "checkpoint",
    "interface": "semantic",
    "checkpoints": [
      "coherence > 0.8",
      "attractor_emerged"
    ],
    "defaults": {
      "lambda": 0.05,
      "alpha": 0.30,
      "tau": 0.40,
      "sigma": 0.50
    }
  },

  "fields": {
    "main": {
      "...": "(field state object)"
    },
    "reasoning": {
      "...": "(field state object)"
    }
  },

  "active_field": "main",

  "routing": [
    {
      "source": "perception",
      "destination": "reasoning",
      "strength": 0.5
    }
  ],

  "coupling": [
    {
      "field_a": "main",
      "field_b": "reasoning",
      "gamma": 0.3
    }
  ],

  "repository": {
    "head": "main",
    "branches": {
      "main": "c_003",
      "mitigation_analysis": "c_005"
    },
    "commits": {
      "c_001": {
        "id": "c_001",
        "message": "Initial session",
        "timestamp": "2024-01-15T10:00:00Z",
        "parent": null,
        "state_hash": "sha256:abc123..."
      },
      "c_002": {
        "id": "c_002",
        "message": "Patterns injected",
        "timestamp": "2024-01-15T10:15:00Z",
        "parent": "c_001",
        "state_hash": "sha256:def456..."
      },
      "c_003": {
        "id": "c_003",
        "message": "SQL injection vulnerability identified",
        "timestamp": "2024-01-15T10:30:00Z",
        "parent": "c_002",
        "state_hash": "sha256:ghi789..."
      }
    },
    "states": {
      "sha256:abc123...": { "...": "(compressed field state)" },
      "sha256:def456...": { "...": "(compressed field state)" },
      "sha256:ghi789...": { "...": "(compressed field state)" }
    }
  },

  "history": [
    {
      "timestamp": "2024-01-15T10:05:00Z",
      "command": "nf inject \"sql_query_construction\" 0.9",
      "result": "success"
    },
    {
      "timestamp": "2024-01-15T10:06:00Z",
      "command": "nf inject \"user_input_handling\" 0.85",
      "result": "success"
    },
    {
      "timestamp": "2024-01-15T10:10:00Z",
      "command": "nf cycle 3 --trace",
      "result": "attractor_emerged"
    }
  ],

  "checksum": "sha256:xyz..."
}
```

### 3.2 Section Descriptions

| Section | Description |
|---------|-------------|
| `session` | Session metadata |
| `config` | Runtime configuration |
| `fields` | All field states |
| `active_field` | Currently active field |
| `routing` | Field-to-field connections |
| `coupling` | Field coupling strengths |
| `repository` | Git-like version history |
| `history` | Command execution log |

### 3.3 Repository Section

The repository section implements Git-like versioning:

```json
{
  "repository": {
    "head": "main",
    "branches": {
      "branch_name": "commit_id"
    },
    "commits": {
      "commit_id": {
        "id": "commit_id",
        "message": "Commit message",
        "timestamp": "ISO-8601",
        "parent": "parent_commit_id | null",
        "parents": ["id1", "id2"],
        "state_hash": "sha256:...",
        "author": "optional_author",
        "metadata": {}
      }
    },
    "states": {
      "sha256:hash": { "...": "compressed state" }
    }
  }
}
```

---

## 4. Compression

### 4.1 State Deduplication

States are stored content-addressably:

```
state_hash = sha256(serialize(state))
```

Identical states share storage.

### 4.2 Delta Compression

For similar states, store deltas:

```json
{
  "type": "delta",
  "base": "sha256:base_hash",
  "operations": [
    {"op": "replace", "path": "/patterns/p_001/activation", "value": 0.92},
    {"op": "add", "path": "/patterns/p_005", "value": {...}}
  ]
}
```

### 4.3 Binary Compression

Large sessions can use gzip compression:

```
.nfsession.gz - gzip compressed JSON
```

---

## 5. Validation

### 5.1 Schema Validation

```
VALIDATE_NFSTATE(data):
  // Check required fields
  REQUIRE data.version
  REQUIRE data.format == "nfstate"
  REQUIRE data.field
  REQUIRE data.field.parameters
  REQUIRE data.field.patterns
  REQUIRE data.checksum

  // Validate parameters
  ASSERT 0 <= data.field.parameters.lambda <= 1
  ASSERT 0 <= data.field.parameters.alpha <= 1
  ASSERT 0 <= data.field.parameters.tau <= 1
  ASSERT data.field.parameters.sigma >= 0

  // Validate patterns
  FOR id, pattern IN data.field.patterns:
    ASSERT pattern.id == id
    ASSERT 0 <= pattern.activation <= 1
    ASSERT len(pattern.position) == 3

  // Validate attractors
  FOR attractor IN data.field.attractors:
    FOR pattern_id IN attractor.core_patterns:
      ASSERT pattern_id IN data.field.patterns

  // Verify checksum
  computed = sha256(serialize(data without checksum))
  ASSERT computed == data.checksum

  RETURN VALID
```

### 5.2 Integrity Checks

| Check | Purpose |
|-------|---------|
| Checksum | Data hasn't been corrupted |
| References | All pattern/attractor refs valid |
| Ranges | All values in valid ranges |
| Consistency | Metrics match computed values |

---

## 6. File Operations

### 6.1 Save State

```
SAVE_STATE(field, filename):
  state = {
    version: "1.0",
    format: "nfstate",
    metadata: {
      created_at: now(),
      created_by: "nfos-v1.0"
    },
    field: serialize_field(field)
  }

  state.checksum = sha256(serialize(state))

  IF filename.endswith(".gz"):
    write_gzip(filename, serialize(state))
  ELSE:
    write_json(filename, state)
```

### 6.2 Load State

```
LOAD_STATE(filename):
  IF filename.endswith(".gz"):
    data = parse_json(read_gzip(filename))
  ELSE:
    data = parse_json(read(filename))

  VALIDATE_NFSTATE(data)

  field = deserialize_field(data.field)
  RETURN field
```

### 6.3 Save Session

```
SAVE_SESSION(session, filename):
  data = {
    version: "1.0",
    format: "nfsession",
    session: session.metadata,
    config: session.config,
    fields: {},
    repository: session.repository,
    history: session.history
  }

  FOR name, field IN session.fields:
    data.fields[name] = serialize_field(field)

  data.checksum = sha256(serialize(data))
  write_json(filename, data)
```

---

## 7. Migration

### 7.1 Version Upgrades

```
MIGRATE(data):
  IF data.version == "0.9":
    data = migrate_0_9_to_1_0(data)

  RETURN data
```

### 7.2 Migration Functions

```
MIGRATE_0_9_TO_1_0(data):
  // Add new required fields
  IF "metrics" NOT IN data.field:
    data.field.metrics = compute_metrics(data.field)

  // Rename deprecated fields
  IF "decay_rate" IN data.field.parameters:
    data.field.parameters.lambda = data.field.parameters.decay_rate
    DELETE data.field.parameters.decay_rate

  data.version = "1.0"
  RETURN data
```

---

## 8. Examples

### 8.1 Minimal .nfstate

```json
{
  "version": "1.0",
  "format": "nfstate",
  "metadata": {
    "created_at": "2024-01-15T10:30:00Z",
    "created_by": "nfos-v1.0"
  },
  "field": {
    "id": "f_001",
    "name": "main",
    "parameters": {
      "lambda": 0.05,
      "alpha": 0.30,
      "tau": 0.40,
      "sigma": 0.50
    },
    "patterns": {},
    "metrics": {
      "cycle_count": 0,
      "coherence": 0,
      "energy": 0,
      "pattern_count": 0,
      "attractor_count": 0
    }
  },
  "checksum": "sha256:..."
}
```

### 8.2 Minimal .nfsession

```json
{
  "version": "1.0",
  "format": "nfsession",
  "session": {
    "id": "sess_001",
    "name": "New Session",
    "created_at": "2024-01-15T10:00:00Z"
  },
  "config": {
    "mode": "step",
    "interface": "semantic"
  },
  "fields": {
    "main": { "...": "field state" }
  },
  "active_field": "main",
  "repository": {
    "head": "main",
    "branches": { "main": null },
    "commits": {},
    "states": {}
  },
  "history": [],
  "checksum": "sha256:..."
}
```

---

## Related Documents

- `versioning.md` - Git-like versioning design
- `storage-engine.md` - Object storage implementation
- `../commands/versioning.md` - Version control commands
