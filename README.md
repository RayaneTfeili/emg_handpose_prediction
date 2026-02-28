# EMG Hand Pose Prediction (51 Joints)

**Prediction of 51 hand joint positions from 8-channel EMG signals**

This project implements several machine learning models to estimate hand joint positions (51 outputs) from forearm EMG recordings.

---

##  Dataset

The dataset consists of synchronized EMG signals and hand pose labels:

- **EMG (8 channels)**  
  Signals recorded from 8 forearm sensors.

- **Hand poses (51 joints)**  
  Each sample contains 51 joint values representing the hand configuration.

To adapt the time-series signals to regression models, the EMG data is segmented using sliding windows.

---

## Methodology

### Sliding Window

The EMG signals are segmented into fixed-length sliding windows.  
Each window is used to predict the corresponding hand pose.

---

### Feature Extraction

For each window, different feature representations can be used:

- **Raw flattened window**
- **Standard EMG features** (RMS, Mean Absolute Value, Variance, etc.)
- **Covariance matrices** (for geometry-based approaches)

---

### Principal Component Analysis (PCA)

To further reduce dimensionality and handle correlations between features, we apply Principal Component Analysis (PCA).

PCA also helps reduce noise, since noise typically contributes less variance than the actual muscle signals.

However, PCA reduces interpretability because the components are linear combinations of the original features. For this reason, some models (such as Neural Networks) are trained without PCA.
---

### Cross-Validation

We used 5-fold cross-validation for model evaluation.

This approach provides a better estimation of model performance than a simple train-test split, since each data point is reused multiple times while maintaining separation between training and validation sets.

It is also computationally more efficient than leave-one-out cross-validation given the size of the dataset.

Since windows are generated independently for each session, using 5 folds ensures that no data leakage occurs between training and validation sets.


---

##  Models

The following regression models are implemented and compared:

- **Ridge Regression**
- **Decision Tree Regressor**
- **Random Forest Regressor**
- **Neural Network (Fully Connected)**
- **Riemannian-based Regression (optional)**

---

### Ensemble Strategy

We combined model predictions using averaging and a meta-learning approach in order to improve predictive performance and reduce overfitting.

---

##  Evaluation Metrics

Model Performance was evaluated using:


- **RMSE (Root Mean Squared Error)**  
- **NMSE (Normalized Mean Squared Error)**  


---

##  Project Structure

```
emg_handpose_prediction/
│
├── projet_sfml.ipynb          # Main training & evaluation notebook
├── preprocessing/             # Windowing & feature extraction
├── models/                    # Model implementations
├── evaluation/                # Metrics & comparison scripts
├── team_submission.csv        # Final predictions
└── README.md
```

---

## 📌 Results

Models are compared using regression metrics and visualized to highlight performance differences.

The best-performing model can be selected based on RMSE or MAE.

---

## Credits

Project completed as part of a master's course in statistics.

Team members:
- Aerts Robin
- Laloy Anouar
- Tfeili Rayane
---

## 📦 Installation

```bash
git clone https://github.com/RayaneTfeili/emg_handpose_prediction.git
cd emg_handpose_prediction
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -U pip
pip install numpy pandas scikit-learn jupyter matplotlib
jupyter notebook
```

---

## 📜 License

MIT License.
