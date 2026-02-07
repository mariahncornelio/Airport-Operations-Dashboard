# Airport Operations Performance and Risk Dashboard

This project analyzes U.S. flight operations data to evaluate **key performance indicators (KPIs)** for airports across the United States, including **on-time performance, cancellation rates, delay causes, and overall operational risk**.

An interactive **Power BI dashboard** presents historical trends and comparisons across airports, while a **machine learning–based risk score** is included as one of the KPIs to summarize operational reliability.

A core component of the analysis is **temporal segmentation by COVID era**, allowing users to explore how airport performance changed over time:
- **Pre-COVID:** 2016–2019  
- **COVID:** 2020–2021  
- **Post-COVID:** 2022–2025  

All KPIs and risk metrics can be analyzed by airport and COVID era, enabling side-by-side comparison of operational performance before, during, and after the pandemic.


<img width="1264" height="709" alt="Screenshot 2026-02-06 at 8 18 52 PM" src="https://github.com/user-attachments/assets/a877a511-cc36-4089-b069-5a6d6c552fb4" />

## Data Source

All raw flight delay and operational data used in this project comes from the **U.S. Bureau of Transportation Statistics (BTS)**: https://transtats.bts.gov/OT_Delay/OT_DelayCause1.asp
- Raw data files are not included in this repository due to size constraints  

## Project Overview

The project consists of three major components:

1. **Exploratory & Descriptive Analytics**
   - Airport-level KPIs (on-time performance, delays, cancellations)
   - Pre-COVID, COVID, and Post-COVID trend analysis
   - Delay causes by frequency and severity

2. **Predictive Risk Modeling**
   - Binary classification model to predict high-delay-risk airports
   - Model trained on **Pre-COVID** flight data
   - Tested on **COVID and Post-COVID** periods (out-of-sample)

3. **Interactive Dashboard**
   - Power BI dashboard with maps, KPI cards, trends, and risk distributions
   - Airport-level drilldown via slicers
   - Risk probability visualization and model evaluation metrics
  
## Project Workflow

1. **Data extraction & filtering**
   - Downloaded raw U.S. flight operations data from the Bureau of Transportation Statistics
   - Filtered records to include **2016–2025** only, aligning with pre-COVID, COVID, and post-COVID analysis
  
2. **SQL-based aggregation & KPI generation**
   - Used SQL (`airline_data.sqbpro`) to clean, aggregate, and transform flight-level data
   - Engineered **six airport-level KPIs**, including on-time performance, cancellation rate, delay minutes, and delay causes
   - Produced a consolidated **`master_kpi` table** to serve as the analytical foundation for modeling and visualization
3. **Feature engineering & machine learning modeling (Python)**
   - Used the `master_kpi` dataset as input for a binary airport risk classification model
   - Trained the model on **pre-COVID data (2016–2019)** to establish baseline operational behavior
   - Tested the model on **COVID and post-COVID data (2020–2025)** to evaluate generalization under structural disruption
4. **Model evaluation**
   - Assessed performance using **ROC-AUC**, **confusion matrix**, and **feature importance**
   - Exported evaluation outputs for integration into the dashboard
5. **Deployment-style analytics & visualization**
   - Built an interactive **Power BI dashboard** to visualize historical KPIs and model outputs
   - Included **risk scores and predicted probabilities** as deployable decision-support metrics
   - Enabled filtering by airport, carrier, year, month, and COVID era

## Machine Learning Model Strategy

- **Dataset size:** 207,377 rows x 22 columns
- **Training data:** Pre-COVID flights
- **Testing data:** COVID and Post-COVID flights
- **Target variable:** Flight risk classification (high vs low delay risk)
  - 1 = delayed if 15 min or more or cancelled, 0 = on-time
- **Key evaluation outputs:**
  - ROC / AUC
  - Confusion Matrix
  - Feature Importance
  - Risk Score Distribution

**Disclaimer:**  
Model performance should be interpreted with caution, as operational patterns changed significantly during and after COVID-19. The same trained model is used for both evaluation and dashboard deployment.

## Tools & Technologies

### Data Processing & Analysis
- **Python**
  - `pandas` – data manipulation and feature engineering
  - `sqlite3` – querying aggregated KPI tables from SQLite
  - `matplotlib` – model evaluation and diagnostic visualizations
### Machine Learning
- **scikit-learn**
  - Classification modeling (Random Forest)
  - Model evaluation using ROC-AUC, confusion matrix, and classification reports
### Database
- **SQLite**
  - Used to aggregate flight-level data into airport-level KPIs
  - Served as the intermediate layer between raw data and Python modeling
### Visualization & Deployment
- **Power BI**
  - Interactive dashboards for KPI monitoring and risk analysis
  - DAX measures for dynamic metrics, filtering, and slicers

## Repository Structure
- **`AirOp_Pipeline.ipynb`:** End-to-end pipeline including data cleaning, feature engineering, model training, evaluation, and generation of prediction outputs
- **`airline_data.sqbpro`:** SQL source database/project file used during data extraction and preprocessing for KPIs
- **`airport_latlon.csv`:** Raw airport latitude and longitude reference data from https://openflights.org/data
- **`airport_latlon_cleaned.csv`:** Cleaned and standardized airport location data used for mapping and visualization
- **`classification_results.csv`:** Precision, recall, F1-score, and support metrics from the classification model
- **`confusion_matrix.csv`:** Confusion matrix values summarizing model prediction performance
- **`feature_importance.csv`:** Feature importance scores showing which operational factors most influenced risk predictions
- **`roc_curve_data.csv`:** False Positive Rate (FPR) and True Positive Rate (TPR) values used to generate ROC curves.
- **`master_kpi_predictions.csv`**  
  Final calssification model dataset containing:
  - Actual flight risk
  - Predicted flight risk
  - Predicted risk probability  
  Used directly in the Power BI dashboard for analysis and interaction.
- **`airline_pr_dashboard.pbix`:** Interactive Power BI dashboard
 
## Key Insights

- Late aircraft delays were the dominant contributor to total delay minutes.
- Average delay duration increased during COVID relative to Pre-COVID.
- The model successfully identifies high-risk airports using operational KPIs.
- Risk probabilities enable interpretable airport-level decision support.



