# Game Design

## Grid & Placement
- 20x12 tile grid canvas
- Components snap to grid, occupy 1-2 tiles depending on type
- Two placement modes:
  - Click palette item to select, click grid to place
  - Drag directly from palette onto grid (ghost shows actual component size)
- Click and drag placed components to move them
- Drag component back to palette to delete it
- Box selection: Click and drag on empty grid to select multiple components
- Shift+click to add/remove from selection
- Drag any selected component to move entire selection
- Components cannot overlap each other or waveguides

## Connections
- Click output port → click input port to create connection
- Connections auto-route with rectilinear (Manhattan) paths
- Visual styles:
  - Blue solid lines for optical waveguides
  - Yellow dashed lines for electrical wires
- Delete connection by clicking on it
- Ports:
  - Optical ports: circles (blue = input, orange = output)
  - Electrical ports: squares (yellow)

## Component Configuration
Click a component to open configuration panel:
- Panel appears beside the component
- Parameters take effect when simulation runs
- Press Enter in any text field to apply and close panel
- Examples:
  - Coupler: split ratio (0-1) with live power fraction display
  - Phase shifter: type (thermal/EO), Vπ voltage
  - Ring: radius, coupling, add/drop mode, sensing window

## Instruments
Controlled via JavaScript script editor:
```javascript
// Laser control
const laser = rm.open("LASER-1");
laser.write(":WAV 1550nm");
laser.write(":POW 0dBm");
laser.write(":OUTP ON");

// DAC control (for phase shifters)
const dac = rm.open("DAC-1");
dac.write(":CH1 2.5V");  // Sets phase to π/2 (with Vπ=5V)

// Power meter readout
const pm = rm.open("PM-1");
const power = parseFloat(pm.query(":POW?"));
```

## Simulation Flow
```
[Script Execution] → [Physics Simulation] → [Compare to Expected] → [Pass/Fail]
```

Each puzzle defines test cases with:
- Conditions (e.g., control_voltage: 5, gas_present: true)
- Expected outputs (e.g., PM-1: > -1 dBm)

## Results Display
- Pass/fail status for each test case
- Actual vs expected values
- Plot visualization (when `plot()` called in script)
- Console output from script
- Metrics: Component count, Waveguide count

## Keyboard Shortcuts
- Ctrl+Z: Undo
- Ctrl+Y / Ctrl+Shift+Z: Redo
- Delete: Delete selected component(s)
- Escape: Cancel selection/connection, clear multi-selection
- Enter (in config panel): Apply settings and close

## UI Layout
```
┌─────────────────────────────────────────────────────────────────┐
│ LIGHTPATH                                    Arc 1   Puzzle 1/6 │
├────────────────────────────────────┬────────────────────────────┤
│                                    │ CONTRACT                   │
│     DESIGN CANVAS (SVG Grid)       │ - Title & Client           │
│                                    │ - Description              │
│     [Components placed here]       │ - Objective                │
│     [Waveguides connect them]      │ - Hints (expandable)       │
│                                    ├────────────────────────────┤
├────────────────────────────────────┤ SCRIPT EDITOR              │
│ COMPONENTS                         │ const laser = rm.open(...) │
│ [L][DC][φ][R][DAC][D]              │ ...                        │
│ (uniform height, centered symbols) ├────────────────────────────┤
│                                    │ [▶ RUN] [↻ RESET]          │
├────────────────────────────────────┴────────────────────────────┤
│ Components: 5  Waveguides: 4                                    │
└─────────────────────────────────────────────────────────────────┘
```

## Visual Style
- Dark theme (#1e1e1e background)
- Accent colors:
  - Blue (#4a9eff) for optical inputs
  - Orange (#ff9f43) for optical outputs
  - Yellow (#fdcb6e) for electrical
  - Green (#00b894) for success/detectors
  - Red (#ff6b6b) for errors/lasers
- Components as rectangles with symbols
- Monospace font throughout
- SVG-based rendering
