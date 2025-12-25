# Game Design

## Grid & Placement
- 20x15 tile grid canvas
- Components snap to grid, occupy 1-3 tiles depending on type
- Click to select component from palette, click grid to place
- Click placed component to configure or delete

## Connections
- Click output port → click input port to create connection
- Connections route as straight lines (orthogonal where possible)
- Visual: blue lines for optical paths, orange for electrical
- Delete connection by clicking it

## Component Configuration
Photonic components have static "fabrication-time" parameters configured via inline panel:
- Click component → panel appears beside it
- Parameters take effect when simulation runs

## Instruments
Controlled via JavaScript script editor:
```javascript
const laser = rm.open("LASER-1");
laser.write(":WAV 1550nm");
laser.write(":POW 0dBm");
laser.write(":OUTP ON");

const pm = rm.open("PM-1");
const power = pm.query(":POW?");
```

## Simulation Flow
```
[Setup Script] → [Run Test Cases] → [Compare Output] → [Score]
```

Each puzzle defines test cases with:
- Conditions (e.g., gas_present: true)
- Expected outputs (e.g., output_voltage: "> 2.0V")

## Results Display
- Pass/fail for each test case
- Visual comparison: expected vs actual
- Hints on failure
- Metrics: Area, Component count

## UI Layout
```
┌─────────────────────────────────────────────────────────┐
│ LIGHTPATH                                Level [x/y]    │
├─────────────────────────────────────────────────────────┤
│ ┌───────────────────────────┐ ┌───────────────────────┐ │
│ │     DESIGN CANVAS         │ │ CONTRACT              │ │
│ │                           │ │ - Objective           │ │
│ │                           │ │ - Requirements        │ │
│ │                           │ │ - Hints               │ │
│ └───────────────────────────┘ └───────────────────────┘ │
│ ┌───────────────────────────┐ ┌───────────────────────┐ │
│ │ COMPONENTS                │ │ SCRIPT EDITOR         │ │
│ │ [Laser][WG][DC][Ring]...  │ │ // setup code         │ │
│ └───────────────────────────┘ └───────────────────────┘ │
│ ┌─────────────────────────────────────────────────────┐ │
│ │ RESULTS   [▶ RUN]  [⟳ RESET]         Score: --/100  │ │
│ └─────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────┘
```

## Visual Style
- Dark theme with accent colors
- Blue for optical paths, orange for electrical
- Components as simple geometric shapes with labels
- Monospace font for technical feel
- SVG-based, no heavy graphics
