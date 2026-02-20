# SVG Generator

Specifications for generating Scalable Vector Graphics visualizations.

---

## 1. Overview

SVG generation produces high-quality, scalable visualizations suitable for documentation, export, and web display.

---

## 2. Document Structure

### 2.1 Base Template

```svg
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg"
     viewBox="0 0 {width} {height}"
     width="{width}" height="{height}">

  <defs>
    <!-- Gradients, filters, markers -->
  </defs>

  <style>
    /* CSS styles */
  </style>

  <!-- Content -->
  <g class="visualization">
    <!-- Generated content here -->
  </g>

  <!-- Legend -->
  <g class="legend" transform="translate(10, {height-100})">
    <!-- Legend items -->
  </g>

  <!-- Title -->
  <text class="title" x="{width/2}" y="30" text-anchor="middle">
    {title}
  </text>
</svg>
```

### 2.2 Standard Dimensions

| Type | Width | Height | ViewBox |
|------|-------|--------|---------|
| Network | 600 | 400 | 0 0 600 400 |
| Topology | 500 | 400 | 0 0 500 400 |
| Heatmap | 400 | 400 | 0 0 400 400 |
| Timeline | 800 | 200 | 0 0 800 200 |

---

## 3. Definitions (defs)

### 3.1 Gradients

```svg
<defs>
  <!-- Activation gradient (low to high) -->
  <linearGradient id="activation-gradient" x1="0%" y1="100%" x2="0%" y2="0%">
    <stop offset="0%" style="stop-color:#95a5a6"/>
    <stop offset="50%" style="stop-color:#f39c12"/>
    <stop offset="100%" style="stop-color:#e74c3c"/>
  </linearGradient>

  <!-- Energy landscape gradient -->
  <linearGradient id="energy-gradient" x1="0%" y1="0%" x2="0%" y2="100%">
    <stop offset="0%" style="stop-color:#e74c3c"/>
    <stop offset="30%" style="stop-color:#f39c12"/>
    <stop offset="60%" style="stop-color:#f1c40f"/>
    <stop offset="100%" style="stop-color:#27ae60"/>
  </linearGradient>

  <!-- Basin fill -->
  <radialGradient id="basin-gradient" cx="50%" cy="50%" r="50%">
    <stop offset="0%" style="stop-color:#27ae60;stop-opacity:0.4"/>
    <stop offset="100%" style="stop-color:#27ae60;stop-opacity:0.1"/>
  </radialGradient>
</defs>
```

### 3.2 Filters

```svg
<defs>
  <!-- Glow effect for attractors -->
  <filter id="glow" x="-50%" y="-50%" width="200%" height="200%">
    <feGaussianBlur stdDeviation="3" result="blur"/>
    <feMerge>
      <feMergeNode in="blur"/>
      <feMergeNode in="SourceGraphic"/>
    </feMerge>
  </filter>

  <!-- Shadow for nodes -->
  <filter id="shadow" x="-20%" y="-20%" width="140%" height="140%">
    <feDropShadow dx="2" dy="2" stdDeviation="2" flood-opacity="0.3"/>
  </filter>

  <!-- Blur for background -->
  <filter id="blur">
    <feGaussianBlur stdDeviation="5"/>
  </filter>
</defs>
```

### 3.3 Markers

```svg
<defs>
  <!-- Arrow marker for directed edges -->
  <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5"
          markerWidth="6" markerHeight="6" orient="auto">
    <path d="M 0 0 L 10 5 L 0 10 z" fill="#666"/>
  </marker>

  <!-- Resonance dot -->
  <marker id="resonance-dot" viewBox="0 0 10 10" refX="5" refY="5"
          markerWidth="4" markerHeight="4">
    <circle cx="5" cy="5" r="4" fill="#27ae60"/>
  </marker>
</defs>
```

---

## 4. Styles

### 4.1 CSS Stylesheet

```svg
<style>
  /* Typography */
  .title { font: bold 16px sans-serif; fill: #2c3e50; }
  .label { font: 12px sans-serif; fill: #34495e; }
  .value { font: 10px monospace; fill: #7f8c8d; }

  /* Nodes */
  .node { stroke-width: 2; }
  .node-high { fill: #e74c3c; stroke: #c0392b; }
  .node-medium { fill: #f39c12; stroke: #d68910; }
  .node-low { fill: #95a5a6; stroke: #7f8c8d; }

  /* Edges */
  .edge { fill: none; }
  .edge-strong { stroke: #27ae60; stroke-width: 4; }
  .edge-moderate { stroke: #95a5a6; stroke-width: 2; }
  .edge-weak { stroke: #bdc3c7; stroke-width: 1; stroke-dasharray: 4,4; }
  .edge-tension { stroke: #e74c3c; stroke-width: 2; stroke-dasharray: 2,2; }

  /* Attractors */
  .attractor-boundary { fill: none; stroke: #3498db; stroke-width: 2; stroke-dasharray: 5,5; }
  .attractor-fill { fill: #e8f4f8; opacity: 0.5; }

  /* Interactive */
  .node:hover { filter: url(#glow); cursor: pointer; }
  .edge:hover { stroke-width: 6; }
</style>
```

---

## 5. Network Generation

### 5.1 Node Element

```svg
<g class="node" transform="translate({x}, {y})">
  <!-- Background circle -->
  <circle r="{radius}" class="node-{class}" filter="url(#shadow)"/>

  <!-- Label -->
  <text class="label" y="-{radius+5}" text-anchor="middle">{name}</text>

  <!-- Activation value -->
  <text class="value" y="4" text-anchor="middle">{activation}</text>
</g>
```

### 5.2 Edge Element

```svg
<g class="edge-group">
  <!-- Edge line -->
  <line class="edge edge-{strength}"
        x1="{x1}" y1="{y1}" x2="{x2}" y2="{y2}"/>

  <!-- Resonance label (optional) -->
  <text class="value" x="{midX}" y="{midY-5}" text-anchor="middle">
    R={resonance}
  </text>
</g>
```

### 5.3 Curved Edges

```svg
<!-- Quadratic curve for overlapping edges -->
<path class="edge edge-{strength}"
      d="M {x1},{y1} Q {ctrlX},{ctrlY} {x2},{y2}"/>

<!-- Bezier for complex paths -->
<path class="edge edge-{strength}"
      d="M {x1},{y1} C {ctrl1X},{ctrl1Y} {ctrl2X},{ctrl2Y} {x2},{y2}"/>
```

### 5.4 Complete Network Example

```svg
<svg viewBox="0 0 600 400" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <filter id="glow">...</filter>
    <marker id="arrow">...</marker>
  </defs>

  <style>...</style>

  <!-- Title -->
  <text class="title" x="300" y="25" text-anchor="middle">
    Resonance Network: main
  </text>

  <!-- Attractor group -->
  <g class="attractor">
    <rect x="80" y="60" width="280" height="200" rx="20"
          class="attractor-fill attractor-boundary"/>
    <text x="220" y="55" class="label" text-anchor="middle">
      security_focus (C=0.82)
    </text>
  </g>

  <!-- Edges -->
  <line class="edge edge-strong" x1="150" y1="150" x2="280" y2="150"/>
  <text class="value" x="215" y="140">R=0.85</text>

  <line class="edge edge-strong" x1="150" y1="150" x2="215" y2="220"/>
  <line class="edge edge-moderate" x1="280" y1="150" x2="215" y2="220"/>

  <!-- Weak external connection -->
  <line class="edge edge-weak" x1="150" y1="150" x2="480" y2="180"/>

  <!-- Nodes -->
  <g class="node" transform="translate(150, 150)">
    <circle r="30" class="node-high"/>
    <text class="label" y="-35" text-anchor="middle">sql_injection</text>
    <text class="value" y="4" text-anchor="middle">0.92</text>
  </g>

  <g class="node" transform="translate(280, 150)">
    <circle r="28" class="node-high"/>
    <text class="label" y="-33" text-anchor="middle">user_input</text>
    <text class="value" y="4" text-anchor="middle">0.88</text>
  </g>

  <g class="node" transform="translate(215, 220)">
    <circle r="24" class="node-medium"/>
    <text class="label" y="-29" text-anchor="middle">validation</text>
    <text class="value" y="4" text-anchor="middle">0.75</text>
  </g>

  <g class="node" transform="translate(480, 180)">
    <circle r="16" class="node-low"/>
    <text class="label" y="-21" text-anchor="middle">logging</text>
    <text class="value" y="4" text-anchor="middle">0.35</text>
  </g>

  <!-- Legend -->
  <g transform="translate(450, 320)">
    <text class="label" y="0">Legend:</text>
    <line x1="0" y1="15" x2="30" y2="15" class="edge edge-strong"/>
    <text class="value" x="35" y="18">Strong (R>0.7)</text>
    <line x1="0" y1="30" x2="30" y2="30" class="edge edge-moderate"/>
    <text class="value" x="35" y="33">Moderate</text>
    <line x1="0" y1="45" x2="30" y2="45" class="edge edge-weak"/>
    <text class="value" x="35" y="48">Weak</text>
  </g>
</svg>
```

---

## 6. Topology Generation

### 6.1 Contour Paths

```svg
<!-- Energy contours -->
<g class="contours">
  <path d="M 50,200 Q 100,150 200,180 T 350,200"
        fill="none" stroke="#bdc3c7" stroke-width="1"/>
  <path d="M 80,180 Q 120,140 200,160 T 320,180"
        fill="none" stroke="#95a5a6" stroke-width="1"/>
  <path d="M 110,160 Q 140,130 200,140 T 290,160"
        fill="none" stroke="#7f8c8d" stroke-width="1"/>
</g>
```

### 6.2 Basin Regions

```svg
<!-- Basin of attraction -->
<ellipse cx="200" cy="150" rx="100" ry="70"
         fill="url(#basin-gradient)" stroke="none"/>

<!-- Basin boundary -->
<ellipse cx="200" cy="150" rx="100" ry="70"
         fill="none" stroke="#27ae60" stroke-width="2" stroke-dasharray="5,5"/>
```

### 6.3 Attractor Points

```svg
<g class="attractor-point" filter="url(#glow)">
  <circle cx="200" cy="150" r="10" fill="#c0392b"/>
  <text x="200" y="175" class="label" text-anchor="middle">
    security_focus
  </text>
  <text x="200" y="190" class="value" text-anchor="middle">
    C=0.82
  </text>
</g>
```

---

## 7. Heatmap Generation

### 7.1 Grid Cells

```svg
<g class="heatmap">
  <rect x="0" y="0" width="20" height="20" fill="#27ae60"/>
  <rect x="20" y="0" width="20" height="20" fill="#2ecc71"/>
  <rect x="40" y="0" width="20" height="20" fill="#f1c40f"/>
  <!-- ... more cells ... -->
</g>
```

### 7.2 Color Scale

```
VALUE_TO_COLOR(value):
  // Green (low) to Red (high)
  colors = [
    [39, 174, 96],   // #27ae60
    [241, 196, 15],  // #f1c40f
    [230, 126, 34],  // #e67e22
    [231, 76, 60]    // #e74c3c
  ]

  index = value * (len(colors) - 1)
  i = floor(index)
  t = index - i

  r = lerp(colors[i][0], colors[i+1][0], t)
  g = lerp(colors[i][1], colors[i+1][1], t)
  b = lerp(colors[i][2], colors[i+1][2], t)

  RETURN f"rgb({r},{g},{b})"
```

---

## 8. Animation (SMIL)

### 8.1 Node Pulse

```svg
<circle cx="200" cy="150" r="30" fill="#e74c3c">
  <animate attributeName="r"
           values="30;35;30"
           dur="2s"
           repeatCount="indefinite"/>
  <animate attributeName="fill-opacity"
           values="1;0.8;1"
           dur="2s"
           repeatCount="indefinite"/>
</circle>
```

### 8.2 Edge Flow

```svg
<line x1="100" y1="100" x2="300" y2="100" stroke="#27ae60" stroke-width="3">
  <animate attributeName="stroke-dasharray"
           values="0,1000;100,900;200,800"
           dur="1s"
           repeatCount="indefinite"/>
</line>
```

### 8.3 Transform Animation

```svg
<g>
  <animateTransform attributeName="transform"
                    type="scale"
                    values="1;1.1;1"
                    dur="0.5s"
                    repeatCount="indefinite"/>
  <circle r="20" fill="#e74c3c"/>
</g>
```

---

## 9. API

```python
class SVGGenerator:
    def __init__(self, width=600, height=400):
        self.width = width
        self.height = height
        self.defs = []
        self.styles = []
        self.elements = []

    def add_gradient(self, id, colors, direction="vertical"):
        """Add gradient definition"""

    def add_filter(self, id, filter_type, **params):
        """Add filter definition"""

    def add_node(self, x, y, radius, label, activation, **style):
        """Add network node"""

    def add_edge(self, x1, y1, x2, y2, weight, **style):
        """Add edge between nodes"""

    def add_contour(self, points, level):
        """Add topology contour"""

    def add_basin(self, center, size, attractor_name):
        """Add basin of attraction"""

    def render(self):
        """Generate complete SVG string"""

    def save(self, filename):
        """Save to file"""
```

---

## 10. Export Options

```bash
# Basic SVG
nf plot network --format svg --output network.svg

# With options
nf plot network --format svg --output network.svg \
    --width 800 --height 600 \
    --style dark \
    --animate
```

---

## Related Documents

- `ascii.md` - ASCII generation
- `mermaid.md` - Mermaid diagrams
- `plotly.md` - Interactive plots
- `../networks.md` - Network visualization
