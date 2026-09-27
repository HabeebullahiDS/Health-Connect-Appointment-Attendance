# HealthConnect Appointment No-Show Prediction

## Project Overview

HealthConnect is a healthcare data science project focused on predicting whether a scheduled appointment will result in a **No-Show or Attendance**.

The project progressed from problem definition and baseline modelling to model improvement, validation, refinement, calibration, and final integration.

The dataset is synthetic and contains **5,000 appointment records**:

* 2,423 No-Shows
* 2,314 Attended
* 263 Cancelled

Cancelled appointments were excluded from binary modelling, leaving **4,737 modelling records** with a **51.15% No-Show rate**.

---

## Week 4 — Problem Definition

The problem was defined as **supervised binary classification**:

| Appointment Outcome |   Target |
| ------------------- | -------: |
| No-Show             |        1 |
| Attended            |        0 |
| Cancelled           | Excluded |

The objective was to predict no-show risk using information available before the appointment outcome.

---

## Week 5 — Baseline Modelling

Week 5 focused on:

* Data preparation and quality assessment
* Exploratory data analysis
* Feature engineering
* Leakage prevention
* Patient-aware train/test separation
* Logistic Regression baseline
* Model evaluation using Accuracy, Precision, Recall, F1-score and ROC-AUC

Important predictive signals included **booking lead time, previous no-show behaviour, reminder information, appointment context and distance**.

`waiting_time_minutes` was excluded from the predictive feature set because it may not be reliably available before the appointment.

---

## Week 6 — Model Improvement & Validation

Week 6 focused on:

* Error analysis
* Segment-level validation
* Feature refinement
* Patient-grouped cross-validation using `GroupKFold`
* Comparison of Decision Tree, Random Forest and Gradient Boosting
* Testing additional feature interactions
* Cross-track validation with Data Analytics
* Cumulative-gains analysis

### Model Results

| Model               | Test ROC-AUC |    Recall |        F1 |
| ------------------- | -----------: | --------: | --------: |
| Logistic Regression |        0.671 |     0.619 |     0.617 |
| Decision Tree       |        0.674 |     0.594 |     0.620 |
| **Random Forest**   |    **0.683** |     0.625 |     0.630 |
| Gradient Boosting   |        0.678 | **0.650** | **0.641** |

Random Forest was selected as the primary candidate based on its cross-validated and test ROC-AUC performance.

---

## Week 7 — Model Testing, Refinement & Validation

Week 7 focused on systematically testing and refining the Random Forest model.

Key outcomes:

* Random Forest outperformed the baseline in **5 out of 5 independent splits**.
* An overfitting issue was identified and addressed.
* `max_depth` was reduced from **8 to 5**, reducing the train-test ROC-AUC gap from **0.073 to 0.026**.
* Isotonic calibration improved probability reliability.
* Bootstrap testing produced a 95% confidence interval for ROC-AUC improvement of **[0.0036, 0.0295]**.
* Specialist Consultation and the **65+ age group** were identified as weaker-performing segments.
* Additional Data Analytics findings were validated and tested as potential model features.
* Cost-sensitive threshold analysis showed that a final operational threshold requires real HealthConnect cost information.

The refined Random Forest with isotonic calibration was carried forward to Week 8.

---

# Week 8 — Final Integration & Presentation

Week 8 focused on **finalising, documenting and presenting the tested model**.

Key outcomes:

* Confirmed the **depth-refined Random Forest with isotonic probability calibration** as the final candidate.
* Final model achieved **0.689 test ROC-AUC** and **0.643 test accuracy**.
* Baseline Logistic Regression achieved **0.671 test ROC-AUC** and **0.616 test accuracy**.
* Confirmed the model's suitability for **risk ranking and outreach prioritisation**.
* Validated Data Analytics findings and tested additional features without automatically adopting them.
* Documented the final model requirements for Machine Learning Engineering.
* Saved the final model and model interface specification.
* Documented the model's limitations, risks and outstanding integration requirements.

### Final Model

**Random Forest + Isotonic Probability Calibration**

The model is intended to help HealthConnect **prioritise appointments by predicted no-show risk**, rather than make fully automated scheduling decisions.

### Final Artefacts

```text
healthconnect_random_forest_model_week8_final.joblib
healthconnect_model_interface_spec_week8_final.csv
```

---

## Key Limitations

* The dataset is synthetic and does not establish real-world clinical performance.
* Overall predictive performance remains moderate.
* Specialist Consultation and patients aged 65+ remain weaker-performing segments.
* A production classification threshold requires real operational cost information.
* Machine Learning Engineering integration remains outstanding.

---

## Project Status

| Week                       | Status    | Main Outcome                                      |
| -------------------------- | --------- | ------------------------------------------------- |
| **Week 4**                 | Completed | Problem definition                                |
| **Week 5**                 | Completed | Baseline Logistic Regression                      |
| **Week 6**                 | Completed | Model comparison and Random Forest selection      |
| **Week 7**                 | Completed | Testing, refinement and calibration               |
| **Week 8**                 | Completed | Final model integration and presentation          |
| **Production Integration** | Pending   | ML pipeline integration and operational threshold |
