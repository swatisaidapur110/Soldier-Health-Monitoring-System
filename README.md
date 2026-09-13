# Soldier Health Monitoring System

## 📌 Overview

The **Soldier Health Monitoring System** is an embedded health-monitoring solution designed to continuously monitor important physiological parameters in real time. The system is intended for soldiers and personnel operating in physically demanding, remote, or high-risk environments where timely detection of health abnormalities can improve safety and emergency response.

The system uses an **STM32 microcontroller** to interface with health sensors and an LCD display. Temperature and pulse-related sensor readings are acquired, processed, and displayed in real time. An alert mechanism using a buzzer is activated when predefined threshold conditions are detected.

The proposed system can be further extended with **GPS tracking, wireless communication, data logging, and additional biomedical sensors** to enable remote health monitoring and centralized emergency response.

---

## 🎯 Objectives

* Monitor health-related parameters continuously in real time.
* Measure body/environment temperature using temperature sensors.
* Acquire pulse sensor readings for monitoring heart activity.
* Detect abnormal sensor readings using predefined thresholds.
* Generate an immediate audible alert using a buzzer.
* Display sensor information through a 16×2 LCD.
* Develop a compact and portable embedded monitoring system.
* Provide a foundation for future GPS and wireless remote-monitoring capabilities.

---

## ✨ Features

* **Real-time sensor monitoring**
* **STM32-based embedded processing**
* **Temperature monitoring using LM35**
* **Pulse sensor interfacing**
* **DHT11 temperature and humidity monitoring**
* **16×2 LCD-based user interface**
* **Threshold-based emergency alert**
* **Buzzer notification system**
* **Serial communication for debugging/monitoring**
* Portable and extendable architecture

---

## 🏗️ System Architecture

```text
              ┌─────────────────────┐
              │      Sensors        │
              │                     │
              │  LM35 Temperature   │
              │  Pulse Sensor       │
              │  DHT11              │
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │   STM32 Controller  │
              │                     │
              │  Sensor Acquisition│
              │  Data Processing    │
              │  Threshold Checking │
              └───────┬───────┬─────┘
                      │       │
             ┌────────┘       └─────────┐
             ▼                          ▼
    ┌─────────────────┐        ┌─────────────────┐
    │   16×2 LCD      │        │     Buzzer      │
    │                 │        │                 │
    │ Temperature     │        │ Emergency Alert │
    │ Humidity        │        │                 │
    │ Pulse Reading   │        └─────────────────┘
    └─────────────────┘
```

---

## 🔧 Hardware Components

| Component                   | Purpose                                                |
| --------------------------- | ------------------------------------------------------ |
| **STM32 Microcontroller**   | Main controller for sensor interfacing and processing  |
| **LM35 Temperature Sensor** | Measures temperature through an analog output          |
| **Pulse/Heartbeat Sensor**  | Provides an analog pulse-related signal                |
| **DHT11 Sensor**            | Measures temperature and humidity                      |
| **16×2 LCD**                | Displays sensor readings                               |
| **Buzzer**                  | Provides an audible warning during abnormal conditions |
| **Power Supply**            | Provides power to the embedded system                  |
| **GPS Module**              | Planned extension for location tracking                |

---

## 💻 Software Requirements

* **Arduino IDE**
* **Embedded C / Arduino-compatible C/C++**
* STM32 board support package
* `LiquidCrystal` library
* `DHT` library

---

## ⚙️ Methodology

### 1. Sensor Data Acquisition

The STM32 microcontroller periodically reads data from the connected sensors.

The **LM35** produces an analog voltage proportional to temperature. The STM32 ADC converts this analog signal into a digital value, which is then converted into a temperature reading.

The **pulse sensor** is connected to an analog input and provides a pulse-related analog signal. The current implementation displays this raw sensor value.

The **DHT11** provides temperature and humidity measurements through a digital interface.

---

### 2. Data Processing

The STM32 processes the acquired sensor values.

For the LM35, the ADC value is converted into voltage:

```text
Voltage = ADC Value × (3.3 / 4095)
```

The LM35 temperature is then calculated using:

```text
Temperature (°C) = Voltage × 100
```

The DHT11 library is used to obtain temperature and humidity values.

---

### 3. LCD Display

The 16×2 LCD provides a simple real-time interface.

The current display format contains:

```text
T1: Temperature
H : Humidity

T2: Temperature
P : Pulse Reading
```

This allows the user to observe the sensor readings directly from the embedded device.

---

### 4. Abnormal Condition Detection

The system compares sensor readings against predefined threshold values.

The buzzer is activated when any of the following conditions occur:

```text
Pulse Reading > 800
OR
LM35 Temperature > 38°C
OR
Humidity > 80%
```

If none of the conditions are satisfied, the buzzer remains OFF.

---

### 5. Emergency Alert

When an abnormal condition is detected, the STM32 sets the buzzer output HIGH.

```text
Abnormal Reading
       ↓
STM32 Threshold Check
       ↓
Condition Satisfied?
       ↓
      YES
       ↓
Buzzer ON
```

This provides an immediate local warning to the soldier or nearby personnel.

---

## 🔌 Pin Configuration

The current implementation uses the following STM32 pins:

| Component           | STM32 Pin |
| ------------------- | --------- |
| DHT11 Data          | PB9       |
| LCD RS              | PB13      |
| LCD Enable          | PB12      |
| LCD D4              | PB0       |
| LCD D5              | PB1       |
| LCD D6              | PC13      |
| LCD D7              | PC14      |
| LM35 Analog Output  | PA0       |
| Pulse Sensor Output | PA1       |
| Buzzer              | PA8       |

> **Note:** Pin assignments may need to be modified depending on the STM32 board and hardware configuration being used.

---

## 🔄 Program Flow

```text
START
  │
  ▼
Initialize LCD
  │
  ▼
Initialize DHT11
  │
  ▼
Configure Sensor and Buzzer Pins
  │
  ▼
Initialize Serial Communication
  │
  ▼
Read LM35 Sensor
  │
  ▼
Calculate Temperature
  │
  ▼
Read DHT11
  │
  ├── Temperature
  └── Humidity
  │
  ▼
Read Pulse Sensor
  │
  ▼
Display Values on LCD
  │
  ▼
Check Threshold Conditions
  │
  ├── Abnormal → Buzzer ON
  │
  └── Normal → Buzzer OFF
  │
  ▼
Wait 1 Second
  │
  ▼
Repeat
```

---

## 🧠 Algorithm

1. Start the system.
2. Initialize the LCD and DHT11 sensor.
3. Configure the LM35, pulse sensor, and buzzer pins.
4. Initialize serial communication.
5. Read the analog value from the LM35.
6. Convert the ADC value into temperature.
7. Read temperature and humidity from the DHT11.
8. Read the pulse sensor's analog value.
9. Display the readings on the LCD.
10. Compare the readings with predefined threshold values.
11. Turn ON the buzzer if an abnormal condition is detected.
12. Otherwise, keep the buzzer OFF.
13. Wait for one second.
14. Repeat the monitoring process continuously.

---

## 🧪 Alert Conditions

| Parameter                   | Current Threshold | Action     |
| --------------------------- | ----------------: | ---------- |
| Pulse sensor reading        |           `> 800` | Buzzer ON  |
| LM35 temperature            |          `> 38°C` | Buzzer ON  |
| Humidity                    |           `> 80%` | Buzzer ON  |
| All values within threshold |                 — | Buzzer OFF |

> **Important:** These thresholds are prototype values for demonstration and should not be interpreted as medically validated limits.

---

## 📊 Current Implementation

The current prototype successfully demonstrates:

* STM32 sensor interfacing
* Analog sensor data acquisition
* Temperature calculation using LM35
* DHT11 temperature/humidity acquisition
* Pulse sensor signal acquisition
* LCD data visualization
* Threshold-based buzzer activation
* Continuous real-time monitoring

---

## ⚠️ Current Limitations

The current prototype has several areas that can be improved:

* The pulse sensor currently displays its **raw analog reading** rather than calculating BPM.
* GPS hardware is part of the proposed system architecture but is **not implemented in the current code**.
* Wireless communication is not currently implemented.
* The threshold values are demonstration values and require calibration and validation for practical medical use.
* The LCD provides local monitoring but does not currently transmit data to a remote monitoring station.

---

## 🚀 Future Scope

The system can be enhanced with the following features:

### 📍 GPS-Based Location Tracking

Integration of a GPS module can provide the soldier's latitude and longitude during an emergency.

### 📡 Wireless Communication

Wi-Fi, GSM, LoRa, or other communication technologies can be integrated to transmit health information to a remote monitoring station.

### ❤️ Advanced Health Monitoring

Additional sensors can be integrated for:

* SpO₂
* ECG
* Heart-rate monitoring
* Blood pressure estimation
* Motion/fall detection
* Hydration monitoring

### 🤖 AI-Based Health Prediction

Machine learning models could analyze sensor data to identify patterns associated with:

* Fatigue
* Heat stress
* Abnormal heart activity
* Physical exhaustion
* Potential medical emergencies

### ☁️ Cloud-Based Monitoring

Sensor readings can be stored in a cloud platform for:

* Long-term analysis
* Remote monitoring
* Health-data visualization
* Historical reporting

### 🦺 Wearable Implementation

The system can be converted into a wearable device such as:

* Smart vest
* Wristband
* Belt-mounted unit
* Soldier equipment attachment

---

## 🌍 Applications

Although designed around military health monitoring, the underlying technology can be adapted for other high-risk environments.

Potential applications include:

* Military operations
* Disaster-response teams
* Firefighters
* Mining personnel
* Industrial workers
* Remote field workers
* Search-and-rescue operations
* Healthcare monitoring in remote environments

---

## 🛠️ Technologies Used

```text
Microcontroller : STM32
Programming      : Embedded C / Arduino-compatible C/C++
IDE              : Arduino IDE
Sensors          : LM35, DHT11, Pulse Sensor
Display          : 16×2 LCD
Alert            : Buzzer
Communication    : Serial
Planned Extension: GPS + Wireless Communication
```

---

## 📁 Project Structure

```text
Soldier-Health-Monitoring-System/
│
├── README.md
│
├── src/
│   └── soldier_health_monitor.ino
│
├── images/
│   ├── circuit.jpg
│   ├── hardware_setup.jpg
│   ├── lcd_output.jpg
│   └── system_flowchart.png
│
└── docs/
    └── Project_Documentation.md
```

---

## ▶️ How to Run

### 1. Clone the repository

```bash
git clone <YOUR-GITHUB-REPOSITORY-LINK>
```

### 2. Open the project

Open the `.ino` file in **Arduino IDE**.

### 3. Install required libraries

Install:

```text
LiquidCrystal
DHT
```

### 4. Select the STM32 board

Select the appropriate STM32 board and COM port in Arduino IDE.

### 5. Connect the hardware

Connect the sensors, LCD, and buzzer according to the pin configuration provided above.

### 6. Upload the program

Compile and upload the program to the STM32 microcontroller.

### 7. Observe the output

The LCD displays the sensor readings, while the buzzer provides an alert when predefined threshold conditions are exceeded.

---

## 📷 Project Output

Add your actual hardware/output photographs here.

Example:

```text
images/
├── hardware_setup.jpg
├── lcd_output.jpg
└── circuit.jpg
```

Then include them in the README:

```markdown
## 📷 Project Output

### Hardware Setup

![Hardware Setup](images/hardware_setup.jpg)

### LCD Output

![LCD Output](images/lcd_output.jpg)

### Circuit

![Circuit](images/circuit.jpg)
```

---

## 🔮 Planned Enhancements

The next version of the project can focus on:

* [ ] Actual BPM calculation from the pulse sensor
* [ ] GPS module integration
* [ ] Wireless health-data transmission
* [ ] Remote monitoring dashboard
* [ ] Data logging
* [ ] SpO₂ sensor integration
* [ ] Fall detection
* [ ] AI-based anomaly detection
* [ ] Wearable implementation

---

## 👩‍💻 Author

**Swati Saidapur**

Electrical and Electronics

Interested in **Embedded Systems, and Real-Time Applications**.

---

## 📄 License

This project is intended for **educational and prototype development purposes**.

You may modify and extend the project for learning, experimentation, and academic use.
