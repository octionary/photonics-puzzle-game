# Component Catalog

## Optical Components

### Laser Source
- **Symbol:** Rectangle with "L" label
- **Size:** 1x1 tile
- **Ports:** out (right)
- **Parameters:**
  - wavelength: 1550 nm (default)
  - power: 0 dBm (default)
- **Behavior:** Outputs E = sqrt(P) at specified wavelength

### Waveguide
- **Symbol:** Line segment
- **Size:** Variable (1+ tiles)
- **Ports:** in (left), out (right)
- **Parameters:**
  - length: derived from routing
  - loss: 0.2 dB/cm (default)
- **Behavior:** E_out = E_in * exp(-αL/2) * exp(iβL)

### Directional Coupler
- **Symbol:** Two parallel lines with crossing
- **Size:** 2x1 tiles
- **Ports:** in1 (top-left), in2 (bottom-left), out1 (top-right), out2 (bottom-right)
- **Parameters:**
  - kappa: 0.5 (coupling coefficient, 0-1)
- **Behavior:** 2x2 transfer matrix with t=√(1-κ²), κ

### Phase Shifter (Thermal)
- **Symbol:** Rectangle with "φ" label, heater symbol
- **Size:** 1x1 tile
- **Ports:** in (left), out (right)
- **Parameters:**
  - delta_phi: 0 rad (can be set by voltage/temperature)
- **Behavior:** E_out = E_in * exp(i*Δφ)

### Ring Resonator
- **Symbol:** Circle with bus waveguide
- **Size:** 2x2 tiles
- **Ports:** in (left), through (right), drop (bottom, optional)
- **Parameters:**
  - radius: 10 μm
  - kappa: 0.1 (bus-ring coupling)
  - sensing_window: false (enables index sensing)
- **Behavior:** Lorentzian transfer function, resonance at λ = n_eff * L / m

### Photodetector
- **Symbol:** Trapezoid with "D" label
- **Size:** 1x1 tile
- **Ports:** in (left)
- **Parameters:**
  - responsivity: 1.0 A/W
  - name: "PM-1" (instrument name for readout)
- **Behavior:** Outputs power P = |E|² to named instrument

## Electronic Components (Future)

### DAC
- **Symbol:** Rectangle with "DAC" label
- **Ports:** digital_in, analog_out
- **Parameters:**
  - bits: 8
  - v_ref: 3.3V

### Comparator
- **Symbol:** Triangle with +/-
- **Ports:** in+, in-, out
- **Parameters:**
  - threshold: 0.5V
