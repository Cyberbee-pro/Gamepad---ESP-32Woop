# ESP32 Native Bluetooth LE Gamepad

An open-source, high-performance wireless controller firmware built natively for the ESP32 platform. This project leverages the dual-core Tensilica Xtensa architecture to achieve sub-millisecond polling loops across four analog channels while maintaining a plug-and-play Bluetooth Low Energy (BLE) HID interface.

## System Architecture

* **Native BLE HID:** Automatically registers as a standard wireless gamepad on Windows, Linux, Android, and iOS. No intermediate driver or mapping software required on the host machine.
* **Direct Analog Sampling:** Utilizes the internal Successive Approximation Register (SAR) ADC1 bank to sample dual thumbstick potentiometers directly at 12-bit resolution ($0$ to $4095$).
* **Vector Deadzone DSP:** Implements a true radial/circular deadzone algorithm ($\sqrt{X^2 + Y^2}$) to filter center noise uniformly, preventing stick drift without forcing axial snapping or square clipping.

## Hardware Pin Allocation Matrix

| Peripherals | Signal / Axis | ESP32 GPIO Pin | Operation Mode |
| :--- | :--- | :--- | :--- |
| **Left Joystick** | X-Axis (Horizontal) | `GPIO34` | Analog Input (ADC1_CH6) |
| | Y-Axis (Vertical) | `GPIO35` | Analog Input (ADC1_CH7) |
| | L3 Click Switch | `GPIO25` | Digital Input (External Pull-Down) |
| **Right Joystick**| X-Axis (Horizontal) | `GPIO32` | Analog Input (ADC1_CH4) |
| | Y-Axis (Vertical) | `GPIO33` | Analog Input (ADC1_CH5) |
| | R3 Click Switch | `GPIO26` | Digital Input (External Pull-Down) |
| **D-Pad Array** | Up / Down / Left / Right | `GPIO12`, `GPIO13`, `GPIO14`, `GPIO27` | Digital Input (External Pull-Down) |
| **Action Matrix** | A / B / X / Y | `GPIO16`, `GPIO17`, `GPIO18`, `GPIO19` | Digital Input (External Pull-Down) |
| **System Buttons**| Start / Select | `GPIO21`, `GPIO22` | Digital Input (External Pull-Down) |
| **Triggers** | L1 Bumper / R1 Bumper | `GPIO23`, `GPIO4` | Digital Input (External Pull-Down) |

## Hardware Prototype Simulation (Wokwi)

To validate the multi-channel ADC reading sequences and button input states before flashing physical hardware, drop this layout matrix into the `diagram.json` tab of a Wokwi ESP32 workspace:

```json
{
  "version": 1,
  "author": "Shibraj Das",
  "editor": "wokwi",
  "parts": [
    { "type": "wokwi-esp32-devkit-v1", "id": "esp32", "top": 0, "left": 0, "attrs": {} },
    { "type": "wokwi-analog-joystick", "id": "joyLeft", "top": -120, "left": -160, "attrs": {} },
    { "type": "wokwi-analog-joystick", "id": "joyRight", "top": -120, "left": 160, "attrs": {} }
  ],
  "connections": [
    [ "esp32:GND.1", "joyLeft:GND", "black", [] ],
    [ "esp32:3V3", "joyLeft:VCC", "red", [] ],
    [ "esp32:GPIO34", "joyLeft:HORIZ", "blue", [] ],
    [ "esp32:GPIO35", "joyLeft:VERT", "green", [] ],
    
    [ "esp32:GND.2", "joyRight:GND", "black", [] ],
    [ "esp32:3V3", "joyRight:VCC", "red", [] ],
    [ "esp32:GPIO32", "joyRight:HORIZ", "blue", [] ],
    [ "esp32:GPIO33", "joyRight:VERT", "green", [] ]
  ]
}

---
Maintained under the MIT License.