# HealthConnect-Appointment-Attendance

# HealthConnect Appointment No-Show Prediction — Week 5

## Project Overview

This project is part of the **AnalystLab Africa Week 5 Experience Lab** on the **Data Science Track**.

The Week 5 Data Science work is building directly on the machine-learning problem defined in Week 4 and moving the project from **problem definition into practical baseline modelling**.

The project is focusing on predicting whether a scheduled HealthConnect appointment is likely to result in a **patient No-Show**, using information that can reasonably be available before the appointment outcome is known.

The solution is being treated as a **supervised binary classification problem** distinguishing between:

- **No-Show**
- **Attended**

Week 5 covers data preparation, evidence-based quality assessment, exploratory analysis, feature engineering, patient-aware train/test separation, baseline model development, model evaluation, interpretation, modelling risks, and recommendations for further improvement.

---

## Project Objectives

The Week 5 Data Science objectives are to:

- Implement the machine-learning decisions established in Week 4.
- Prepare the binary modelling population.
- Assess and treat data-quality issues using evidence from the dataset.
- Examine relationships among relevant variables.
- Examine relationships between predictors and the no-show target.
- Engineer useful pre-appointment features.
- Prevent potential target leakage.
- Account for repeated patients during model evaluation.
- Develop a Logistic Regression baseline model.
- Evaluate the baseline using appropriate classification metrics.
- Interpret model outputs from both a technical and HealthConnect decision perspective.
- Document assumptions, limitations, risks, dependencies, and future improvements.

---

## Dataset and Resources

The following resources were used:

- `HealthConnect_Appointment_Data.csv`
- `HealthConnect_Data_Dictionary.xlsx`

The original dataset contains **5,000 appointment records and 18 variables** covering:

- Patient demographics
- Appointment information
- Booking information
- Previous appointment behaviour
- Previous no-shows
- Reminder information
- Distance to clinic
- Waiting time
- Appointment outcomes

The original dataset is being preserved while transformations are being performed on working copies.

---

# Week 4 → Week 5 Transition

Week 4 established the machine-learning problem as a supervised binary classification task.

The Week 4 target definition was:

| Appointment Outcome | Binary Target |
|---|---:|
| No-Show | 1 |
| Attended | 0 |
| Cancelled | Excluded |

Week 5 is implementing this decision rather than leaving it as a proposed modelling approach.

The complete dataset initially contained:

- **5,000** appointment records
- **2,423 No-Shows**
- **2,314 Attended**
- **263 Cancelled**

The 263 cancelled appointments were excluded from the binary modelling population, resulting in:

- **4,737 modelling records**
- **2,423 No-Shows**
- **2,314 Attended**

The resulting No-Show rate within the modelling population was **51.15%**.

This produced a relatively balanced binary target, meaning that the modelling task does not begin with severe class imbalance.

---

# Week 5 Data Preparation

## Missing Values

Missing-value assessment identified:

| Variable | Missing Records | Missing % |
|---|---:|---:|
| `reminder_channel` | 1,285 | 27.13% |
| `distance_to_clinic_km` | 86 | 1.82% |
| `waiting_time_minutes` | 58 | 1.22% |

The missing values were treated according to the nature of each variable.

`reminder_channel` was treated as a categorical variable and missing observations were represented as **`Not Recorded`** rather than deleting the affected appointments.

The numerical variables `distance_to_clinic_km` and `waiting_time_minutes` were treated using median-based replacement where appropriate during preparation.

The treatment was designed to preserve the eligible appointment population while ensuring that the modelling data could be processed reliably.

---

## Duplicate Records

Duplicate records were checked as part of the Week 5 preparation process.

The analysis distinguished between:

- complete duplicate records; and
- legitimate repeated appointments belonging to the same patient.

Repeated patient records were **not automatically removed**, because a patient having multiple appointments does not mean those appointments are duplicates.

This distinction became particularly important for the train/test strategy.

---

# Feature Engineering

Week 5 moved beyond the original Week 4 feature list by developing additional pre-appointment features where they could provide useful modelling information.

Feature engineering included behavioural and calendar-related information.

A particularly important engineered behavioural measure was:

- `prior_no_show_rate`

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

## 1. No-show behaviour is multifactorial

The analysis indicates that no-show behaviour should not be explained using a single patient characteristic.

Useful predictive signals are coming from a combination of:

- Previous attendance behaviour
- Previous no-shows
- Prior no-show rate
- Booking lead time
- Reminder exposure and channel
- Appointment context
- Distance/accessibility
- Calendar timing

This supports a modelling approach that combines multiple characteristics rather than relying on simplistic rules.

## 2. Historical behaviour is important

Previous appointment behaviour provides useful information about future appointment adherence.

Variables such as `previous_no_shows` and the engineered `prior_no_show_rate` were therefore retained as important predictive candidates.

**HealthConnect implication:** historical attendance behaviour can support more targeted reminder prioritisation rather than treating every appointment in exactly the same way.

## 3. Booking lead time provides a useful signal

The Week 5 analysis found higher average booking lead time among no-show appointments.

This suggests that appointments scheduled further in advance may require additional confirmation closer to the appointment date.

**HealthConnect implication:** long-lead appointments could potentially be prioritised for stronger confirmation workflows.

## 4. Reminder variables contain useful information

`reminder_sent` and `reminder_channel` showed observable differences in no-show behaviour and were retained because they represent information available before the appointment outcome.

However, the analysis does **not** establish that reminders themselves cause patients to attend.

Reminder variables may partly reflect existing clinic processes and patient characteristics.

**HealthConnect implication:** the eventual model could help prioritise reminder and support actions, while operational testing would still be required to establish intervention effectiveness.

## 5. Appointment context and accessibility contribute additional information

Appointment characteristics, calendar timing and accessibility-related variables provide additional predictive information when considered alongside behavioural and booking variables.

The analysis therefore supports a broader view of attendance behaviour rather than attributing no-shows to one demographic characteristic.

---

# Machine Learning Development

## Modelling Population

The modelling dataset contains:

- **4,737 appointments**
- **2,423 No-Shows**
- **2,314 Attended**

The target variable is:

```text
no_show = 1 → No-Show
no_show = 0 → Attended
