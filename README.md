# EMG Hand Pose Prediction (51 Joints) 
**Real-time regression of 51 hand-joint positions from multi-channel EMG (8 forearm sensors).** :contentReference[oaicite:0]{index=0}

> This project tackles a challenging **biosignal-to-kinematics** mapping problem: decoding noisy, non-stationary EMG streams into a high-dimensional hand pose representation suitable for **HCI/AR/VR**, **robotic teleoperation**, **rehabilitation**, and **assistive tech**.

---

## Why this is interesting (for an AI Engineer)
- **High-dimensional structured regression**: predict **51 joints** (multi-output) rather than a simple class label. :contentReference[oaicite:1]{index=1}  
- **Signal ML in the real world**: EMG requires robust preprocessing, windowing, normalization, and careful evaluation due to drift and subject variability.
- **Real-time constraints**: the pipeline is designed with **streaming inference** in mind (low-latency sliding windows, stable predictions). :contentReference[oaicite:2]{index=2}  
- **End-to-end deliverable**: a single notebook contains the full workflow and produces an exportable prediction file (`team_submission.csv`). :contentReference[oaicite:3]{index=3}

---

## Problem statement
Given an 8-channel EMG stream from forearm sensors, estimate the corresponding hand pose as **51 joint positions** in real time. :contentReference[oaicite:4]{index=4}

- **Input**: EMG time-series, 8 channels (forearm sensors) :contentReference[oaicite:5]{index=5}  
- **Output**: 51 joint positions (pose vector; exact format depends on the dataset/benchmark) :contentReference[oaicite:6]{index=6}  

---

## Approach (system-level)
1. **Streaming windowing**  
   Convert raw EMG into fixed-length overlapping windows (sliding window) to enable continuous predictions.
2. **Preprocessing (typical EMG)**  
   Normalization (per-channel), optional rectification / smoothing, and drift-robust scaling.
3. **Modeling**  
   A regression model maps each EMG window → pose vector (multi-target regression).  
   *(The notebook is the source of truth for the chosen architecture and hyperparameters.)*
4. **Post-processing (optional but useful for demos)**  
   Temporal smoothing / EMA to stabilize jitter in real-time visualization.
5. **Export**  
   Predictions are written to `team_submission.csv` for evaluation/submission. :contentReference[oaicite:7]{index=7}

---

## Repository structure
- `projet_sfml.ipynb` — main notebook (data pipeline → training → inference/export). :contentReference[oaicite:8]{index=8}  
- `team_submission.csv` — example output file containing model predictions. :contentReference[oaicite:9]{index=9}  
- `README.md` — project documentation.

---

## Quickstart
### 1) Clone
```bash
git clone https://github.com/RayaneTfeili/emg_handpose_prediction.git
cd emg_handpose_prediction
