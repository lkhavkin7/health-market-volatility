# How Health Crises Move Markets
### Predicting Sector Volatility Using Public Health Data
**Leo Khavkin | Wentworth Institute of Technology | Senior Capstone 2026**

---

## Project Overview

This project investigates whether granular public health signals — CDC influenza surveillance,
Google symptom search trends, and Medicare utilization data — precede volatility spikes in
US healthcare, pharma, biotech, and insurance sector ETFs before official announcements.

The core question: was the data already telling the story before anyone was listening?

---

## Key Findings — EDA Phase

- Health signals show near-zero correlation with sector volatility during normal conditions
- During COVID-19, correlations jumped from -0.02 to 0.92 (shortness-of-breath search trends vs. XLV volatility)
- Rolling 52-week correlation confirms the health-volatility relationship activates only during novel crisis events, not routine flu seasons
- This pattern holds consistently across all four sector ETFs tested (XLV, XBI, PJP, KIE) — the relationship is crisis-driven, not sector-specific
- Google searches for "shortness of breath" show a 0.317 correlation with volatility one week later — the strongest lagged signal found
- 14.9% of weeks qualify as volatility spikes (1.5x historical mean threshold) — the binary classification target used in modeling

## Key Findings — Modeling Phase

Four independent model architectures were built and evaluated: a binary spike classifier, a two-stage ARIMA+Random Forest model, an LSTM, and a 52-week rolling-correlation regime detector.

Across the spike classifier, ARIMA+Random Forest, and LSTM (evaluated two ways), every honestly-evaluated out-of-sample result is at or below baseline. For the two regression models, that baseline is R² = 0 (predicting the mean): ARIMA+RF scores R² = −0.27 on the full-period split and R² = −0.14 on the COVID-blind split; the LSTM scores R² = −1.18 full-period and R² = −1.34 COVID-blind. For the spike classifier, the relevant baseline is chance-level classification (ROC-AUC = 0.5): the genuinely blind COVID-period test scores ROC-AUC = 0.53 — no better than a coin flip. This convergence across three model families with different mechanisms and assumptions is the central finding: **lagged public-health signals do not carry exploitable out-of-sample predictive power for healthcare-sector volatility, even during the novel COVID shock.** Additional model capacity did not help — the LSTM, the most expressive architecture tested, performed no better than simpler models, indicating the limiting factor is an absence of predictive signal in pre-crisis data, not insufficient model complexity.

The one result that does hold up is detection rather than prediction: the regime detector identified the COVID-driven shift in the health-volatility relationship with zero false positives across 14 years of data — confirming the relationship is real and measurable, but only after it has begun, not before.

Together, these results define a clear boundary condition: the health-volatility relationship exists and is detectable in real time during a genuine crisis, but it is not predictable in advance from historical health-signal data alone — a finding reached independently across four model families, which makes it considerably more defensible than any single model's output would be.

Each notebook documents its own leakage checks and corrections in detail — including two cases (ARIMA full-series fit, LSTM scaler fit) where an initial inflated result was caught and corrected before being reported.

---

## Dataset

| Source | Description | Frequency |
|--------|-------------|-----------|
| yfinance | XLV, XBI, PJP, KIE sector ETFs | Daily → Weekly |
| FRED API | CPI, unemployment, interest rates | Monthly → Weekly |
| Google Trends | Flu symptom search volume | Weekly |
| CDC FluView | Influenza-like illness surveillance | Weekly |
| NCHS | Leading causes of death | Annual → Weekly |
| CMS Medicare | Inpatient provider utilization | Snapshot |

**Master dataset: 730 weeks × 19 columns | January 2010 – December 2023**

---

## Project Structure

health-market-volatility/

├── data/

│   ├── raw/                      # Raw API pulls

│   └── processed/                # master_weekly.csv

├── notebooks/

│   ├── data_pulling.ipynb

│   ├── data_cleaning.ipynb

│   ├── eda.ipynb

│   ├── arima-forest.ipynb        # Stage 1+2 model, leakage-corrected

│   ├── lstm.ipynb                # LSTM, leakage-corrected, two eval splits

│   ├── spike_classifier.ipynb    # Binary spike classifier

│   ├── regime_detection.ipynb    # Rolling-correlation regime detector

│   └── plots/                    # All saved figures (plot1–plot20+)

├── src/

│   ├── features/                 # Saved feature matrices (X, y) per model

│   └── models/                   # Saved fitted models (.pkl / .keras)


All modeling and analysis logic lives in the notebooks above; `src/` holds saved artifacts (fitted models and feature matrices) exported from those notebooks so they're inspectable and reusable without re-running everything. A full `src/` package refactor — extracting the modeling code itself into reusable modules — remains a separate, larger task for post-capstone.

---

## Roadmap

- [x] Data acquisition and pipeline (6 sources)
- [x] Weekly alignment and master dataset
- [x] Exploratory data analysis
- [x] Feature engineering (lag features)
- [x] Volatility spike classifier (Random Forest)
- [x] ARIMA + Random Forest residual model (leakage identified and corrected)
- [x] LSTM model (leakage identified and corrected; restructured to a true (4, 8) multi-step sequence architecture)
- [x] Regime detection model
- [x] `src/` artifacts saved (trained models + feature sets)
- [ ] `src/` full package refactor (code extraction, still open)
- [ ] AWS deployment (post-capstone)

---

## Requirements

```bash
pip install -r requirements.txt
```
