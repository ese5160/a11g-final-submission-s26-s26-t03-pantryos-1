[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/Y5lYn2wb)

# a11g-final-submission

**Team Number: 03**

**Team Name: PantryOS**

| Team Member Name | Email Address          | GitHub Username |
| ---------------- | ---------------------- | --------------- |
| Harita Trivedi   | haritant@seas.upenn.edu| haritant        |
| Arik Singh       | ags007@seas.upenn.edu  | ariksingh007    |

**GitHub Repository URL: https://github.com/ese5160/a11g-final-submission-s26-s26-t03-pantryos-1**

## 1. Video Presentation

https://www.youtube.com/watch?v=t6V7ENgFceo

## 2. Project Summary

### Device Description

**What is PantryOS?**  
PantryOS is a smart pantry monitoring device built around a custom Silicon Labs MCU on an IoT PCBA that combines temperature/humidity sensing, proximity detection, barcode scanning, servo actuation, and Wi-Fi connectivity. It communicates with a Node-RED dashboard over MQTT so a user can scan pantry items, enter purchase and expiration dates, monitor environmental conditions, and visualize overall pantry freshness in real time.

**Motivation / Problem**  
This project was inspired by a need to better track grocery freshness and maintain a live inventory of pantry items. We wanted a system that provides a clear, visual representation of food freshness while also helping users understand what they currently have available. A future extension of this idea is recipe suggestion based on available ingredients.

**Use of the Internet**  
The device connects to a Node-RED dashboard over Wi-Fi using MQTT, allowing it to publish real-time sensor data and receive user inputs. This enables remote inventory tracking, environmental monitoring, freshness visualization, time synchronization, and OTA firmware updates—significantly enhancing usability beyond the standalone device.

---

### Device Functionality

PantryOS is built around a **Silicon Labs SIWG917 MCU** on a custom PCBA, which acts as the central controller for sensing, actuation, and communication.

**Key Components:**
- **Sensors:**
  - SHT40 (temperature & humidity, I2C)
  - VCNL4040 (proximity sensor, I2C)
- **Actuator:**
  - SG90 servo (indicates pantry freshness)
- **Input System:**
  - DE2120 barcode scanner (UART)
- **Connectivity:**
  - Wi-Fi + MQTT → Node-RED dashboard

The system allows users to scan items, log expiration dates, and monitor pantry conditions through a connected dashboard.

<img width="902" height="775" alt="image" src="https://github.com/user-attachments/assets/1e71f5dc-4312-4363-989c-03eee429c7f8" />

---

### Challenges

We faced several challenges across hardware, firmware, and system integration:

- **Lost UART Debug Access**
  - Accidentally removed UART debug pins in PCB design
  - → Switched to SEGGER RTT logging

- **Servo Power Issues**
  - SG90 servo overloaded the onboard 5V rail and damaged it
  - → Moved servo to external power source

- **Barcode Scanner Integration**
  - Required careful UART buffering and synchronization with FreeRTOS tasks
  - → Improved parsing and event-driven handling

- **Proximity Sensor Reliability**
  - Interrupt-based triggering was unstable
  - → Switched to polling via I2C values for reliability

---

### Prototype Learnings

- Hardware limitations (especially power design) can break an otherwise solid system
- Small PCB mistakes (routing, pin selection) can have major downstream impacts
- Simpler MQTT topic structures made debugging Node-RED significantly easier
- The dashboard should reflect real device state directly—not infer logic
- Most importantly: **hardware, firmware, and cloud systems must be designed together**

---

### What We Would Do Differently

- Perform a **full net-by-net PCB review** before fabrication
- Redesign the **5V power path** to properly support servo loads
- Add **more accessible GPIO/test points** for debugging flexibility
- Preserve **UART debugging access**
- Validate pin mappings earlier (especially I2C compatibility)

---

### Next Steps & Takeaways

**Future Improvements:**
- Renew cloud infrastructure (Azure Node-RED hosting expired)
- Expand barcode database for better item recognition
- Implement recipe recommendation system based on inventory
- Add AI-based suggestions for meals and food usage

**Course Takeaways:**
ESE5160 taught us how to build a complete IoT system—from PCB design and power architecture to firmware, RTOS, cloud integration, and debugging. The biggest lesson was that success depends on tight coordination across all layers of the system.

---

### Project Links

- **Node-RED Backend:** http://172.212.177.245:1880/  
- **Node-RED Dashboard:** http://172.212.177.245:1880/ui  
⚠️ Note: Azure hosting expired April 30, 2026

- **Altium 365 Design:**  
https://upenn-eselabs.365.altium.com/designs/folder-DC856870-9335-4574-956F-E6FD47C46101

## 3. Hardware & Software Requirements

We evaluated our system against the original hardware and software requirements and validated each component through testing.

---

### Hardware Requirements

| ID | Requirement | Validation | Evidence | Result |
|----|------------|------------|----------|--------|
| **HRS-01 MCU** | SIWG917 MCU with FreeRTOS + interfaces | Verified via firmware execution, I2C, PWM, RTT logging | <img src="images/requirements/mcu.jpg" width="200"> | ✅ |
| **HRS-02 Power** | Battery + 5V support + low power | Measured current (38 mA idle, 166 mA scanning) | <img src="images/requirements/battery.jpg" width="200"><br><img src="images/requirements/nominal_current.jpg" width="200"><br><img src="images/requirements/barcode_scan_current.jpg" width="200"> | ⚠️ |
| **HRS-03 Sensors** | Temp + humidity via I2C | Verified via RTT logs | <img src="images/requirements/temp_humidity_readings.png" width="200"> | ✅ |
| **HRS-04 Barcode** | UART barcode scanner | Verified scan → decode → log | <img src="images/requirements/barcode_scan.png" width="200"><br><img src="images/requirements/unknown_barcode.png" width="200"> | ✅ |
| **HRS-05 Wireless** | BLE + Wi-Fi | Wi-Fi + MQTT working; BLE not implemented | — | ⚠️ |
| **HRS-06 Actuation** | Servo + LEDs | Servo functional (external power), no LEDs | <img src="images/requirements/servo.jpg" width="200"><br><img src="images/requirements/servo_freshness_update.png" width="200"> | ⚠️ |
| **HRS-07 Debug** | Logging + programming | SEGGER RTT logs verified | <img src="images/requirements/Segger_RTT.png" width="200"> | ✅ |
| **HRS-08 Cost** | ≤ $30 | $71.74 with scanner | — | ❌ |

---

### Software Requirements

| ID | Requirement | Validation | Evidence | Result |
|----|------------|------------|----------|--------|
| **SRS-01 Firmware** | FreeRTOS modular system | Verified task-based architecture | <img src="images/requirements/free_rtos.png" width="200"> | ✅ |
| **SRS-02 Concurrency** | Event-driven architecture | Verified via task execution and logs | <img src="images/requirements/logs.png" width="200"> | ✅ |
| **SRS-03 Persistence** | Data survives reset | Not implemented | — | ❌ |
| **SRS-04 Barcode Logic** | Scan → update inventory | Verified via logs + system behavior | <img src="images/requirements/barcode_scan.png" width="200"><br><img src="images/requirements/inventory_item_struct.png" width="200"> | ✅ |
| **SRS-05 BLE** | BLE support | Not implemented | — | ❌ |
| **SRS-06 Sensors** | Sampling + freshness | Verified periodic readings | <img src="images/requirements/temp_humidity_readings.png" width="200"> | ✅ |
| **SRS-07 Actuation** | Servo response | ~250 ms response time | <img src="images/requirements/servo_rtt.png" width="200"><br><img src="images/requirements/servo_freshness_update_nodered.png" width="200"> | ✅ |
| **SRS-08 MQTT** | Publish + subscribe | Verified before Azure expired | <img src="images/requirements/logs.png" width="200"> | ⚠️ |
| **SRS-09 Recipes** | Suggest meals | Not implemented | — | ❌ |
| **SRS-10 Grocery List** | Generate list | Not implemented | — | ❌ |
| **SRS-11 Low Power** | Sleep modes | Not implemented | — | ❌ |
| **SRS-12 Logging** | Debug logs | Verified via RTT | <img src="images/requirements/logs.png" width="200"> | ✅ |

---

### Key Validation Results

- **Power Consumption**
  - Idle: ~38 mA  
  - Scanning: ~166 mA  
  - Measured using USB-C power meter  

- **Barcode Performance**
  - Scan latency: ~250 ms  
  - Duplicate scans handled correctly  
  - Unknown barcode handling verified  

- **System Integration**
  - Sensor + actuator + firmware integration verified  
  - MQTT communication verified prior to Azure expiration  

---

### Takeaways

- Core system functionality (sensing, scanning, actuation, Wi-Fi) was successfully implemented  
- Hardware limitations—especially power delivery—significantly impacted design decisions  
- Advanced features (BLE, persistence, low-power) remain future work  
- Full-stack integration (hardware + firmware + cloud) is essential for reliable IoT systems  

## 4. Project Photos & Screenshots

## 5. Codebase

Do *not* commit any of your source code to this repository. Rather, provide links to the other GitHub repository you've already been using with your firmware.

- A link to your final embedded C firmware codebases
- A link to your Node-RED dashboard code
- Links to any other software required for the functionality of your device
