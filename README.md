# NYC Taxi Tipping Behavior Analysis and Prediction

## Project Overview

This project analyzes **NYC Yellow Taxi trip records from Q1 2024** to understand customer tipping behavior and predict whether a completed taxi ride receives a **high tip**.

The project is designed as an end-to-end data science case study, covering:

- Data cleaning and preprocessing
- Exploratory data analysis
- Feature engineering
- Classification modeling
- Time-based train/test validation
- Data leakage prevention
- Model interpretation
- Business-style insight generation

The main goal is not only to build a predictive model, but also to explain which trip, fare, time, and location characteristics are associated with stronger tipping behavior.

---

## Problem Statement

Can we predict whether a NYC Yellow Taxi ride receives a high tip using completed trip information?

In this project, a ride is classified as a **high-tip ride** if:

```python
tip_percentage = tip_amount / fare_amount
high_tip = 1 if tip_percentage >= 0.20 else 0
```

A 20% tip threshold is used because it is a common practical benchmark for identifying stronger tipping behavior.

---

## Dataset

The dataset comes from the official **NYC Taxi & Limousine Commission (TLC) Trip Record Data**.

- Dataset: NYC Yellow Taxi Trip Records
- Period used: January 2024, February 2024, and March 2024
- File format: Monthly Parquet files
- Official source: https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page
- Data dictionary: https://www.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf

Only **credit-card trips** are used because the TLC data dictionary states that `tip_amount` is automatically populated for credit-card tips, while cash tips are not included.

```python
payment_type == 1
```

---

## Dataset Availability

The full dataset is **not stored directly in this GitHub repository** because the raw and processed Parquet files are too large for normal GitHub tracking.

GitHub warns when files are larger than 50 MiB and blocks files larger than 100 MiB. For that reason, this repository only contains the notebooks and project files, while the dataset is provided separately as a compressed folder.

Download the dataset package here:

```text
(https://drive.google.com/file/d/1cx2Xcm9mu1gJuD4UTQcVTW4Vw6ENLDH3/view?usp=sharing)
```

After downloading, extract the compressed folder and place it in the project directory using the structure below:

```text
nyc-taxi-tip/
├── data
├── 01_preprocessing_cleaning.ipynb
├── 02_exploratory_data_analysis.ipynb
└── 03_feature_engineering_modeling_interpretation.ipynb
└── README.md
```

---

## Repository Structure

```text
nyc-taxi-tip/
├── data
|  ├── yellow_tripdata_2024-01.parquet
|  ├── yellow_tripdata_2024-02.parquet
|  └── yellow_tripdata_2024-03.parquet
├── 01_preprocessing_cleaning.ipynb
├── 02_exploratory_data_analysis.ipynb
└── 03_feature_engineering_modeling_interpretation.ipynb
└── README.md
```

### Notebook Descriptions

| Notebook | Purpose |
|---|---|
| `01_preprocessing_cleaning.ipynb` | Loads raw Parquet files, filters credit-card trips, removes invalid records and outliers, creates the target variable, and saves the cleaned dataset. |
| `02_exploratory_data_analysis.ipynb` | Explores tipping behavior through charts and summary statistics, including tip distribution, high-tip balance, time patterns, fare patterns, and location-based patterns. |
| `03_feature_engineering_modeling_interpretation.ipynb` | Creates model features, trains classification models, evaluates performance, interprets feature importance, and summarizes business insights. |

---

## Data Cleaning Summary

The preprocessing stage removes records that are not useful or realistic for the prediction task.

Main filtering decisions:

- Keep credit-card trips only
- Remove trips with non-positive fare amount
- Remove trips with non-positive trip distance
- Remove trips with negative tip amount
- Remove trips where pickup time is after drop-off time
- Remove extreme trip duration, distance, fare, and speed outliers
- Create `tip_percentage`
- Create binary target variable `high_tip`

---

## Feature Engineering

The final modelling dataset includes features from several groups.

### Time-based features

- Pickup hour
- Pickup day of week
- Pickup month
- Weekend indicator
- Rush-hour indicator
- Night-trip indicator

### Trip-based features

- Trip duration
- Trip distance
- Average speed
- Passenger count

### Fare-based features

- Fare amount
- Tolls amount
- Congestion surcharge
- Airport fee

### Location-based features

- Pickup location ID
- Drop-off location ID
- Same-zone trip indicator

---

## Data Leakage Prevention

Some columns are removed from the model because they directly reveal or partially reveal the target.

Excluded leakage columns:

- `tip_amount`
- `tip_percentage`
- `high_tip`
- `total_amount`

These variables are not used as model features because the goal is to predict tipping behavior, not to allow the model to directly read the answer.

---

## Modelling Approach

The project uses a **time-based train/test split** instead of a random split.

```text
Training set: January-February 2024
Test set: March 2024
```

This split better reflects a realistic prediction setting because the model is trained on earlier trips and evaluated on later trips.

Models compared:

- Logistic Regression
- Decision Tree
- Random Forest
- LightGBM

Evaluation metrics:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- PR-AUC
- Confusion matrix

Accuracy is not used alone because the high-tip class may be imbalanced.

---

## Model Results

The final model comparison is reported in the modelling notebook.

| Model | Accuracy | Precision | Recall | F1-score | ROC-AUC | PR-AUC |
|---|---:|---:|---:|---:|---:|---:|
| Logistic Regression | 0.610660 | 0.808043 | 0.643583 | 0.716497 | 0.604036 | 0.818090 |
| Decision Tree | 0.584572 | 0.814701 | 0.590985 | 0.685041 | 0.607343 | 0.818319 |
| Random Forest | 0.523450 | 0.823977 | 0.478923 | 0.605759 | 0.607738 | 	0.821538 |
| LightGBM | 0.579535	 | 0.819968 | 0.576572 | 0.677060 | 0.617883 | 0.826843 |

---

## Key Insights

The exploratory analysis and model interpretation focus on questions such as:

- What percentage of credit-card rides are high-tip rides?
- Does tipping behavior vary by pickup hour?
- Are weekday and weekend tipping patterns different?
- Do longer trips receive higher or lower tip percentages?
- Are fare amount and trip distance associated with tipping behavior?
- Which pickup and drop-off locations show stronger tipping patterns?
- Which features are most important for predicting high-tip rides?

The findings are interpreted as **associations**, not causal effects.

---

## How to Run the Project

### Download the dataset

Download the compressed dataset folder from the external link above and extract it into the project directory.

Expected local structure:

```text
data/
├── yellow_tripdata_2024-01.parquet
├── yellow_tripdata_2024-02.parquet
└── yellow_tripdata_2024-03.parquet
```

### Run the notebooks in order

```text
01_preprocessing_cleaning.ipynb
02_exploratory_data_analysis.ipynb
03_feature_engineering_modeling_interpretation.ipynb
```

---

## Requirements

Main Python libraries:

```text
pandas
numpy
pyarrow
matplotlib
seaborn
scikit-learn
lightgbm
jupyter
notebook
```

---

## Limitations

- Cash tips are not included in the dataset, so the analysis focuses only on credit-card trips.
- The dataset is observational, so the project identifies associations rather than causal relationships.
- Location IDs are useful for prediction, but additional zone metadata would be needed for richer geographic interpretation.
- The model predicts tipping behavior after trip characteristics are known; it is not framed as a real-time pre-trip prediction system.
- Results may change if a longer time period or newer taxi data is used.

---

## Future Work

Possible improvements include:

- Add more months of 2024 data
- Test the model on 2025 data to examine behavior drift
- Add taxi zone lookup tables for clearer location interpretation
- Use SHAP values for more detailed model explainability
- Build a small dashboard to present tipping behavior patterns

---

## Portfolio Summary

Built an end-to-end machine learning case study using NYC Yellow Taxi trip records to predict high-tip rides. The project includes data cleaning, exploratory data analysis, feature engineering, time-based validation, model benchmarking, leakage prevention, feature-importance analysis, and business-style recommendations.
