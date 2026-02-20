# Dynamics Animation Specification

Specifications for animating field evolution: activation changes, resonance dynamics, attractor emergence, and state transitions.

---

## 1. Overview

Animation visualizes field dynamics over time:
- **Activation changes**: Patterns growing/shrinking
- **Resonance pulses**: Energy flowing between patterns
- **Cluster formation**: Attractors emerging
- **State transitions**: Field reorganization

---

## 2. Animation Types

### 2.1 Field Evolution

Shows how the activation landscape changes over cycles.

```
nf animate dynamics [--cycles n] [--fps f]
```

### 2.2 Network Evolution

Shows resonance network forming and strengthening.

```
nf animate network [--cycles n]
```

### 2.3 Attractor Emergence

Focused animation showing attractor formation.

```
nf animate attractor [--name <name>]
```

### 2.4 Flow Animation

Shows trajectories through semantic space.

```
nf animate flow [--from @pattern]
```

---

## 3. Frame Generation

### 3.1 Frame Structure

Each frame captures a snapshot of field state:

```json
{
  "frame": 0,
  "cycle": 0,
  "timestamp": 0.0,
  "state": {
    "patterns": {
      "p_001": {"activation": 0.9, "position": [0.4, 0.6]},
      "p_002": {"activation": 0.85, "position": [0.5, 0.5]}
    },
    "resonances": {
      "p_001:p_002": 0.72
    },
    "coherence": 0.65,
    "attractors": []
  },
  "events": [
    {"type": "inject", "pattern": "p_001", "strength": 0.9}
  ]
}
```

### 3.2 Interpolation

Between cycles, interpolate for smooth animation:

```
INTERPOLATE_FRAMES(frame_a, frame_b, steps=10):
  frames = []

  FOR i IN range(steps):
    t = i / steps
    frame = {
      "frame": frame_a.frame + t,
      "cycle": frame_a.cycle,
      "patterns": {}
    }

    FOR pattern_id IN frame_a.patterns:
      a_val = frame_a.patterns[pattern_id].activation
      b_val = frame_b.patterns[pattern_id].activation
      frame.patterns[pattern_id] = {
        "activation": lerp(a_val, b_val, t),
        "position": lerp(frame_a.patterns[pattern_id].position,
                        frame_b.patterns[pattern_id].position, t)
      }

    frames.append(frame)

  RETURN frames
```

### 3.3 Easing Functions

```
EASE_IN_OUT(t):
  RETURN t * t * (3 - 2 * t)

EASE_ELASTIC(t):
  RETURN pow(2, -10 * t) * sin((t - 0.1) * 5 * PI) + 1

LERP(a, b, t):
  RETURN a + (b - a) * t
```

---

## 4. Visual Elements

### 4.1 Pattern Visualization

**Activation as size:**
```
radius = base_radius + activation * scale_factor
```

**Activation as color:**
```
color = interpolate_color(low_color, high_color, activation)
```

**Pulse effect for changes:**
```
IF activation_increased:
  apply_glow_effect(pattern, intensity=delta * 2)
```

### 4.2 Resonance Visualization

**Edge thickness animation:**
```
thickness = base_thickness + resonance * max_thickness
```

**Energy pulse along edge:**
```
ANIMATE_PULSE(edge, duration=0.5):
  particle = create_particle(edge.source.position)
  animate(particle, to=edge.target.position, duration=duration)
  on_arrival: flash(edge.target)
```

### 4.3 Attractor Visualization

**Basin forming:**
```
ANIMATE_BASIN(attractor):
  // Start with small highlight
  highlight = create_circle(attractor.center, radius=0)

  // Expand to basin size
  animate(highlight.radius, to=basin_radius, duration=1.0)

  // Fade to subtle background
  animate(highlight.opacity, to=0.2, duration=0.5)
```

**Attractor label emergence:**
```
SHOW_ATTRACTOR_LABEL(attractor):
  label = create_text(attractor.name)
  label.opacity = 0
  animate(label.opacity, to=1.0, duration=0.3)
  animate(label.scale, from=0.5, to=1.0, duration=0.3)
```

---

## 5. Animation Scripts

### 5.1 Dynamics Evolution Script

```
GENERATE_DYNAMICS_ANIMATION(field, cycles, fps=30):
  frames = []
  events_log = []

  // Capture initial state
  frames.append(capture_frame(field, cycle=0))

  FOR cycle IN range(1, cycles + 1):
    // Execute cycle phases
    decay_events = execute_decay(field)
    events_log.extend(decay_events)

    resonance_events = execute_resonance(field)
    events_log.extend(resonance_events)

    dynamics_events = execute_dynamics(field)
    events_log.extend(dynamics_events)

    attractor_events = check_attractors(field)
    events_log.extend(attractor_events)

    // Capture end-of-cycle state
    frame = capture_frame(field, cycle)
    frame.events = [e for e in events_log if e.cycle == cycle]
    frames.append(frame)

  // Interpolate between key frames
  smooth_frames = []
  FOR i IN range(len(frames) - 1):
    smooth_frames.extend(
      interpolate_frames(frames[i], frames[i+1], steps=fps)
    )

  RETURN Animation(frames=smooth_frames, fps=fps)
```

### 5.2 Event Types

| Event | Visual Effect |
|-------|---------------|
| `decay` | Pattern shrinks, dims slightly |
| `resonance_compute` | Edge appears/updates |
| `amplify` | Pattern grows, glows |
| `attenuate` | Pattern shrinks, fades |
| `attractor_emerge` | Basin highlight, label appears |
| `attractor_dissolve` | Basin fades, label disappears |

---

## 6. Output Formats

### 6.1 ASCII Animation (Terminal)

Sequential frames with clear screen:

```
[ANIMATION] Dynamics evolution (5 cycles)

╔══════════════════════════════════════════════════════════════╗
║ Cycle: 1/5                          Coherence: 0.45          ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║    ┌────────┐         ┌────────┐                            ║
║    │security│─────────│ input  │                            ║
║    │  0.86  │  R=0.72 │  0.81  │                            ║
║    └────────┘         └────────┘                            ║
║         │                 │                                  ║
║         └────────┬────────┘                                  ║
║              ┌───┴───┐                                       ║
║              │ valid │                                       ║
║              │ 0.68  │                                       ║
║              └───────┘                                       ║
║                                                              ║
╠══════════════════════════════════════════════════════════════╣
║ [Space] Pause  [←/→] Step  [Q] Quit                         ║
╚══════════════════════════════════════════════════════════════╝
```

### 6.2 GIF Animation

```
GENERATE_GIF(frames, output_path, fps=10):
  images = []

  FOR frame IN frames:
    img = render_frame_to_image(frame)
    images.append(img)

  save_gif(images, output_path, duration=1000/fps)
```

### 6.3 SVG Animation (SMIL)

```svg
<svg viewBox="0 0 500 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Pattern node with animation -->
  <circle cx="200" cy="150" fill="#e74c3c">
    <animate attributeName="r"
             values="30;35;38;35;30"
             keyTimes="0;0.25;0.5;0.75;1"
             dur="2s"
             repeatCount="indefinite"/>
    <animate attributeName="fill-opacity"
             values="0.8;1;1;0.9;0.8"
             dur="2s"
             repeatCount="indefinite"/>
  </circle>

  <!-- Resonance edge with pulse -->
  <line x1="200" y1="150" x2="300" y2="150" stroke="#27ae60" stroke-width="3">
    <animate attributeName="stroke-width"
             values="3;5;3"
             dur="1s"
             repeatCount="indefinite"/>
  </line>

  <!-- Energy pulse particle -->
  <circle r="5" fill="#f39c12">
    <animateMotion dur="0.5s" repeatCount="indefinite">
      <mpath href="#edge-path"/>
    </animateMotion>
  </circle>
</svg>
```

### 6.4 Plotly Animation

```json
{
  "data": [{
    "type": "scatter",
    "mode": "markers",
    "x": [200, 300, 250],
    "y": [150, 150, 250],
    "marker": {"size": [30, 28, 25]}
  }],
  "layout": {
    "updatemenus": [{
      "type": "buttons",
      "buttons": [
        {"label": "Play", "method": "animate",
         "args": [null, {"frame": {"duration": 500}}]},
        {"label": "Pause", "method": "animate",
         "args": [[null], {"mode": "immediate"}]}
      ]
    }],
    "sliders": [{
      "steps": [
        {"label": "Cycle 0", "method": "animate", "args": [["frame0"]]},
        {"label": "Cycle 1", "method": "animate", "args": [["frame1"]]},
        {"label": "Cycle 2", "method": "animate", "args": [["frame2"]]}
      ]
    }]
  },
  "frames": [
    {"name": "frame0", "data": [{"marker": {"size": [30, 28, 25]}}]},
    {"name": "frame1", "data": [{"marker": {"size": [35, 32, 28]}}]},
    {"name": "frame2", "data": [{"marker": {"size": [38, 35, 30]}}]}
  ]
}
```

### 6.5 Video Export

```
nf animate dynamics --cycles 10 --output evolution.mp4 --fps 30

[EXPORT] Generating video...
  Frames: 300 (10 cycles × 30 fps)
  Resolution: 1920x1080
  Codec: H.264

  Progress: [████████████████████] 100%
  Saved: evolution.mp4 (2.4 MB)
```

---

## 7. Playback Controls

### 7.1 Terminal Controls

| Key | Action |
|-----|--------|
| `Space` | Play/Pause |
| `←` | Previous frame |
| `→` | Next frame |
| `[` | Previous cycle |
| `]` | Next cycle |
| `Home` | First frame |
| `End` | Last frame |
| `+/-` | Speed up/down |
| `Q` | Quit |

### 7.2 Interactive Controls (HTML)

```html
<div class="animation-controls">
  <button onclick="play()">▶ Play</button>
  <button onclick="pause()">⏸ Pause</button>
  <button onclick="step(-1)">⏮ Step Back</button>
  <button onclick="step(1)">⏭ Step Forward</button>

  <input type="range" id="timeline" min="0" max="100"
         oninput="seekTo(this.value)">

  <label>Speed:
    <select onchange="setSpeed(this.value)">
      <option value="0.5">0.5x</option>
      <option value="1" selected>1x</option>
      <option value="2">2x</option>
    </select>
  </label>

  <span id="frame-counter">Frame: 0/100</span>
  <span id="cycle-counter">Cycle: 0/10</span>
</div>
```

---

## 8. Animation Presets

### 8.1 Quick Preview

```bash
> nf animate dynamics --preset quick
[ANIMATION] Quick preview (5 cycles, 10 fps, ASCII)
```

### 8.2 High Quality

```bash
> nf animate dynamics --preset quality
[ANIMATION] High quality (all cycles, 30 fps, interpolated)
```

### 8.3 Presentation

```bash
> nf animate dynamics --preset presentation --output slides.gif
[ANIMATION] Presentation quality (1080p, smooth transitions)
```

---

## 9. Command Examples

### 9.1 Basic Animation

```bash
> nf animate dynamics
[ANIMATION] Field evolution

  Cycles: 10
  Frames generated: 100

  Playing... (press Q to stop)
```

### 9.2 With Options

```bash
> nf animate dynamics --cycles 5 --fps 15 --style network
[ANIMATION] Network evolution

  Cycles: 5
  Frames: 75
  Style: network graph

  [Animation playing]
```

### 9.3 Export

```bash
> nf animate dynamics --cycles 10 --output evolution.gif
[ANIMATION] Generating animation...

  Rendering frames: [████████████████████] 100%
  Optimizing GIF...
  Saved: evolution.gif (450 KB)
```

### 9.4 Attractor Focus

```bash
> nf animate attractor security_focus
[ANIMATION] Attractor emergence: security_focus

  Emerged at: cycle 4
  Showing: cycles 2-6 (before/after)

  [Animation showing attractor formation]
```

---

## 10. Performance Considerations

### 10.1 Frame Budget

| Quality | FPS | Interpolation | Max Cycles |
|---------|-----|---------------|------------|
| Low | 10 | None | 20 |
| Medium | 15 | Linear | 15 |
| High | 30 | Eased | 10 |

### 10.2 Optimization

```
OPTIMIZE_ANIMATION(frames):
  // Skip unchanged elements
  FOR frame IN frames:
    frame.changes_only = diff(frame, previous_frame)

  // Reduce color depth for GIF
  palette = extract_palette(frames, max_colors=256)

  // Delta encoding for video
  encode_with_keyframes(frames, keyframe_interval=30)
```

---

## Related Documents

- `topology.md` - Energy landscape visualization
- `networks.md` - Network visualization
- `generators/` - Output format generators
- `../commands/visualization.md` - Command reference
