# Technical Specification

## Physics Model: Steady-State Complex Amplitude

Track complex field E = A·e^(iφ) through circuit.

### Propagation Rules

**Waveguide:**
```
E_out = E_in * exp(-α*L/2) * exp(i*β*L)
```
Where α = loss coefficient, L = length, β = 2πn_eff/λ

**Directional Coupler (2x2):**
```
[E_out1]   [t    iκ] [E_in1]
[E_out2] = [iκ   t ] [E_in2]

t² + κ² = 1
```
t = transmission, κ = coupling coefficient

**Phase Shifter:**
```
E_out = E_in * exp(i*Δφ)
```
Thermal: Δφ = α_TO * ΔT
Electro-optic: Δφ = π * V / V_π

**Ring Resonator (all-pass):**
```
H(λ) = (t - a*exp(iφ)) / (1 - t*a*exp(iφ))

φ = 2π * n_eff * L_ring / λ
a = exp(-α * L_ring / 2)  // round-trip amplitude
FSR = λ² / (n_g * L_ring)
```

**Photodetector:**
```
P = |E|²
I = R * P  // R = responsivity
```

## Simulation Algorithm

1. Build directed graph from components and connections
2. Topological sort to determine evaluation order
3. For each wavelength in simulation:
   - Initialize source fields
   - Propagate through network following sort order
   - Record detector outputs
4. Collect results for test script

## Multi-Wavelength Support
- Track field at discrete wavelengths when needed
- For ring resonators, sweep λ to find resonances

## Architecture

```
index.html
├── CSS (embedded)
├── JavaScript (embedded)
│   ├── GameState
│   │   ├── components[]
│   │   ├── connections[]
│   │   └── currentPuzzle
│   ├── Simulation
│   │   ├── buildGraph()
│   │   ├── propagate()
│   │   └── getDetectorOutputs()
│   ├── Instruments
│   │   ├── Laser
│   │   ├── PowerMeter
│   │   └── ResourceManager (rm)
│   ├── ScriptRunner
│   │   └── execute(code)
│   ├── UI
│   │   ├── Canvas (SVG)
│   │   ├── ComponentPalette
│   │   ├── ScriptEditor
│   │   └── ResultsPanel
│   └── Puzzles[]
└── SVG Canvas
```

## Data Structures

### Component
```javascript
{
  id: "comp_1",
  type: "ring",
  x: 5, y: 3,  // grid position
  params: { radius: 10, kappa: 0.1, sensing: false },
  ports: {
    in: { x: 0, y: 0.5 },
    out: { x: 1, y: 0.5 },
    drop: { x: 0.5, y: 1 }
  }
}
```

### Connection
```javascript
{
  id: "conn_1",
  from: { componentId: "comp_1", port: "out" },
  to: { componentId: "comp_2", port: "in" }
}
```

### Puzzle
```javascript
{
  id: "puzzle_1",
  name: "Hello Photon",
  arc: "basics",
  description: "Connect laser to detector...",
  objective: "Measure 0 dBm at detector",
  availableComponents: ["laser", "waveguide", "detector"],
  testCases: [
    { name: "Basic", conditions: {}, expected: { "PM-1": { power: 0, tolerance: 0.5, unit: "dBm" } } }
  ],
  hints: ["Try connecting the laser output to the detector input"]
}
```
