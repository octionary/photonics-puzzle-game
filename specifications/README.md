# Lightpath - Specification

## Vision
A Shenzhen I/O-inspired puzzle game about designing integrated photonic circuits. Players design optical systems to meet specifications by connecting components, tuning parameters, and writing instrument control scripts.

## Core Loop
1. Receive contract/challenge with narrative framing
2. Design circuit by placing and connecting components on grid
3. Configure component parameters (click to open settings)
4. Write/edit script to control instruments (lasers, DACs, power meters)
5. Run simulation, observe results
6. Iterate until spec met
7. Get scored on component count and waveguide count

## Components
- **Laser** - Light source with configurable wavelength and power
- **Directional Coupler** - 2x2 beam splitter with configurable split ratio
- **Phase Shifter** - Voltage-controlled phase modulator (connects to DAC)
- **Ring Resonator** - Wavelength-selective filter (optional add/drop configuration)
- **DAC** - Digital-to-analog converter with 2 channels for phase shifter control
- **Detector** - Power meter for measuring optical power

## Connections
- **Optical (waveguides)** - Blue lines connecting optical ports (circles)
- **Electrical (wires)** - Yellow dashed lines connecting electrical ports (squares)

## Features
- Grid-based component placement with drag-and-drop
- Rectilinear auto-routing for connections
- Undo/Redo (Ctrl+Z/Y)
- JavaScript instrument API (PyVISA-like)
- 6 puzzles across 2 arcs (Basics + Sensing)

## Implementation
- Browser-based single HTML file with embedded JS/CSS
- SVG rendering for components and connections
- Steady-state complex amplitude simulation

## Specification Files
- [COMPONENTS.md](COMPONENTS.md) - Component catalog with ports and parameters
- [INSTRUMENTS.md](INSTRUMENTS.md) - Instrument API documentation
- [TECHNICAL.md](TECHNICAL.md) - Physics model and architecture
- [PUZZLES.md](PUZZLES.md) - Puzzle definitions and progression
- [GAME_DESIGN.md](GAME_DESIGN.md) - Game design document
