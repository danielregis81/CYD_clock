*[Leia em Português](README.pt-BR.md)*

# Digital Clock - ESP32 CYD 2.8"

Digital clock with weather forecast and alarms for the **ESP32 CYD** board (ESP32-2432S028R).

## Features

- **Captive portal** for WiFi configuration (no code editing)
- Time sync via NTP
- Display with large time (7-segment), seconds, full date
- **Weather forecast** via OpenWeatherMap (current + 3 days)
- **Up to 5 alarms** configurable via web interface
- Visual alarm: screen flashes yellow/red until you touch the display
- Alarm always fires at 100% brightness
- **Brightness control** with 4 levels (25%, 50%, 75%, 100%)
- **Two themes**: dark and light (toggle via button)
- Screen showing active alarms
- WebUI accessible via IP on local network (shows current settings)
- Geographic coordinates (lat/lon) for weather precision
- UTF-8 accent handling (displays without accents)
- Settings saved to flash (survives reboot)
- Long touch (3s): resets everything and returns to config mode

## How to use

### 1. Flash the board

```bash
pio run -t upload --upload-port COM6
```

### 2. Configure (first time)

1. The board creates WiFi network **Relogio-CYD** (password: `12345678`)
2. Connect your phone/PC to this network
3. Open **http://192.168.4.1** in browser
4. Fill in: SSID, password, city, latitude, longitude, timezone, API key and alarms
5. Click "Save and Connect"

### 3. Reconfigure (already connected)

Access the IP shown at the bottom of the display (e.g., `http://192.168.0.17`) via browser on the same network. The page shows current settings already filled in.

### 4. Display interaction

| Button | Action |
|---|---|
| **ESC/LIGHT** | Toggle dark/light theme |
| **AL** | Show active alarms screen |
| **75%** | Cycle brightness: 25% → 50% → 75% → 100% |
| **Hold 3s** | Reset config, return to AP mode |

### 5. Weather forecast

Create a free account at [openweathermap.org](https://openweathermap.org/api) and paste the API key in settings. Free tier: 1000 calls/day. The clock queries every 10 minutes.

For your city's coordinates, use [Google Maps](https://maps.google.com) (right-click → "What's here?" shows lat/lon).

## Hardware

- ESP32 CYD 2.8" (ESP32-2432S028R)
- ILI9341 240x320 display (integrated)
- XPT2046 resistive touch (integrated)

## Structure

```
CYD/
├── platformio.ini    # PlatformIO config + display pins
├── src/
│   └── main.cpp      # Main code
├── .gitignore
└── README.md
```

## Libraries

| Library | Function |
|---|---|
| TFT_eSPI | ILI9341 display driver |
| XPT2046_Touchscreen | Touch driver |
| ArduinoJson | OpenWeatherMap API parsing |
| WebServer | Web config interface |
| DNSServer | Captive portal |
| Preferences | Save config to flash |

## Build

```bash
pio run              # Compile
pio run -t upload    # Flash to board
pio device monitor   # Serial monitor (debug)
```
