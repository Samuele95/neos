# ASCII Art Generator

Specifications for generating ASCII visualizations for terminal display.

---

## 1. Overview

ASCII generation produces text-based visualizations that work in any terminal without external dependencies.

---

## 2. Character Sets

### 2.1 Box Drawing

```
Standard Box:
  ┌───┐
  │   │
  └───┘

Double Box:
  ╔═══╗
  ║   ║
  ╚═══╝

Rounded:
  ╭───╮
  │   │
  ╰───╯
```

### 2.2 Lines and Connectors

```
Lines:
  Horizontal: ─ ═ ━ ╌ ╍ ┄ ┅
  Vertical:   │ ║ ┃ ╎ ╏ ┆ ┇

Corners:
  ┌ ┐ └ ┘  (standard)
  ╔ ╗ ╚ ╝  (double)
  ╭ ╮ ╰ ╯  (rounded)

T-Junctions:
  ├ ┤ ┬ ┴ ┼  (standard)
  ╠ ╣ ╦ ╩ ╬  (double)

Arrows:
  → ← ↑ ↓ ↔ ↕
  ▶ ◀ ▲ ▼
```

### 2.3 Shading Blocks

```
Full blocks:   █ ▓ ▒ ░
Half blocks:   ▄ ▀ ▌ ▐
Quarter:       ▖ ▗ ▘ ▝
```

### 2.4 Node Shapes

```
Circle:    ●  ○  ◉  ◎
Square:    ■  □  ▪  ▫
Diamond:   ◆  ◇
Triangle:  ▲  △  ▼  ▽
```

---

## 3. Network Generation

### 3.1 Simple Network

```
GENERATE_ASCII_NETWORK(nodes, edges, width=60, height=20):
  canvas = create_canvas(width, height, fill=' ')

  // Position nodes
  positions = layout_nodes(nodes, width, height)

  // Draw edges first (under nodes)
  FOR edge IN edges:
    p1 = positions[edge.source]
    p2 = positions[edge.target]
    draw_line(canvas, p1, p2, style=edge_style(edge.weight))

  // Draw nodes
  FOR node IN nodes:
    pos = positions[node.id]
    draw_node(canvas, pos, node)

  RETURN canvas.to_string()
```

### 3.2 Edge Styles

```
EDGE_STYLE(weight):
  IF weight > 0.7:
    RETURN "═══"  // Strong
  ELSE IF weight > 0.4:
    RETURN "───"  // Moderate
  ELSE IF weight > 0.2:
    RETURN "╌╌╌"  // Weak
  ELSE:
    RETURN "···"  // Very weak
```

### 3.3 Node Drawing

```
DRAW_NODE(canvas, pos, node):
  // Calculate box size based on activation
  width = 6 + len(node.name)
  height = 3

  // Draw box
  draw_box(canvas, pos.x - width/2, pos.y - 1, width, height)

  // Draw name and activation
  draw_text(canvas, pos.x, pos.y, node.name)
  draw_text(canvas, pos.x, pos.y + 1, format_activation(node.activation))
```

### 3.4 Output Example

```
[NETWORK] Field: main

    ┌──────────┐             ┌─────────┐
    │ security │═════════════│  input  │
    │   0.92   │   R=0.85    │  0.88   │
    └────┬─────┘             └────┬────┘
         │                        │
         │   ╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌│
         │         R=0.35        │
         │                        │
    ┌────┴─────┐             ┌────┴────┐
    │  valid   │─────────────│ logging │
    │   0.75   │   R=0.45    │  0.35   │
    └──────────┘             └─────────┘
```

---

## 4. Heatmap Generation

### 4.1 Value to Character

```
VALUE_TO_CHAR(value, scale="standard"):
  IF scale == "standard":
    chars = " ░▒▓█"
  ELSE IF scale == "dots":
    chars = " ·∙●◉"
  ELSE IF scale == "blocks":
    chars = " ▁▂▃▄▅▆▇█"

  index = floor(value * (len(chars) - 1))
  RETURN chars[index]
```

### 4.2 Grid Heatmap

```
GENERATE_HEATMAP(grid, width=40, height=20):
  canvas = []

  FOR row IN range(height):
    line = ""
    FOR col IN range(width):
      // Sample grid at this position
      value = sample_grid(grid, col/width, row/height)
      char = VALUE_TO_CHAR(value)
      line += char
    canvas.append(line)

  RETURN "\n".join(canvas)
```

### 4.3 Output Example

```
[HEATMAP] Energy distribution

   0.0       0.5       1.0
    ├─────────┼─────────┤
1.0 │░░░░░░░░░░░░░░░░░░░│
    │░░░▒▒▒▒▒▒░░░░░░░░░░│
0.8 │░░▒▓▓▓▓▓▓▒░░░░░▒▒░░│
    │░▒▓██████▓▒░░░▒▓▓▒░│
0.6 │░▒▓████●█▓▒░░▒▓██▓▒│
    │░▒▓██████▓▒░░░▒▓▓▒░│
0.4 │░░▒▓▓▓▓▓▓▒░░░░░▒▒░░│
    │░░░▒▒▒▒▒▒░░░░░░░░░░│
0.2 │░░░░░░░░░░░░░░░░░░░│
    │░░░░░░░░░░░░░░░░░░░│
0.0 └───────────────────┘

● Attractor: security_focus (0.82)
```

---

## 5. Bar Charts

### 5.1 Horizontal Bars

```
GENERATE_BAR_CHART(data, width=40):
  output = []
  max_val = max(data.values())

  FOR name, value IN data.items():
    bar_width = int(value / max_val * width)
    bar = "█" * bar_width
    output.append(f"{name:15} │{bar} {value:.2f}")

  RETURN "\n".join(output)
```

### 5.2 Output Example

```
[ACTIVATION] Pattern strengths

sql_injection   │████████████████████████████████████ 0.92
user_input      │█████████████████████████████████  0.88
validation      │████████████████████████████     0.75
logging         │██████████████                   0.35
                └────────────────────────────────────
                0.0              0.5              1.0
```

### 5.3 Vertical Bars

```
[ENERGY] Distribution

   ██
   ██ ██
   ██ ██
   ██ ██ ██
   ██ ██ ██
   ██ ██ ██ ░░
  ─────────────
   sq  us  va  lo
   l   er  li  gg
```

---

## 6. Topology Visualization

### 6.1 Contour Lines

```
DRAW_CONTOUR(canvas, contour, char='─'):
  FOR point IN contour.points:
    x = scale_x(point.x)
    y = scale_y(point.y)
    canvas[y][x] = char
```

### 6.2 Basin Regions

```
DRAW_BASIN(canvas, basin, char):
  FOR y IN range(canvas.height):
    FOR x IN range(canvas.width):
      IF point_in_basin(x, y, basin):
        IF canvas[y][x] == ' ':
          canvas[y][x] = char
```

### 6.3 Output Example

```
[TOPOLOGY] Attractor landscape

    ┌────────────────────────────────────────────┐
 1.0│  ·    ·    ·    ·    ·    ·    ·    ·     │
    │     ╭─────────────╮           ╭───╮       │
 0.8│   ╭─┤    Basin    ├─╮       ╭─┤ B │       │
    │  ╭┤ │      A      │ ├╮     ╭┤ │   │       │
 0.6│  ││ │     ●       │ ││     ││ │ ● │       │
    │  ╰┤ │   (0.82)    │ ├╯     ╰┤ │   │       │
 0.4│   ╰─┤             ├─╯       ╰─┤   │       │
    │     ╰─────────────╯           ╰───╯       │
 0.2│  ·    ·    ·    ·    ·    ·    ·    ·     │
 0.0└────────────────────────────────────────────┘

Legend: ● Attractor   ─ Basin boundary   · High energy
```

---

## 7. Animation Frames

### 7.1 Frame Template

```
╔══════════════════════════════════════════════════════════════╗
║ NFOS Animation                    Cycle: {cycle}/{total}     ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  {visualization content}                                     ║
║                                                              ║
╠══════════════════════════════════════════════════════════════╣
║ Coherence: {coherence}  Patterns: {patterns}  {status}       ║
╠══════════════════════════════════════════════════════════════╣
║ [Space] Play/Pause  [←→] Step  [Q] Quit                     ║
╚══════════════════════════════════════════════════════════════╝
```

### 7.2 Clear and Redraw

```
ANIMATE_TERMINAL(frames, fps=10):
  delay = 1.0 / fps

  FOR frame IN frames:
    clear_screen()  // ANSI: \033[2J\033[H
    print(render_frame(frame))
    sleep(delay)

    // Check for input
    IF key_pressed():
      handle_input(get_key())
```

---

## 8. Color Support (ANSI)

### 8.1 Color Codes

```
ANSI_COLORS = {
  "red": "\033[31m",
  "green": "\033[32m",
  "yellow": "\033[33m",
  "blue": "\033[34m",
  "magenta": "\033[35m",
  "cyan": "\033[36m",
  "white": "\033[37m",
  "reset": "\033[0m",
  "bold": "\033[1m"
}
```

### 8.2 Colored Output

```
COLORIZE(text, color):
  RETURN ANSI_COLORS[color] + text + ANSI_COLORS["reset"]

// Usage
print(COLORIZE("●", "red") + " High activation")
print(COLORIZE("●", "yellow") + " Medium activation")
print(COLORIZE("●", "green") + " Low activation")
```

### 8.3 Colored Network

```
[NETWORK] Field: main (colored)

    ┌──────────┐             ┌─────────┐
    │ security │══[green]═══│  input  │
    │  [red]●  │   R=0.85    │ [red]●  │
    └──────────┘             └─────────┘
```

---

## 9. Responsive Layout

### 9.1 Terminal Size Detection

```
GET_TERMINAL_SIZE():
  // Try environment
  cols = os.environ.get('COLUMNS', 80)
  rows = os.environ.get('LINES', 24)

  // Try ioctl
  TRY:
    size = os.get_terminal_size()
    cols, rows = size.columns, size.lines

  RETURN (cols, rows)
```

### 9.2 Adaptive Layout

```
GENERATE_ADAPTIVE(viz_type, data):
  width, height = GET_TERMINAL_SIZE()

  IF width < 40:
    RETURN generate_minimal(data)
  ELSE IF width < 80:
    RETURN generate_compact(data, width, height)
  ELSE:
    RETURN generate_full(data, width, height)
```

---

## 10. API

```python
class ASCIIGenerator:
    def __init__(self, width=80, height=24, color=True):
        self.width = width
        self.height = height
        self.color = color

    def network(self, field, threshold=0.2):
        """Generate network visualization"""

    def heatmap(self, grid, legend=True):
        """Generate heatmap visualization"""

    def bar_chart(self, data, orientation="horizontal"):
        """Generate bar chart"""

    def topology(self, field, style="contour"):
        """Generate topology visualization"""

    def animation_frame(self, frame, template="standard"):
        """Generate single animation frame"""
```

---

## Related Documents

- `svg.md` - SVG generation
- `mermaid.md` - Mermaid diagrams
- `../networks.md` - Network visualization
- `../topology.md` - Topology visualization
