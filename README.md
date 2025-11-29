# LUMA-Hack-2025
FlameLite Guardian is a fireless safety torch system combining high-power LEDs, laser signaling, and an alarm for emergency protection. Designed by students of Sacred Heart International School, it provides safe illumination, alerts, and visibility in hazardous or low-light situations.
# FlameLite Guardian — Fireless Safety Torch System

**Team:** CHRISBIN LIANA, CHRISBIN JAEDON, JOHAN DANIEO R

**Affiliation:** 11TH, 8TH, 7TH Standard, Sacred Heart International School, Tamilnadu, India.

---

## Project Overview

FlameLite Guardian is a compact, hand-held, **fireless safety torch** designed to provide emergency illumination and situational awareness without an open flame. It combines high-efficiency LEDs, a low-power laser pointer for signaling, an audible alarm, and environmental sensing to create a multi-functional personal safety device.

**Keywords:** fireless torch, emergency light, personal safety, LED torch, signal laser

---

## Table of contents

* [Project Overview](#project-overview)
* [Motivation](#motivation)
* [Features](#features)
* [Hardware Components](#hardware-components)
* [Software Structure](#software-structure)
* [Installation](#installation)
* [Usage](#usage)
* [Demo & Video Links](#demo--video-links)
* [Contributing](#contributing)
* [License](#license)

---

## Motivation

Open flames (matches, wicks) are hazardous in many environments (flammable liquids, forests, gas leaks). FlameLite Guardian provides a safer alternative for camping, emergency kits, industrial settings, and household use.

---

## Features

* High-output LED illumination with adjustable brightness (three modes)
* Laser pointer for long-distance signaling
* Audible alarm (siren / beep) for attention or scare-off effect
* USB‑C rechargeable battery with charging indicator
* Low-power sensors (temperature, ambient light) for auto-dimming and safety warnings
* Simple push-button user interface with long‑press and double‑tap gestures

---

## Hardware Components (suggested)

* Microcontroller: ESP32 or Arduino Nano 33
* LEDs: 1 × high-power 3W LED + 2 × 1W auxiliary LEDs
* Laser diode (class 2 or 3R) with current limiting
* Buzzer / Piezo speaker
* Battery: 18650 Li-ion or LiPo cell + charging circuit (TP4056)
* Ambient light sensor: BH1750 or photoresistor
* Temperature sensor: DS18B20 or LM35
* Power switch, push button, USB-C port, enclosure

---

## Software Structure

```
/ (repo root)
├─ README.md
├─ firmware/
│  ├─ arduino/
│  │  ├─ FlameLite.ino        # Arduino sketch (control logic)
│  │  └─ libraries/          # helper libs
│  └─ esp32/
│     ├─ main.cpp            # ESP32 C++ firmware
│     └─ platformio.ini
├─ docs/
│  └─ circuit-diagram.pdf
├─ mobile-demo/
│  └─ flutter-app/          # optional remote control app
└─ assets/
   └─ images/
```

---

## Sample code snippets

### Arduino (FlameLite.ino)

```cpp
// Minimal stub: toggle LED, beep on long-press
#include <Arduino.h>

const int ledPin = 5;    // LED output
const int buzzerPin = 12;
const int buttonPin = 2;

unsigned long pressStart = 0;

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
  pinMode(buttonPin, INPUT_PULLUP);
  digitalWrite(ledPin, LOW);
}

void loop() {
  int btn = digitalRead(buttonPin);
  if (btn == LOW) { // pressed
    if (pressStart == 0) pressStart = millis();
    unsigned long d = millis() - pressStart;
    if (d > 1000) {
      // long press: alarm
      tone(buzzerPin, 2000);
    } else {
      // short press: toggle LED
      digitalWrite(ledPin, HIGH);
    }
  } else {
    if (pressStart != 0) {
      // button released
      noTone(buzzerPin);
      digitalWrite(ledPin, LOW);
      pressStart = 0;
      delay(50);
    }
  }
}
```

### ESP32 (main.cpp) — Wi‑Fi status and simple HTTP endpoint

```cpp
#include <WiFi.h>
#include <WebServer.h>

const char* ssid = "your-ssid";
const char* password = "your-pass";
WebServer server(80);

void handleRoot() {
  server.send(200, "text/plain", "FlameLite Guardian: OK");
}

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) { delay(500); Serial.print("."); }
  Serial.println("WiFi connected");
  server.on("/", handleRoot);
  server.begin();
}

void loop() {
  server.handleClient();
}
```

---

## Installation (firmware)

1. Clone the repository: `git clone https://github.com/yourusername/FlameLite-Guardian.git`
2. Open `firmware/arduino/FlameLite.ino` in Arduino IDE (or open `esp32` in PlatformIO for ESP32 build).
3. Install dependencies (Wire, OneWire, DS18B20 library, BH1750, etc.).
4. Connect board and flash.

---

## Usage

* Short press the button: cycle LED modes (Low → Medium → High → Off)
* Long press (>1s): enable audible alarm
* Double tap: toggle laser on/off (use responsibly — follow local laser safety rules)
* Connect USB-C to recharge; LED indicates battery level during charging

---

## Demo & Video Links

* Devpost project page (primary demo): [https://devpost.com/software/flamelite-guardian-fireless-safety-torch-system](https://devpost.com/software/flamelite-guardian-fireless-safety-torch-system)

**Latest video / demo links**

*Pitech 60 Sec https://youtu.be/2ajoNjLY8l8
Prototype demonstration https://youtu.be/3PyKnLujNpI
Presentation Video https://youtu.be/e8O7swY7Znw
Live demo https://youtu.be/7fAvMzZEqE0

---

## Contributing

Contributions are welcome. Please open issues or pull requests with clear descriptions.

---

## License

MIT License — see `LICENSE` file.

---

## Contact

**Team lead:** Chrisbin Jeeva — [chrisbinjaedon2012@gmail.com](mailto:chrisbinjaedon2012@gmail.com)

*Generated README — edit details (team affiliation, exact hardware list, and video links) before publishing.*
