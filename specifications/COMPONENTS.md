# Component Catalog

## Optical Components

### Laser Source
- **Symbol:** Rectangle with "L" label
- **Size:** 1x1 tile
- **Ports:** out (right, optical output)
- **Parameters:**
  - name: Instrument ID (default: "LASER-1", auto-increments)
  - wavelength: 1550 nm (default)
  - power: 0 dBm (default)
- **Behavior:** Outputs E = sqrt(P) at specified wavelength

### Directional Coupler
- **Symbol:** Two parallel lines with crossing
- **Size:** 2x2 tiles
- **Ports:** in1 (top-left), in2 (bottom-left), out1 (top-right), out2 (bottom-right)
- **Parameters:**
  - split: Split ratio 0-1 (default: 0.5 for 50/50)
- **Behavior:** 2x2 transfer matrix. Split ratio determines power coupling (0.5 = 50/50 split)

### Phase Shifter
- **Symbol:** Rectangle with "φ" label
- **Size:** 1x1 tile
- **Ports:**
  - in (left, optical input)
  - out (right, optical output)
  - ctrl (bottom, electrical input)
- **Parameters:**
  - ps_type: "thermal" or "electro-optic" (default: thermal)
  - v_pi: Voltage for π phase shift (default: 5V)
- **Behavior:** E_out = E_in * exp(i * V/Vπ * π), where V is DAC voltage

### Ring Resonator
- **Symbol:** Circle with tangent bus waveguide(s)
- **Size:** 2x2 tiles
- **Ports (default, single bus):**
  - in (left, y=0.25)
  - through (right, y=0.25)
- **Ports (add/drop mode):**
  - in (left, y=0.25)
  - through (right, y=0.25)
  - add (right, y=0.75)
  - drop (left, y=0.75)
- **Parameters:**
  - radius: Ring radius in μm (default: 10)
  - kappa: Bus-ring coupling coefficient (default: 0.1)
  - add_drop: Enable add/drop ports (default: false)
  - sensing: Enable sensing window for refractive index changes (default: false)
- **Behavior:** Lorentzian transfer function, resonance at λ = n_eff * 2πR / m

### Photodetector
- **Symbol:** Rectangle with "D" label
- **Size:** 1x1 tile
- **Ports:** in (left, optical input)
- **Parameters:**
  - name: Instrument ID (default: "PM-1", auto-increments)
  - responsivity: 1.0 A/W (default)
- **Behavior:** Outputs power P = |E|² to named power meter instrument

## Electronic Components

### DAC (Digital-to-Analog Converter)
- **Symbol:** Rectangle with "DAC" label
- **Size:** 1x2 tiles
- **Ports:**
  - ch1 (right, y=0.25, electrical output)
  - ch2 (right, y=0.75, electrical output)
- **Parameters:**
  - name: Instrument ID (default: "DAC-1")
- **Behavior:** Outputs voltage set via instrument API to control phase shifters

## Connections

### Optical Waveguides
- Created by connecting optical ports (circles)
- Blue solid lines with rectilinear routing
- Cannot split without a coupler (one output to one input only)

### Electrical Wires
- Created by connecting electrical ports (squares)
- Yellow dashed lines with rectilinear routing
- Connect DAC outputs to phase shifter control inputs
