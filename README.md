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

## Key Findings (EDA Phase)
- Health signals show near-zero correlation with sector volatility during normal conditions
- During COVID-19, correlations jumped from ~0 to 0.88 (shortness of breath vs XLV volatility)
- Rolling correlation confirms the health-volatility relationship activates only during novel crisis events
- 14.9% of weeks qualify as volatility spikes — the binary classification target for Phase 2
- Google searches for shortness of breath show 0.302 correlation with volatility one week later

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

│   ├── raw/          # Raw API pulls

│   └── processed/    # master_weekly.csv

├── notebooks/

│   ├── data_pulling.ipynb

│   ├── data_cleaning.ipynb

│   ├── eda.ipynb

│   └── plots/

├── src/

│   ├── features/     # Feature engineering (Phase 2)

│   └── models/       # Trained models (Phase 2)


---

## Roadmap
- [x] Data acquisition and pipeline
- [x] Weekly alignment and master dataset
- [x] Exploratory data analysis
- [ ] Feature engineering (lag features, rolling averages)
- [ ] Volatility spike classifier (Random Forest)
- [ ] Regime detection model
- [ ] AWS deployment

---

## Requirements
```bash
pip install -r requirements.txt
```