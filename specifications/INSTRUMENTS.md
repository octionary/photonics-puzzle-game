# Instrument API

## Resource Manager

```javascript
const rm = {
  open(name) {
    // Returns instrument handle
    // e.g., rm.open("LASER-1"), rm.open("PM-1"), rm.open("DAC-1")
  }
};
```

## Laser (LASER-1, LASER-2, ...)

### Commands
| Command | Description | Example |
|---------|-------------|---------|
| `:WAV <value>` | Set wavelength | `:WAV 1550nm` |
| `:WAV?` | Query wavelength | Returns `"1550.00nm"` |
| `:POW <value>` | Set power | `:POW 0dBm` |
| `:POW?` | Query power | Returns `"0.00dBm"` |
| `:OUTP ON\|OFF` | Enable/disable output | `:OUTP ON` |
| `:OUTP?` | Query output state | Returns `"ON"` or `"OFF"` |

## Power Meter (PM-1, PM-2, ...)

### Commands
| Command | Description | Example |
|---------|-------------|---------|
| `:POW?` | Measure power | Returns `"-3.01dBm"` |

## DAC (DAC-1, DAC-2, ...)

### Commands
| Command | Description | Example |
|---------|-------------|---------|
| `:CH1 <value>` | Set channel 1 voltage | `:CH1 2.5V` |
| `:CH2 <value>` | Set channel 2 voltage | `:CH2 0V` |
| `:CH1?` | Query channel 1 voltage | Returns `"2.50V"` |
| `:CH2?` | Query channel 2 voltage | Returns `"0.00V"` |

### Notes
- DAC channels connect to phase shifter control inputs via electrical wires
- With default Vπ=5V: 0V = 0 phase, 2.5V = π/2 phase, 5V = π phase

## Usage Examples

### Basic Measurement
```javascript
const laser = rm.open("LASER-1");
laser.write(":WAV 1550nm");
laser.write(":POW 0dBm");
laser.write(":OUTP ON");

const pm = rm.open("PM-1");
const power = parseFloat(pm.query(":POW?"));
console.log(`Measured power: ${power} dBm`);
```

### Controlling Phase Shifter via DAC
```javascript
const laser = rm.open("LASER-1");
laser.write(":WAV 1550nm");
laser.write(":POW 0dBm");
laser.write(":OUTP ON");

// Set DAC voltage to control phase shifter
const dac = rm.open("DAC-1");
dac.write(":CH1 5V");  // π phase shift (with Vπ=5V)

const pm = rm.open("PM-1");
const power = parseFloat(pm.query(":POW?"));
```

### Wavelength Sweep
```javascript
function findResonance() {
  const laser = rm.open("LASER-1");
  const pm = rm.open("PM-1");

  laser.write(":POW 0dBm");
  laser.write(":OUTP ON");

  let minPower = Infinity;
  let resonanceWav = null;

  for (let wav = 1545; wav <= 1555; wav += 0.1) {
    laser.write(`:WAV ${wav}nm`);
    const power = parseFloat(pm.query(":POW?"));
    if (power < minPower) {
      minPower = power;
      resonanceWav = wav;
    }
  }

  return resonanceWav;
}
```

## Helper Functions

Available in script environment:

```javascript
console.log(...)    // Debug output (shown in results)
parseFloat(str)     // Parse number from response
condition           // Object containing test conditions

// Plotting function
plot(xData, yData, options)  // Create a plot in the results panel
// Options: { title: "...", xlabel: "...", ylabel: "..." }
```

### Plotting Example

```javascript
const wavelengths = [];
const powers = [];

for (let wav = 1545; wav <= 1555; wav += 0.1) {
    laser.write(`:WAV ${wav}nm`);
    const power = parseFloat(pm.query(":POW?"));
    wavelengths.push(wav);
    powers.push(power);
}

plot(wavelengths, powers, {
    title: "Ring Resonator Transmission Spectrum",
    xlabel: "Wavelength (nm)",
    ylabel: "Power (dBm)"
});
```

## Notes

- All query commands end with `?`
- Numeric responses include units (e.g., `"1550.00nm"`, `"2.50V"`)
- Use `parseFloat()` to extract numeric values
- Instruments are connected to circuit components by matching instrument IDs
- Instrument IDs auto-increment (LASER-1, LASER-2, PM-1, PM-2, DAC-1, etc.)
