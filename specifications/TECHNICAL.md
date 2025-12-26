# Technical Specification

## Physics Model: Steady-State Complex Amplitude

Track complex field E = A·e^(iφ) through circuit.

### Propagation Rules

**Directional Coupler (2x2):**
```
[E_out1]   [t    iκ] [E_in1]
[E_out2] = [iκ   t ] [E_in2]

split_ratio = κ²  (power coupling)
t = √(1 - split_ratio)
κ = √(split_ratio)
```

**Phase Shifter (DAC-controlled):**
```
E_out = E_in * exp(i*Δφ)
Δφ = π * V / V_π
```
Where V is the voltage from the connected DAC channel.

**Ring Resonator (all-pass):**
```
H(λ) = (t - a*exp(iφ)) / (1 - t*a*exp(iφ))

φ = 2π * n_eff * L_ring / λ
a = exp(-α * L_ring / 2)  // round-trip amplitude (default 0.99)
t = √(1 - κ²)
```
For sensing mode: n_eff includes delta_n from test conditions.

**Photodetector:**
```
P = |E|² (in linear scale)
P_dBm = 10 * log10(P)  (converted to dBm for output)
```

## Simulation Algorithm

1. Build directed graph from components and connections
2. Identify source components (lasers with output enabled)
3. Iteratively propagate fields:
   - Initialize source fields from laser parameters
   - For each iteration, propagate through connected components
   - Repeat until all reachable detectors have values
4. For phase shifters: read voltage from connected DAC via electrical connections
5. Collect detector outputs for test script

## Electrical Connections

- DAC outputs connect to phase shifter control inputs
- Voltage propagation is instantaneous (no delay modeling)
- DAC state is set via instrument API before simulation runs

## Architecture

```
index.html (single file)
├── CSS (embedded)
│   ├── Grid styling
│   ├── Component visuals
│   ├── Port styles (optical: circles, electrical: squares)
│   └── Connection styles (optical: blue solid, electrical: yellow dashed)
├── JavaScript (embedded)
│   ├── GameState
│   │   ├── components[]
│   │   ├── connections[]
│   │   ├── undoStack[], redoStack[]
│   │   └── currentPuzzleIndex
│   ├── Simulation
│   │   ├── propagate()
│   │   ├── getControlVoltage()  // reads DAC state
│   │   └── run() → detector outputs
│   ├── Instruments
│   │   ├── LaserInstrument
│   │   ├── PowerMeterInstrument
│   │   ├── DACInstrument
│   │   └── ResourceManager (rm.open)
│   ├── ScriptRunner
│   │   └── runSimulation(code)
│   ├── UI
│   │   ├── Canvas (SVG grid)
│   │   ├── ComponentPalette (with drag support)
│   │   ├── ScriptEditor (textarea)
│   │   └── ResultsPanel (modal overlay)
│   └── Puzzles[]
└── SVG Canvas
    ├── Grid cells
    ├── Components group
    └── Connections group
```

## Data Structures

### Component
```javascript
{
  id: "comp_1234567890",
  type: "phase_shifter",
  x: 5, y: 3,  // grid position
  params: {
    ps_type: "thermal",
    v_pi: 5
  }
}
```

### Connection
```javascript
{
  id: "conn_1234567890",
  from: { componentId: "comp_1", port: "out" },
  to: { componentId: "comp_2", port: "in" },
  isElectrical: false  // true for DAC-to-phase-shifter connections
}
```

### Puzzle
```javascript
{
  id: "the_switch",
  name: "The Switch",
  arc: "Arc 1: Basics",
  client: "DataLink Corp",
  description: "Create an optical switch...",
  objective: "Route light based on DAC voltage",
  availableComponents: ["laser", "coupler", "phase_shifter", "dac", "detector"],
  testCases: [
    {
      name: "Switch to Port 1 (0V)",
      conditions: { control_voltage: 0 },
      expected: {
        "PM-1": { value: -1, unit: "dBm", comparison: "greater" },
        "PM-2": { value: -10, unit: "dBm", comparison: "less" }
      }
    }
  ],
  hints: ["Connect DAC CH1 to phase shifter control input"],
  starterCode: `const laser = rm.open("LASER-1");...`
}
```

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| Ctrl+Z | Undo |
| Ctrl+Y / Ctrl+Shift+Z | Redo |
| Delete | Delete selected component |
| Escape | Cancel selection/connection |

## Interaction Modes

1. **Click-to-place:** Click palette item to select, click grid to place
2. **Drag-to-place:** Drag from palette directly onto grid
3. **Component drag:** Click and drag existing components to move them
4. **Port connection:** Click output port, then click input port to connect
5. **Connection delete:** Click on a connection (waveguide/wire) to delete it
