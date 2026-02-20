# Mermaid Diagram Generator

Specifications for generating Mermaid.js diagrams for network and flowchart visualizations.

---

## 1. Overview

Mermaid generates text-based diagram definitions that render in Markdown, GitHub, and documentation platforms.

---

## 2. Supported Diagram Types

| Type | Use Case | Mermaid Type |
|------|----------|--------------|
| Network | Resonance graphs | `graph` |
| Flow | Dynamics flow | `flowchart` |
| State | Field states | `stateDiagram-v2` |
| Timeline | History | `timeline` |

---

## 3. Network Graphs

### 3.1 Basic Structure

```mermaid
graph TB
    node1((Label))
    node2((Label))
    node1 --- node2
```

### 3.2 Node Shapes

| Shape | Syntax | Use For |
|-------|--------|---------|
| Circle | `((text))` | Patterns |
| Round rect | `(text)` | Concepts |
| Stadium | `([text])` | Events |
| Diamond | `{text}` | Decisions |
| Hexagon | `{{text}}` | Attractors |

### 3.3 Edge Types

| Type | Syntax | Meaning |
|------|--------|---------|
| Solid | `---` | Strong resonance |
| Arrow | `-->` | Flow direction |
| Dotted | `-.-` | Weak resonance |
| Thick | `===` | Very strong |
| With label | `--\|label\|` | Resonance value |

### 3.4 Full Network Example

```mermaid
graph TB
    subgraph Attractor["security_focus (C=0.82)"]
        sql((sql_injection<br/>0.92))
        user((user_input<br/>0.88))
        valid((validation<br/>0.75))
    end

    sql ---|"R=0.85"| user
    sql ---|"R=0.78"| valid
    user ---|"R=0.72"| valid

    log((logging<br/>0.35))
    log -.-|"R=0.25"| sql

    style sql fill:#ff6b6b,stroke:#c0392b,stroke-width:3px
    style user fill:#ff6b6b,stroke:#c0392b,stroke-width:3px
    style valid fill:#e74c3c,stroke:#c0392b,stroke-width:2px
    style log fill:#95a5a6,stroke:#7f8c8d,stroke-width:1px
```

---

## 4. Generation Algorithm

### 4.1 Network Generation

```
GENERATE_MERMAID_NETWORK(field, threshold=0.2):
  lines = ["graph TB"]

  // Group by attractors
  FOR attractor IN field.attractors:
    lines.append(f'    subgraph {sanitize(attractor.name)}["{attractor.name} (C={attractor.coherence:.2f})"]')
    FOR pattern_id IN attractor.core_patterns:
      pattern = field.patterns[pattern_id]
      lines.append(f'        {pattern_id}(({pattern.name}<br/>{pattern.activation:.2f}))')
    lines.append('    end')

  // Non-attractor patterns
  FOR pattern IN field.patterns:
    IF NOT in_any_attractor(pattern):
      lines.append(f'    {pattern.id}(({pattern.name}<br/>{pattern.activation:.2f}))')

  // Edges
  FOR (p1, p2), resonance IN field.resonance_cache:
    IF resonance >= threshold:
      edge = format_edge(p1, p2, resonance)
      lines.append(f'    {edge}')

  // Styles
  lines.extend(generate_styles(field))

  RETURN "\n".join(lines)
```

### 4.2 Edge Formatting

```
FORMAT_EDGE(p1, p2, resonance):
  IF resonance > 0.7:
    connector = "===|\"R={:.2f}\"|".format(resonance)
  ELSE IF resonance > 0.4:
    connector = "---|\"R={:.2f}\"|".format(resonance)
  ELSE IF resonance > 0.2:
    connector = "-.-|\"R={:.2f}\"|".format(resonance)
  ELSE IF resonance < 0:  // Tension
    connector = "-.->|\"T={:.2f}\"|".format(abs(resonance))

  RETURN f"{p1} {connector} {p2}"
```

### 4.3 Style Generation

```
GENERATE_STYLES(field):
  styles = []

  FOR pattern IN field.patterns:
    color = activation_to_color(pattern.activation)
    border = get_border_style(pattern, field)
    width = activation_to_width(pattern.activation)

    styles.append(
      f"    style {pattern.id} fill:{color},stroke:{border},stroke-width:{width}px"
    )

  RETURN styles
```

### 4.4 Color Mapping

```
ACTIVATION_TO_COLOR(activation):
  IF activation > 0.8:
    RETURN "#ff6b6b"  // High - red
  ELSE IF activation > 0.6:
    RETURN "#e74c3c"  // Medium-high
  ELSE IF activation > 0.4:
    RETURN "#f39c12"  // Medium - orange
  ELSE IF activation > 0.2:
    RETURN "#f1c40f"  // Low-medium - yellow
  ELSE:
    RETURN "#95a5a6"  // Low - gray
```

---

## 5. Flowchart Diagrams

### 5.1 Dynamics Flow

```mermaid
flowchart TD
    subgraph Cycle["Dynamics Cycle"]
        A[Decay] --> B[Resonance]
        B --> C[Amplify/Attenuate]
        C --> D{Attractor Check}
    end

    D -->|Emerged| E[Collapse]
    D -->|Not yet| A

    E --> F[Output]
```

### 5.2 State Transitions

```mermaid
stateDiagram-v2
    [*] --> Injection
    Injection --> Resonating
    Resonating --> Amplifying
    Amplifying --> Coherent: coherence > 0.6
    Amplifying --> Resonating: coherence < 0.6
    Coherent --> AttractorEmerged: stability check
    AttractorEmerged --> Collapsed
    Collapsed --> [*]
```

---

## 6. Direction Options

| Direction | Code | Layout |
|-----------|------|--------|
| Top-Bottom | `TB` | Hierarchical |
| Bottom-Top | `BT` | Inverted |
| Left-Right | `LR` | Horizontal |
| Right-Left | `RL` | Reversed |

```mermaid
graph LR
    A((Start)) --> B((Middle)) --> C((End))
```

---

## 7. Subgraph Styling

### 7.1 Attractor Grouping

```mermaid
graph TB
    subgraph cluster1["Attractor: security_focus"]
        direction TB
        p1((sql_inj))
        p2((user_input))
        p3((validation))
    end

    subgraph cluster2["Attractor: logging_concern"]
        direction TB
        p4((audit))
        p5((errors))
    end

    p1 --- p2
    p2 --- p3
    p4 --- p5
    p1 -.- p4
```

### 7.2 Nested Subgraphs

```mermaid
graph TB
    subgraph Field["Field: main"]
        subgraph Core["Core Patterns"]
            p1((Primary))
            p2((Secondary))
        end
        subgraph Peripheral["Peripheral"]
            p3((Support))
        end
    end
```

---

## 8. Advanced Features

### 8.1 Click Actions

```mermaid
graph TB
    p1((sql_injection))
    click p1 "javascript:showPattern('sql_injection')"
```

### 8.2 Tooltips

```mermaid
graph TB
    p1((sql_injection))
    p1:::tooltip
    classDef tooltip title:Activation 0.92, Coherence 0.85
```

### 8.3 Links with URLs

```mermaid
graph TB
    p1((pattern))
    click p1 href "#pattern-details"
```

---

## 9. Multi-Field Networks

```mermaid
graph TB
    subgraph Field1["Field: perception"]
        a((visual))
        b((audio))
    end

    subgraph Field2["Field: reasoning"]
        c((logic))
        d((inference))
    end

    a -.->|"cross-field"| c
    b -.->|"cross-field"| d

    style Field1 fill:#e8f4f8
    style Field2 fill:#fdf2e9
```

---

## 10. Output Formats

### 10.1 Raw Mermaid

```bash
> nf plot network --format mermaid
```

Output:
```
graph TB
    subgraph security_focus["security_focus (C=0.82)"]
        p_001((sql_injection<br/>0.92))
        p_002((user_input<br/>0.88))
    end
    p_001 ---|"R=0.85"| p_002
    style p_001 fill:#ff6b6b
```

### 10.2 Markdown Wrapped

```bash
> nf plot network --format mermaid --wrap markdown
```

Output:
````markdown
```mermaid
graph TB
    ...
```
````

### 10.3 HTML Embedded

```bash
> nf plot network --format mermaid --wrap html --output network.html
```

Output:
```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
</head>
<body>
  <div class="mermaid">
    graph TB
        ...
  </div>
  <script>mermaid.initialize({startOnLoad:true});</script>
</body>
</html>
```

---

## 11. Limitations

| Limitation | Workaround |
|------------|------------|
| No curved edges | Use intermediate nodes |
| Limited styling | Use classDef for groups |
| No animation | Export to other format |
| Size constraints | Split large graphs |

---

## 12. API

```python
class MermaidGenerator:
    def __init__(self, direction="TB"):
        self.direction = direction
        self.nodes = []
        self.edges = []
        self.subgraphs = []
        self.styles = []

    def add_node(self, id, label, shape="circle"):
        """Add node to diagram"""

    def add_edge(self, source, target, label=None, style="solid"):
        """Add edge between nodes"""

    def add_subgraph(self, name, title, nodes):
        """Group nodes in subgraph"""

    def add_style(self, node_id, fill, stroke, stroke_width):
        """Add node styling"""

    def render(self):
        """Generate Mermaid code"""

    def to_markdown(self):
        """Wrap in markdown code block"""

    def to_html(self):
        """Generate standalone HTML"""
```

---

## 13. Examples

### 13.1 Simple Network

```bash
> nf inject "a" 0.9
> nf inject "b" 0.8
> nf cycle 2
> nf plot network --format mermaid
```

```mermaid
graph TB
    p1((a<br/>0.88))
    p2((b<br/>0.82))
    p1 ---|"R=0.72"| p2
    style p1 fill:#ff6b6b
    style p2 fill:#e74c3c
```

### 13.2 Complex with Attractors

See network examples in `../networks.md`.

---

## Related Documents

- `ascii.md` - ASCII generation
- `svg.md` - SVG generation
- `plotly.md` - Interactive plots
- `../networks.md` - Network visualization
