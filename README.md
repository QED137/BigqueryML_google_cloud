# Google Cloud ML Project: Logistic Regression with BigQuery

## Overview
This project demonstrates the use of **Google Cloud Platform (GCP)** to build, train, and evaluate a logistic regression model using **BigQuery ML**. The goal is to predict customer transactions based on Google Analytics sample data.

## Tools and Technologies
- **Google BigQuery ML**: For training and evaluating the machine learning model.
- **Google Cloud Storage**: For storing additional data.
- **Google Data Studio**: For visualizing predictions and insights.
- **SQL**: For querying and managing data.

## Workflow
1. **Data Preparation**:
   - Query Google Analytics public data in BigQuery.
   - Prepare features like operating system, mobile usage, country, and pageviews.

2. **Model Training**:
   - Train a logistic regression model in BigQuery ML.
   - Save the model for evaluation and predictions.

3. **Model Evaluation**:
   - Use BigQuery ML evaluation metrics like precision, recall, and accuracy.

4. **Predictions**:
   - Predict transaction probabilities for July 2017 data.
   - Aggregate predictions by country and user.

5. **Visualization**:
   - Visualize insights in **Google Data Studio** (e.g., top countries for predicted purchases).

## GCP Setup
1. **BigQuery**:
   - Enabled BigQuery in GCP.
   - Used Google Analytics public dataset: `bigquery-public-data.google_analytics_sample`.

2. **Data Studio**:
   - Connected BigQuery to Data Studio.
   - Created interactive dashboards for predictions.

## How to Run
1. Set up GCP and enable BigQuery API.
2. Clone this repository:
   ```bash
   git clone https://github.com/QED137/BigqueryML_google_cloud.git
   ```
MIT Lisence. 
   
