# Raspberry Pi 5 — LED Blink & HC-SR04 Ultrasonic Distance Sensor

Production-ready embedded C project using **WiringPi** on **Raspberry Pi 5**. This README is written so you can **directly paste** it as `README.md` and follow step-by-step using **nano**.

---

## Project Overview

This project demonstrates:

* GPIO output control (LED blink)
* GPIO input/output timing (HC-SR04 ultrasonic sensor)
* Real-time distance measurement
* Clean compilation and execution workflow on Raspberry Pi 5

All programs are written in **C**, compiled with **gcc**, and tested on real hardware.

---

## Hardware Used

* Raspberry Pi 5
* Breadboard
* LED (5 mm)
* 220 Ω resistor
* HC-SR04 Ultrasonic Sensor
* Male–male jumper wires
* 5 V Raspberry Pi power supply

---

## GPIO Pin Connections (BCM Numbering)

### LED

| Signal | Raspberry Pi Pin |
| ------ | ---------------- |
| LED +  | GPIO 17 (Pin 11) |
| LED −  | GND (Pin 6)      |

Connection:

```
GPIO17 → 220Ω → LED Anode
LED Cathode → GND
```

---

### HC-SR04 Sensor

| HC-SR04 Pin | Raspberry Pi     |
| ----------- | ---------------- |
| VCC         | 5V (Pin 2 / 4)   |
| GND         | GND (Pin 6)      |
| TRIG        | GPIO 17 (Pin 11) |
| ECHO        | GPIO 27 (Pin 13) |

Note: GPIO ECHO is 3.3 V tolerant on Raspberry Pi 5, but use a voltage divider if required for safety.

---

## Software Requirements

* Raspberry Pi OS (64-bit recommended)
* GCC compiler
* WiringPi library

---

## System Preparation

Update system:

```bash
sudo apt update
sudo apt upgrade -y
```

Install build tools:

```bash
sudo apt install -y build-essential git
```

Verify GCC:

```bash
gcc --version
```

---

## WiringPi Installation

Clone and install WiringPi:

```bash
git clone https://github.com/WiringPi/WiringPi.git
cd WiringPi
./build
```

Verify installation:

```bash
gpio -v
```

---

## Project Directory Structure

Create project folder:

```bash
cd ~
mkdir Raspberry-Pi-LED-Ultrasonic-Project
cd Raspberry-Pi-LED-Ultrasonic-Project
mkdir src photos docs
```

---

## Using nano (Important)

### Create a file

```bash
nano src/led_blink.c
```

### Save and exit nano

* Save: `CTRL + O` → Enter
* Exit: `CTRL + X`

---

## Program 1: LED Blink (src/led_blink.c)

```c
#include <wiringPi.h>
#include <stdio.h>
#include <unistd.h>

#define LED_PIN 17

int main(void)
{
    if (wiringPiSetupGpio() == -1)
    {
        printf("WiringPi setup failed\n");
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
```

---

## Program 2: Ultrasonic Sensor (src/ultrasonic_sensor.c)

```c
#include <wiringPi.h>
#include <stdio.h>
#include <time.h>
#include <unistd.h>

#define TRIG_PIN 17
#define ECHO_PIN 27

float measureDistance()
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
    float distance = (duration * 0.0343) / 2.0;

    return distance;
}

int main(void)
{
    if (wiringPiSetupGpio() == -1)
    {
        printf("WiringPi setup failed\n");
        return 1;
    }

    pinMode(TRIG_PIN, OUTPUT);
    pinMode(ECHO_PIN, INPUT);

    while (1)
    {
        float d = measureDistance();
        printf("Distance: %.2f cm\n", d);
        sleep(1);
    }

    return 0;
}
```

---

## Compilation

From project root:

```bash
cd ~/Raspberry-Pi-LED-Ultrasonic-Project
```

Compile LED program:

```bash
gcc src/led_blink.c -o led_blink -lwiringPi
```

Compile ultrasonic program:

```bash
gcc src/ultrasonic_sensor.c -o ultrasonic_sensor -lwiringPi
```

---

## Running the Programs

GPIO access **requires sudo**.

### Run LED blink

```bash
sudo ./led_blink
```

### Run ultrasonic sensor

```bash
sudo ./ultrasonic_sensor
```

Stop execution:

```text
CTRL + C
```

---

## Common Errors & Fixes

### Permission denied

```bash
chmod +x led_blink ultrasonic_sensor
```

### WiringPi not found

```bash
sudo apt install wiringpi
```

### LED not glowing

* Check LED polarity
* Check 220 Ω resistor
* Confirm GPIO 17 connection

---

## Learning Outcomes

* GPIO configuration using WiringPi
* Digital input/output control
* Ultrasonic time-of-flight measurement
* Embedded C compilation on Linux
* Hardware–software integration

---

## Author

**Ram Axay**
Embedded Systems & Firmware Engineer
VGEC Ahmedabad

GitHub: [https://github.com/AxayRam](https://github.com/AxayRam)

---

## License

MIT License

---

This README is **ready to paste** and use directly with `nano` on Raspberry Pi 5.
