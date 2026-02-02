# Advanced Intra-day Revenue Forecasting

## 🚀 Business Impact & Results

This project was developed to address a critical gap in daily revenue predictability for a large retail company. Moving from a static monthly target model to a dynamic **intra-day forecasting system**, the solution enabled the business to react to sales deviations in near real-time.

**Key Achievements:**
* **51% Reduction in Forecast Error:** The Machine Learning model significantly outperformed the previous historical average method (baseline).
* **Intra-day Granularity:** Transitioned from D-1 analysis to **4 daily updates** (12:00, 15:00, 17:30, 23:59), allowing the Growth/Marketing team to trigger corrective campaigns within the same day.
* **Risk Management:** Implemented **Probabilistic Forecasting** (90% Confidence Interval), providing Pessimistic, Median, and Optimistic scenarios rather than a single point estimate.
* **Automated Governance:** Full lifecycle automation with drift detection and structural break checks.

---

## ⚠️ Portfolio Disclaimer

**This repository contains an anonymized version of the original project.**

To protect proprietary information and adhere to Non-Disclosure Agreements (NDA):
1.  **Data Anonymization:** All real sales values, product names, and business unit identifiers have been obfuscated or replaced with synthetic distributions.
2.  **Core Logic Preserved:** The code demonstrates the actual modeling techniques, feature engineering strategies, and pipeline architecture used in production.
3.  **Infrastructure Abstraction:** Specific paths, credentials, and cloud-specific configurations (Azure/Databricks) have been genericized.

---

## 🏗️ Solution Architecture

The solution follows the **CRISP-DM** methodology and operates as an end-to-end pipeline on Databricks, orchestrated to handle high-frequency data.

### Technical Highlights
The project utilizes advanced time-series analysis techniques beyond standard regression:
* **Signal Processing:** Applied **Fast Fourier Transform (FFT)** and **Wavelet Transforms (Morlet)** during EDA to uncover hidden frequencies and non-stationary seasonality patterns.
* **Structural Break Detection:** Implemented the **Chow Test** to statistically validate regime changes in sales behavior, automatically triggering model retraining if the data distribution shifts significantly.
* **Statistical Process Control (SPC):** Used **I-MR Control Charts** (Individuals and Moving Range) to monitor model residuals and production metrics (WAPE) for anomalies.

### Tech Stack
* **Languages:** Python (Pandas, NumPy)
* **Modeling:** LightGBM (Gradient Boosting), Darts (Time Series Library)
* **Stats & Math:** SciPy, Statsmodels (ACF/PACF, Decomposition, Hypothesis Testing)
* **MLOps:** MLflow (Experiment Tracking & Model Registry)
* **Visualization:** Plotly, Seaborn, Matplotlib

---

## 📂 Repository Structure

The codebase is organized into sequential notebooks representing the data science lifecycle:

| Notebook | Description & Techniques |
| :--- | :--- |
| **01_raw_data_transformation** | **Data Engineering:** Ingestion of raw transactional data, cleaning, and resampling to 30-minute granularity. |
| **02_eda_feature_analysis** | **Advanced EDA:** Stationarity tests (ADF), Normality tests (Shapiro-Wilk), Seasonal Decomposition, FFT & Wavelet analysis, and correlation matrices (Spearman/Pearson). |
| **03_baseline_metrics** | **Benchmarking:** Calculation of the "AS-IS" process error (manual historical averages) using metrics like WAPE, MAPE, and RMSE to establish a baseline. |
| **04_model_training** | **Modeling:** Training a **LightGBM** model with **Quantile Loss** (q0.05, q0.5, q0.95) to generate prediction intervals. Includes feature engineering (lags, rolling windows, holidays) and MLflow logging. |
| **05_structural_break_check** | **Statistical Monitoring:** Execution of the **Chow Test** to compare regression coefficients between time periods, determining if the model requires retraining due to market shifts. |
| **06_model_evaluation** | **Production Monitoring:** Continuous tracking of model performance using control charts (I-MR) to detect drift, trends, or sequence anomalies in error metrics. |
| **07_inference_pipeline** | **Deployment:** The production inference script that loads the registered MLflow model, processes live data, and writes forecasts to the serving layer. |

---

## 📈 Methodology

### 1. Feature Engineering
* **Temporal Features:** Hour, day of week, month, and cyclic encoding of time.
* **Event Flags:** Custom holiday mapping (including floating holidays like Carnival, Mother's Day, and Black Friday) tailored to the Brazilian market.
* **Lag Features:** Autoregressive components identified via ACF/PACF analysis.

### 2. Modeling Strategy
* **Algorithm:** LightGBM (Gradient Boosting Decision Tree).
* **Objective:** Quantile Regression to support uncertainty estimation.
* **Validation:** Time Series Split (Sliding Window) to respect temporal order and prevent data leakage.

### 3. Monitoring & Retraining
* The system does not just predict; it monitors itself.
* **Trigger:** If the **Structural Break Check (Notebook 05)** rejects the null hypothesis (H0), a retraining pipeline is automatically triggered.
* **Alerting:** Drifts in WAPE (Weighted Absolute Percentage Error) detected via Control Charts alert the data science team.

---

## 📬 Contact
* **Author:** [Your Name]
* **Role:** Data Scientist
* **Links:** [LinkedIn Profile] | [Portfolio Website]