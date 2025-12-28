# Lightpath - Project Context

## Overview
Lightpath is a Shenzhen I/O-inspired puzzle game about designing integrated photonic circuits. Players place optical and electronic components on a grid, connect them with waveguides and wires, and write scripts to control instruments and meet puzzle objectives.

## Project Structure
```
/
├── index.html	        # Single-file game (HTML + CSS + JS)
├── CLAUDE.md           # This file
└── specifications/		# Design specifications
    ├── README.md       # Overview
    ├── COMPONENTS.md   # Component catalog
    ├── INSTRUMENTS.md  # Instrument API
    ├── TECHNICAL.md    # Physics & architecture
    ├── PUZZLES.md      # Puzzle definitions
    └── GAME_DESIGN.md  # UI/UX design
```

## Key Concepts

### Components
- **Laser** (1x1) - Light source, instrument: LASER-n
- **Coupler** (2x2) - Beam splitter with split ratio parameter
- **Phase Shifter** (1x1) - Voltage-controlled phase, has electrical control input
- **Ring Resonator** (2x2) - Wavelength filter, optional add/drop ports
- **DAC** (1x2) - 2-channel voltage source for phase shifters, instrument: DAC-n
- **Detector** (1x1) - Power meter, instrument: PM-n

### Connections
- **Optical (waveguides)** - Blue solid lines, connect circular ports
- **Electrical (wires)** - Yellow dashed lines, connect square ports

### Physics
- Steady-state complex amplitude simulation
- Phase shifter: φ = V/Vπ × π (voltage from DAC)
- Coupler: 2x2 transfer matrix based on split ratio
- Ring: Lorentzian transfer function

### Instrument API
```javascript
const laser = rm.open("LASER-1");
laser.write(":WAV 1550nm");
laser.write(":POW 0dBm");
laser.write(":OUTP ON");

const dac = rm.open("DAC-1");
dac.write(":CH1 2.5V");

const pm = rm.open("PM-1");
const power = parseFloat(pm.query(":POW?"));
```

## Code Conventions

### GameState
Central state object containing:
- `components[]` - Placed components with id, type, x, y, params
- `connections[]` - Connections with from/to ports, isElectrical flag
- `selectedComponents` - Set of selected component IDs (multi-selection)
- `undoStack[]`, `redoStack[]` - For undo/redo

### Component Definitions
In `ComponentTypes` object with:
- `ports` - Port definitions with x, y (0-1 relative), type, optional `conditional`
- `params` - Parameter definitions with label, default, type, constraints

### Simulation
`Simulation` class propagates complex fields through the circuit:
1. Initialize laser outputs
2. Iteratively propagate through connected components
3. Read DAC voltages for phase shifters via `getControlVoltage()`
4. Collect detector readings

## Common Tasks

### Adding a Component
1. Add definition to `ComponentTypes`
2. Add rendering logic in `createComponentElement()` if custom visual needed
3. Add processing logic in `Simulation.processComponent()`
4. Update palette thumbnail in `initPalette()` if needed

### Adding an Instrument
1. Add state to `instrumentState` object
2. Create instrument class (e.g., `DACInstrument`)
3. Add to `ResourceManager.open()` switch

### Modifying Puzzles
Puzzles are in the `Puzzles` array with:
- `availableComponents` - Which components can be used
- `testCases` - Conditions and expected outputs
- `starterCode` - Initial script content

## Keyboard Shortcuts
- Ctrl+Z - Undo
- Ctrl+Y / Ctrl+Shift+Z - Redo
- Delete - Delete selected component(s)
- Escape - Cancel selection/connection, clear multi-selection
- Enter (in config panel) - Apply settings and close

## User Interactions
- Drag from palette to place (shows real component size during drag)
- Drag component to palette to delete
- Box select: Drag on empty grid to select multiple components
- Shift+click to add/remove from selection
- Drag selected components to move as group

## Script API
- `console.log(...)` - Debug output
- `plot(xData, yData, options)` - Create plot in results
  - Options: `{ title, xlabel, ylabel }`
