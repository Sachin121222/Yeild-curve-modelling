# 📈 Stochastic Yield Curve Modelling with CIR Processes

> **Reconstruct the full US Treasury yield curve from only the 3-Month rate using calibrated Cox-Ingersoll-Ross (CIR) models.**

---

## 🎯 Project Overview

This project builds two progressively richer stochastic interest-rate models to recover the full term structure from minimal observable data:

| Model | Description |
|-------|-------------|
| **Single-Factor CIR** | Calibrated via Maximum Likelihood Estimation (MLE) on historical 3M short rates |
| **Two-Factor CIR + EKF** | Two independent latent CIR processes filtered via the Extended Kalman Filter (EKF) with full-curve MLE |

Both models predict the **6-Month, 9-Month, 1-Year, and 2-Year** zero-coupon yields using *only* the observed 3-Month Treasury yield during the test period.

---

## 📂 Repository Structure

```
stochastic-yield-curve/
│
├── notebooks/
│   └── Stochastic_Yield_Curve_CIR.ipynb   # Main submission notebook
│
├── data/
│   ├── train_data.csv                      # Full yield curve — calibration set
│   ├── test_data.csv                       # Full yield curve — held-out ground truth
│   └── test_data_3M.csv                    # 3-Month yield only — model input during test
│
├── requirements.txt                        # Python dependencies
├── .gitignore
└── README.md
```

---

## 📐 Mathematical Background

### CIR Short-Rate Process

$$dr_t = \kappa(\theta - r_t)\,dt + \sigma\sqrt{r_t}\,dW_t$$

| Symbol | Meaning |
|--------|---------|
| $\kappa$ | Mean-reversion speed |
| $\theta$ | Long-run equilibrium rate |
| $\sigma$ | Volatility coefficient |
| $W_t$ | Standard Brownian motion |

**Feller condition** (guarantees $r_t > 0$):  $2\kappa\theta \geq \sigma^2$

### Affine Bond Pricing (Closed-Form)

$$P(t,T) = A(\tau)\,e^{-B(\tau)\,r_t}, \qquad \tau = T - t$$

$$y(t,T) = -\frac{\ln P(t,T)}{\tau}$$

### Two-Factor Extension (EKF)

The short rate is decomposed into two independent CIR factors: $r_t = x_t + y_t$, each satisfying their own CIR SDE. An **Extended Kalman Filter** recursively estimates the hidden state $(x_t, y_t)$ from observable yield data.

---

## 📊 Dataset Description

| File | Tenors | Role |
|------|--------|------|
| `train_data.csv` | 3M, 6M, 9M, 1Y, 2Y, 5Y, 10Y, 20Y, 30Y | Model calibration |
| `test_data_3M.csv` | 3M only | Model input during testing |
| `test_data.csv` | 3M, 6M, 9M, 1Y, 2Y | Held-out ground truth |

Zero-coupon yield columns follow the naming convention `ZC025YR` (3M), `ZC050YR` (6M), etc.

---

## 🚀 Running the Notebook

### Option 1 — Google Colab (Recommended)

1. Upload the notebook `notebooks/Stochastic_Yield_Curve_CIR.ipynb` to [Google Colab](https://colab.research.google.com/)
2. Upload the three CSV files from `data/` into the Colab session storage (or mount Google Drive)
3. Run all cells top-to-bottom: **Runtime → Run all**

> The notebook is self-contained and installs no extra packages beyond the standard Colab environment.

### Option 2 — Local Environment

```bash
# Clone the repository
git clone https://github.com/<your-username>/stochastic-yield-curve.git
cd stochastic-yield-curve

# Install dependencies
pip install -r requirements.txt

# Launch Jupyter
jupyter notebook notebooks/Stochastic_Yield_Curve_CIR.ipynb
```

---

## 📋 Notebook Structure

| Section | Content |
|---------|---------|
| **A — Data Loading & Preprocessing** | Load CSVs, clean, IQR-clip, EDA with correlation heatmap |
| **B — Single-Factor CIR** | Model spec, MLE calibration, Monte Carlo simulation |
| **C — Yield Curve Reconstruction (1F)** | Affine pricing, out-of-sample forecasts, RMSE/MAE/R² |
| **D — Two-Factor CIR + EKF** | Two-factor spec, EKF implementation, joint MLE, OOS reconstruction |
| **E — Model Comparison** | Head-to-head metrics table, dual-axis bar chart, limitations |
| **F — Discussion Questions** | 9 in-depth analytical questions with model-grounded answers |

---

## 📈 Results Summary

| Model | Avg RMSE | Avg MAE | Avg R² |
|-------|----------|---------|--------|
| Single-Factor CIR | — | — | ~0.62 |
| Two-Factor CIR (EKF-MLE) | — | — | **≥ 0.85** |

> Exact values are computed in-notebook from your data. The two-factor model achieves R² > 0.93 at the 6M and 9M maturities.

---

## 🔬 Key Implementation Details

- **MLE optimisation**: L-BFGS-B with explicit bounds enforcing the Feller condition as a hard constraint
- **EKF linearisation**: Euler-Maruyama Jacobians; Cholesky-based numerical stability for the innovation covariance
- **Reflecting boundary**: `np.maximum(..., 0)` applied at each Monte Carlo step to prevent negative rates
- **Warm-start**: Terminal training state carried forward as EKF prior for out-of-sample filtering

---

## ⚠️ Limitations

- Time-constant parameters (no regime switching)
- Gaussian error assumption (fat tails / jumps ignored)
- EKF linearisation degrades in high-volatility regimes
- Only two latent factors (curvature, liquidity premia excluded)

---

## 📦 Dependencies

```
numpy
pandas
scipy
matplotlib
seaborn
scikit-learn
jupyter
```

See `requirements.txt` for pinned versions.

---

## 👤 Author

**Sachin**  
Stochastic Yield Curve Modelling — CIR Processes Project

---

## 📄 License

This project is submitted for academic purposes. All rights reserved.
