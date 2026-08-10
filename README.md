# EM-IoT3

**EM-IoT3** is a three-class electromagnetic side-channel dataset collected from a Raspberry Pi 4 Model B configured as an edge IoT platform. The dataset captures EM activity under three operating conditions: **Idle**, **Operational**, and **MQTT Flooding**.

This repository contains a representative subset of the raw recordings together with example feature data and the classification notebook used for the feature-based analysis. The full experimental study was conducted using **1,300 raw EM recordings**.

## Class Distribution

| Class | Experimental condition | Number of recordings |
|---|---|---:|
| Idle | Device powered with the experimental workload inactive | 300 |
| Operational | Periodic ultrasonic sensing with normal MQTT communication | 500 |
| MQTT Flooding | Repeated MQTT message transmission at an elevated rate | 500 |
| **Total** |  | **1,300** |

## Acquisition Configuration

The EM traces were obtained using the following hardware and receiver settings:

| Parameter | Value |
|---|---|
| Target platform | Raspberry Pi 4 Model B |
| Device function | Edge IoT node |
| EM probe | Near-field H-field probe |
| Receiver | RTL-SDR V3 |
| Acquisition environment | GNU Radio |
| Centre frequency | 200 MHz |
| Sampling frequency | 2.4 MS/s |
| Receiver gain | 38.6 dB |
| Signal representation | Complex I/Q |
| Storage format | complex64 |
| Recording duration | 1 s per trace |
| Capture procedure | Automated |

## Activity Definitions

### Idle

For the Idle condition, the Raspberry Pi remained powered while the application-specific workload was not executed. These recordings were used as the reference operating state.

### Operational

During normal operation, an HC-SR04 ultrasonic sensor was used to obtain distance measurements. Valid readings were published through MQTT on the topic:

`smart/vehicle/ultrasonic/data`

A measurement-and-publish cycle was performed approximately once every 10 s, representing the intended sensing and communication workload.

### MQTT Flooding

For the MQTT Flooding condition, spoofed sensor messages were transmitted repeatedly through the same MQTT topic at approximately 10 messages per second. This produced a substantially higher MQTT traffic rate than the normal operational condition and was treated as an abnormal communication state.

## Repository Contents

```text
EM-IoT3/
├── sample_data/
│   ├── Idle/
│   ├── Operational/
│   └── MQTT_Flooding/
├── classification.ipynb
├── sample_features.csv
└── README.md
