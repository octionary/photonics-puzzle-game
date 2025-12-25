# Instrument API

## Resource Manager

```javascript
const rm = {
  open(name) {
    // Returns instrument handle
    // e.g., rm.open("LASER-1"), rm.open("PM-1")
  }
};
```

## Laser (LASER-1)

### Commands
| Command | Description | Example |
|---------|-------------|---------|
| `:WAV <value>` | Set wavelength | `:WAV 1550nm`, `:WAV 193.4THz` |
| `:WAV?` | Query wavelength | Returns `"1550.00nm"` |
| `:POW <value>` | Set power | `:POW 0dBm`, `:POW 1mW` |
| `:POW?` | Query power | Returns `"0.00dBm"` |
| `:OUTP ON\|OFF` | Enable/disable output | `:OUTP ON` |
| `:OUTP?` | Query output state | Returns `"ON"` or `"OFF"` |

### Sweep Commands
| Command | Description |
|---------|-------------|
| `:WAV:SWE:STAR <nm>` | Set sweep start |
| `:WAV:SWE:STOP <nm>` | Set sweep stop |
| `:WAV:SWE:STEP <pm>` | Set sweep step |
| `:WAV:SWE:EXEC` | Execute sweep (blocking) |

## Power Meter (PM-1, PM-2, ...)

### Commands
| Command | Description | Example |
|---------|-------------|---------|
| `:POW?` | Measure power | Returns `"-3.01dBm"` |
| `:POW:UNIT DBM\|MW\|W` | Set unit | `:POW:UNIT MW` |
| `:POW:AVG <count>` | Set averaging | `:POW:AVG 10` |

## Usage Example

```javascript
// Basic measurement
const laser = rm.open("LASER-1");
laser.write(":WAV 1550nm");
laser.write(":POW 0dBm");
laser.write(":OUTP ON");

const pm = rm.open("PM-1");
const power = parseFloat(pm.query(":POW?"));
console.log(`Measured power: ${power} dBm`);

// Wavelength sweep
function findResonance() {
  const laser = rm.open("LASER-1");
  const pm = rm.open("PM-1");

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
sleep(ms)           // Pause execution (for sweeps)
console.log(...)    // Debug output
parseFloat(str)     // Parse number from response
```

## Notes

- All query commands end with `?`
- Numeric responses include units (e.g., `"1550.00nm"`)
- Use `parseFloat()` to extract numeric values
- Instruments are pre-connected to circuit components by name
