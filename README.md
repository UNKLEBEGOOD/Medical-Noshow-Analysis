# 🏥 Medical Appointment No-Show Analysis

### Exploring patient, scheduling, and reminder factors behind missed medical appointments

**Author:** Odoh Ekenedirichukwu Johnpaul  
**Role:** Junior Data Analyst  
**Tools:** Python | Pandas | NumPy | Matplotlib | Seaborn | SQL Server | Power BI

---

## 📌 Project Overview

This project analyzes medical appointment data to understand the factors associated with patients missing their scheduled appointments.

The analysis focuses on:

- Patient demographics
- Appointment scheduling patterns
- SMS reminder status
- Waiting time between scheduling and appointment
- Health conditions
- Gender
- Neighbourhood
- Overall appointment no-show rates

The project demonstrates an end-to-end data analysis workflow, from **data cleaning and exploratory analysis in Python to SQL analysis and interactive dashboard development in Power BI**.

---

## 🎯 Business Problem

Missed medical appointments can affect healthcare service delivery, scheduling efficiency, and the utilization of clinical resources.

The objective of this analysis is to answer questions such as:

1. What percentage of appointments were missed?
2. Does receiving an SMS reminder relate to appointment attendance?
3. Does the waiting time between booking and appointment affect no-show rates?
4. Are no-show rates different between male and female patients?
5. Are certain health conditions associated with attendance?
6. Which neighbourhoods have higher or lower no-show rates?
7. What patterns can help healthcare organizations better understand missed appointments?

---

## 🗂️ Dataset

The dataset contains medical appointment records with information about:

- Patient demographics
- Appointment dates
- Scheduling dates
- Gender
- Hypertension
- Diabetes
- Alcoholism
- Handicap status
- Scholarship status
- SMS reminder status
- Neighbourhood
- Appointment attendance status

The analysis initially contained **110,526 appointment records**.

During data validation, **5 records with negative waiting times** were identified and removed because the appointment date occurred before the scheduled date.

The cleaned dataset therefore contains approximately **110,521 records**.

---

# 🔧 Project Workflow

The project followed these major stages:

### 1. Data Loading

The original Excel dataset was loaded into Python using Pandas.

```python
df = pd.read_excel("data/raw/Medical-no-show.xlsx")
```

### 2. Initial Data Inspection

Checked the shape, column data types, missing values, and duplicate rows before making any changes.

```python
df.shape
df.info()
df.isna().sum()
df.duplicated().sum()
```

No missing values or duplicate rows were found.

### 3. Data Cleaning

- Renamed columns with typos or inconsistent naming (`Hipertension` → `Hypertension`, `No-show` → `No_show`)
- Converted `ScheduledDay` and `AppointmentDay` to proper datetime format
- Removed 1 record with an invalid negative age
- Fixed a character encoding issue affecting Portuguese neighbourhood names (9 of 81 names retain minor, unrecoverable character loss from a pre-existing defect in the source file)
- Simplified the `Handcap` field (0–4 disability count) into a binary `Has_Handicap` flag, since categories above 1 were too rare to analyze meaningfully on their own
- Engineered a `Wait_Days` column, the number of days between scheduling and the appointment itself
- Identified and removed 5 records with a logically impossible negative wait time

### 4. Descriptive Statistics

Reviewed count, mean, min, max, and quartiles across all numeric fields to understand the overall shape of the data before visualizing it.

### 5. Outlier and Rare Category Checks

Applied the IQR method to `Age`, the only genuinely continuous numeric field, and used frequency thresholds to flag rare categories (e.g. `Alcoholism`, and `Handcap` levels above 1) rather than misapplying outlier detection to binary flags.

### 6. Univariate Analysis

Visualized the distribution of Age and the proportions of each categorical variable individually.

### 7. Bivariate Analysis

Compared `No_show` against Age, Gender, SMS_received, and each health condition, using both raw counts and calculated no-show rate percentages, since group sizes varied significantly.

### 8. Correlation Analysis

Built a correlation heatmap across the numeric fields to check for meaningful relationships at a glance.

### 9. SQL Analysis

Loaded the cleaned dataset into a SQL Server database and independently re-verified every major Python finding using SQL aggregate queries (`GROUP BY`, `CASE WHEN`, percentage calculations).

### 10. Power BI Dashboard

Connected Power BI directly to the SQL Server database and built an interactive dashboard with KPI cards, a status donut chart, a time trend, comparative bar charts, slicers, and a written key insights panel.

---

## 💡 Key Insights

**1. SMS reminders are linked to a *higher* no-show rate, not a lower one, and the reminder itself is not the cause.**  
Patients who received an SMS reminder missed their appointment 27.6% of the time, versus 16.7% for those who did not. This is explained by wait time: patients who received an SMS waited a median of 14 days between booking and their appointment, compared to a median of 0 days for those who did not. A longer gap between booking and the appointment is independently linked to a higher chance of missing it, confirmed by an average wait of 19.0 days (SMS) vs 6.0 days (no SMS), and a 0.40 correlation, the strongest relationship in the dataset.

**2. Chronic health conditions are linked to slightly *lower* no-show rates.**  
Hypertension: 17.3% vs 20.9%. Diabetes: 18.0% vs 20.4%. Patients managing an ongoing condition may be more consistent about attending care.

**3. Gender and Alcoholism show no meaningful relationship with no-show behavior.**  
Gender: 20.3% (female) vs 20.0% (male). Alcoholism: 20.19% vs 20.15%.

**4. No-show rate varies meaningfully by neighbourhood.**  
Among neighbourhoods with a meaningful volume of appointments, rates ranged from roughly 16% to 26%, suggesting location-based factors (such as distance or transport access) may play a role.

**5. The dataset only spans 3 months (April–June 2016), with April and June only partially covered.**  
This limits any time-based or seasonal conclusions; an apparent dip in the appointments-over-time trend reflects this partial coverage, not a real behavioral pattern.

---

## 🗂️ Project Structure

```
medical-noshow-portfolio/
├── data/
│   ├── raw/           → original dataset
│   └── clean/         → cleaned CSV and SQL exports
├── notebooks/         → full Jupyter Notebook analysis
├── sql/               → SQL queries used for independent verification
├── visuals/           → exported chart images
├── dashboard/         → Power BI dashboard file (.pbix)
└── README.md
```

---

## 📊 Power BI Dashboard

![Dashboard Screenshot](Medical-Noshow-Portfolio/dashboard/Dashboard_screenshot.png)

The full interactive file (`APPOINTMENTS db.pbix`) is available in the `dashboard/` folder — open with Power BI Desktop to filter and explore.

The dashboard includes:

- Summary KPI cards (Total Appointments, No-Show Count, No-Show Rate, Average Wait Days)
- Appointment Status donut chart
- Appointments Over Time trend
- No-Show Rate by Neighbourhood (filtered for meaningful volume)
- No-Show Rate by SMS Reminder Status
- Interactive slicers (Gender, SMS Status, Appointment Date)
- A written Key Insights panel

---

## ⚠️ Limitations

- The dataset covers only a short, 3-month window, limiting seasonal analysis
- A small number of neighbourhood names retain minor, unrecoverable character corruption from a pre-existing encoding issue in the source file
- Correlational findings (e.g. SMS reminders, wait time) describe association, not proven causation

---

## Files in This Project

- `notebooks/Medical-Noshow-Analysis.ipynb` — full analysis: Python (pandas) + SQL Server (T-SQL), cross-validated
- `data/clean/medical_noshow_clean.csv` — cleaned dataset (feeds Power BI)
- `visuals/` — all exported charts (PNG)
- `dashboard/` — Power BI dashboard file (.pbix)

---

## 📬 Contact

**Odoh Ekenedirichukwu Johnpaul**  
LinkedIn: [linkedin.com/in/kene08](https://linkedin.com/in/kene08)

