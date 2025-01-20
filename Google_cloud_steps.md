## Step 1. Create a dataset

  Create a new dataset within your project by clicking the three dots next to your project ID in the Explorer section, then clicking on Create dataset.

### The Create dataset dialog opens.

  Enter bqml_lab for Dataset ID, and click on CREATE DATASET (accepting the other default values).
## Step 2. Explore the data

The data we will use in this lab sits in the bigquery-public-data project, that is available to all. Let's take a look at a sample of this data.

  1.Add the query to Untitled query box, and click the Run button.
  ```bash
#standardSQL
SELECT
  IF(totals.transactions IS NULL, 0, 1) AS label,
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(geoNetwork.country, "") AS country,
  IFNULL(totals.pageviews, 0) AS pageviews
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20160801' AND '20170631'
LIMIT 10000;
```
The data tables have a lot of columns, but there are few of interest to us that we will use to create our ML model. Here the visitor's device's operating system is used, whether said device is a mobile device, the visitor's country or region and the number of page views will be used as the criteria for whether a transaction has been made. In this case, label is what you're trying to fit to (or predict).

This data will be the training data for the ML model you create. The training data is being limited to those collected from 1 August 2016 to 31 June 2017. This is done to save the last month of data for "prediction". It is further limited to 10,000 data points to save some time.

2. Let's save this as the training data. Click on the Save and then select Save view from the dropdown to save this query as a view. In the popup, select Dataset as bqml_lab and type training_data as the Table Name and click Save.
## Step 3. Create a model
1. Now replace the query with the following to create a model to predict whether a visitor will make a transaction:
   ```bash
   #standardSQL
   CREATE OR REPLACE MODEL `bqml_lab.sample_model`
   OPTIONS(model_type='logistic_reg') AS
   SELECT * from `bqml_lab.training_data`;
   ```
In this case, bqml_lab is the name of the dataset, sample_model is the name of the model, training_data is the transactions data we looked at in the previous task. The model type specified is binary logistic regression.

Running the CREATE MODEL command creates a Query Job that will run asynchronously so you can, for example, close or refresh the BigQuery UI window.   
## Step 4. Evaluate the Model
 1. Now replace the query with the following:
    ```bash
    #standardSQL
    SELECT
     *
    FROM
     ml.EVALUATE(MODEL `bqml_lab.sample_model`);
    ```
## Step 5. Use the model
 1. Now click SQL query and run the below query:
    ```bash
    #standardSQL
    SELECT
     IF(totals.transactions IS NULL, 0, 1) AS label,
     IFNULL(device.operatingSystem, "") AS os,
     device.isMobile AS is_mobile,
     IFNULL(geoNetwork.country, "") AS country,
     IFNULL(totals.pageviews, 0) AS pageviews,
     fullVisitorId
    FROM
     `bigquery-public-data.google_analytics_sample.ga_sessions_*`
    WHERE
    _TABLE_SUFFIX BETWEEN '20170701' AND '20170801';
    '''
 2. Let's save this July data so we can use it in the next 2 steps to make predictions using our model. Click on the Save and then select Save view from the dropdown to save this query as a view. In the popup, select Dataset as bqml_lab and type july_data as the Table Name and click Save.
 3. Predict purchases per country/region
    With this query you will try to predict the number of transactions made by visitors of each country or region, sort the results, and select the top 10 by purchases:
    ```bash
    #standardSQL
    SELECT
      country,
      SUM(predicted_label) as total_predicted_purchases
    FROM
       ml.PREDICT(MODEL `bqml_lab.sample_model`, (
    SELECT * FROM `bqml_lab.july_data`))
    GROUP BY country
    ORDER BY total_predicted_purchases DESC
    LIMIT 10;
    ```
 Here is another example. This time you will try to predict the number of transactions each visitor makes, sort the results, and select the top 10 visitors by transactions:
 ```bash
#standardSQL
SELECT
  fullVisitorId,
  SUM(predicted_label) as total_predicted_purchases
FROM
  ml.PREDICT(MODEL `bqml_lab.sample_model`, (
SELECT * FROM `bqml_lab.july_data`))
GROUP BY fullVisitorId
ORDER BY total_predicted_purchases DESC
LIMIT 10;
