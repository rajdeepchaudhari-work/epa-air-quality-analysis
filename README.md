# EPA Air Quality: Data Preparation, Quality Review and Regression

Data preparation, data quality review and regression modelling on a public US Environmental Protection Agency (EPA) air quality dataset of **218,635 records**, in Python.

## What this project shows

- Loading and profiling a large real-world dataset with pandas
- A data quality review: missing values, duplicates, data types, value ranges, and unit consistency
- Transforming data for modelling: feature selection, one-hot encoding, and a log transform to handle a heavily skewed target
- A linear regression model benchmarked against a simple baseline
- Model explainability with SHAP
- An honest account of the limitations and what I would change

## Data

- **Source:** [EPA Air Quality Data on Kaggle](https://www.kaggle.com/datasets/thedevastator/epa-air-quality-data), derived from public US EPA data
- **Size:** 218,635 rows, 15 columns
- **Content:** air quality measures by US state, county and year
- **Personal data:** none. The data is aggregated at county level, so there are no UK GDPR concerns. If the data did include personal information, I would minimise the fields collected, pseudonymise identifiers, and restrict access before any analysis.

## Steps

| Step | What happens |
|---|---|
| 1. Extract | Download the dataset with `kagglehub` and load it into pandas |
| 2. Data quality review | Check types, missing values, duplicates, units per measure, value ranges and coverage |
| 3. Transform | Select features, handle missing values, one-hot encode the pollutant type, log-transform the skewed target |
| 4. Model | 80/20 train-test split, scaling, linear regression, comparison with a mean-only baseline |
| 5. Explainability | SHAP summary of which features drive predictions |
| 6. Findings | Results, limitations and improvements |

## Results

| Model | MAE | R² |
|---|---|---|
| Baseline (always predicts the mean) | 3.76 | 0.00 |
| Linear regression | 2.78 | 0.35 |

Values are on the log scale. The model clearly beats the baseline, but explains only about a third of the variation, which is expected without weather, traffic or industrial data.

## Limitations and what I would do differently

- **Mixed units in the target.** The `Value` column combines different measures (for example counts of days and concentrations) with different units. I would model one measure at a time, or standardise values within each measure.
- **Location codes used as numbers.** `StateFips` and `CountyFips` are codes, not quantities. I would encode them as categories or replace them with meaningful regional features.
- **Notebook, not a pipeline.** For production, I would move the cleaning steps into a repeatable script with automated data quality checks, and load the clean data into a database.

## How to run

Open `epa_air_quality_analysis.ipynb` in Google Colab and choose **Runtime, then Run all**. The dataset downloads automatically through `kagglehub`.

## Context

This started as my section of a group coursework project for CN6005 Artificial Intelligence at the University of East London (module mark 95%). I have since extended it with the data quality review and the limitations analysis.

**Tools:** Python, pandas, NumPy, scikit-learn, SHAP, Matplotlib, seaborn
