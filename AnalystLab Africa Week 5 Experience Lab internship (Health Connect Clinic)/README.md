# HealthConnect Appointment No-Show Prediction — Week 5

## Project Overview

This project is part of the **AnalystLab Africa Experience Lab**.

The Data Science work focuses on predicting whether a scheduled HealthConnect appointment is likely to result in a patient no-show, with the goal of supporting targeted interventions that reduce missed appointments and improve the patient support experience.

**Week 5** moves from the Week 4 problem definition into practical modelling: data preparation, feature engineering, patient-aware train/test splitting, baseline model development, rigorous evaluation, and decision-ready interpretation.

A complete production system is **not** delivered at this stage. The output is an evidence-based **baseline classifier** and a clear roadmap for Week 6 improvement.

---

## Central Project Question

> How can HealthConnect Clinic use data and AI to reduce missed appointments and improve the patient support experience?

---

## Project Objectives (Week 5)

- Confirm the Week 4 machine-learning problem definition against the live dataset.
- Prepare a clean binary modelling population (Attended vs No-Show).
- Perform systematic data preparation with measurable before-and-after evidence.
- Engineer and select features that are available before the appointment outcome.
- Implement a patient-aware train/test split to avoid leakage from repeated appointments.
- Train and evaluate a logistic regression baseline.
- Interpret coefficients and performance for clinic stakeholders.
- Document limitations, cross-track alignment, and Week 6 priorities.

---

## Dataset and Resources

| Resource | Description |
|----------|-------------|
| `HealthConnect_Appointment_Data.csv` | 5,000 appointment records, 18 variables |
| `HealthConnect_Data_Dictionary.xlsx` | Variable definitions and notes |
| Week 4 notebook / README | Problem definition, target, candidate features, assumptions |

**Key variables covered:** patient demographics, appointment characteristics, booking lead time, previous appointment history, previous no-shows, reminder information, distance to clinic, waiting time, and appointment outcome.

---

## Machine Learning Problem

**Task:** Supervised **binary classification**

**Prediction unit:** One scheduled appointment

**Target definition:**

| Appointment Outcome | Target value |
|---------------------|--------------|
| No-Show             | 1            |
| Attended            | 0            |
| Cancelled           | Excluded from the binary modelling population |

**Modelling population after cancellation treatment:**

- Records before filtering: **5,000**
- Cancelled excluded: **263**
- Records retained: **4,737**
- No-Show rate after filtering: **~51.2%**

The model uses only information that can reasonably be known **before** the appointment outcome is observed.

---

## Data Preparation Summary

Data preparation followed an evidence-based sequence (What was found → What was done → What changed → What it means):

- Missing values assessed and handled (`reminder_channel` given an explicit category; `distance_to_clinic_km` imputed where needed).
- Duplicate and repeated-patient records reviewed; repeated appointments treated as legitimate history.
- Data types, invalid values, date consistency, and logical consistency between related variables checked.
- Potential leakage sources identified and excluded (notably `waiting_time_minutes`, raw outcome, and raw date columns used only for derived features).
- Final processed feature set restricted to pre-appointment predictors.

---

## Feature Engineering & Selection

**Candidate groups retained for assessment:**

- Patient characteristics: `age`, `gender`
- Appointment characteristics: `appointment_type`, `appointment_day`, `appointment_time`
- Previous history: `previous_appointments`, `previous_no_shows` (and engineered forms)
- Operational / accessibility: `distance_to_clinic_km`, `reminder_channel`, `reminder_sent`
- Booking information: `booking_lead_days` (and engineered lead-time bands)

**Engineered / selected signals used in the baseline:**

- Lead-time bands (e.g. 0–7, 8–21, 22–40, 41–60 days)
- Previous no-show indicators / rate (including a high previous no-show flag)
- Distance flag (long distance)
- Reminder-related flags

**Explicitly excluded or dropped for this baseline:**

- `waiting_time_minutes` (leakage risk)
- Raw `appointment_outcome` (source of target)
- Raw identifier columns for modelling (kept only for grouping)
- Highly redundant representations after correlation review (e.g. overlap between raw and engineered history/lead-time/distance features)

---

## Modelling Approach

1. Binary modelling dataset (Attended / No-Show only).
2. Binary target `no_show`.
3. Preprocessing pipeline: imputation, scaling for numerics, one-hot encoding for categoricals.
4. **GroupShuffleSplit by `patient_id`** so the same patient does not appear in both train and test sets (zero patient overlap confirmed).
5. **Logistic Regression** as the interpretable baseline.
6. Evaluation with Accuracy, Precision, Recall, F1-score, ROC-AUC, confusion matrix, and ROC curve.
7. Coefficient inspection for stakeholder-facing interpretation.

---

## Baseline Results (Summary)

| Metric              | Approximate result | Note                                      |
|---------------------|--------------------|-------------------------------------------|
| ROC-AUC             | 0.671              | Modest discrimination                     |
| Accuracy            | ~61.6%             | Above ~50% chance-level reference         |
| False Negatives     | 184                | Actual no-shows missed by the model       |
| False Positives     | 187                | Attended appointments flagged as risk     |

**Strongest coefficient-level associations (illustrative):**

- Longer lead time (especially 41–60 days) → higher predicted no-show risk
- High previous no-show history → higher predicted risk
- Long distance → modest increase in predicted risk
- Short lead time (0–7 days) → lower predicted risk

Coefficients are **associations within the model**, not causal effects.

---

## Key Decisions Carried from Week 4 → Week 5

| Decision                         | Status in Week 5                                      |
|----------------------------------|-------------------------------------------------------|
| Binary no-show prediction        | Retained                                              |
| Target: No-Show = 1, Attended = 0| Retained                                              |
| Exclude Cancelled appointments   | Retained and re-confirmed                             |
| Patient-aware splitting          | Implemented (GroupShuffleSplit, zero overlap)         |
| Leakage control                  | Strengthened; post-appointment fields excluded        |
| Logistic Regression baseline     | Trained and evaluated                                 |
| Multi-metric evaluation          | Accuracy + Precision/Recall/F1 + ROC-AUC + CM         |

No fundamental change was made to the Week 4 problem definition. Execution was strengthened around leakage, splitting, and feature redundancy.

---

## Assumptions, Limitations & Risks

**Assumptions (still in force):**

- Each record is a valid scheduled appointment.
- `appointment_outcome` reflects the true final status.
- History features represent information available before the current appointment.
- Selected features will be available at prediction time.

**Main limitations of the baseline:**

- Modest predictive performance (AUC ≈ 0.671).
- Balanced false negatives and false positives at the current threshold; operational cost of each error type not yet quantified.
- Single-clinic, limited sample; no external or temporal validation yet.
- Linear model may miss non-linear interactions.
- Single train/test split (cross-validation planned for Week 6).
- Some features (e.g. reminder-related) require careful interpretation to avoid over-claiming causality.

---

## Cross-Track Collaboration

Findings from the **Data Analytics** track were reviewed and aligned with the baseline:

- Longer booking lead time associated with higher no-show rates → reflected in the strongest positive coefficients.
- Previous no-show history predictive of future risk → confirmed by engineered history features.
- Appointment-type differences present in descriptive analysis → weakly visible in model coefficients.
- Reminder status shows descriptive differences; not treated as strong causal evidence in the model.

This alignment supports feature prioritisation and provides stakeholder-friendly context for the modelling results.

---

## Repository Structure (Updated for Week 5)

```
HealthConnect-Appointment-No-Show-Prediction/
│
├── data/
│   ├── HealthConnect_Appointment_Data.csv
│   └── HealthConnect_Data_Dictionary.xlsx
│
├── notebooks/
│   ├── HealthConnect_DataScience_Week4_Problem_Definition.ipynb
│   └── HealthConnect_DataScience_Week5_Baseline_Model.ipynb
│
├── docs/                          # optional location for summaries
│   └── HealthConnect_Week5_Project_Summary.docx
│
├── README.md
└── requirements.txt
```

---

## Proposed Focus for Week 6

1. **Model enhancement** — Evaluate tree-based and gradient-boosted models (Random Forest, XGBoost/LightGBM) against the logistic baseline (reference AUC ≈ 0.671).
2. **Evaluation design** — Cross-validation; threshold optimisation using operational costs; probability calibration.
3. **Feature refinement** — Limited interaction terms (e.g. lead time × previous no-shows) with leakage safeguards.
4. **Interpretability** — SHAP (or similar) for global and individual explanations for clinic stakeholders.
5. **Operational framing** — Simple risk-tier framework and a monitoring checklist for any future pilot.

---

## Dependencies

- Appointment dataset and Data Dictionary
- Confirmed target and cancellation-handling strategy
- Python environment with pandas, NumPy, scikit-learn, matplotlib, seaborn
- Features available at the time a prediction would be made
- Patient identifiers for group-aware splitting

---

## Author

**Suleiman Habeebullahi**  
Data Scientist  
AnalystLab Africa — HealthConnect Clinic Experience Lab
