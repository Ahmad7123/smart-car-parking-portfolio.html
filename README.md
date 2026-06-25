# Smart Car Parking System using IoT

![ESP32](https://img.shields.io/badge/ESP32-IoT-blue)
![Arduino](https://img.shields.io/badge/Arduino-C++-green)
![MQTT](https://img.shields.io/badge/MQTT-Enabled-orange)

## Overview


The Smart Car Parking System, an Internet of Things (IoT). The system addresses the growing urban challenge of inefficient parking space management through real-time automated monitoring and cloud-based data analytics.
Microcontroller with four HC-SR04 ultrasonic sensors, an OLED display, and a servo motor to create a fully automated parking management solution. Vehicle detection occurs with occupancy data transmitted securely to AWS using the MQTT over Wi-Fi connectivity.
Key outcomes include sub-2-second slot detection latency, 99%+ sensor accuracy at distances up to 400 cm. The solution demonstrates practical application of embedded systems, IoT protocols, cloud computing, and software engineering principles within a single integrated platform.

---
##Wiring Diagram

                                 +5V Rail
         ┌──────────────────┬──────────────────┬──────────────────┐
         │                  │                  │                  │
    ┌────┴────┐        ┌────┴────┐        ┌────┴────┐        ┌────┴────┐
    │ HC-SR04 │        │ HC-SR04 │        │ HC-SR04 │        │ HC-SR04 │
    │  Slot1  │        │  Slot2  │        │  Slot3  │        │  Slot4  │
    └─┬─────┬─┘        └─┬─────┬─┘        └─┬─────┬─┘        └─┬─────┬─┘
      │TRIG │ECHO        │TRIG │ECHO        │TRIG │ECHO        │TRIG │ECHO
    GPIO5 GPIO4        GPIO18 GPIO2       GPIO19 GPIO15      GPIO23 GPIO27
      │     │             │     │           │     │            │     │
      └─────┴─────────────┴─────┴───────┬───┴─────┴────────────┴─────┘
                                        │
                               ┌────────┴──────────┐
                               │       ESP32       │
                               │ (Main Controller) │
                               └───┬──────────┬────┘
                            SDA/SCL│          │PWM
                              GPIO21/22       GPIO13
                                   ▼          ▼
                           ┌──────────────┐ ┌──────────┐
                           │ SSD1306 OLED │ │  Servo   │
                           │   Display    │ │  Motor   │
                           └──────────────┘ └──────────┘
Schematic interconnection of sensors, OLED, and servo to the ESP32.

---

## Features

- Real-time parking monitoring
- Automatic gate control
- ESP32-based controller
- OLED display interface
- MQTT cloud communication
- Mobile dashboard monitoring
- Slot availability detection

---

## System Architecture

```text
                     ┌─────────────────┐
                     │   System Boot   │
                     └────────┬────────┘
                              ▼
                 ┌──────────────────────────┐
                 │ Initialize Wi-Fi, MQTT,  │
                 │ OLED, Servo, GPIO pins   │
                 └────────────┬─────────────┘
                              ▼
                 ┌──────────────────────────┐
                 │ Connect to AWS IoT Core  │
                 │ (TLS Mutual Auth)        │
                 └────────────┬─────────────┘
                              ▼
                      ┌──────────────────┐
                ┌────▶│    MAIN LOOP     │◀────────────────┐
                │     └────────┬─────────┘                  │
                │              ▼                            │
                │   ┌──────────────────────┐                │
                │   │ Read all 4 ultrasonic│                │
                │   │ sensors (distance)   │                │
                │   └──────────┬───────────┘                │
                │              ▼                            │
                │   ┌──────────────────────┐                │
                │   │ Classify each slot:  │                │
                │   │ distance < 10 cm?    │                │
                │   │ YES → Occupied       │                │
                │   │ NO → Available       │                │
                │   └──────────┬───────────┘                │
                │              ▼                            │
                │   ┌──────────────────────┐                │
                │   │ Compute Available =  │                │
                │   │ TOTAL_SLOTS-Occupied │                │
                │   └──────────┬───────────┘                │
                │              ▼                            │
                │      ┌──────────────-─┐                   │
                │      │ Available == 0?│                   │
                │      └───┬───────┬──-─┘                   │
                │       YES│       │NO                      │
                │          ▼       ▼                        │
                │  ┌────────────┐ ┌────────────────-┐       │
                │  │ OLED shows │ │ OLED shows      │       │
                │  │"PARKING    │ │ available slot  │       │
                │  │  FULL"     │ │ count           │       │
                │  └─────┬──────┘ └────────┬────────┘       │
                │        ▼                 ▼                │
                │  ┌────────────┐  ┌────────────────-┐      │
                │  │ Gate stays │  │ Servo opens gate│      │
                │  │ CLOSED     │  │ automatically   │      │
                │  └─────┬──────┘  └────────┬──────-─┘      │
                │        └──────────┬───────┘               │
                │                   ▼                       │
                │       ┌───────────────────────-──┐        │
                │       │ Build JSON payload &     │        │
                │       │ Publish via MQTT to      │        │
                │       │ AWS IoT Core             │        │
                │       └────────────┬─────────────┘        │
                │                    ▼                      │
                │       ┌──────────────────────--───┐       │
                │       │ Delay (poll interval)     │       │
                │       └────────────┬───────────-──┘       │
                └────────────────────┴──────────────────────┘

Main control-flow logic executed on every loop () iteration.
```

## Hardware Components

| Component | Quantity |
|------------|------------|
| ESP32 | 1 |
| Ultrasonic Sensor | 2 |
| Servo Motor | 1 |
| OLED Display | 1 |
| Breadboard | 1 |
| Jumper Wires | Several |

---

## Software Requirements

- Arduino IDE
- ESP32 Board Package
- MQTT Broker
- GitHub Pages

---

## Installation

### 1. Clone Repository

```bash
git clone https://github.com/Ahmad7123/smart-car-parking-portfolio.html.git
```

### 2. Install Libraries

- WiFi.h
- PubSubClient.h
- Adafruit SSD1306
- Adafruit GFX

### 3. Upload Code

Open Arduino IDE and upload the code to ESP32.

---


## Project Website

https://ahmad7123.github.io/smart-car-parking-portfolio.html/

https://github.com/kanza-12/smart-car-parking-portfolio

---

## Author

Ahmad Nawaz | Kanza Sohail 


Electronic & Computing Engineering Student

Skills:
- C++
- Arduino
- ESP32
- IoT Systems
- Embedded Systems
- MQTT
- Web Development

---

## License

This project is for educational and research purposes.
