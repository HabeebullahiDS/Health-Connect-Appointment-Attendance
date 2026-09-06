# HealthConnect-Appointment-Attendance

# HealthConnect Appointment No-Show Prediction — Week 5

## Project Overview

This project is part of the **AnalystLab Africa Week 5 Experience Lab** on the **Data Science Track**.

The Week 5 Data Science work is building directly on the machine-learning problem defined in Week 4 and moving the project from **problem definition into practical baseline modelling**.

The project is focusing on predicting whether a scheduled HealthConnect appointment is likely to result in a **patient No-Show**, using information that can reasonably be available before the appointment outcome is known.

The solution is being treated as a **supervised binary classification problem** distinguishing between:

* **No-Show**
* **Attended**

Week 5 covers data preparation, evidence-based quality assessment, exploratory analysis, feature engineering, patient-aware train/test separation, baseline model development, model evaluation, interpretation, modelling risks, and recommendations for further improvement.

---

## Project Objectives

The Week 5 Data Science objectives are to:

* Implement the machine-learning decisions established in Week 4.
* Prepare the binary modelling population.
* Assess and treat data-quality issues using evidence from the dataset.
* Examine relationships among relevant variables.
* Examine relationships between predictors and the no-show target.
* Engineer useful pre-appointment features.
* Prevent potential target leakage.
* Account for repeated patients during model evaluation.
* Develop a Logistic Regression baseline model.
* Evaluate the baseline using appropriate classification metrics.
* Interpret model outputs from both a technical and HealthConnect decision perspective.
* Document assumptions, limitations, risks, dependencies, and future improvements.

---

## Dataset and Resources

The following resources were used:

* `HealthConnect_Appointment_Data.csv`
* `HealthConnect_Data_Dictionary.xlsx`

The original dataset contains **5,000 appointment records and 18 variables** covering:

* Patient demographics
* Appointment information
* Booking information
* Previous appointment behaviour
* Previous no-shows
* Reminder information
* Distance to clinic
* Waiting time
* Appointment outcomes

The original dataset is being preserved while transformations are being performed on working copies.

---

# Week 4 → Week 5 Transition

Week 4 established the machine-learning problem as a supervised binary classification task.

The Week 4 target definition was:

| Appointment Outcome | Binary Target |
| ------------------- | ------------: |
| No-Show             |             1 |
| Attended            |             0 |
| Cancelled           |      Excluded |

Week 5 is implementing this decision rather than leaving it as a proposed modelling approach.

The complete dataset initially contained:

* **5,000** appointment records
* **2,423 No-Shows**
* **2,314 Attended**
* **263 Cancelled**

The 263 cancelled appointments were excluded from the binary modelling population, resulting in:

* **4,737 modelling records**
* **2,423 No-Shows**
* **2,314 Attended**

The resulting No-Show rate within the modelling population was **51.15%**.

This produced a relatively balanced binary target, meaning that the modelling task does not begin with severe class imbalance.

---

# Week 5 Data Preparation

## Missing Values

Missing-value assessment identified:

| Variable                | Missing Records | Missing % |
| ----------------------- | --------------: | --------: |
| `reminder_channel`      |           1,285 |    27.13% |
| `distance_to_clinic_km` |              86 |     1.82% |
| `waiting_time_minutes`  |              58 |     1.22% |

The missing values were treated according to the nature of each variable.

`reminder_channel` was treated as a categorical variable and missing observations were represented as **`Not Recorded`** rather than deleting the affected appointments.

The numerical variables `distance_to_clinic_km` and `waiting_time_minutes` were treated using median-based replacement where appropriate during preparation.

The treatment was designed to preserve the eligible appointment population while ensuring that the modelling data could be processed reliably.

---

## Duplicate Records

Duplicate records were checked as part of the Week 5 preparation process.

The analysis distinguished between:

* complete duplicate records; and
* legitimate repeated appointments belonging to the same patient.

Repeated patient records were **not automatically removed**, because a patient having multiple appointments does not mean those appointments are duplicates.

This distinction became particularly important for the train/test strategy.

---

# Feature Engineering

Week 5 moved beyond the original Week 4 feature list by developing additional pre-appointment features where they could provide useful modelling information.

Feature engineering included behavioural and calendar-related information.

A particularly important engineered behavioural measure was:

* `prior_no_show_rate`

This was developed to represent a patient's historical no-show behaviour rather than relying only on the raw number of previous no-shows.

Calendar and appointment-related transformations were also considered where they provided information available before the appointment.

---

# Data Leakage Prevention

Potential data leakage was treated as a major modelling consideration.

`waiting_time_minutes` was excluded from the predictive feature set because it may not be reliably available before the appointment takes place.

This decision was made even though the variable could potentially contain predictive information.

The priority was to ensure that the model learns from information that could genuinely be available when HealthConnect needs to make a pre-appointment prediction.

This makes the baseline more credible for future operational use.

---

# Major Exploratory Analysis Findings

The Week 5 analysis examined relationships among numerical variables, categorical variables, appointment characteristics, historical behaviour, reminders, accessibility variables, calendar timing, and the no-show target.

### 1. No-show behaviour is multifactorial

The analysis indicates that no-show behaviour should not be explained using a single patient characteristic.

Useful predictive signals are coming from a combination of:

* Previous attendance behaviour
* Previous no-shows
* Prior no-show rate
* Booking lead time
* Reminder exposure and channel
* Appointment context
* Distance/accessibility
* Calendar timing

This supports a modelling approach that combines multiple characteristics rather than relying on simplistic rules.

### 2. Historical behaviour is important

Previous appointment behaviour provides useful information about future appointment adherence.

Variables such as `previous_no_shows` and the engineered `prior_no_show_rate` were therefore retained as important predictive candidates.

**HealthConnect implication:** historical attendance behaviour can support more targeted reminder prioritisation rather than treating every appointment in exactly the same way.

### 3. Booking lead time provides a useful signal

The Week 5 analysis found higher average booking lead time among no-show appointments.

This suggests that appointments scheduled further in advance may require additional confirmation closer to the appointment date.

**HealthConnect implication:** long-lead appointments could potentially be prioritised for stronger confirmation workflows.

### 4. Reminder variables contain useful information

`reminder_sent` and `reminder_channel` showed observable differences in no-show behaviour and were retained because they represent information available before the appointment outcome.

However, the analysis does **not** establish that reminders themselves cause patients to attend.

Reminder variables may partly reflect existing clinic processes and patient characteristics.

**HealthConnect implication:** the eventual model could help prioritise reminder and support actions, while operational testing would still be required to establish intervention effectiveness.

### 5. Appointment context and accessibility contribute additional information

Appointment characteristics, calendar timing and accessibility-related variables provide additional predictive information when considered alongside behavioural and booking variables.

The analysis therefore supports a broader view of attendance behaviour rather than attributing no-shows to one demographic characteristic.

---

# Machine Learning Development

## Modelling Population

The modelling dataset contains:

* **4,737 appointments**
* **2,423 No-Shows**
* **2,314 Attended**

The target variable is:

```text
no_show = 1 → No-Show
no_show = 0 → Attended
```

---

## Train/Test Strategy

Repeated patient records were retained because they represent legitimate appointments.

However, patient overlap between the training and testing datasets was prevented.

A **patient-aware train/test strategy** was used so that records belonging to the same patient were not allowed to appear in both training and testing groups.

This produces a more conservative and credible estimate of model performance, particularly when the model may eventually encounter patients not represented in its training data.

---

# Baseline Model

A **Logistic Regression** classifier was developed as the initial baseline model.

Logistic Regression was selected as a suitable baseline because it:

* provides a transparent classification approach;
* provides interpretable coefficient signals;
* establishes a benchmark for future model improvement; and
* allows HealthConnect to understand which features are contributing positively or negatively to predicted no-show probability.

The baseline was evaluated using:

* Accuracy
* Precision
* Recall
* F1-score
* ROC-AUC
* Confusion Matrix

The ROC-AUC was also examined to assess the model's ability to rank no-show appointments above attended appointments across different probability thresholds.

---

# Model Interpretation

The model coefficients were examined to understand the signals being learned by the baseline.

The strongest areas of analytical interest included:

* Prior attendance behaviour
* Booking lead time
* Reminder exposure/channel
* Appointment context
* Distance/accessibility
* Calendar timing

These signals reinforce the conclusion that **no-show risk is multifactorial**.

The coefficients are being interpreted as predictive associations rather than causal effects.

For example, a relationship between reminder information and no-show probability should not automatically be interpreted as evidence that sending a reminder causes or prevents a no-show.

---

# HealthConnect Decision Implications

The Week 5 analysis provides several practical directions for HealthConnect clinics.

### Targeted appointment support

Rather than applying identical reminder strategies to every appointment, a future validated model could help identify appointments that may require additional attention.

### Historical behaviour

Patients with stronger historical evidence of missed appointments may warrant different levels of appointment support, subject to appropriate operational and ethical considerations.

### Long-lead appointments

Appointments scheduled substantially in advance may benefit from confirmation closer to the appointment date.

### Reminder strategy

The model can potentially help identify patterns associated with reminder exposure and channel, while separate intervention testing would be required to determine which reminder strategy actually improves attendance.

### Data-driven resource allocation

A validated risk model could eventually help clinics prioritise limited reminder and support resources toward appointments where intervention is most likely to be useful.

---

# Key Week 5 Achievements

The Week 5 Data Science workflow has:

* Carried forward the Week 4 no-show prediction problem.
* Validated the dataset and data dictionary structure.
* Created a clear binary target.
* Excluded cancelled appointments from the binary modelling population.
* Assessed missing values.
* Checked duplicate records.
* Retained legitimate repeated patient appointments.
* Investigated relationships among variables.
* Investigated relationships between predictors and the target.
* Engineered pre-appointment behavioural and calendar features.
* Identified and controlled potential data leakage.
* Excluded `waiting_time_minutes` from the predictive feature set.
* Implemented patient-aware train/test separation.
* Developed a Logistic Regression baseline.
* Evaluated the baseline using multiple classification metrics.
* Interpreted model coefficients and confusion-matrix results.
* Translated analytical findings into HealthConnect decision implications.
* Documented modelling assumptions, limitations, risks and dependencies.

---

# Assumptions

The Week 5 work continues to assume that:

* Appointment records represent valid scheduled appointments.
* `appointment_outcome` correctly represents the final appointment status.
* Historical appointment variables are available before the current appointment.
* Selected predictive features are available when predictions are required.
* Multiple appointments belonging to the same patient represent legitimate separate appointments.

---

# Limitations and Risks

Important limitations and risks remain:

* The dataset is fictional and anonymized.
* Model performance therefore cannot automatically be assumed to generalise to real HealthConnect patients.
* Future implementation must continue to verify that every predictive feature is available before the appointment.
* Reminder variables may reflect existing clinic processes rather than pure patient behaviour.
* Feature redundancy may affect model interpretation.
* The operational probability threshold has not yet been established.
* A single baseline model is not sufficient for production deployment.
* Model performance should not be judged using one metric alone.

---

# Dependencies

Future development depends on:

* Continued availability of the appointment dataset and Data Dictionary.
* Consistent reproduction of the Week 5 target definition.
* Consistent reproduction of preprocessing and feature-engineering logic.
* Appropriate patient-aware validation.
* Comparison with stronger and potentially nonlinear models.
* Threshold selection based on HealthConnect intervention priorities.
* Probability calibration.
* Broader validation and monitoring before operational deployment.

---

# Week 6 Direction

The Week 5 Logistic Regression model is serving as the baseline against which future improvements can be measured.

Recommended Week 6 work includes:

* Comparing Logistic Regression with appropriate nonlinear classifiers.
* Tuning model hyperparameters.
* Evaluating different prediction thresholds.
* Assessing probability calibration.
* Refining the feature set.
* Conducting broader validation.
* Monitoring model performance and stability.
* Assessing the operational consequences of false positives and false negatives.

The objective should not simply be to obtain a higher model score.

The stronger objective is to determine whether the model can provide **reliable, interpretable and operationally useful no-show risk information for HealthConnect clinics while keeping unnecessary interventions manageable**.

---

# Repository Structure

```text
HealthConnect-Appointment-No-Show-Prediction/
│
├── data/
│   ├── HealthConnect_Appointment_Data.csv
│   └── HealthConnect_Data_Dictionary.xlsx
│
├── notebooks/
│   ├── HealthConnect_DataScience_Week4_Problem_Definition.ipynb
│   └── HealthConnect_DataScience_Week5_Baseline_Modeling_Executed.ipynb
│
├── README.md
└── requirements.txt
```

---

# Project Status

**Week 4:** Machine-learning problem definition completed.

**Week 5:** Data preparation, exploratory analysis, feature engineering, patient-aware evaluation strategy and Logistic Regression baseline completed.

**Current status:** Baseline established; further model improvement and validation recommended before operational deployment.

---

# Author

**Suleiman Habeebullahi**
Data Scientist

**AnalystLab Africa — HealthConnect Clinic Experience Lab**
