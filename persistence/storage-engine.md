# NFOS Storage Engine Specification

Content-addressable object storage for neural field states, implementing efficient persistence with deduplication and compression.

---

## 1. Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                       STORAGE ENGINE                                 │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐            │
│  │   Object    │ →  │    Index    │ →  │   Backend   │            │
│  │  Serializer │    │   Manager   │    │   Adapter   │            │
│  └─────────────┘    └─────────────┘    └─────────────┘            │
│                                                                     │
│  ┌─────────────────────────────────────────────────────┐          │
│  │                  BACKENDS                            │          │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐  │          │
│  │  │ Memory  │ │  File   │ │  JSON   │ │  Cloud  │  │          │
│  │  │ Store   │ │ System  │ │  Store  │ │  Store  │  │          │
│  │  └─────────┘ └─────────┘ └─────────┘ └─────────┘  │          │
│  └─────────────────────────────────────────────────────┘          │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 2. Object Model

### 2.1 Object Types

| Type | Description | Content |
|------|-------------|---------|
| `state` | Field state snapshot | Complete field data |
| `commit` | Version commit | Metadata + state reference |
| `branch` | Branch pointer | Name + head commit |
| `delta` | State difference | Base ref + operations |

### 2.2 Object Structure

```json
{
  "type": "state|commit|branch|delta",
  "id": "content-hash or generated-id",
  "content": { "...": "type-specific content" },
  "metadata": {
    "created_at": "ISO-8601",
    "size_bytes": 1234,
    "compressed": true
  }
}
```

### 2.3 Object ID Generation

For content-addressable objects (states, deltas):
```
object_id = sha256(canonical_json(content))[:12]
```

For reference objects (commits, branches):
```
object_id = base62(random_bytes(8))  // e.g., "a1B2c3D4"
```

---

## 3. Content-Addressable Storage

### 3.1 Store Operation

```
STORE(object):
  // Serialize to canonical form
  content = canonical_serialize(object.content)

  // Compute content hash
  hash = sha256(content)
  short_hash = hash[:12]

  // Check for duplicates
  IF EXISTS(short_hash):
    existing = GET(short_hash)
    IF existing.content == object.content:
      RETURN short_hash  // Deduplicated
    ELSE:
      // Hash collision (extremely rare)
      short_hash = hash[:16]  // Use longer hash

  // Compress if beneficial
  compressed = compress(content)
  IF len(compressed) < len(content) * 0.9:
    stored_content = compressed
    is_compressed = true
  ELSE:
    stored_content = content
    is_compressed = false

  // Store
  storage[short_hash] = {
    content: stored_content,
    compressed: is_compressed,
    original_size: len(content),
    stored_size: len(stored_content)
  }

  RETURN short_hash
```

### 3.2 Retrieve Operation

```
GET(hash):
  IF hash NOT IN storage:
    RETURN null

  record = storage[hash]

  IF record.compressed:
    content = decompress(record.content)
  ELSE:
    content = record.content

  object = deserialize(content)
  RETURN object
```

### 3.3 Canonical Serialization

Ensures identical content produces identical hashes:

```
CANONICAL_SERIALIZE(object):
  // Sort keys alphabetically
  // Use consistent number formatting
  // No extra whitespace
  // UTF-8 encoding

  IF is_dict(object):
    pairs = []
    FOR key IN sorted(object.keys()):
      pairs.append(quote(key) + ":" + canonical_serialize(object[key]))
    RETURN "{" + ",".join(pairs) + "}"

  ELSE IF is_array(object):
    items = [canonical_serialize(item) FOR item IN object]
    RETURN "[" + ",".join(items) + "]"

  ELSE IF is_number(object):
    RETURN format_number(object)  // Consistent decimal places

  ELSE IF is_string(object):
    RETURN quote(escape(object))

  ELSE IF is_bool(object):
    RETURN object ? "true" : "false"

  ELSE IF is_null(object):
    RETURN "null"
```

---

## 4. Delta Storage

### 4.1 Delta Computation

For similar states, store only the differences:

```
COMPUTE_DELTA(base_state, new_state):
  operations = []

  // Compare patterns
  FOR id IN union(base_state.patterns.keys(), new_state.patterns.keys()):
    IF id NOT IN base_state.patterns:
      operations.append({
        op: "add",
        path: "/patterns/" + id,
        value: new_state.patterns[id]
      })
    ELSE IF id NOT IN new_state.patterns:
      operations.append({
        op: "remove",
        path: "/patterns/" + id
      })
    ELSE IF base_state.patterns[id] != new_state.patterns[id]:
      // Compute sub-delta for pattern
      pattern_ops = diff_pattern(base_state.patterns[id], new_state.patterns[id])
      operations.extend(pattern_ops)

  // Compare other fields similarly...

  RETURN operations
```

### 4.2 Delta Application

```
APPLY_DELTA(base_state, delta):
  state = deep_copy(base_state)

  FOR op IN delta.operations:
    MATCH op.op:
      "add":
        set_path(state, op.path, op.value)
      "remove":
        delete_path(state, op.path)
      "replace":
        set_path(state, op.path, op.value)

  RETURN state
```

### 4.3 Delta Storage Decision

```
SHOULD_USE_DELTA(base_state, new_state):
  // Compute full size
  full_size = len(serialize(new_state))

  // Compute delta size
  delta = compute_delta(base_state, new_state)
  delta_size = len(serialize(delta)) + 12  // +12 for base reference

  // Use delta if significantly smaller
  RETURN delta_size < full_size * 0.5
```

---

## 5. Index Manager

### 5.1 Index Structure

```json
{
  "objects": {
    "hash": {
      "type": "state|commit|branch|delta",
      "size": 1234,
      "compressed": true,
      "references": ["hash1", "hash2"],
      "referenced_by": ["hash3", "hash4"]
    }
  },
  "by_type": {
    "state": ["hash1", "hash2"],
    "commit": ["hash3", "hash4"],
    "branch": ["hash5"]
  },
  "commits_by_time": [
    {"id": "hash3", "time": 1234567890},
    {"id": "hash4", "time": 1234567900}
  ]
}
```

### 5.2 Index Operations

```
INDEX_ADD(hash, object_meta):
  index.objects[hash] = object_meta
  index.by_type[object_meta.type].append(hash)

  IF object_meta.type == "commit":
    index.commits_by_time.append({id: hash, time: object_meta.timestamp})
    sort(index.commits_by_time, by: time)

INDEX_REMOVE(hash):
  meta = index.objects[hash]
  index.by_type[meta.type].remove(hash)
  DELETE index.objects[hash]

INDEX_GET_REFS(hash):
  RETURN index.objects[hash].references

INDEX_GET_REFED_BY(hash):
  RETURN index.objects[hash].referenced_by
```

### 5.3 Reference Tracking

```
TRACK_REFERENCES(hash, object):
  refs = extract_references(object)
  index.objects[hash].references = refs

  FOR ref IN refs:
    IF ref IN index.objects:
      index.objects[ref].referenced_by.append(hash)

EXTRACT_REFERENCES(object):
  refs = []

  IF object.type == "commit":
    refs.append(object.state_hash)
    IF object.parent:
      refs.append(object.parent)
    refs.extend(object.parents OR [])

  IF object.type == "delta":
    refs.append(object.base)

  IF object.type == "branch":
    refs.append(object.head)

  RETURN refs
```

---

## 6. Garbage Collection

### 6.1 Mark and Sweep

```
GARBAGE_COLLECT():
  // Mark phase
  marked = set()

  // Start from roots (branches)
  FOR branch IN index.by_type["branch"]:
    mark_reachable(branch, marked)

  // Sweep phase
  FOR hash IN index.objects.keys():
    IF hash NOT IN marked:
      delete_object(hash)

  // Compact storage
  compact_storage()

  RETURN {
    collected: len(index.objects) - len(marked),
    remaining: len(marked)
  }
```

### 6.2 Mark Reachable

```
MARK_REACHABLE(hash, marked):
  IF hash IN marked:
    RETURN

  marked.add(hash)

  FOR ref IN index.objects[hash].references:
    mark_reachable(ref, marked)
```

### 6.3 Compaction

```
COMPACT_STORAGE():
  // Remove gaps in file storage
  // Rebuild index
  // Defragment

  new_storage = {}
  FOR hash, record IN storage:
    IF hash IN index.objects:
      new_storage[hash] = record

  storage = new_storage
  save_index()
```

---

## 7. Backend Adapters

### 7.1 Memory Backend

In-memory storage for temporary sessions:

```
class MemoryBackend:
  def __init__():
    self.data = {}

  def put(hash, content):
    self.data[hash] = content

  def get(hash):
    return self.data.get(hash)

  def delete(hash):
    del self.data[hash]

  def exists(hash):
    return hash in self.data

  def list():
    return list(self.data.keys())
```

### 7.2 File System Backend

Persistent storage in files:

```
class FileBackend:
  def __init__(base_path):
    self.base_path = base_path
    self.objects_dir = base_path + "/objects"
    self.index_file = base_path + "/index.json"

  def put(hash, content):
    // Use first 2 chars as directory (like git)
    dir = self.objects_dir + "/" + hash[:2]
    mkdir(dir)
    file = dir + "/" + hash[2:]
    write(file, content)

  def get(hash):
    file = self.objects_dir + "/" + hash[:2] + "/" + hash[2:]
    return read(file)

  def delete(hash):
    file = self.objects_dir + "/" + hash[:2] + "/" + hash[2:]
    remove(file)
```

### 7.3 JSON Store Backend

Single-file storage for portability:

```
class JSONStoreBackend:
  def __init__(file_path):
    self.file_path = file_path
    self.data = load_json(file_path) OR {}
    self.dirty = false

  def put(hash, content):
    self.data[hash] = base64_encode(content)
    self.dirty = true

  def get(hash):
    encoded = self.data.get(hash)
    return base64_decode(encoded) if encoded else null

  def flush():
    if self.dirty:
      save_json(self.file_path, self.data)
      self.dirty = false
```

---

## 8. Transactions

### 8.1 Transaction Model

```
BEGIN_TRANSACTION():
  transaction = {
    operations: [],
    snapshot: copy(index)
  }
  RETURN transaction

COMMIT_TRANSACTION(transaction):
  // All operations already applied
  save_index()
  RETURN success

ROLLBACK_TRANSACTION(transaction):
  // Restore index
  index = transaction.snapshot

  // Undo storage operations
  FOR op IN reversed(transaction.operations):
    IF op.type == "put":
      backend.delete(op.hash)
    ELSE IF op.type == "delete":
      backend.put(op.hash, op.content)
```

### 8.2 Atomic Operations

```
ATOMIC_COMMIT(message, state):
  tx = BEGIN_TRANSACTION()

  TRY:
    // Store state
    state_hash = store_in_transaction(tx, state)

    // Create commit
    commit = create_commit(message, state_hash)
    commit_hash = store_in_transaction(tx, commit)

    // Update branch
    update_branch_in_transaction(tx, commit_hash)

    COMMIT_TRANSACTION(tx)
    RETURN commit_hash

  CATCH error:
    ROLLBACK_TRANSACTION(tx)
    THROW error
```

---

## 9. Caching

### 9.1 LRU Cache

```
class ObjectCache:
  def __init__(max_size=100):
    self.cache = OrderedDict()
    self.max_size = max_size

  def get(hash):
    if hash in self.cache:
      // Move to end (most recently used)
      self.cache.move_to_end(hash)
      return self.cache[hash]
    return null

  def put(hash, object):
    if len(self.cache) >= self.max_size:
      // Remove least recently used
      self.cache.popitem(last=False)
    self.cache[hash] = object

  def invalidate(hash):
    self.cache.pop(hash, null)
```

### 9.2 Cached Storage

```
class CachedStorage:
  def __init__(backend, cache_size=100):
    self.backend = backend
    self.cache = ObjectCache(cache_size)

  def get(hash):
    // Check cache first
    cached = self.cache.get(hash)
    if cached:
      return cached

    // Load from backend
    content = self.backend.get(hash)
    if content:
      object = deserialize(content)
      self.cache.put(hash, object)
      return object

    return null

  def put(hash, object):
    content = serialize(object)
    self.backend.put(hash, content)
    self.cache.put(hash, object)
```

---

## 10. Statistics

### 10.1 Storage Stats

```
GET_STORAGE_STATS():
  stats = {
    total_objects: len(index.objects),
    by_type: {},
    total_size: 0,
    compressed_size: 0,
    dedup_savings: 0
  }

  FOR hash, meta IN index.objects:
    stats.by_type[meta.type] = stats.by_type.get(meta.type, 0) + 1
    stats.total_size += meta.original_size
    stats.compressed_size += meta.stored_size

  stats.compression_ratio = stats.compressed_size / stats.total_size
  stats.dedup_savings = calculate_dedup_savings()

  RETURN stats
```

### 10.2 Example Output

```
nf storage stats

[STORAGE STATISTICS]
  Total objects: 45
    States: 12
    Commits: 15
    Branches: 3
    Deltas: 15

  Size:
    Original: 2.4 MB
    Stored: 0.8 MB
    Compression ratio: 33%
    Dedup savings: 0.5 MB

  Cache:
    Hit rate: 78%
    Size: 50/100 objects
```

---

## Related Documents

- `format-spec.md` - File format specifications
- `versioning.md` - Version control design
- `../commands/versioning.md` - Command reference
