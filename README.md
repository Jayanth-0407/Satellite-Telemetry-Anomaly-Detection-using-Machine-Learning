# 🛰️ Satellite Telemetry Anomaly Detection & Drift Analysis

## 📌 Project Overview
The mission-critical nature of Low Earth Orbit (LEO) satellite operations requires robust autonomous monitoring. Undetected subsystem faults or the misclassification of natural orbital shifts can lead to catastrophic hardware damage or complete loss of the spacecraft.

This project develops a Machine Learning framework to detect anomalies in satellite telemetry data. It successfully addresses the challenge of distinguishing between genuine hardware/software anomalies (e.g., critical voltage drops, OBC resets) and natural "Concept Drift" caused by seasonal Beta angle variations in the space environment.

## 📊 Datasets
The project utilizes three distinct phases of telemetry data, focusing primarily on **Channel 15 (Housekeeping)** and **Channel 19 (Power)**:

1. **TVAC Dataset (Ground Truth - Hardware):** Thermal Vacuum Chamber test data containing labeled, injected anomalies (e.g., battery voltage drops, thermal sensor failures). Used for supervised training.
2. **Flight-Test Dataset (Ground Truth - Software/Power):** "FlatSat" mission simulation data containing labeled anomalies like On-Board Computer (OBC) resets and power limit breaches. Used for supervised training.
3. **Flight Dataset (On-Orbit / Unlabeled):** Real telemetry downlinked from the satellite over four months (December to March). Used for unsupervised inference, concept drift analysis, and proving deployment readiness.

## 🛠️ Methodology & Pipeline

### 1. Exploratory Data Analysis (EDA)
* **Gap Analysis:** Handled asynchronous downlinks and transmission gaps inherent in space communication.
* **Correlation Drift:** Analyzed `(Correlation_March - Correlation_Dec)` heatmaps to observe how physical relationships between components decouple over time.
* **Distribution Shift (KDE):** Visualized thermal drift to prove that static `if/else` thresholds fail in space due to seasonal temperature shifts.
* **PCA State Space Analysis:** Projected 100+ sensors into a 2D state space to quantify the "Seasonal Concept Drift" occurring between winter and spring.

### 2. Preprocessing Strategy
* **Strict Imputation:** Adopted a strict dropping policy for columns with missing values in critical telemetry channels to prevent synthetic data from masking real anomalies.
* **Dimensionality Reduction:** Utilized Principal Component Analysis (PCA) to compress high-dimensional telemetry into actionable feature vectors.

### 3. Machine Learning Framework
* **Baseline Identification:** Random Forest feature importance was used to rank critical predictive sensors (e.g., `batteryVoltage`).
* **Anomaly Classification:** A [Insert Your Model: e.g., SAML-PCA / Autoencoder / Random Forest] framework was trained on the labeled ground tests to separate nominal operations from critical faults.

## 📈 Key Findings
* **Concept Drift is Real:** The PCA analysis confirmed that the satellite's definition of "Normal" changes fundamentally between December and March due to varying eclipse durations and Beta angles.
* **Model Robustness:** The proposed model achieved an F1-Score of [Insert Score, e.g., 95.28%] on ground-test data while maintaining stability (avoiding false alarms) during the seasonal drift observed in the flight data.
* **Execution Efficiency:** The framework processes telemetry frames in [Insert Time, e.g., 1.60 secs], making it computationally viable for resource-constrained systems.

## 💻 Repository Structure
```text
├── data/
│   ├── ground_tests/      # tvac_dataset.csv, flighttest_dataset.csv
│   └── flight_data/       # channel_15Dec.csv, channel_15Jan.csv, etc.
├── notebooks/
│   ├── 01_EDA_and_Drift_Analysis.ipynb
│   ├── 02_PCA_State_Space.ipynb
│   └── 03_Model_Training_and_Evaluation.ipynb
├── src/
│   ├── data_cleaning.py   # Robust cleaning and gap analysis scripts
│   └── model_pipeline.py  # PCA and ML classifier classes
├── visuals/               # Output graphs (KDE shifts, PCA clusters, Voltage cycles)
└── README.md
