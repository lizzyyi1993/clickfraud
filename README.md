# Click Fraud Analysis

## Overview
- Investigated the journey of users’ clicks behaviors to detect click fraud and predicted whether clicks on the mobile ads will lead to app download

- Performed dimension reduction using PCA, and model selection among various machine learning algorithms such as Logistic, LDA, KNN, CART, SVM, and Random Forest; results in the ROC-AUC score of 0.9907

## Background
In the modern world where people spend significant amounts of time online, businesses invest a lot of money studying users’ online browsing behaviors and digital advertising to drive up the revenues. However, according to Forrester, 69% of brands spending $1 million on a monthly basis reported that at least 20% of their budgets were being lost due to digital ad fraud (Boutcher, 2020).

This project is to identify and help prevent the mobile ad fraud in the digital advertising industry using data from TalkingData, one of the world's largest independent big data service platforms, covering over 70% of active mobile devices nationwide. Essentially, we suspected that the IP addresses that have multiple click activities but failed to download the app are potentially fraudulent.

## Exploratory Data Analysis
The dataset contains seven columns, where categorical features have been encoded by TalkingData before releasing.
1. ip: ip address of click (numeric)
2. app: app id for marketing (category)
3. device: device type id of user mobile phone (e.g., iphone 6 plus, iphone 7, huawei mate 7, etc.) (category)
4. os: os version id of user mobile phone (category)
5. channel: channel id of mobile ad publisher (category)
6. click_time: timestamp of click (UTC) (time)
7. is_attributed: the target that is to be predicted, value 1 indicating the app was downloaded (binary)

We conducted data cleaning and manipulation on 100,000 randomly selected rows of training data and conducted feature engineering on the dataset. We found the data appeared
to be highly imbalanced, having 99.77% of the clicks leading to not downloading, and 0.23% of the clicks leading to app download. We later solved this issue by using the SMOTE Python package.

Next, we investigated the click pattern of different dimensions: app, device, os and channel. The orange line represents the behavior of people who didn’t end up
downloading and the blue line represents who downloaded. We can see that their behavior patterns on those four dimensions are quite similar.

We then manipulated the time data, breaking click_time into year/month/day/hour/minute/second so we could better analyze the click behavior time-series patterns. Based on our observations, it seems that the peak days are the 7th, 8th and 9th of the month, and the click activities for those who downloaded peaked at around 5-10 AM. And the click activities for those who didn’t download peaked at around 4 AM and 1 PM.

We further separated the dataset into two categories, downloaded (is_attributed = 1) and not downloaded (is_attributed = 0) and visualized the click activities within the 4 days timeframe. For users who didn’t download the app and the number of clicks has a clear pattern throughout the days. However, for users who downloaded the app, there are no clear
time-series patterns throughout the days. Based on the visualizations below, we assume that the fraud clicks are conducted by bots with scripts setting clicks at a certain time every day.

## Feature Engineering
The key trait for fraudulent ad clicks is that the abnormally high amount of clicks in a sequence which generate no download. Hence we thought it might be interesting to explore how long it takes for a user given ip-app-channel-os-device before he or she performs the next click.
Therefore, we grouped the data by the combination of ['ip', 'os', 'device', 'app', 'channel'] and calculated the "time till next click" to create 7 new features. Notice that this created null values under the new columns, and we replaced them with 0.

## Model Building 


## Performance Evaluation
