# Raspberry Pi 5
## LED Blink & HC-SR04 Ultrasonic Distance Sensor
**Embedded C | GPIO Programming | WiringPi | Linux**

---

## 📌 Project Overview

This project demonstrates **low-level GPIO control and real-time sensor interfacing**
on **Raspberry Pi 5** using **Embedded C** and the **WiringPi** library.

The project follows a **professional embedded systems workflow** and is suitable for:
- Embedded Systems practice
- Firmware fundamentals
- Raspberry Pi GPIO learning
- Academic labs and technical interviews

All programs are written in **C**, compiled using **gcc**, and tested on real
Raspberry Pi 5 hardware.

---

## 🎯 Features

- GPIO output control (LED blinking)
- GPIO input pulse measurement (HC-SR04)
- Ultrasonic distance calculation using Time-of-Flight
- BCM GPIO numbering
- nano-based development workflow
- Clean compilation and execution on Linux

---

## 🧰 Hardware Requirements

| Component | Specification |
|---------|---------------|
| Raspberry Pi | Raspberry Pi 5 |
| LED | 5 mm |
| Resistor | 220 Ω |
| Ultrasonic Sensor | HC-SR04 |
| Breadboard | Standard |
| Jumper Wires | Male–Male |
| Power Supply | 5 V (Official Recommended) |

---

## 🔌 GPIO Pin Configuration (BCM Numbering)

### LED Wiring

| Signal | GPIO | Physical Pin |
|------|------|--------------|
| LED Anode (+) | GPIO 17 | Pin 11 |
| LED Cathode (−) | GND | Pin 6 |

Connection:
GPIO17 → 220Ω → LED(+)
LED(−) → GND

yaml
Copy code

---

### HC-SR04 Ultrasonic Sensor Wiring

| Sensor Pin | GPIO | Physical Pin |
|-----------|------|--------------|
| VCC | 5 V | Pin 2 / 4 |
| GND | GND | Pin 6 |
| TRIG | GPIO 17 | Pin 11 |
| ECHO | GPIO 27 | Pin 13 |

⚠️ **Important Note:**  
HC-SR04 ECHO outputs **5 V**, while Raspberry Pi GPIO operates at **3.3 V**.  
For production-safe designs, use a **voltage divider** on the ECHO pin.

---

## 💻 Software Requirements

- Raspberry Pi OS (64-bit recommended)
- GCC Compiler
- WiringPi Library
- nano Text Editor

---

## 🔧 System Setup

Update system:
```bash
sudo apt update
sudo apt upgrade -y
Install build tools:

bash
Copy code
sudo apt install -y build-essential git
Verify GCC:

bash
Copy code
gcc --version
📦 WiringPi Installation
bash
Copy code
git clone https://github.com/WiringPi/WiringPi.git
cd WiringPi
./build
Verify installation:

bash
Copy code
gpio -v
📁 Project Directory Structure
css
Copy code
Raspberry-Pi-LED-Ultrasonic-Project/
│
├── README.md
├── src/
│   ├── led_blink.c
│   └── ultrasonic_sensor.c
│
├── docs/
└── photos/
✍️ Using nano Editor
Create a file:

bash
Copy code
nano src/led_blink.c
Save and exit:

Save → CTRL + O → Enter

Exit → CTRL + X

🧪 Program 1: LED Blink
File: src/led_blink.c

c
Copy code
#include <wiringPi.h>
#include <stdio.h>
#include <unistd.h>

#define LED_PIN 17

int main(void)
{
    if (wiringPiSetupGpio() == -1)
    {
        printf("Error: WiringPi initialization failed\n");
        return 1;
    }

    pinMode(LED_PIN, OUTPUT);

    for (int i = 0; i < 10; i++)
    {
        digitalWrite(LED_PIN, HIGH);
        printf("LED ON\n");
        sleep(1);

        digitalWrite(LED_PIN, LOW);
        printf("LED OFF\n");
        sleep(1);
    }

    return 0;
}
📏 Program 2: Ultrasonic Distance Measurement
File: src/ultrasonic_sensor.c

c
Copy code
#include <wiringPi.h>
#include <stdio.h>
#include <time.h>
#include <unistd.h>

#define TRIG_PIN 17
#define ECHO_PIN 27

float measureDistance(void)
{
    digitalWrite(TRIG_PIN, HIGH);
    delayMicroseconds(10);
    digitalWrite(TRIG_PIN, LOW);

    while (digitalRead(ECHO_PIN) == LOW);

    struct timespec start, end;
    clock_gettime(CLOCK_MONOTONIC, &start);

    while (digitalRead(ECHO_PIN) == HIGH);

    clock_gettime(CLOCK_MONOTONIC, &end);

    long duration = (end.tv_nsec - start.tv_nsec) / 1000;
    return (duration * 0.0343f) / 2.0f;
}

int main(void)
{
    if (wiringPiSetupGpio() == -1)
    {
        printf("Error: WiringPi initialization failed\n");
        return 1;
    }

    pinMode(TRIG_PIN, OUTPUT);
    pinMode(ECHO_PIN, INPUT);

    while (1)
    {
        float distance = measureDistance();
        printf("Distance: %.2f cm\n", distance);
        sleep(1);
    }

    return 0;
}
⚙️ Compilation
bash
Copy code
cd ~/Raspberry-Pi-LED-Ultrasonic-Project

gcc src/led_blink.c -o led_blink -lwiringPi
gcc src/ultrasonic_sensor.c -o ultrasonic_sensor -lwiringPi
▶️ Execution
⚠️ GPIO access requires sudo

bash
Copy code
sudo ./led_blink
sudo ./ultrasonic_sensor
Stop program:

objectivec
Copy code
CTRL + C
🛠 Troubleshooting
WiringPi not found

bash
Copy code
sudo apt install wiringpi
Permission denied

bash
Copy code
chmod +x led_blink ultrasonic_sensor
LED not glowing

Check LED polarity

Verify 220 Ω resistor

Confirm GPIO 17 connection

📚 Learning Outcomes
GPIO configuration using WiringPi

Embedded C programming on Linux

Ultrasonic sensor timing & distance calculation

Hardware–software integration

Professional embedded workflow

👤 Author
Ram Axay
Embedded Systems & Firmware Engineer


GitHub: https://github.com/AxayRam

📄 License
MIT License — Free to use, modify, and distribute.

✅ Status
✔ Tested on Raspberry Pi 5
✔ Hardware verified
✔ Interview & resume ready


