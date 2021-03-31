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
### Train Test Split
We used the smote Python package to solve the data imbalance problem, and randomly splitted the data into 70/30, with 70% of the data being training data and the rest 30% being testing data. We then scaled all the data to keep them at the same level for Logistic Regression and K-Nearest-Neighbour (KNN) model.

For a better Logistic Regression model performance, we also conducted PCA and the minimum number of principal components to retain such that 95% of the variance was retained is 12. Moreover, we conducted a clustering analysis on the training data, which was visualized using t-sne.

### Performance Evaluation
We built 6 models: Logistic Regression, Linear Discrimination Analysis, K-Nearest- Neighbour (KNN), Decision Tree (CART), Support Vector Machine (SVM) and Random Forest. We then compared the models based on their ROC_AUC score using 10-fold cross validation. Because Logistic Regression is the most frequently used machine learning model in practise, we tuned the logistic regression model using L2 regularization to improve the performance metric.

Turned out that Random Forest is still the best model with an accuracy rate of 99.9 AUC_ROC score. Therefore, we further conducted the variable importance analysis on Random Forest, and found that app, device and ip are the top 3 most important factors in predicting the fraud behavior. It indicates that fraud clicks tend to centralize on certain apps in order to get higher premiums for click counts and fraud clicks also tend to happen on certain device types and some ip addresses.

## Recommendations
We recommended that Random Forest should be used to measure the journey of users’ clicks across their activities and predict if the click will lead to downloading the app or not. It will add tremendous value to business because it will prevent the potential click frauds for app developers by analyzing users’ click behaviors. With this information, companies can prevent illegitimate clicks more proactively, and create a blacklist of users who have abnormal behaviors and avoid paying the ads commissioner for fraudulent clicking activities. which saves companies tons of money on paying for fake clicks.
