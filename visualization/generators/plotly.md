# Plotly Interactive Plot Generator

Specifications for generating interactive 2D and 3D visualizations using Plotly.

---

## 1. Overview

Plotly generates interactive HTML visualizations with:
- Pan, zoom, rotate controls
- Hover tooltips
- Click interactions
- Animation playback
- Export capabilities

---

## 2. Supported Plot Types

| Type | Plotly Type | Use Case |
|------|-------------|----------|
| Network 2D | `scatter` + shapes | Resonance graphs |
| Network 3D | `scatter3d` | Semantic space |
| Surface | `surface` | Energy landscape |
| Heatmap | `heatmap` | Activation grids |
| Contour | `contour` | Topology |
| Animation | frames | Dynamics evolution |

---

## 3. Network Visualization (2D)

### 3.1 Basic Structure

```json
{
  "data": [
    {
      "type": "scatter",
      "mode": "markers+text",
      "x": [100, 200, 300],
      "y": [150, 150, 250],
      "text": ["sql_injection", "user_input", "validation"],
      "textposition": "top center",
      "marker": {
        "size": [35, 32, 28],
        "color": ["#e74c3c", "#e74c3c", "#f39c12"],
        "line": {"width": 2, "color": "#c0392b"}
      },
      "hovertemplate": "<b>%{text}</b><br>Activation: %{customdata}<extra></extra>",
      "customdata": [0.92, 0.88, 0.75]
    }
  ],
  "layout": {
    "title": "Resonance Network: main",
    "showlegend": false,
    "hovermode": "closest",
    "xaxis": {"visible": false},
    "yaxis": {"visible": false},
    "shapes": [
      {
        "type": "line",
        "x0": 100, "y0": 150,
        "x1": 200, "y1": 150,
        "line": {"color": "#27ae60", "width": 4}
      }
    ],
    "annotations": [
      {"x": 150, "y": 145, "text": "R=0.85", "showarrow": false}
    ]
  }
}
```

### 3.2 Generation Algorithm

```
GENERATE_PLOTLY_NETWORK(field, layout="force"):
  // Compute layout
  positions = compute_layout(field, layout)

  // Create node trace
  node_trace = {
    "type": "scatter",
    "mode": "markers+text",
    "x": [],
    "y": [],
    "text": [],
    "textposition": "top center",
    "marker": {
      "size": [],
      "color": [],
      "line": {"width": 2, "color": []}
    },
    "customdata": [],
    "hovertemplate": "<b>%{text}</b><br>Activation: %{customdata:.2f}<extra></extra>"
  }

  FOR pattern IN field.patterns:
    pos = positions[pattern.id]
    node_trace.x.append(pos.x)
    node_trace.y.append(pos.y)
    node_trace.text.append(pattern.name)
    node_trace.marker.size.append(10 + pattern.activation * 30)
    node_trace.marker.color.append(activation_to_color(pattern.activation))
    node_trace.customdata.append(pattern.activation)

  // Create edge shapes
  shapes = []
  annotations = []

  FOR (p1, p2), resonance IN field.resonance_cache:
    IF resonance > 0.2:
      pos1 = positions[p1]
      pos2 = positions[p2]

      shapes.append({
        "type": "line",
        "x0": pos1.x, "y0": pos1.y,
        "x1": pos2.x, "y1": pos2.y,
        "line": {
          "color": resonance_to_color(resonance),
          "width": 1 + resonance * 5
        }
      })

      // Add resonance label
      annotations.append({
        "x": (pos1.x + pos2.x) / 2,
        "y": (pos1.y + pos2.y) / 2 - 10,
        "text": f"R={resonance:.2f}",
        "showarrow": False,
        "font": {"size": 10}
      })

  // Attractor backgrounds
  FOR attractor IN field.attractors:
    shapes.insert(0, create_attractor_shape(attractor, positions))

  RETURN {
    "data": [node_trace],
    "layout": {
      "title": f"Resonance Network: {field.name}",
      "shapes": shapes,
      "annotations": annotations,
      "xaxis": {"visible": False},
      "yaxis": {"visible": False, "scaleanchor": "x"},
      "hovermode": "closest"
    }
  }
```

---

## 4. 3D Network Visualization

### 4.1 Scatter3D

```json
{
  "data": [
    {
      "type": "scatter3d",
      "mode": "markers+text",
      "x": [0.42, 0.38, 0.45],
      "y": [0.78, 0.72, 0.65],
      "z": [0.33, 0.41, 0.28],
      "text": ["sql_injection", "user_input", "validation"],
      "marker": {
        "size": [15, 14, 12],
        "color": [0.92, 0.88, 0.75],
        "colorscale": "RdYlGn_r",
        "cmin": 0,
        "cmax": 1,
        "colorbar": {"title": "Activation"}
      },
      "hovertemplate": "<b>%{text}</b><br>Position: (%{x:.2f}, %{y:.2f}, %{z:.2f})<br>Activation: %{marker.color:.2f}<extra></extra>"
    }
  ],
  "layout": {
    "title": "Semantic Space: main",
    "scene": {
      "xaxis": {"title": "Dimension 1"},
      "yaxis": {"title": "Dimension 2"},
      "zaxis": {"title": "Dimension 3"},
      "camera": {
        "eye": {"x": 1.5, "y": 1.5, "z": 1.5}
      }
    }
  }
}
```

### 4.2 3D Edges

```json
{
  "type": "scatter3d",
  "mode": "lines",
  "x": [0.42, 0.38, null, 0.42, 0.45],
  "y": [0.78, 0.72, null, 0.78, 0.65],
  "z": [0.33, 0.41, null, 0.33, 0.28],
  "line": {
    "color": "#27ae60",
    "width": 4
  },
  "hoverinfo": "skip"
}
```

---

## 5. Energy Surface (3D)

### 5.1 Surface Plot

```json
{
  "data": [
    {
      "type": "surface",
      "x": [0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0],
      "y": [0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0],
      "z": [[...]],
      "colorscale": "Viridis",
      "reversescale": true,
      "showscale": true,
      "colorbar": {"title": "Energy"},
      "contours": {
        "z": {"show": true, "usecolormap": true, "project": {"z": true}}
      }
    },
    {
      "type": "scatter3d",
      "mode": "markers+text",
      "x": [0.42],
      "y": [0.65],
      "z": [0.18],
      "text": ["security_focus"],
      "marker": {"size": 8, "color": "red"},
      "textposition": "top center"
    }
  ],
  "layout": {
    "title": "Energy Landscape",
    "scene": {
      "xaxis": {"title": "Semantic X"},
      "yaxis": {"title": "Semantic Y"},
      "zaxis": {"title": "Energy"}
    }
  }
}
```

### 5.2 Generation

```
GENERATE_ENERGY_SURFACE(field, resolution=50):
  // Compute energy grid
  x = linspace(0, 1, resolution)
  y = linspace(0, 1, resolution)
  z = zeros(resolution, resolution)

  FOR i IN range(resolution):
    FOR j IN range(resolution):
      z[i][j] = compute_energy(field, x[i], y[j])

  // Find attractor positions
  attractor_markers = []
  FOR attractor IN field.attractors:
    pos = attractor_center(attractor, field)
    energy = compute_energy(field, pos.x, pos.y)
    attractor_markers.append({
      "x": pos.x, "y": pos.y, "z": energy,
      "name": attractor.name
    })

  RETURN {
    "data": [
      {"type": "surface", "x": x, "y": y, "z": z, ...},
      {"type": "scatter3d", "mode": "markers+text", ...attractor_markers}
    ],
    "layout": {...}
  }
```

---

## 6. Heatmap

### 6.1 Activation Heatmap

```json
{
  "data": [
    {
      "type": "heatmap",
      "z": [[0.2, 0.4, 0.6], [0.5, 0.9, 0.7], [0.3, 0.6, 0.8]],
      "x": ["Pattern A", "Pattern B", "Pattern C"],
      "y": ["Cycle 1", "Cycle 2", "Cycle 3"],
      "colorscale": "RdYlGn",
      "reversescale": true,
      "colorbar": {"title": "Activation"}
    }
  ],
  "layout": {
    "title": "Activation Over Time",
    "xaxis": {"title": "Patterns"},
    "yaxis": {"title": "Cycles"}
  }
}
```

### 6.2 Resonance Matrix

```json
{
  "data": [
    {
      "type": "heatmap",
      "z": [[1.0, 0.85, 0.72], [0.85, 1.0, 0.65], [0.72, 0.65, 1.0]],
      "x": ["sql_inj", "user_input", "validation"],
      "y": ["sql_inj", "user_input", "validation"],
      "colorscale": "Greens",
      "zmin": 0,
      "zmax": 1,
      "text": [["1.00", "0.85", "0.72"], ...],
      "texttemplate": "%{text}",
      "hovertemplate": "R(%{x}, %{y}) = %{z:.2f}<extra></extra>"
    }
  ],
  "layout": {
    "title": "Resonance Matrix",
    "xaxis": {"title": "Pattern"},
    "yaxis": {"title": "Pattern", "autorange": "reversed"}
  }
}
```

---

## 7. Animation

### 7.1 Frame Structure

```json
{
  "data": [{...initial_state}],
  "layout": {
    "updatemenus": [{
      "type": "buttons",
      "showactive": false,
      "y": 0,
      "x": 0.1,
      "buttons": [
        {
          "label": "▶ Play",
          "method": "animate",
          "args": [null, {
            "frame": {"duration": 500, "redraw": true},
            "fromcurrent": true,
            "transition": {"duration": 300}
          }]
        },
        {
          "label": "⏸ Pause",
          "method": "animate",
          "args": [[null], {
            "frame": {"duration": 0, "redraw": false},
            "mode": "immediate",
            "transition": {"duration": 0}
          }]
        }
      ]
    }],
    "sliders": [{
      "active": 0,
      "yanchor": "top",
      "xanchor": "left",
      "currentvalue": {
        "prefix": "Cycle: ",
        "visible": true,
        "xanchor": "right"
      },
      "steps": [
        {"label": "0", "method": "animate", "args": [["frame0"], ...]},
        {"label": "1", "method": "animate", "args": [["frame1"], ...]},
        {"label": "2", "method": "animate", "args": [["frame2"], ...]}
      ]
    }]
  },
  "frames": [
    {"name": "frame0", "data": [{...state_at_cycle_0}]},
    {"name": "frame1", "data": [{...state_at_cycle_1}]},
    {"name": "frame2", "data": [{...state_at_cycle_2}]}
  ]
}
```

### 7.2 Animation Generation

```
GENERATE_ANIMATION(field, cycles):
  // Capture initial state
  initial = capture_plotly_state(field)

  // Generate frames
  frames = []
  slider_steps = []

  FOR cycle IN range(cycles + 1):
    frame_name = f"frame{cycle}"

    // Execute cycle (except for initial)
    IF cycle > 0:
      execute_cycle(field)

    // Capture state
    frame_data = capture_plotly_state(field)
    frames.append({
      "name": frame_name,
      "data": frame_data.data
    })

    slider_steps.append({
      "label": str(cycle),
      "method": "animate",
      "args": [[frame_name], {
        "frame": {"duration": 300, "redraw": True},
        "mode": "immediate",
        "transition": {"duration": 300}
      }]
    })

  RETURN {
    "data": initial.data,
    "layout": {
      ...initial.layout,
      "updatemenus": [play_pause_buttons],
      "sliders": [{"steps": slider_steps, ...}]
    },
    "frames": frames
  }
```

---

## 8. Interactive Features

### 8.1 Hover Templates

```
hovertemplate = """
<b>%{text}</b><br>
Activation: %{customdata[0]:.2f}<br>
Position: (%{x:.2f}, %{y:.2f})<br>
Tags: %{customdata[1]}
<extra></extra>
"""
```

### 8.2 Click Events

```javascript
// JavaScript for handling clicks
plot.on('plotly_click', function(data) {
    var point = data.points[0];
    var patternName = point.text;
    showPatternDetails(patternName);
});
```

### 8.3 Selection

```json
{
  "layout": {
    "dragmode": "select",
    "selectdirection": "any"
  }
}
```

---

## 9. Export Options

### 9.1 HTML Standalone

```python
def export_html(fig, filename):
    fig.write_html(filename, include_plotlyjs='cdn')
```

### 9.2 HTML with Embedded JS

```python
def export_html_offline(fig, filename):
    fig.write_html(filename, include_plotlyjs=True)
```

### 9.3 Static Image

```python
def export_image(fig, filename, format='png', scale=2):
    fig.write_image(filename, format=format, scale=scale)
```

### 9.4 JSON Data

```python
def export_json(fig, filename):
    with open(filename, 'w') as f:
        f.write(fig.to_json())
```

---

## 10. Command Examples

```bash
# Interactive network
> nf plot network --format plotly --output network.html
[EXPORT] Saved interactive plot to network.html

# 3D semantic space
> nf plot network --3d --format plotly --output space.html
[EXPORT] Saved 3D visualization to space.html

# Energy surface
> nf plot topology --3d --format plotly --output energy.html
[EXPORT] Saved energy landscape to energy.html

# Animation
> nf animate dynamics --cycles 10 --format plotly --output evolution.html
[EXPORT] Saved animated visualization to evolution.html
```

---

## 11. API

```python
class PlotlyGenerator:
    def __init__(self):
        self.fig = None

    def network_2d(self, field, layout="force", threshold=0.2):
        """Generate 2D network scatter plot"""

    def network_3d(self, field, show_edges=True):
        """Generate 3D semantic space visualization"""

    def energy_surface(self, field, resolution=50):
        """Generate 3D energy landscape"""

    def heatmap(self, data, labels_x, labels_y, title):
        """Generate heatmap"""

    def resonance_matrix(self, field):
        """Generate resonance correlation matrix"""

    def animation(self, field, cycles, fps=2):
        """Generate animated evolution"""

    def export_html(self, filename, offline=False):
        """Export to HTML file"""

    def export_image(self, filename, format='png', scale=2):
        """Export to static image"""
```

---

## Related Documents

- `ascii.md` - ASCII generation
- `svg.md` - SVG generation
- `mermaid.md` - Mermaid diagrams
- `../topology.md` - Topology visualization
- `../dynamics-animation.md` - Animation specs
