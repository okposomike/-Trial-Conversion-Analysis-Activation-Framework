---

# 📊 Trial Conversion Analysis & Activation Framework

## 🧠 Project Overview

This project analyzes user behavior during a free trial period to understand what drives conversion from trial to paid users.

The goal is to identify key product interactions that signal successful adoption and to build a data-driven **trial activation framework** that can be used to improve conversion rates.

---

## 🎯 Business Problem

Many organizations sign up for a free trial, but only a small percentage convert to paid users.

This project answers:

* What actions do successful users take?
* Does more activity lead to conversion?
* Which features truly drive product value?
* How can we define a “successful trial”?

---

## 📁 Dataset Description

The dataset contains **event-level user activity logs**, where each row represents an action performed by an organization during the trial period.

### Key Columns:

* `organization_id` → Unique company identifier
* `activity_name` → Action performed
* `timestamp` → Time of activity
* `trial_start`, `trial_end` → Trial period
* `converted` → Whether the organization converted

---

## 🛠️ Tools & Technologies

* **Python (Pandas, Matplotlib)** → Data cleaning & analysis
* **SQL (SQLite)** → Data modeling
* **Jupyter Notebook** → Development environment

---

## 🔄 Project Workflow

---

### 🔹 1. Data Cleaning & Preparation

* Converted date columns to proper datetime format
* Removed duplicates
* Standardized column names
* Handled missing values

📌 *Why:* Ensures accuracy and consistency for analysis.

---

### 🔹 2. Feature Engineering

Created new features to better understand user behavior:

* `days_since_start` → Tracks activity timing
* `time_to_convert` → Measures conversion speed
* `events_per_day` → Engagement intensity
* `early_events` → Activity within first 7 days

📌 *Why:* Raw data alone is not enough — features reveal patterns.

---

### 🔹 3. Organization-Level Aggregation

Aggregated event data into organization-level metrics:

* Total events
* Unique activities
* Active days
* Conversion status

📌 *Why:* Business decisions are made at the organization level, not event level.

---

### 🔹 4. Exploratory Data Analysis (EDA)

Key analyses performed:

* Overall conversion rate
* Most common activities
* Feature-level conversion rates (bias-free method)
* Engagement vs conversion

📌 *Why:* To identify patterns and potential drivers of conversion.

---

### 🔹 5. Key Findings from EDA

* High activity does **not strongly guarantee conversion**
* Early engagement shows a **moderate positive relationship** with conversion
* Certain product features are **strong indicators of success**

---

### 🔹 6. Trial Goals Definition

Based on analysis, key product actions were selected as **trial goals**:

* `Scheduling.Shift.Created`
* `PunchClock.PunchedIn`
* `Mobile.Schedule.Loaded`
* `Scheduling.Shift.Approved`

📌 *Selection Criteria:*

* High conversion impact
* Used by many organizations
* Represent core product workflows
* Occur early in the trial

---

### 🔹 7. SQL Data Modeling (SQLite)

#### Trial Goals Table

Tracks whether each organization completed key actions:

```sql
SELECT
    organization_id,
    MAX(CASE WHEN activity_name = 'Scheduling.Shift.Created' THEN 1 ELSE 0 END) AS created_shift,
    MAX(CASE WHEN activity_name = 'PunchClock.PunchedIn' THEN 1 ELSE 0 END) AS punch_clock,
    MAX(CASE WHEN activity_name = 'Mobile.Schedule.Loaded' THEN 1 ELSE 0 END) AS viewed_schedule,
    MAX(CASE WHEN activity_name = 'Scheduling.Shift.Approved' THEN 1 ELSE 0 END) AS approved_shift,
    MAX(converted) AS converted
FROM raw_data
GROUP BY organization_id;
```

---

#### Trial Activation Table

Defines whether an organization is **activated**:

```sql
SELECT *,
    CASE 
        WHEN created_shift = 1
        AND punch_clock = 1
        AND viewed_schedule = 1
        AND approved_shift = 1
        THEN 1 ELSE 0
    END AS activated
FROM trial_goals;
```

📌 *Why:* Converts behavior into a measurable success metric.

---

### 🔹 8. Validation of Activation Framework

* Compared conversion rates between activated and non-activated users
* Analyzed number of goals completed vs conversion

📌 *Result:*
Activated users showed significantly higher conversion rates.

---

### 🔹 9. Visualization

Key visualizations:

* Conversion rate: Activated vs Non-Activated
* Conversion rate vs Number of goals completed
* Early engagement vs conversion

📌 *Why:* Makes insights clear and actionable.

---

## 💡 Key Insights

* **Feature usage matters more than activity volume**
* **Early engagement influences conversion but is not sufficient alone**
* **Core product actions are strong indicators of success**
* **Activation is a powerful predictor of conversion**

---

## 📈 Business Recommendations

* Improve onboarding to guide users toward key actions early
* Highlight core features that drive value
* Monitor early engagement and intervene when low
* Track activation as a key product metric

---

## 🏁 Conclusion

This project demonstrates how user behavior data can be transformed into a practical activation framework that helps businesses understand and improve trial-to-paid conversion.

---

## 👨‍💻 Author

**Michael Okposo**
𝐃𝐚𝐭𝐚 𝐒𝐜𝐢𝐞𝐧𝐭𝐢𝐬𝐭 | 𝐃𝐚𝐭𝐚 𝐀𝐧𝐚𝐥𝐲𝐭𝐢𝐜𝐬 | 𝐌𝐚𝐜𝐡𝐢𝐧𝐞 𝐋𝐞𝐚𝐫𝐧𝐢𝐧𝐠 | 𝐏𝐲𝐭𝐡𝐨𝐧 | 𝐒𝐐𝐋 | 𝐏𝐨𝐰𝐞𝐫 𝐁𝐈 | 𝐀𝐝𝐯𝐚𝐧𝐜𝐞𝐝 𝐄𝐱𝐜𝐞𝐥 |𝐏𝐡𝐚𝐫𝐦𝐚𝐜𝐢𝐬𝐭

---

