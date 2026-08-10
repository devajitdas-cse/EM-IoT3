# EM-IoT3

**EM-IoT3** is an electromagnetic (EM) side-channel dataset developed for three-state activity classification of an edge IoT device. The data were acquired from a Raspberry Pi 4 Model B operating under three conditions: **Idle**, **Operational**, and **MQTT Flooding**.

This GitHub repository provides a **representative sample of the raw EM recordings**, a sample feature file, and a classification notebook associated with the study. The complete experimental dataset used in the study contains **1,300 raw EM recordings**.

## Dataset Classes

| Class | Description | Full Dataset Recordings |
|---|---|---:|
| Idle | Raspberry Pi powered on without executing the application workload | 300 |
| Operational | Normal sensing and MQTT communication using an HC-SR04 ultrasonic sensor | 500 |
| MQTT Flooding | Communication-intensive abnormal condition with repeated MQTT message publication | 500 |
| **Total** |  | **1,300** |

## Experimental Setup

| Parameter | Specification |
|---|---|
| Target device | Raspberry Pi 4 Model B |
| Target role | Edge IoT device |
| EM sensor | Near-field H-field probe |
| Signal receiver | RTL-SDR V3 |
| Acquisition software | GNU Radio |
| Centre frequency | 200 MHz |
| Sampling rate | 2.4 MS/s |
| Receiver gain | 38.6 dB |
| Recorded data type | Complex I/Q samples |
| Sample format | complex64 |
| Trace duration | 1 s |
| Acquisition mode | Automated recording |

## Operating Conditions

### Idle
The Raspberry Pi remained powered on without executing the application workload and represented the baseline operating condition.

### Operational
The Raspberry Pi acquired distance measurements from an HC-SR04 ultrasonic sensor and transmitted valid sensor readings to an MQTT broker through the topic:

`smart/vehicle/ultrasonic/data`

Measurements were generated approximately every 10 s to represent normal sensing and communication activity.

### MQTT Flooding
Spoofed sensor messages were repeatedly published to the same MQTT topic at approximately 10 messages per second. This condition represented a communication-intensive abnormal operating state.

## Repository Structure

```text
EM-IoT3/
├── sample_data/
│   ├── Idle/
│   ├── Operational/
│   └── MQTT_Flooding/
├── classification.ipynb
├── sample_features.csv
└── README.md
```

### `sample_data/`
Contains representative raw EM recordings from the three activity classes. These files are provided to illustrate the raw signal format and support reproducibility of the processing workflow.

### `sample_features.csv`
Contains a representative subset of extracted statistical and spectral features used for activity-classification experiments.

### `classification.ipynb`
Contains the machine-learning classification workflow associated with the feature-based evaluation.

## Signal Preprocessing

Each raw recording consists of complex-valued I/Q samples. A constant DC component was removed by subtracting the mean of each complex trace.

The corrected traces were segmented using:

- Window length: **8192 samples**
- Hop length: **8192 samples**
- Overlap: **0%**
- Segment duration: approximately **3.41 ms** at 2.4 MS/s

Source-recording identifiers were retained so that segments derived from the same raw recording could be grouped during model training and evaluation.

## Feature Extraction

A total of **20 statistical and spectral features** were extracted from each signal segment.

### Statistical features

- Mean power
- Standard deviation of power
- Peak power
- Peak-to-average power ratio (PAPR)
- Crest factor
- Skewness
- Kurtosis

### Spectral features

- Spectral peak magnitude
- Spectral peak frequency
- Spectral centroid
- Spectral flatness
- Spectral bandwidth
- Eight normalized band-energy features

A reduced 15-feature representation was also evaluated after excluding:

- Mean power
- Peak power
- Standard deviation of power
- Spectral peak magnitude
- Spectral peak frequency

## Evaluation Protocol

The feature dataset was partitioned at the **source-recording level**. All segments originating from a given EM recording were assigned exclusively to either the training or test partition to avoid information leakage between correlated segments.

Five-fold group-based cross-validation was also performed using the source recording as the grouping variable.

## Reported Result

Using the reduced 15-feature representation, the Random Forest classifier achieved:

- **Test accuracy:** 97.67%
- **Macro-averaged F1-score:** 0.978

These values refer to the experimental study and not to a benchmark computed only from the representative sample files hosted in this repository.

## Data Availability

This repository currently provides a **representative subset** of the EM-IoT3 raw recordings together with supporting feature data and analysis material. It should therefore not be interpreted as containing all 1,300 raw recordings used in the study.

## Intended Use

The repository is intended for research and educational use related to:

- Electromagnetic side-channel analysis
- Edge IoT activity classification
- Machine-learning-based signal classification
- MQTT activity and abnormal-condition analysis


