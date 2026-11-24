# XIAO ESP32-C3 + HLK-LD2410B Presence Sensor (ESPHome)

A compact, local mmWave presence sensor built with a **Seeed Studio XIAO ESP32-C3** and a **HI-LINK HLK-LD2410B** 24 GHz radar module, running **ESPHome** (ESP-IDF) and integrated with **Home Assistant**.

## Files Included

- [`xiao-presence.yaml`](xiao-presence.yaml): Complete sanitized ESPHome configuration with occupancy debounce filters, moving/stationary target sensors, per-gate energy sensors and thresholds (Gates 0-8), Radar Engineering Mode toggle, diagnostics, and maintenance buttons.
- [`secrets.example.yaml`](secrets.example.yaml): Template for your local `secrets.yaml` credentials.

## Hardware

- **Seeed Studio XIAO ESP32-C3**
- **HI-LINK HLK-LD2410B** (24 GHz mmWave radar module)
- **USB-C cable & 5 V USB power supply** (data-capable cable for initial flashing)
- **Four connecting wires**
- **3D-printed enclosure**

## Wiring (USB-C Powered)

| HLK-LD2410B Pin | Function | Seeed Studio XIAO ESP32-C3 |
| :--- | :--- | :--- |
| `VCC` / `5V` | 5 V DC Power Input (> 200 mA) | `5V` |
| `GND` | Ground | `GND` |
| `TX` | Serial Transmit (3.3 V logic) | `D2` / `GPIO4` (`rx_pin`) |
| `RX` | Serial Receive (3.3 V logic) | `D3` / `GPIO5` (`tx_pin`) |
| `OUT` | Digital Presence Output | Unconnected |

> Disconnect power before wiring, verify your board provides ~5 V between `5V` and `GND` under USB power, and keep the 5 V line away from `3V3` and GPIO pins.

## Quick Setup

1. Copy `secrets.example.yaml` to `secrets.yaml` and fill in your 2.4 GHz Wi-Fi credentials, OTA password, fallback AP password, and a freshly generated 32-byte base64 ESPHome API encryption key:
   ```bash
   python3 -c "import os, base64; print(base64.b64encode(os.urandom(32)).decode())"
   ```
2. Compile `xiao-presence.yaml` in **ESPHome Device Builder** and download the installation `.bin` file.
3. Flash the initial `.bin` over USB-C using [ESPHome Web](https://web.esphome.io/) in Chrome or Edge.
   - If the board needs help entering download mode: hold **BOOT**, press and release **RESET**, then release **BOOT**.
4. Reset or reconnect USB so the device connects to Wi-Fi. Future updates can be pushed over the air (OTA).
5. Add the device in Home Assistant under **Settings -> Devices & services -> ESPHome** using its IP address and your `api_encryption_key`.

## Segmented Network Note

If Home Assistant and your IoT Wi-Fi live on separate subnets, allow **TCP port `6053`** from your Home Assistant host to the presence sensor (with return traffic permitted) so Home Assistant can reach the ESPHome native API.
