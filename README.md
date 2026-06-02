# amalgame-hardware-input

Portable **input drivers** (debounced button, rotary encoder, matrix
keypad) for Amalgame over [`amalgame-hal`](https://github.com/amalgame-lang/amalgame-hal) — the controls layer for menus,
settings, e-stops, jog wheels, PIN pads. Runs on Raspberry Pi and future MCU
backends from one codebase.

```sh
amc package add hardware-input
```

```amalgame
import Amalgame.Hardware          // GpioIn, SysClock (Pi backend)
import Amalgame.Hardware.Input

let clk = new SysClock()
let btn = new Button(new GpioIn(17), clk, true)   // active-low
let enc = new RotaryEncoder(new GpioIn(5), new GpioIn(6))

// loop:
btn.Update()
if (btn.WasPressed()) { /* … */ }
let step = enc.Poll()             // -1 / 0 / +1
```

| Class | API |
|---|---|
| `Button(in: DigitalIn, clk: Clock, activeLow)` | `Update()`, `IsPressed()`, `WasPressed()`, `SetDebounceMs(ms)` |
| `RotaryEncoder(a, b: DigitalIn)` | `Poll()` → -1 / 0 / +1 |
| `Keypad(rows: List<DigitalOut>, cols: List<DigitalIn>, keymap)` | `GetKey()` → 1-char string (`""` if none), `AnyPressed()` |

Requires amc ≥ 0.8.73 (matrix keypad needs `List<DigitalOut>` interface storage). Apache-2.0.
