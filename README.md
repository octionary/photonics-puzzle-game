# Lightpath

Lightpath is a Shenzhen I/O-inspired puzzle game about designing integrated photonic circuits. Players place optical and electronic components on a grid, connect them with waveguides and wires, and write scripts to control instruments and meet puzzle objectives.

![Lightpath Screenshot](Screenshot.png)

## Play

Open `index.html` in a modern browser. No installation required.

## About

You're an engineer at Lightpath, a photonics company. Design optical circuits to meet client specifications by:

1. **Placing components** on the grid - lasers, couplers, phase shifters, ring resonators, and detectors
2. **Connecting ports** with waveguides to route light through your circuit
3. **Writing scripts** to control instruments and run measurements
4. **Tuning parameters** to meet the puzzle objectives

## Components

| Component | Description |
|-----------|-------------|
| **Laser** | Light source with configurable wavelength and power |
| **Coupler** | Splits light between two paths (adjustable ratio) |
| **Phase Shifter** | Changes the phase of light (controlled by DAC voltage) |
| **Ring Resonator** | Filters specific wavelengths |
| **DAC** | Voltage source to control phase shifters |
| **Detector** | Measures optical power |

## Controls

- **Place components:** Click palette item, then click grid (or drag directly)
- **Connect ports:** Click an output port, then click an input port
- **Move components:** Click and drag
- **Delete:** Click to select, press Delete
- **Undo/Redo:** Ctrl+Z / Ctrl+Y

## Puzzles

**Arc 1: Basics**
- Hello Photon - Connect laser to detector
- Split Decision - Build a 50/50 splitter
- Interference - Create a Mach-Zehnder interferometer
- The Switch - Route light based on voltage control

**Arc 2: Sensing**
- Ring Around - Find a ring resonator's resonance
- Gas Alarm - Detect gas using resonance shift

## Tech Stack

Single HTML file with embedded CSS and JavaScript. No dependencies, no build step.

## License

MIT
