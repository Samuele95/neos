# Field Topology Visualization

Specifications for visualizing field topology: attractor landscapes, basins, energy surfaces, and semantic space structure.

---

## 1. Overview

Topology visualization shows the "shape" of the semantic field:
- **Attractor basins** as valleys/wells
- **Energy landscape** as 3D surface
- **Pattern positions** as points in semantic space
- **Flow trajectories** showing dynamics

---

## 2. Visualization Types

### 2.1 Energy Landscape

Shows the potential energy surface where:
- Low points = attractors
- High points = unstable regions
- Valleys = basins of attraction

```
nf plot topology [--style <style>] [--3d]
```

**Styles:**
| Style | Description | Best For |
|-------|-------------|----------|
| `contour` | 2D contour lines | Quick overview |
| `heatmap` | Color-coded 2D | Pattern density |
| `surface` | 3D surface plot | Full landscape |
| `wireframe` | 3D wireframe | Structure clarity |

### 2.2 Basin Map

Shows which regions flow to which attractors.

```
nf plot basins [--attractor <name>] [--resolution <n>]
```

### 2.3 Flow Field

Shows dynamics trajectories as vector arrows.

```
nf plot flow [--from @pattern] [--steps <n>]
```

---

## 3. 2D Representations

### 3.1 Contour Plot

```
[TOPOLOGY] Field: main (Contour)

    ┌────────────────────────────────────────────┐
 1.0│     ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·       │
    │   ·  ╭──────────╮  ·  ·  ·  ·  ·  ·       │
 0.8│  · ╭─┤ security │─╮ ·  ·  ╭────╮  ·       │
    │ · ╭┤ │  basin   │ ├╮·  · │log │  ·       │
 0.6│ ·│││ │  ●0.82   │ │││·  · │ ●  │  ·       │
    │ · ╰┤ │          │ ├╯·  · │0.45│  ·       │
 0.4│  · ╰─┤          │─╯ ·  ·  ╰────╯  ·       │
    │   ·  ╰──────────╯  ·  ·  ·  ·  ·  ·       │
 0.2│     ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·       │
    │       ·  ·  ·  ·  ·  ·  ·  ·  ·  ·        │
 0.0└────────────────────────────────────────────┘
    0.0      0.2      0.4      0.6      0.8      1.0

Legend:
  ● Attractor (depth shown)
  ─ Basin boundary (contour)
  · Background (high energy)
```

### 3.2 Heatmap

```
[TOPOLOGY] Field: main (Heatmap)

    0.0   0.2   0.4   0.6   0.8   1.0
    ┌─────┬─────┬─────┬─────┬─────┐
1.0 │░░░░░│░░░░░│░░░░░│░░░░░│░░░░░│
    ├─────┼─────┼─────┼─────┼─────┤
0.8 │░░░░░│▒▒▒▒▒│▓▓▓▓▓│▒▒▒▒▒│░░░░░│
    ├─────┼─────┼─────┼─────┼─────┤
0.6 │░░░░░│▓▓▓▓▓│█████│▓▓▓▓▓│░░▒▒▒│
    ├─────┼─────┼─────┼─────┼─────┤
0.4 │░░░░░│▒▒▒▒▒│▓▓▓▓▓│▒▒▒▒▒│░░░░░│
    ├─────┼─────┼─────┼─────┼─────┤
0.2 │░░░░░│░░░░░│░░░░░│░░░░░│░░░░░│
    └─────┴─────┴─────┴─────┴─────┘

Energy Scale:
  ░ High (unstable)
  ▒ Medium
  ▓ Low (approaching attractor)
  █ Minimum (attractor)
```

### 3.3 Basin Map

```
[BASINS] Field: main

    ┌────────────────────────────────────────────┐
 1.0│ · · · · · · · · · · · · · · · · · · · · · │
    │ · · · · · · · · · · · · · · · B B B · · · │
 0.8│ · · · A A A A A A · · · · · B B B B · · · │
    │ · · A A A A A A A A · · · B B B B · · · · │
 0.6│ · · A A A A●A A A A · · · · B●B · · · · · │
    │ · · A A A A A A A A · · · B B B B · · · · │
 0.4│ · · · A A A A A A · · · · · B B B · · · · │
    │ · · · · · · · · · · · · · · · · · · · · · │
 0.2│ · · · · · · · · · · · · · · · · · · · · · │
    │ · · · · · · · · · · · · · · · · · · · · · │
 0.0└────────────────────────────────────────────┘

Legend:
  A = Basin of security_focus (●)
  B = Basin of logging_concern (●)
  · = Undetermined / high energy
```

---

## 4. 3D Representations

### 4.1 Surface Plot (Plotly)

```json
{
  "type": "surface",
  "data": {
    "x": [0, 0.1, 0.2, ...],
    "y": [0, 0.1, 0.2, ...],
    "z": [[0.8, 0.7, ...], [0.7, 0.5, ...], ...]
  },
  "layout": {
    "title": "Energy Landscape: main",
    "scene": {
      "xaxis": {"title": "Semantic Dim 1"},
      "yaxis": {"title": "Semantic Dim 2"},
      "zaxis": {"title": "Energy"}
    }
  },
  "markers": [
    {"x": 0.4, "y": 0.6, "z": 0.15, "label": "security_focus", "type": "attractor"}
  ]
}
```

### 4.2 SVG 3D Projection

```svg
<svg viewBox="0 0 500 400" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <linearGradient id="depth" x1="0%" y1="0%" x2="0%" y2="100%">
      <stop offset="0%" style="stop-color:#e74c3c"/>
      <stop offset="50%" style="stop-color:#f39c12"/>
      <stop offset="100%" style="stop-color:#27ae60"/>
    </linearGradient>
  </defs>

  <!-- Energy surface (simplified 3D projection) -->
  <path d="M50,300 Q150,200 250,250 T450,280"
        fill="none" stroke="url(#depth)" stroke-width="2"/>

  <!-- Basin depression -->
  <ellipse cx="200" cy="260" rx="60" ry="30"
           fill="#27ae60" fill-opacity="0.3" stroke="#27ae60"/>

  <!-- Attractor point -->
  <circle cx="200" cy="260" r="8" fill="#c0392b"/>
  <text x="200" y="300" text-anchor="middle" font-size="12">
    security_focus (0.82)
  </text>

  <!-- Flow arrows -->
  <path d="M100,220 Q150,240 190,255" fill="none"
        stroke="#666" marker-end="url(#arrow)"/>
  <path d="M300,230 Q260,245 210,258" fill="none"
        stroke="#666" marker-end="url(#arrow)"/>
</svg>
```

---

## 5. Energy Computation

### 5.1 Energy Function

```
E(x) = -Σᵢ A(pᵢ) × G(x, position(pᵢ), σ)
```

Where:
- `A(pᵢ)` = activation of pattern i
- `G(x, μ, σ)` = Gaussian kernel centered at pattern position
- Low energy = high activation concentration

### 5.2 Grid Computation

```
COMPUTE_ENERGY_GRID(field, resolution=50):
  grid = zeros(resolution, resolution)

  FOR i IN range(resolution):
    FOR j IN range(resolution):
      x = i / resolution
      y = j / resolution

      energy = 0
      FOR pattern IN field.patterns:
        dist = euclidean([x, y], pattern.position[:2])
        energy -= pattern.activation * gaussian(dist, sigma)

      grid[i][j] = energy

  RETURN grid
```

### 5.3 Contour Extraction

```
EXTRACT_CONTOURS(grid, levels=[0.2, 0.4, 0.6, 0.8]):
  contours = []

  FOR level IN levels:
    paths = marching_squares(grid, level)
    contours.append({level: level, paths: paths})

  RETURN contours
```

---

## 6. Basin Computation

### 6.1 Flow Simulation

```
COMPUTE_BASINS(field, resolution=50, steps=100):
  basins = zeros(resolution, resolution, dtype=int)
  attractors = field.attractors

  FOR i IN range(resolution):
    FOR j IN range(resolution):
      start = [i/resolution, j/resolution]
      endpoint = simulate_flow(field, start, steps)

      // Find nearest attractor
      nearest = find_nearest_attractor(endpoint, attractors)
      basins[i][j] = nearest.id

  RETURN basins
```

### 6.2 Flow Simulation

```
SIMULATE_FLOW(field, start, steps, dt=0.1):
  position = start

  FOR step IN range(steps):
    // Compute gradient (direction of steepest descent)
    gradient = compute_gradient(field, position)

    // Move along gradient
    position = position - dt * gradient

    // Check convergence
    IF norm(gradient) < 0.01:
      BREAK

  RETURN position
```

---

## 7. Visualization Commands

### 7.1 Basic Topology

```bash
> nf plot topology
[TOPOLOGY] Field: main

  Attractors detected: 2
    security_focus (0.82) at (0.42, 0.65)
    logging_concern (0.45) at (0.78, 0.52)

  [Contour plot displayed]
```

### 7.2 3D Surface

```bash
> nf plot topology --3d
[TOPOLOGY 3D] Field: main

  Generating energy surface...
  Resolution: 50x50
  Attractors marked: 2

  [Interactive 3D plot - rotate with mouse]
```

### 7.3 Basin Analysis

```bash
> nf plot basins
[BASINS] Field: main

  Basin sizes:
    security_focus: 65% of space
    logging_concern: 25% of space
    Undetermined: 10%

  [Basin map displayed]

> nf plot basins --attractor security_focus
[BASIN] security_focus

  Size: 65% of semantic space
  Depth: 0.82
  Boundary patterns:
    @error_handling (on boundary)
    @monitoring (outside)

  [Single basin highlighted]
```

### 7.4 Flow Trajectories

```bash
> nf plot flow --from @new_pattern
[FLOW] Trajectory from @new_pattern

  Start: (0.55, 0.72)
  Steps: 15

  Step  Position      Energy
  ─────────────────────────────
  0     (0.55, 0.72)  0.45
  5     (0.50, 0.68)  0.32
  10    (0.45, 0.65)  0.22
  15    (0.42, 0.65)  0.18 [converged]

  Destination: security_focus attractor

  [Flow trajectory displayed]
```

---

## 8. Output Formats

### 8.1 ASCII (Terminal)

Default for quick inspection.

```bash
nf plot topology --format ascii
```

### 8.2 SVG (Scalable)

```bash
nf plot topology --format svg --output topology.svg
```

### 8.3 Plotly (Interactive)

```bash
nf plot topology --3d --format plotly --output topology.html
```

### 8.4 JSON (Data Export)

```bash
nf plot topology --format json --output topology.json
```

Output:
```json
{
  "type": "topology",
  "field": "main",
  "energy_grid": [[...]],
  "contours": [...],
  "attractors": [...],
  "basins": [...]
}
```

---

## 9. Interactive Features (3D)

### 9.1 Plotly Interactions

| Action | Effect |
|--------|--------|
| Drag | Rotate view |
| Scroll | Zoom |
| Click attractor | Show details |
| Double-click | Reset view |
| Hover | Show coordinates/energy |

### 9.2 Annotations

```json
{
  "annotations": [
    {
      "x": 0.42, "y": 0.65, "z": 0.18,
      "text": "security_focus<br>C=0.82",
      "showarrow": true,
      "arrowhead": 2
    }
  ]
}
```

---

## 10. Examples

### 10.1 Simple Field

```bash
> nf inject "concept_a" 0.9
> nf inject "concept_b" 0.85
> nf cycle 3
> nf plot topology

[TOPOLOGY] Field: main

         Low ─────────────────── High
    1.0 │░░░░░░░░░░░░░░░░░░░░░░│
        │░░░▒▒▒▒▒▒░░░░░░░░░░░░░│
    0.8 │░░▒▒▓▓▓▓▒▒░░░░░░░░░░░░│
        │░░▒▓████▓▒░░░░░░░░░░░░│
    0.6 │░░▒▓██●█▓▒░░░░░░░░░░░░│
        │░░▒▓████▓▒░░░░░░░░░░░░│
    0.4 │░░▒▒▓▓▓▓▒▒░░░░░░░░░░░░│
        │░░░▒▒▒▒▒▒░░░░░░░░░░░░░│
    0.2 │░░░░░░░░░░░░░░░░░░░░░░│
        │░░░░░░░░░░░░░░░░░░░░░░│
    0.0 └──────────────────────┘

  ● concept_cluster (0.78)
```

### 10.2 Complex Multi-Attractor

```bash
> nf plot topology --style contour

[TOPOLOGY] Field: analysis

    ┌──────────────────────────────────────────┐
 1.0│  ·    ·    ·    ·    ·    ·    ·    ·   │
    │    ╭───────╮         ╭─────╮             │
 0.8│   ╭┤ perf  │╮       ╭┤ sec │╮           │
    │  ╭┤│ 0.72  ││╮     ╭┤│0.85 ││╮          │
 0.6│  │││  ●    │││     │││ ●   │││          │
    │  ╰┤│       ││╯     ╰┤│     ││╯          │
 0.4│   ╰┤       │╯       ╰┤     │╯           │
    │    ╰───────╯    ·    ╰─────╯    ·       │
 0.2│  ·    ·    ·    ·    ·    ·    ·    ·   │
    │       ·    ·    ·    ·    ·    ·        │
 0.0└──────────────────────────────────────────┘

  Attractors:
    ● security_focus (0.85) - dominant
    ● performance_concern (0.72) - secondary
```

---

## Related Documents

- `networks.md` - Resonance network visualization
- `dynamics-animation.md` - Animated evolution
- `generators/plotly.md` - Interactive 3D implementation
- `../commands/visualization.md` - Command reference
