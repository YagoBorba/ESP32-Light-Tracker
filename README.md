<a name="readme-top"></a>

<div align="center">

<h1 align="center">ESP32 Light Tracker</h1>

<br>

# Welcome to the ESP32 Light Tracker repository

---

<div align="center">

[![CI Status][ci-shield]][ci-link]
[![PlatformIO][platformio-shield]][platformio-link]
[![MIT License][license-shield]][license-link]

</div>

[ci-shield]: https://img.shields.io/github/actions/workflow/status/YagoBorba/ESP32-Light-Tracker/ci.yml?branch=main&style=flat-square&logo=githubactions&label=CI
[ci-link]: https://github.com/YagoBorba/ESP32-Light-Tracker/actions
[platformio-shield]: https://img.shields.io/badge/PlatformIO-ESP32-F5822A?style=flat-square&logo=platformio&logoColor=white
[platformio-link]: https://platformio.org/
[license-shield]: https://img.shields.io/github/license/YagoBorba/ESP32-Light-Tracker?style=flat-square&logo=github&labelColor=black&color=508CF9
[license-link]: https://github.com/YagoBorba/ESP32-Light-Tracker/blob/main/LICENSE

</br>

</div>


## ❤️ About the Project

> [!IMPORTANT]
> ESP32 Light Tracker is a dual-axis light tracker: an ESP32 reads four light sensors (LDRs) and moves two servos so a small panel or sensor follows the brightest direction.

The firmware is written in C++ with the Arduino framework and built with PlatformIO. A push button cycles between automatic tracking, manual control over Bluetooth and a fixed safe position. The whole circuit can be simulated in [Wokwi](https://wokwi.com/) with the diagram included in the repository.

## ✨ Features

- 🔍 **Startup sweep:** at power-on the horizontal servo sweeps from 10° to 170° and stops where the light reading is highest.
- ☀️ **Automatic tracking:** the four LDRs are averaged in pairs (top, bottom, left, right). When a side drops below the reference level, the matching servo moves 2° toward the light.
- 📶 **Manual control over Bluetooth:** the board shows up as `ESP32-ServoBot` (Bluetooth Classic, serial). Each command moves a servo 2°.
- 🆘 **Safe position:** moves both servos to a fixed, predefined position (94° horizontal, 118° vertical).
- 📟 **Serial telemetry:** every 100 ms the four LDR readings, both servo angles and the current mode are printed at 115200 baud.

### Operating modes

The push button on GPIO 33 cycles through the modes:

| Press | Mode |
|---|---|
| — (power-on) | Automatic tracking |
| 1st | Manual control over Bluetooth |
| 2nd | Safe position |
| 3rd | Restarts the board (back to automatic tracking) |

### Bluetooth commands

Pair with `ESP32-ServoBot` and send single characters from any Bluetooth serial terminal app:

| Command | Action |
|---|---|
| `1` | Vertical servo −2° |
| `2` | Vertical servo +2° |
| `3` | Horizontal servo +2° |
| `4` | Horizontal servo −2° |

## 🛠️ Under the Hood (Main Technologies)

- **[ESP32](https://www.espressif.com/en/products/socs/esp32)** (DOIT ESP32 DevKit V1) with the Arduino framework.
- **[PlatformIO](https://platformio.org/):** build, dependencies and upload.
- **[ESP32Servo](https://github.com/madhephaestus/ESP32Servo)** and **BluetoothSerial:** servo control and Bluetooth Classic serial.
- **[Wokwi](https://wokwi.com/):** circuit simulation (`diagram.json` and `wokwi.toml`).
- **GitHub Actions:** every push and pull request compiles the firmware.

## 🔌 Hardware

- 1 × DOIT ESP32 DevKit V1
- 4 × LDRs, each in a voltage divider with a resistor (10 kΩ is a common choice)
- 2 × hobby servos (for example SG90 or MG90S), preferably on their own 5 V supply with a common ground
- 1 × push button

| Component | ESP32 pin |
|---|---|
| LDR top right | GPIO 14 |
| LDR top left | GPIO 27 |
| LDR bottom right | GPIO 26 |
| LDR bottom left | GPIO 25 |
| Push button | GPIO 33 |
| Horizontal servo (signal) | GPIO 19 |
| Vertical servo (signal) | GPIO 18 |

Pins, the reference level (`REFERENCIA_LDR`), the step (`INCREMENTO_ANGULO`) and the delay between steps (`PAUSA_MOVIMENTO`) are defined in [`include/config.h`](include/config.h).

## 🚀 Getting Started

### Prerequisites

- [PlatformIO](https://platformio.org/install) (CLI or the VS Code extension)

### Build and upload

```bash
git clone https://github.com/YagoBorba/ESP32-Light-Tracker.git
cd ESP32-Light-Tracker
pio run                 # compile; PlatformIO downloads the libraries in platformio.ini
pio run -t upload       # flash the board
pio device monitor -b 115200
```

### Simulate in Wokwi

1. Build the firmware with `pio run`.
2. Open the folder in VS Code with the [Wokwi extension](https://docs.wokwi.com/vscode/getting-started).
3. Run **Wokwi: Start Simulator**. It loads `diagram.json` and the firmware from `.pio/build/esp32doit-devkit-v1/`.

Bluetooth is not simulated in Wokwi; tracking, the button and the serial output are.

## 📁 Project Structure

```text
.
├── include/
│   ├── bluetooth.h   # Bluetooth command handling
│   ├── config.h      # Pins and tracking parameters
│   ├── ldr.h         # LDR helpers
│   ├── servos.h      # Servo movement and tracking
│   └── utils.h       # Serial telemetry
├── src/
│   ├── bluetooth.cpp
│   ├── ldr.cpp
│   ├── main.cpp      # Setup, mode selection and main loop
│   ├── servos.cpp
│   └── utils.cpp
├── diagram.json      # Wokwi circuit
├── platformio.ini
└── wokwi.toml
```

## 🤝 Want to Contribute?

Contributions are welcome. Create a branch, make sure `pio run` compiles, and open a pull request describing the change and how you tested it (on hardware or in Wokwi).

## 📝 License

This project is licensed under the **MIT License**. See [LICENSE](LICENSE) for more details.

## 📚 References

- [ESP32 Documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/)
- [ESP32Servo library](https://github.com/madhephaestus/ESP32Servo)
- [Servo Motor Control with ESP32](https://randomnerdtutorials.com/esp32-servo-motor-web-server/)
- [Bluetooth Communication with ESP32](https://randomnerdtutorials.com/esp32-bluetooth-classic-arduino-ide/)

---

<div align="right">
    <a href="#readme-top">Back to Top</a>
</div>
