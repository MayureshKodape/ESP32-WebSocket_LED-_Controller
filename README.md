# ESP32 WebSocket LED Controller (ESP-IDF)

## Project Detail
This project implements a **WebSocket Echo Server** on an **ESP32 / ESP32-S3** using **ESP-IDF**.  
The ESP32 connects to a Wi-Fi network in **STA mode**, starts an **HTTP server**, and upgrades the `/ws` endpoint to a **WebSocket** connection.

When a client sends a message:
- The ESP32 **receives** the WebSocket frame
- **Echoes back** the same message (confirmation + debugging)
- Controls the LED based on commands:
  - `start` → LED **ON**
  - `stop`  → LED **OFF**

This project is useful as a base for:
- IoT dashboards / local web control
- Real-time device debugging
- Home automation triggers

---

## Features
- WebSocket server on ESP32 / ESP32-S3
- Echo functionality (server replies with received message)
- LED control using simple commands
- Works with Postman WebSocket, browser WebSocket clients, and Python

---

## Requirements
- ESP32 / ESP32-S3 development board
- ESP-IDF installed and configured (`idf.py` works)
- Wi-Fi network (2.4 GHz)
- WebSocket client (Postman / WebSocket King / browser extension / Python)

---

## How It Works (Flow)
1. ESP32 boots and initializes NVS
2. ESP32 connects to Wi-Fi (STA)
3. After getting IPv4 address, HTTP server starts
4. WebSocket endpoint is available at `/ws`
5. Client connects to: `ws://<ESP32_IP>/ws`
6. Client sends `start` / `stop`
7. ESP32 toggles LED + echoes message back

---

## WebSocket API
**Endpoint**

**Commands**
| Command | Action |
|--------|--------|
| `start` | LED ON |
| `stop`  | LED OFF |

---

## How to Run (Step-by-Step)

>  Run all commands inside the project folder (where `CMakeLists.txt` exists)

### 1) Clone
```bash
git clone https://github.com/MayureshKodape/ESP32-WebSocket_LED-_Controller.git
cd ESP32-WebSocket_LED-_Controller
2) Set Target (run once)
idf.py set-target esp32
# OR
idf.py set-target esp32s3

3) Configure Wi-Fi
idf.py menuconfig


Go to: Example Connection Configuration → set WiFi SSID + WiFi Password (case-sensitive) → Save & Exit.

4) Build
idf.py build

5) Flash
idf.py flash

6) Monitor
idf.py monitor


Find IP in logs:

STA IP: XXX.XXX.XXX.XXX

7) Connect WebSocket

Use this URL in Postman WebSocket / browser WebSocket client:

ws://<ESP32_IP>/ws


Send:

start → LED ON

stop → LED OFF

Example Python Client
import websocket

ws = websocket.WebSocket()
ws.connect("ws://<ESP32_IP>/ws")

ws.send("start")
print(ws.recv())  # echo

ws.send("stop")
print(ws.recv())  # echo

ws.close()


Install:

pip install websocket-client

Notes

ESP32 and client must be on the same Wi-Fi network

If LED doesn’t toggle, change LED GPIO pin in code (board LEDs differ)
