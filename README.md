# HealthConnect Appointment No-Show Prediction

## Project Overview

HealthConnect is a healthcare data science project focused on predicting whether a scheduled appointment will result in a **No-Show or Attendance**.

The project began in **Week 4** with the definition of a supervised binary classification problem and progressed in **Week 5** to data preparation, feature engineering, patient-aware validation, and a Logistic Regression baseline. **Week 6** focused on improving and validating the modelling approach.

The dataset is synthetic and contains **5,000 appointment records**:

- 2,423 No-Shows
- 2,314 Attended
- 263 Cancelled

Cancelled appointments were excluded from binary modelling, leaving **4,737 modelling records** with a **51.15% No-Show rate**.

---

## Week 4 — Problem Definition

The modelling problem was defined as **supervised binary classification**:

| Appointment Outcome | Target |
|---|---:|
| No-Show | 1 |
| Attended | 0 |
| Cancelled | Excluded |

The objective is to predict no-show risk using information reasonably available before the appointment outcome is known.

---

## Week 5 — Baseline Modelling

Week 5 focused on:

- Data-quality assessment and preparation
- Exploratory data analysis
- Feature engineering
- Leakage prevention
- Patient-aware train/test separation
- Logistic Regression baseline development
- Model evaluation using Accuracy, Precision, Recall, F1-score and ROC-AUC
- Error and coefficient interpretation

Important predictive signals included **booking lead time, previous no-show behaviour, reminder information, appointment context and distance**.

`waiting_time_minutes` was excluded from the predictive feature set because it may not be reliably available before the appointment.

---

## Week 6 — Model Improvement & Validation

Week 6 extended the baseline through:

- Error analysis using TP, TN, FP and FN
- Segment-level error investigation
- Feature refinement and testing of previously dropped variables
- Patient-grouped cross-validation using `GroupKFold`
- Comparison of Decision Tree, Random Forest and Gradient Boosting
- Testing a lead-time × previous no-show rate interaction
- Integration of findings from the Data Analytics track
- Cumulative-gains analysis from a practical outreach perspective

### Model Results

| Model | Test ROC-AUC | Recall | F1 |
|---|---:|---:|---:|
| Logistic Regression | 0.671 | 0.619 | 0.617 |
| Decision Tree | 0.674 | 0.594 | 0.620 |
| **Random Forest** | **0.683** | 0.625 | 0.630 |
| Gradient Boosting | 0.678 | **0.650** | **0.641** |

### Model Decision

**Random Forest** was selected as the primary candidate because it achieved the highest cross-validated ROC-AUC (**0.673**) and test ROC-AUC (**0.683**).

**Gradient Boosting** remains a strong alternative because it achieved the highest Recall (**0.650**) and F1-score (**0.641**).

Overall, model improvement was **modest**. The results show that model selection alone does not provide a major improvement, making further validation, threshold analysis and model refinement important.

---

## Key Findings

- Lead time and previous no-show behaviour remain important predictive signals.
- Random Forest provided the best overall discrimination, but only a modest improvement over Logistic Regression.
- Gradient Boosting provided higher Recall and F1-score.
- Error analysis identified segments where the model makes more mistakes.
- Cumulative-gains analysis showed that both the baseline and Random Forest can improve on random targeting, but neither provided a clear practical advantage at the tested outreach levels.
- The lead-time × previous no-show interaction did not improve Random Forest cross-validation performance and was not adopted.

---

## Week 7 Focus

The next stage will focus on:

- Probability calibration
- Classification threshold and cost analysis
- Model stability
- Segment-level validation
- Fairness and consistency checks

> **Note:** The dataset is synthetic. Model results should not be interpreted as evidence of real-world clinical performance.

---

## Project Status

**Week 4:** Problem definition completed  
**Week 5:** Baseline modelling completed  
**Week 6:** Model improvement and validation completed  
**Week 7:** Calibration, threshold analysis and further validation

---

## Author

**Suleiman Habeebullahi**  
Data Science Track — AnalystLab Africa
