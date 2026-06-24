# 🔍 Chicago Crime Analysis (2001–2022)
### Project 3 — Part 1 | Time Series & EDA

[![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python)](https://python.org)
[![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?logo=pandas)](https://pandas.pydata.org)
[![Seaborn](https://img.shields.io/badge/Seaborn-0.13-4C72B0)](https://seaborn.pydata.org)
[![Platform](https://img.shields.io/badge/Platform-Google%20Colab-F9AB00?logo=google-colab)](https://colab.research.google.com)

---

## 📌 Project Overview

This project performs a comprehensive **Exploratory Data Analysis (EDA)** and **Time Series Analysis** on the Chicago Crime Dataset (2001–2022), containing over **7.7 million crime records**.

The analysis answers real-world stakeholder questions about crime trends, geographic distribution, temporal patterns, and seasonal cycles — as if reporting to a local newspaper journalist.

---

## 📂 Dataset

| Property | Detail |
|----------|--------|
| **Source** | [Chicago Data Portal — Crimes 2001 to Present](https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-Present/ijzp-q8t2/data) |
| **Format** | 22 CSV files (one per year), packaged in a ZIP |
| **Total Records** | 7,713,109 rows |
| **Date Range** | January 1, 2001 → December 31, 2022 |
| **Key Columns** | `Date`, `Primary Type`, `Description`, `Location Description`, `Arrest`, `Domestic`, `District`, `Ward`, `Latitude`, `Longitude` |

---

## 🗂️ Project Structure

```
chicago_crime_analysis_1.ipynb   ← Main analysis notebook
README.md                        ← Project documentation
```

---

## 🛠️ Libraries Used

```python
pandas          # Data loading, cleaning, groupby, resampling
numpy           # Numerical operations
matplotlib      # Custom charts and visualizations
seaborn         # Statistical plots with styling
statsmodels     # Seasonal decomposition (seasonal_decompose)
scipy.signal    # Peak detection (find_peaks)
holidays        # US holiday calendar
glob / os / zipfile  # File system and ZIP handling
```

---

## 🔄 Workflow Summary

### Step 0 — Import Libraries
All required libraries imported and environment configured (`sns.set_theme`, `rcParams`).

### Step 1 — Mount Google Drive & Extract ZIP
Dataset loaded from Google Drive and extracted to `/content/chicago_data`.

### Step 2 — Load All CSV Files
22 annual CSV files merged into a single DataFrame using `glob` + `pd.concat`.

### Step 3 — Clean & Prepare Data
- Parsed `Date` column to datetime with `errors='coerce'`
- Dropped unparseable rows (0 found)
- Set `Date` as index and sorted chronologically
- Engineered features: `Year`, `Month`, `Hour`, `date_only`
- Created two data forms:
  - **Form 1:** Individual crime rows (datetime index)
  - **Form 2:** Daily crime counts via `.resample('D').size()`

---

## 📊 Topics Analyzed

### 📍 Topic 1 — Comparing Police Districts (2022)

**Questions answered:**
- Which district had the most crimes in 2022?
- Which district had the least?

**Key Findings:**

| | District | Crime Count |
|--|----------|-------------|
| 🔴 Most | District 8 | 14,805 crimes |
| 🟢 Least | District 31 | 15 crimes |

> There is a significant geographic disparity in crime distribution across Chicago's police districts.

---

### 📈 Topic 2 — Crimes Across the Years

**Questions answered:**
- Is total crime increasing or decreasing?
- Are there individual crime types going in the opposite direction?

**Key Findings:**
- Overall trend: **DECREASING 📉** — from **485,886** (2001) to **238,858** (2022)
- Crime nearly halved over two decades

**Crime types going against the trend (increasing while overall decreases):**

| Crime Type | Pattern |
|------------|---------|
| Weapons Violation | Significant increase, especially post-2015 |
| Criminal Sexual Assault | Gradual upward trend |
| Deceptive Practice | Steady rise |
| Stalking | Emerging upward trend |
| Concealed Carry License Violation | Sharp recent increase |

---

### 🚗 Topic 3 — AM vs PM Rush Hour

**Questions answered:**
- Are crimes more common during AM or PM rush hour?
- What are the top 5 crimes during each period?
- Are Motor Vehicle Thefts more common in AM or PM?

**Time windows:**
- AM Rush: 7:00 AM – 10:00 AM
- PM Rush: 4:00 PM – 7:00 PM

**Key Findings:**

| Period | Total Crimes |
|--------|-------------|
| AM Rush | 770,651 |
| PM Rush | **1,206,353** ✅ more |

| Rank | AM Rush Hour | PM Rush Hour |
|------|-------------|-------------|
| 1 | Theft | Theft |
| 2 | Battery | Battery |
| 3 | Criminal Damage | Criminal Damage |
| 4 | Burglary | Narcotics |
| 5 | Other Offense | Assault |

**Motor Vehicle Theft:**
- AM Rush: 41,578
- PM Rush: **53,716** (more common in PM)

---

### 📅 Topic 4 — Comparing Months

**Questions answered:**
- Which months have the most/least crime?
- Are there crimes that don't follow the seasonal pattern?

**Key Findings:**

| | Month(s) | Crime Count |
|--|---------|-------------|
| 🔴 Most | July | 717,232 |
| 🟢 Least | February | 529,391 |

**Crimes that peak in winter (against the summer pattern):**
- Deceptive Practice
- Domestic Violence
- Human Trafficking
- Offense Involving Children
- Prostitution
- Ritualism

---

### 🔁 Topic 5 — Seasonality & Cycles

**Questions answered:**
- What cycles exist in the data?
- How long is each cycle? What is its magnitude?

**Method:** `statsmodels.tsa.seasonal.seasonal_decompose()` (additive model) applied to weekly-resampled data.

**All Crimes (Weekly):**
| Property | Value |
|----------|-------|
| **Cycle Length** | ~364 days (annual) |
| **Seasonal Fluctuation** | ~1,599 crimes/week |
| **Trend** | Steady decline until ~2015, then stabilizing with slight uptick near 2020–2022 |
| **Residuals** | Random noise around zero |

**Motor Vehicle Theft (Weekly):**
| Property | Value |
|----------|-------|
| **Cycle Length** | Annual |
| **Seasonal Fluctuation** | ~55 crimes/week |
| **Trend** | Long-term decline, but **sharp recent increase** |

> **Key Insight:** Chicago's overall crime declined significantly over two decades, but strong annual seasonal cycles persist. Motor Vehicle Theft is a notable exception showing a concerning recent rise.

---

## 📊 Summary Table

| Topic | Key Finding |
|-------|-------------|
| 🏙️ Districts | District 8 had the most crimes (14,805); District 31 the least (15) in 2022 |
| 📉 Yearly Trend | Overall crime nearly halved: ~486k (2001) → ~239k (2022) |
| 🕐 Rush Hour | PM Rush has 57% more crimes than AM Rush |
| 📅 Monthly | July peaks; February lowest. Winter crimes include Domestic Violence & Human Trafficking |
| 🔁 Seasonality | Strong annual cycle (~1,599 weekly fluctuation); MVT shows recent surge |

---



## 👤 Author

**Mohammed** — Data Science & Machine Learning  
Axsos Academy Bootcamp — Project 3
