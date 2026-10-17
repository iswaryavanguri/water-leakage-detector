# IoT-Based Water Pipeline Leakage Detection System

## 1. Project Overview

The IoT-Based Water Pipeline Leakage Detection System detects possible water leakage in pipeline segments using four flow sensors connected to an ESP32 microcontroller.

The ESP32 monitors water flow, compares the readings between the inlet and outlet of two pipeline segments, and identifies abnormal differences that may indicate leakage. The sensor readings are sent to Blynk Cloud, allowing users to monitor water flow remotely through the Blynk mobile application.

## 2. Objectives

- Detect possible water leakage in multiple pipeline segments.
- Monitor water flow in real time using flow sensors.
- Compare inlet and outlet flow readings to identify abnormalities.
- Send sensor data to Blynk Cloud using Wi-Fi.
- Generate alerts when a possible leak is detected.
- Reduce water wastage and improve pipeline monitoring.

## 3. Technologies Used

- **Microcontroller:** ESP32
- **Programming Language:** C++ (Arduino framework)
- **Development Environment:** Arduino IDE
- **Sensors:** Four water flow sensors
- **IoT Platform:** Blynk Cloud
- **Mobile Application:** Blynk IoT App
- **Communication:** Wi-Fi

## 4. Hardware Requirements

- ESP32 development board
- Four water flow sensors
- Water pipeline setup
- Connecting wires
- Breadboard or suitable connectors
- Stable power supply
- Wi-Fi connection

## 5. Software Requirements

- Arduino IDE
- ESP32 board package for Arduino IDE
- Blynk library for Arduino
- Blynk Cloud account
- Blynk mobile application

## 6. System Architecture

Water Flow Sensors → ESP32 → Wi-Fi → Blynk Cloud → Blynk Mobile App

The four sensors monitor two pipeline segments:

- **Segment 1:** Sensor S1 (inlet) and Sensor S2 (outlet)
- **Segment 2:** Sensor S3 (inlet) and Sensor S4 (outlet)

The ESP32 compares the inlet and outlet flow readings for each segment. A sufficiently large and persistent difference triggers a possible leakage alert.

## 7. Pin Configuration

| Component | ESP32 GPIO Pin |
|---|---|
| Flow Sensor S1 | GPIO 34 |
| Flow Sensor S2 | GPIO 35 |
| Flow Sensor S3 | GPIO 32 |
| Flow Sensor S4 | GPIO 33 |

The sensors are configured to generate interrupts on rising signal edges. The ESP32 counts these pulses to calculate flow values.

## 8. Blynk Configuration

Create a template in Blynk Cloud and configure the following:

| Virtual Pin | Data |
|---|---|
| V0 | Flow Sensor S1 |
| V1 | Flow Sensor S2 |
| V2 | Flow Sensor S3 |
| V3 | Flow Sensor S4 |

Create two Blynk events for leak notifications:

- `leak_seg1` — Leak detected in Segment 1
- `leak_seg2` — Leak detected in Segment 2

Add suitable datastreams and widgets to the Blynk dashboard to display the flow values. Configure event notifications if alerts are required on the mobile device.

## 9. Working Principle

1. The ESP32 connects to the configured Wi-Fi network and Blynk Cloud.
2. The four flow sensors generate pulses as water flows through the pipelines.
3. Interrupt service routines count the pulses from each sensor.
4. The ESP32 converts the pulse counts into flow values using a configurable conversion factor.
5. The calculated readings are printed to the Serial Monitor and transmitted to Blynk using virtual pins V0–V3.
6. The ESP32 compares the inlet and outlet readings for each segment.
7. If the difference exceeds the configured absolute and percentage thresholds, a counter increases.
8. If the condition persists for more than five consecutive one-second measurement windows, a possible leak event is reported.
9. When flow is below the minimum threshold for all sensors, the counters are reset.

## 10. Leakage Detection Parameters

| Parameter | Value | Purpose |
|---|---:|---|
| Minimum flow | 0.3 | Ignores very small readings |
| Absolute threshold | 1.0 | Minimum required flow difference |
| Percentage threshold | 20% | Relative difference requirement |
| Measurement interval | 1 second | Flow monitoring window |
| Confirmation counter | Greater than 5 | Requires persistent abnormal readings |

**Note:** The pulse-to-flow conversion factor (`0.1`) is a placeholder that must be calibrated for the actual sensors. The thresholds should also be adjusted according to measured flow rates and normal pipeline behavior.

## 11. Installation and Setup

1. Install Arduino IDE.
2. Install the ESP32 board package.
3. Install the Blynk library from the Arduino Library Manager.
4. Create a Blynk template and configure the virtual pins and event codes.
5. Connect the four flow sensors to the specified ESP32 pins.
6. Configure the Blynk template ID and authentication token in the sketch.
7. Enter your Wi-Fi credentials locally.
8. Select the correct ESP32 board and COM port in Arduino IDE.
9. Upload the code to the ESP32.
10. Open the Serial Monitor at 115200 baud.
11. Verify sensor readings and check the Blynk dashboard.

## 12. Expected Output

During normal operation, the Serial Monitor displays the flow readings for all four sensors.

Example:

```text
S1: 5.20  S2: 3.80  S3: 4.50  S4: 4.40
```

If an abnormal difference persists, the system may display:

```text
Leak in Segment 1
```

or

```text
Leak in Segment 2
```

The corresponding Blynk event is triggered when the detection condition is satisfied.

These are illustrative outputs; actual values depend on sensor calibration and pipeline conditions.

## 13. Applications

- Domestic water supply pipelines
- Kitchen and bathroom water lines
- College and institutional buildings
- Small commercial buildings
- Water distribution monitoring systems
- Smart water management projects

## 14. Advantages

- Continuous monitoring of multiple pipeline segments
- Remote monitoring through Blynk Cloud
- Early indication of possible leakage
- Simple ESP32-based implementation
- Helps reduce water wastage
- Expandable to additional sensors and pipeline segments

## 15. Limitations

- Incorrect sensor calibration can cause inaccurate readings.
- Normal differences caused by flow variation may trigger false alerts.
- Wi-Fi or cloud connectivity issues can interrupt remote monitoring.
- The system identifies flow imbalance; it does not physically locate a leak.
- The current implementation does not automatically close a valve.
- The conversion factor and thresholds require practical testing.

## 16. Future Enhancements

- Add automatic water shutoff using a solenoid valve.
- Store historical flow data for analysis.
- Add a dashboard showing flow trends and leak history.
- Improve leak detection using averaging and adaptive thresholds.
- Add offline alerts and connectivity monitoring.
- Extend the system to more pipeline segments.

## 17. Safety and Security

Keep Wi-Fi passwords, Blynk authentication tokens, and other credentials private. Do not commit actual credentials to a public GitHub repository. Use placeholders in the published source code and configure private credentials locally.

## 18. Conclusion

This project demonstrates an IoT-based approach to monitoring water flow and identifying possible pipeline leakage using an ESP32, four flow sensors, and Blynk Cloud. By comparing flow measurements across pipeline segments and sending remote alerts, the system provides a foundation for smarter water management and reducing unnecessary water loss.
