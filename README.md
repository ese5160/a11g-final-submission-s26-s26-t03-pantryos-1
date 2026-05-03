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

![System Block Diagram] <img width="902" height="775" alt="image" src="https://github.com/user-attachments/assets/1e71f5dc-4312-4363-989c-03eee429c7f8" />

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

## 4. Project Photos & Screenshots

## 5. Codebase

Do *not* commit any of your source code to this repository. Rather, provide links to the other GitHub repository you've already been using with your firmware.

- A link to your final embedded C firmware codebases
- A link to your Node-RED dashboard code
- Links to any other software required for the functionality of your device
