# Maverik: Rolling Forecast Model using Prophet
This repository outlines the work conducted on the rolling forecast Prophet model for Maverik by the Here for the Cinnabon team. The presentation of this model won first place in the Fall 2023 MSBA Capstone competition. This repository includes exploratory data analysis, the model proposed to Maverik and insights gained from the experience.

# Business Problem
Maverick needed an accurate daily forecast of sales KPIs for a new store's first year of sales. Achieving this allows for more effective financial planning and accurate ROI documents. This project would be a success if it is able to yield a better forecast than their current model. Stakeholders measured success by seeing if there were improvements to the benchmark RMSE. 

This is a supervised regression problem where the predictors involve both categorical and numerical variables, and the target are four sales metrics. In order to solve this problem, we conducted exploratory data analysis, trained several forecasting models, evaluated models on success metrics and generated predictions using the chosen model.

The deliverable is a predictive model that produces daily sales forecasts for each sales metric. This model considers store features, seasonality, as well as having capability to process new data and re-forecast. This project was executed by Biva Sherchan, Roman Brock, Bhoomika John Pedely and Pablo Zarate.

# Exploratory Data Analysis
We identified the following information in our initial analysis of the dataframes provided to us by Maverik:

1. On average, Store 21980 is showing extremely high diesel sales relative to other stores. We also see that store 22260 has the highest unleaded sales and store 22085 has the highest inside sales. Interestingly, food services sales is very low for all stores and relative to other sales metrics.
![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/cfbcc3df-6e55-486d-9e37-5a366fd4ae34)

2. Dataframes are as follows:
Qualitative: (37, 55)

time_series: (13,908, 12)

Time_Series data includes daily sales data for 37 unique sites with sales date ranging 1/12/2021 - 8/16/2023.

3. Sales trends over time
In our time-series, Maverik experiences higher diesel and unleaded sales in the Summer. Total food service sales do not experience any seasonality trend - sales remain flat throughout the time series. The sales by month and sales by week charts show similar trends.
![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/9c1dd2cf-e4bc-405f-8abd-bc44e2f65027)


Looking at average sales by day of week, Maverik experiences higher sales on weekdays compared to weekends. Total inside sales and unleaded sales follow an upward trend throughout the week and peak on Friday. Diesel and food service sales remain steady throughout the weekday.
![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/dc1f97bc-6aad-4e65-9a3f-7a0e4092323f)

Looking at holidays, Diesel sales experience very low sales on Thanksgiving, Christmas Eve, Christmas and New Years holidays. Besides that, there is not much variability in holiday vs non-holiday sales metrics.
![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/5a64999e-135a-44bc-9684-f0d9ac46afed)


4. Sales Correlations
Within the EDA file, there are four correlation plots for each sales metric showing its highest correlation to seven variables in the datasets.

Correlations to highlight: Stores selling pizza to inside sales:
![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/7054bb21-6581-41c0-89bb-6df00ad2e186)

Overall, every sales metric is positively correlated to each other. The highest correlation is between inside sales and food service sales.

5. Removing features
These features can be removed as they only have one data value: Front door, god_father_pizza, Diesel, non-24-hr, self_checkout, car_wash, and ev_charging.

The first column for each dataset has a 'count' for each row in the datasets. This can be removed.

Store 21980 will also be removed for modeling due to its exceptionally high diesel sales.
