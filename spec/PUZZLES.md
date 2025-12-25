# Puzzle Definitions

## Arc 1: Basics (Welcome to Lightpath)

### Puzzle 1: Hello Photon
**Objective:** Connect laser to detector, read power
**Components:** Laser, Waveguide, Detector
**Script Task:** Configure laser, query power meter
**Test Cases:**
- Power reading within 0.5 dB of 0 dBm
**Hint:** Connect laser output to waveguide, waveguide to detector

### Puzzle 2: Split Decision
**Objective:** Split light 50/50 to two detectors
**Components:** + Directional Coupler
**Script Task:** Read both power meters
**Test Cases:**
- PM-1 power: -3 dBm ± 0.5 dB
- PM-2 power: -3 dBm ± 0.5 dB
**Hint:** A 50/50 coupler has κ = 0.707

### Puzzle 3: Interference
**Objective:** Build MZI, tune to constructive interference at port 1
**Components:** + Phase Shifter
**Script Task:** Sweep phase, find maximum
**Test Cases:**
- PM-1 power > -0.5 dBm
- PM-2 power < -20 dBm
**Hint:** MZI = coupler + two arms + coupler. Phase difference controls output.

### Puzzle 4: The Switch
**Objective:** Route light to port 1 OR port 2 based on condition
**Script Task:** Set phase based on test condition
**Test Cases:**
- Condition A: PM-1 high, PM-2 low
- Condition B: PM-1 low, PM-2 high
**Hint:** π phase shift swaps outputs

## Arc 2: Sensing (Environmental Division)

### Puzzle 5: Ring Around
**Objective:** Find resonance wavelength of ring
**Components:** + Ring Resonator
**Script Task:** Sweep wavelength, find dip
**Test Cases:**
- Report correct resonance ±0.1 nm
**Hint:** Ring resonates when λ = n*L/m. Look for power dip.

### Puzzle 6: Gas Alarm
**Objective:** Detect gas presence via ring shift
**Components:** Ring with sensing window
**Script Task:** Set threshold, output detection flag
**Test Cases:**
- Air: output LOW (< 0.5V)
- Gas present: output HIGH (> 2V)
- Low concentration: output HIGH
**Hint:** Gas shifts resonance. Position laser on slope of resonance.

## Puzzle Data Format

```javascript
{
  id: "hello_photon",
  arc: 1,
  name: "Hello Photon",
  client: "Lightpath Training",
  description: "Welcome to your first day at Lightpath! Let's start simple: connect a laser to a detector and measure the output power.",
  objective: "Measure 0 dBm ± 0.5 dB at the detector",
  availableComponents: ["laser", "waveguide", "detector"],
  preplacedComponents: [],
  testCases: [
    {
      name: "Basic measurement",
      conditions: {},
      expected: {
        "PM-1": { type: "power", value: 0, tolerance: 0.5, unit: "dBm" }
      }
    }
  ],
  hints: [
    "Click the Laser component, then click the grid to place it",
    "Connect the laser's output port to the detector's input port",
    "Use rm.open('PM-1').query(':POW?') to read the power"
  ],
  starterCode: `// Configure instruments and run measurement
const laser = rm.open("LASER-1");
laser.write(":WAV 1550nm");
laser.write(":POW 0dBm");
laser.write(":OUTP ON");

const pm = rm.open("PM-1");
return { "PM-1": parseFloat(pm.query(":POW?")) };
`
}
```
