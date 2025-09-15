# Rusty Bargain: Used Car Price Prediction

## Project Overview

This project was developed for **Rusty Bargain**, a used car sales service that is creating a new application to attract customers. The core feature of this app is to allow users to quickly find out the market value of their car.

The main goal is to build a machine learning model that accurately predicts the price of a used car based on its historical data, technical specifications, and other features.

## Key Business Objectives

Rusty Bargain is interested in a model that balances three key criteria:
* **Prediction Quality:** The accuracy of the price prediction, measured by the **Root Mean Squared Error (RMSE)**.
* **Prediction Speed:** The time it takes for the model to make a prediction for a user.
* **Training Time:** The time required to train the model on the dataset.

## Data

The dataset (`car_data.csv`) contains historical data of used car ads and includes the following key features:

* **Features:** `VehicleType`, `RegistrationYear`, `Gearbox`, `Power`, `Brand`, `NotRepaired`, `Mileage`, etc.
* **Target:** `Price` (in Euros).

## Project Workflow

The project followed a standard data science pipeline:

1.  **Data Cleaning and Preparation:**
    * Loaded the initial dataset with 354,369 entries.
    * Handled a significant amount of missing values in columns like `VehicleType`, `Gearbox`, and `NotRepaired` using mode imputation or by creating a new 'unknown' category.
    * Removed 262 duplicate rows to ensure data integrity.
    * Filtered out anomalies and outliers in `RegistrationYear` (kept years between 1980-2016) and `Power` (kept HP between 50-500).
    * Removed irrelevant columns that do not contribute to price prediction (e.g., `DateCrawled`, `PostalCode`).

2.  **Exploratory Data Analysis (EDA):**
    * Visual analysis was conducted to understand data distributions and relationships. Key findings include a strong right-skew in the `Price` distribution and logical correlations between price and features like `Power`, `RegistrationYear` (positive), and `Mileage` (negative).

3.  **Modeling and Evaluation:**
    * **Methodology Correction:** The initial modeling approach revealed a critical **data leakage** issue, where the test set was used for hyperparameter tuning. This was corrected by implementing a proper **Train/Validation/Test split (80%/10%/10%)**, ensuring that hyperparameter tuning was performed *only* on the validation set and the final evaluation was performed *only* on the untouched test set.
    * **Feature Encoding:** Two separate encoding strategies were used to meet the requirements of different models:
        * **One-Hot Encoding** for Linear Regression and Random Forest.
        * **Native Categorical Handling** for LightGBM.
    * **Models Trained:** Three different models were trained and compared:
        * Linear Regression (as a baseline).
        * Random Forest (with hyperparameter tuning).
        * LightGBM (with hyperparameter tuning).

## Model Performance Comparison

The final models were evaluated on the test set based on the three business criteria. The results are summarized below:

| Model | RMSE (Quality) | Training Time (s) | Prediction Time (s) |
| :--- | :---: | :---: | :---: |
| **LightGBM** | **1670.50** | **12.9** | **0.252** |
| **Random Forest** | 1727.38 | 100.0 | 0.251 |
| **Linear Regression** | 2631.68 | 1.0 | **0.00684** |

*(Note: The times for Random Forest and LightGBM reflect the hyperparameter search on the validation set, and the final prediction time on the test set.)*

## Conclusion and Recommendation

The analysis shows that both **Random Forest** and **LightGBM** significantly outperform the **Linear Regression** baseline in terms of prediction quality (lower RMSE).

While Random Forest provides a good quality prediction, its training time is substantially longer than the other models.

The **LightGBM** model stands out as the best overall solution. It achieved the **lowest RMSE (1670.50)**, making it the most accurate model. Crucially, it combines this high accuracy with an **extremely fast training time** (12.9 seconds for the search) and an excellent prediction speed, making it ideal for a user-facing application.

**Therefore, the final recommendation for Rusty Bargain is to implement the LightGBM model.** It offers the best trade-off between prediction quality, training speed, and prediction speed, perfectly aligning with the company's business objectives.
