# Puzzle Definitions

## Arc 1: Basics (Welcome to Lightpath)

### Puzzle 1: Hello Photon
**Objective:** Connect laser to detector, read power
**Components:** Laser, Detector
**Script Task:** Configure laser, query power meter
**Test Cases:**
- Power reading within 0.5 dB of 0 dBm
**Hint:** Connect laser output port to detector input port (connections act as waveguides)

### Puzzle 2: Split Decision
**Objective:** Split light 50/50 to two detectors
**Components:** + Directional Coupler
**Script Task:** Read both power meters
**Test Cases:**
- PM-1 power: -3 dBm ± 0.5 dB
- PM-2 power: -3 dBm ± 0.5 dB
**Hint:** Set coupler split ratio to 0.5 for 50/50 power splitting

### Puzzle 3: Interference
**Objective:** Build MZI, tune to constructive interference at port 1
**Components:** + Phase Shifter, DAC
**Script Task:** Set DAC voltage to achieve constructive interference
**Test Cases:**
- PM-1 power > -0.5 dBm
- PM-2 power < -15 dBm
**Hint:** MZI = coupler + two arms (one with phase shifter) + coupler. Connect DAC to phase shifter control.

### Puzzle 4: The Switch
**Objective:** Route light to port 1 OR port 2 based on DAC voltage
**Components:** Laser, Coupler, Phase Shifter, DAC, Detector
**Script Task:** Set DAC voltage based on test condition
**Test Cases:**
- 0V: PM-1 high (>-1 dBm), PM-2 low (<-10 dBm)
- Vπ (5V): PM-1 low (<-10 dBm), PM-2 high (>-1 dBm)
**Hint:** With Vπ=5V, setting DAC to 5V gives π phase shift which swaps outputs

## Arc 2: Sensing (Environmental Division)

### Puzzle 5: Ring Around
**Objective:** Find resonance wavelength of ring resonator
**Components:** Laser, Ring Resonator, Detector
**Script Task:** Sweep wavelength, find transmission dip
**Test Cases:**
- Report correct resonance ±0.5 nm
**Hint:** Ring resonates when light constructively interferes. Look for power dip in wavelength sweep.

### Puzzle 6: Gas Alarm
**Objective:** Detect gas presence via ring resonance shift
**Components:** Ring with sensing window enabled
**Script Task:** Set operating wavelength, detect resonance shift
**Test Cases:**
- Air (no gas): output LOW (< -10 dBm)
- Gas present: output HIGH (> -3 dBm)
**Hint:** Position laser on slope of resonance. Gas shifts the resonance, changing transmitted power.

## Puzzle Data Format

```javascript
{
  id: "the_switch",
  arc: "Arc 1: Basics",
  name: "The Switch",
  client: "DataLink Corp",
  description: "Create an optical switch! Your MZI should route light to different outputs based on a DAC control voltage.",
  objective: "Condition A (0V): PM-1 high, PM-2 low. Condition B (Vπ): PM-1 low, PM-2 high",
  availableComponents: ["laser", "coupler", "phase_shifter", "dac", "detector"],
  testCases: [
    {
      name: "Switch to Port 1 (0V)",
      conditions: { control_voltage: 0 },
      expected: {
        "PM-1": { value: -1, unit: "dBm", comparison: "greater" },
        "PM-2": { value: -10, unit: "dBm", comparison: "less" }
      }
    },
    {
      name: "Switch to Port 2 (Vπ)",
      conditions: { control_voltage: 5 },
      expected: {
        "PM-1": { value: -10, unit: "dBm", comparison: "less" },
        "PM-2": { value: -1, unit: "dBm", comparison: "greater" }
      }
    }
  ],
  hints: [
    "Place a DAC and connect its CH1 output to the phase shifter control input",
    "Use condition.control_voltage to set the DAC output",
    "With default Vπ=5V, setting DAC to 5V gives π phase shift"
  ],
  starterCode: `// Optical switch - control phase via DAC voltage
const laser = rm.open("LASER-1");
laser.write(":WAV 1550nm");
laser.write(":POW 0dBm");
laser.write(":OUTP ON");

// Get the control voltage from test condition
const voltage = condition.control_voltage || 0;

// Set the DAC voltage (connect DAC CH1 to phase shifter!)
const dac = rm.open("DAC-1");
dac.write(":CH1 " + voltage + "V");

const pm1 = rm.open("PM-1");
const pm2 = rm.open("PM-2");

const p1 = parseFloat(pm1.query(":POW?"));
const p2 = parseFloat(pm2.query(":POW?"));

console.log("DAC voltage: " + voltage.toFixed(2) + " V");
console.log("PM-1: " + p1.toFixed(2) + " dBm");
console.log("PM-2: " + p2.toFixed(2) + " dBm");

return { "PM-1": p1, "PM-2": p2 };`
}
```
