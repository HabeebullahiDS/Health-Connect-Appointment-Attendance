# Health-Connect-Appointment-Attendance# HealthConnect Appointment No-Show Prediction — Week 4

## Project Overview

This project is part of the AnalystLab Africa Week 4 Experience Lab.

The Data Science work focuses on defining a machine learning solution for predicting whether a scheduled HealthConnect appointment is likely to result in a patient no-show.

The proposed solution is a supervised binary classification approach that distinguishes between:

* **No-Show**
* **Attended**

Week 4 focuses on problem understanding, resource review, initial data assessment, target and feature definition, and the development of an initial modelling approach. A complete machine learning model is not developed at this stage.

---

## Project Objectives

The main objectives of the Week 4 Data Science work are to:

* Review the appointment dataset and relevant variables.
* Assess the quality and suitability of the data.
* Define the machine learning problem.
* Identify the proposed target variable.
* Identify potential input features.
* Define how cancelled appointments will be handled.
* Develop an initial modelling approach.
* Identify key assumptions, limitations, risks, and dependencies.

---

## Dataset and Resources

The following resources were used:

* `HealthConnect_Appointment_Data.csv`
* `HealthConnect_Data_Dictionary.xlsx`

The appointment dataset contains 5,000 records and 18 variables covering:

* Patient demographics
* Appointment information
* Booking details
* Previous appointment history
* Previous no-shows
* Reminder information
* Distance to the clinic
* Waiting time
* Appointment outcomes

---

## Key Week 4 Findings

The initial assessment found that:

* The dataset contains 5,000 appointment records and 18 variables.
* The dataset includes relevant demographic, appointment, historical, reminder, distance, and outcome information.
* No fully duplicated records were identified.
* Some patients have multiple appointment records, representing repeated appointments rather than automatically duplicated data.
* `reminder_channel` contains missing values that are structurally related to appointments where no reminder was sent.
* `distance_to_clinic_km` and `waiting_time_minutes` contain limited missing values.
* Key relationships between appointment history and date-related variables were found to be internally consistent.
* `appointment_outcome` contains three outcomes: `Attended`, `No-Show`, and `Cancelled`.
* The proposed prediction target is a binary no-show indicator.
* `Cancelled` appointments should be excluded from the binary modelling dataset.
* Potential features must be available before the appointment outcome is known.
* `waiting_time_minutes` was excluded because it may introduce data leakage.
* Repeated patient records should be considered when developing the future train-test splitting strategy.

---

## Machine Learning Problem

The proposed machine learning problem is:

> **To develop a machine learning solution that predicts whether a scheduled appointment is likely to result in a patient no-show using information available before the appointment takes place.**

This is a **supervised binary classification problem**.

The unit of prediction is **one scheduled appointment**.

---

## Proposed Target Variable

The target will be derived from `appointment_outcome`.

| Appointment Outcome |                                     Target |
| ------------------- | -----------------------------------------: |
| No-Show             |                                          1 |
| Attended            |                                          0 |
| Cancelled           | Excluded from the binary modelling dataset |

The binary target will represent:

* **1 = No-Show**
* **0 = Attended**

---

## Potential Input Features

The initial potential feature set includes:

* `gender`
* `age`
* `appointment_type`
* `appointment_day`
* `appointment_time`
* `booking_lead_days`
* `previous_appointments`
* `previous_no_shows`
* `reminder_sent`
* `reminder_channel`
* `distance_to_clinic_km`

### Variables Requiring Further Consideration

* `booking_date`
* `appointment_date`

These date variables may require transformation into model-relevant features.

### Variables Initially Excluded

* `appointment_id` — Unique appointment identifier.
* `patient_id` — Patient identifier; retained only for possible patient-aware data splitting.
* `age_group` — Derived from `age`.
* `appointment_outcome` — Source of the target variable.
* `waiting_time_minutes` — Excluded due to potential data leakage.

---

## Proposed Modelling Approach

The proposed approach is to:

1. Prepare a binary modelling dataset containing only Attended and No-Show appointments.
2. Create a binary no-show target.
3. Select appropriate input features.
4. Prepare and transform the data for modelling.
5. Handle missing values.
6. Encode categorical variables.
7. Use a suitable train-test splitting strategy that considers repeated patient records.
8. Train Logistic Regression as an initial baseline model.
9. Evaluate performance using appropriate classification metrics.
10. Compare additional models during later development if necessary.

### Initial Evaluation Metrics

The future model will be assessed using:

* Accuracy
* Precision
* Recall
* F1-score
* ROC-AUC, where appropriate
* Confusion Matrix

---

## Assumptions

The proposed approach assumes that:

* Each record represents a valid scheduled appointment.
* `appointment_outcome` accurately reflects the final appointment status.
* `previous_appointments` and `previous_no_shows` represent information available before the current appointment.
* Selected features will be available when a prediction is made.
* Multiple records for the same patient represent legitimate different appointments.

---

## Limitations and Risks

Key considerations include:

* Missing values in some variables.
* Potential data leakage from information unavailable at prediction time.
* Repeated patient records affecting model evaluation if not handled appropriately.
* Target definition and the treatment of cancelled appointments.
* Feature redundancy from derived or closely related variables.
* Limited information beyond the variables available in the dataset.
* The fictional and anonymized nature of the dataset.
* The risk of relying on a single model evaluation metric.

---

## Dependencies

Future model development depends on:

* Availability of the appointment dataset and Data Dictionary.
* A confirmed target and cancellation-handling strategy.
* Appropriate data preparation and feature engineering.
* A suitable patient-aware data-splitting strategy.
* Required Python data science and machine learning libraries.
* Availability of selected features at the time predictions are made.

---

## Proposed Focus for Week 5

The proposed Week 5 focus is to begin the model development stage by:

* Preparing the binary modelling dataset.
* Finalizing the target variable.
* Preparing and selecting model features.
* Handling missing values.
* Transforming and encoding required variables.
* Selecting an appropriate data-splitting strategy.
* Beginning initial model development and evaluation.

---

## Repository Structure

```text
HealthConnect-Appointment-No-Show-Prediction/
│
├── data/
│   ├── HealthConnect_Appointment_Data.csv
│   └── HealthConnect_Data_Dictionary.xlsx
│
├── notebooks/
│   └── HealthConnect_DataScience_Week4_Problem_Definition.ipynb
│
├── README.md
└── requirements.txt
```

---

## Author

**Suleiman Habeebullahi**
Data Scientist
