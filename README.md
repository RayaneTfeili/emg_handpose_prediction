# EMG Hand Pose Prediction (51 Joints) 
**Multi-model pipeline to predict 51 hand-joint positions from 8-channel forearm EMG, with a strong baseline suite + Riemannian geometry features.**

This repository explores **biosignal-to-kinematics** learning: mapping noisy, non-stationary EMG time-series into a high-dimensional hand pose representation. The focus is on **robust baselines**, **feature engineering for EMG**, and **reproducible evaluation**.

---

## Key contributions (what a recruiter should notice)
- **Model suite, not a single attempt**: linear (Ridge), tree-based (Random Forest), deep learning (Neural Network), and **Riemannian geometry**-based approaches.
- **Signal-aware feature pipeline**: windowing + normalization + features tailored to EMG.
- **Clear evaluation protocol**: multi-output regression metrics + per-joint breakdown.
- **Reproducible workflow**: training → validation → prediction export (`team_submission.csv`) in one place.

---

## Problem
Given an **8-channel EMG stream**, predict **51 hand joint values** (multi-output regression; e.g., 51 joints × coordinates depending on the dataset format).

- **Input**: EMG (8 sensors), time-series
- **Output**: hand pose vector for 51 joints

---

## Methods
### 1) Ridge Regression (strong linear baseline)
A reliable baseline for high-dimensional regression with L2 regularization.
- Pros: fast, stable, good sanity check
- Useful to quantify how much “non-linearity” is actually needed

### 2) Random Forest Regressor (non-linear baseline)
Handles non-linear interactions between channels/features and can work well with engineered EMG features.
- Pros: strong baseline without heavy tuning, interpretable feature importance
- Cons: take much more time to train than the Ridge Regression


### 3) Neural Network Regressor (learned non-linear mapping)
A neural regressor trained on windowed EMG representations.
- Pros: flexible, can learn complex mappings
- Cons: needs careful regularization and evaluation to avoid overfitting 

### 4) Riemannian Geometry (covariance-manifold features)
We leverage the structure of **covariance matrices** computed over EMG windows:
- Build SPD covariance matrices per window (Symmetric Positive Definite)
- Map SPD matrices to a tangent space / apply Riemannian metrics
- Train a regressor on these geometry-aware embeddings

Why it matters:
- Covariance captures channel interactions robustly
- Riemannian approaches are widely used in biosignals (EEG/EMG) due to **stability under noise and variability**

---

## Pipeline
1. **Windowing (sliding windows)**: convert streaming EMG into fixed-length segments  
2. **Preprocessing**: per-channel normalization (and optional smoothing)  
3. **Feature extraction** (depending on model):
   - raw window vector / handcrafted EMG features
   - covariance matrices → Riemannian embeddings
4. **Training**: Ridge / Random Forest / Neural Net / Riemannian-based regression
5. **Evaluation**: multi-output regression metrics + per-joint analysis
6. **Export**: `team_submission.csv`

---

## Metrics (multi-output regression)
We evaluate pose prediction quality using standard regression metrics:

### **MAE (Mean Absolute Error)**
Measures average absolute deviation per output dimension:
\[
MAE = \frac{1}{N}\sum_{i=1}^{N} |y_i - \hat{y}_i|
\]


### **MSE / RMSE**
Penalizes larger errors more strongly:
\[
MSE = \frac{1}{N}\sum_{i=1}^{N} (y_i - \hat{y}_i)^2,\quad
RMSE = \sqrt{MSE}
\]


---

## Repository structure
- `projet_sfml.ipynb` — end-to-end training & evaluation notebook
- `team_submission.csv` — example exported predictions
- `README.md` — documentation

---

## Quickstart
```bash
git clone https://github.com/RayaneTfeili/emg_handpose_prediction.git
cd emg_handpose_prediction
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -U pip
pip install numpy pandas scikit-learn jupyter matplotlib
jupyter notebook
