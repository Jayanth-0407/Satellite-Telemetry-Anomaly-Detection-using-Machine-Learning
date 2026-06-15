# 🛰️ Satellite Telemetry Anomaly Detection using Machine Learning

## 📖 Overview
This repository contains the machine learning pipeline developed to detect hardware faults and systemic anomalies in the telemetry of the EIRSAT-1 spacecraft. 

Standard anomaly detection models trained exclusively in static laboratory environments (TVAC) suffer from catastrophic "domain shift" when deployed to space, frequently misclassifying natural orbital thermal cycles as hardware failures. This project successfully engineered a **Sim-to-Real Cross-Domain Validation Pipeline** utilizing synthetic flight-test data to teach the AI the dynamic rhythms of Low Earth Orbit, drastically reducing operator fatigue and false alarms.

## ✨ Key Achievements
* **Eliminated False Positive Bias:** Mathematically reduced the false-positive hardware alert rate by **11.09%** (eliminating over 16,600 fake alerts) when transitioning from TVAC-trained models to Flight-Test-trained models.
* **State-of-the-Art F1 Scores:** Achieved a peak F1-score of **0.91** on the highly volatile Channel 17 subsystem, vastly outperforming the base paper's score of **0.03** on the exact same dataset.
* **Robust Edge-Case Handling:** Engineered a custom mathematical clipping algorithm to safely neutralize massive float overflow errors ($3.4 \times 10^{38}$) caused by real-world corrupted downlink packets.

## 🛠️ Pipeline Architecture
The pipeline processes high-dimensional, time-series telemetry (149,602 rows) through a rigorous, modular workflow:

1. **Data Ingestion & Integrity Check:** Aggregates scattered flight CSVs, coerces corrupted strings, and mathematically bounds extreme outliers to `float32` physical limits.
2. **Dimensionality Reduction (PCA):** Compresses high-dimensional sensor noise into principal components, retaining 95% of the critical mathematical variance to prevent memory overload.
3. **Stratified K-Fold Cross-Validation:** Eradicates the severe class imbalance problem by ensuring the exact ratio of nominal-to-anomaly data is preserved across all training cycles.
4. **Optimized Champion Models:** Deploys specific, hyperparameter-tuned algorithms optimized for individual telemetry channels, including:
    * Multi-Layer Perceptrons (Neural Networks)
    * Random Forest Classifiers
    * Logistic Regression
    * Gradient Boosting

## 📊 Performance Metrics
Validation on live, unseen in-orbit telemetry yielded the following performance improvements over traditional ground-testing methods:

| Training Environment | Total Rows Evaluated | Flagged Anomalies | Anomaly Rate | Workload Reduction |
| :--- | :--- | :--- | :--- | :--- |
| **TVAC (Ground Lab Baseline)** | 149,602 | 26,708 | 17.85% | 82.15% |
| **Flight-Test (Synthetic)** | 149,602 | 10,107 | 6.76% | 93.24% |
| **The Domain Shift Reduction** | - | **- 16,601** | **- 11.09%** | - |

## 🚀 Installation & Setup

**Prerequisites:**
Ensure you have Python 3.8+ installed. 

1. Clone the repository:
```bash
git clone https://github.com/YourUsername/EIRSAT-1-Anomaly-Detection.git
cd EIRSAT-1-Anomaly-Detection
