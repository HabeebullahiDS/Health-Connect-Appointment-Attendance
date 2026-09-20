# HealthConnect Appointment No-Show Prediction

## Project Overview

HealthConnect is a healthcare data science project focused on predicting whether a scheduled appointment will result in a **No-Show or Attendance**.

The project began in **Week 4** with the definition of a supervised binary classification problem and progressed through data preparation, feature engineering, patient-aware validation, baseline modelling, model improvement, and systematic testing and refinement.

The dataset is synthetic and contains **5,000 appointment records**:

* 2,423 No-Shows
* 2,314 Attended
* 263 Cancelled

Cancelled appointments were excluded from binary modelling, leaving **4,737 modelling records** with a **51.15% No-Show rate**.

---

## Week 4 — Problem Definition

The modelling problem was defined as **supervised binary classification**:

| Appointment Outcome |   Target |
| ------------------- | -------: |
| No-Show             |        1 |
| Attended            |        0 |
| Cancelled           | Excluded |

The objective is to predict no-show risk using information reasonably available before the appointment outcome is known.

---

## Week 5 — Baseline Modelling

Week 5 focused on:

* Data-quality assessment and preparation
* Exploratory data analysis
* Feature engineering
* Leakage prevention
* Patient-aware train/test separation
* Logistic Regression baseline development
* Model evaluation using Accuracy, Precision, Recall, F1-score and ROC-AUC
* Error and coefficient interpretation

Important predictive signals included **booking lead time, previous no-show behaviour, reminder information, appointment context and distance**.

`waiting_time_minutes` was excluded from the predictive feature set because it may not be reliably available before the appointment.

---

## Week 6 — Model Improvement & Validation

Week 6 extended the baseline through:

* Error analysis using TP, TN, FP and FN
* Segment-level error investigation
* Feature refinement
* Patient-grouped cross-validation using `GroupKFold`
* Comparison of Decision Tree, Random Forest and Gradient Boosting
* Testing a lead-time × previous no-show rate interaction
* Integration of findings from the Data Analytics track
* Cumulative-gains analysis from a practical outreach perspective

### Model Results

| Model               | Test ROC-AUC |    Recall |        F1 |
| ------------------- | -----------: | --------: | --------: |
| Logistic Regression |        0.671 |     0.619 |     0.617 |
| Decision Tree       |        0.674 |     0.594 |     0.620 |
| **Random Forest**   |    **0.683** |     0.625 |     0.630 |
| Gradient Boosting   |        0.678 | **0.650** | **0.641** |

### Model Decision

**Random Forest** was selected as the primary candidate because it achieved the highest cross-validated ROC-AUC (**0.673**) and test ROC-AUC (**0.683**).

**Gradient Boosting** remained a strong alternative because it achieved the highest Recall (**0.650**) and F1-score (**0.641**).

Overall, model improvement was **modest**, making further testing and refinement important.

---

## Key Findings

* Lead time and previous no-show behaviour remain important predictive signals.
* Random Forest provided the best overall discrimination, but only a modest improvement over Logistic Regression.
* Gradient Boosting provided higher Recall and F1-score.
* Error analysis identified segments where the model makes more mistakes.
* Cumulative-gains analysis showed that both the baseline and Random Forest can improve on random targeting.
* The lead-time × previous no-show interaction did not improve Random Forest cross-validation performance and was not adopted.

---

# Week 7 — Model Testing, Refinement & End-to-End Validation

Week 7 focused on systematically testing the Week 6 Random Forest candidate beyond a single train/test split.

Key activities included:

* **Stability testing:** Random Forest outperformed the baseline in **5 out of 5 independent train/test splits**, confirming that its advantage was not specific to one split.
* **Segment validation:** Performance was generally consistent, but **Specialist Consultation** and the **65+ age group** showed weaker performance and were documented as limitations.
* **Overfitting testing:** An initial train-test ROC-AUC gap of **0.073** was identified. Reducing model complexity narrowed the gap to **0.026** and improved test ROC-AUC to approximately **0.689**.
* **Probability calibration:** Isotonic calibration improved the model's probability reliability, reducing the Brier score by approximately **0.82%**, particularly improving calibration at the higher-risk end.
* **Statistical validation:** Bootstrap testing produced a positive ROC-AUC improvement with a 95% confidence interval that excluded zero, providing evidence that the improvement over the baseline was statistically meaningful.
* **Cross-track validation:** Six Data Analytics findings were independently validated. A resulting high-risk combination showed an **83.3% No-Show rate**, but adding it as a model feature did not improve cross-validated performance, so it was not adopted.
* **Threshold analysis:** Cost-sensitive threshold testing showed that the optimal threshold is highly dependent on real HealthConnect cost information, which was not available.

### Final Week 7 Model

The final candidate carried forward is a **depth-refined Random Forest with isotonic probability calibration**.

The model is considered suitable for **risk ranking and prioritisation**, but not for fully automated scheduling decisions at this stage.

### Week 7 Limitations

* Specialist Consultation and 65+ remain weaker-performing segments.
* The operational classification threshold requires real business cost information.
* The dataset is synthetic and therefore does not establish real-world clinical performance.
* Machine Learning Engineering integration remains an outstanding cross-track dependency.

---

## Project Status

**Week 4:** Problem definition completed
**Week 5:** Baseline modelling completed
**Week 6:** Model improvement and validation completed
**Week 7:** Testing, refinement, calibration and end-to-end validation completed
**Week 8:** Final integration and presentation
