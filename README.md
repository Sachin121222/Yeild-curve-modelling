# Stochastic Yield Curve Modelling with CIR Processes

**Reconstruct the full US Treasury yield curve from only the 3-Month rate using calibrated Cox-Ingersoll-Ross (CIR) models.**

---

## Project Overview

This project builds two progressively richer stochastic interest-rate models to recover the full term structure from minimal observable data.

| Model | Description |
|-------|-------------|
| Single-Factor CIR | Calibrated via Maximum Likelihood Estimation (MLE) on historical 3M short rates |
| Two-Factor CIR + EKF | Two independent latent CIR processes filtered via the Extended Kalman Filter with full-curve MLE |

Both models predict the **6-Month, 9-Month, 1-Year, and 2-Year** zero-coupon yields using *only* the observed 3-Month Treasury yield during the test period.

---

## Repository Structure

```
stochastic-yield-curve/
├── notebooks/
│   └── Stochastic_Yield_Curve_CIR.ipynb
├── data/
│   ├── train_data.csv
│   ├── test_data.csv
│   └── test_data_3M.csv
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Mathematical Background

### CIR Short-Rate Process

The instantaneous short rate follows:

```
dr_t = κ(θ − r_t) dt + σ√r_t dW_t
```

| Symbol | Meaning |
|--------|---------|
| κ (kappa) | Mean-reversion speed |
| θ (theta) | Long-run equilibrium rate |
| σ (sigma) | Volatility coefficient |
| W_t | Standard Brownian motion |

**Feller condition** — guarantees r_t stays positive: `2κθ ≥ σ²`

### Affine Bond Pricing

```
P(t,T) = A(τ) · exp(−B(τ) · r_t)     where τ = T − t

y(t,T) = −ln P(t,T) / τ
```

### Two-Factor Extension

The short rate is decomposed as `r_t = x_t + y_t`, where each factor follows its own CIR SDE. An **Extended Kalman Filter** recursively estimates the hidden state `(x_t, y_t)` from observable yield data.

---

## Dataset Description

| File | Tenors Available | Role |
|------|-----------------|------|
| `train_data.csv` | 3M, 6M, 9M, 1Y, 2Y, 5Y, 10Y, 20Y, 30Y | Model calibration |
| `test_data_3M.csv` | 3M only | Model input during testing |
| `test_data.csv` | 3M, 6M, 9M, 1Y, 2Y | Held-out ground truth |

Yield columns follow the naming convention: `ZC025YR` (3M), `ZC050YR` (6M), `ZC075YR` (9M), etc.

---

## Running the Notebook

### Google Colab (Recommended)

1. Open [colab.research.google.com](https://colab.research.google.com)
2. Go to **File → Upload notebook** and select `Stochastic_Yield_Curve_CIR.ipynb`
3. In the left sidebar, click the Files icon and upload all three CSV files from `data/`
4. Run all cells: **Runtime → Run all**

### Local Environment

```bash
git clone https://github.com/<your-username>/stochastic-yield-curve.git
cd stochastic-yield-curve
pip install -r requirements.txt
jupyter notebook notebooks/Stochastic_Yield_Curve_CIR.ipynb
```

---

## Notebook Structure

| Section | Content |
|---------|---------|
| A — Data Loading & Preprocessing | Load CSVs, clean data, IQR-clip outliers, EDA with correlation heatmap |
| B — Single-Factor CIR | Model specification, MLE calibration, Monte Carlo simulation |
| C — Yield Curve Reconstruction (1F) | Affine pricing, out-of-sample forecasts, RMSE / MAE / R² metrics |
| D — Two-Factor CIR + EKF | Two-factor model, EKF implementation, joint MLE, OOS reconstruction |
| E — Model Comparison | Head-to-head metrics table, visualisations, limitations analysis |
| F — Discussion Questions | 9 in-depth analytical questions with model-grounded answers |

---

## Results Summary

| Model | Avg R² | Notes |
|-------|--------|-------|
| Single-Factor CIR | ~0.62 | Level factor only |
| Two-Factor CIR (EKF-MLE) | >= 0.85 | Level + slope factors; R² > 0.93 at 6M and 9M |

---

## Key Implementation Details

- **MLE optimisation** — L-BFGS-B with explicit bounds enforcing the Feller condition as a hard constraint
- **EKF stability** — Cholesky decomposition used for the innovation covariance; symmetrisation at each step
- **Reflecting boundary** — `np.maximum(..., 0)` applied at each Monte Carlo step to prevent negative rates
- **Warm-start** — Terminal training state carried forward as EKF prior for out-of-sample filtering
- **Modular code** — Preprocessing, calibration, filtering, and scoring implemented as reusable functions/classes

---

## Limitations

- Time-constant parameters (no regime switching)
- Gaussian error assumption (fat tails and jumps are not captured)
- EKF linearisation degrades in high-volatility regimes
- Only two latent factors — curvature, liquidity premia, and macro factors are excluded

---

## Dependencies

```
numpy >= 1.24
pandas >= 2.0
scipy >= 1.11
matplotlib >= 3.7
seaborn >= 0.12
scikit-learn >= 1.3
jupyter
```

---

## Author

**Sachin**
Stochastic Yield Curve Modelling — CIR Processes Project
