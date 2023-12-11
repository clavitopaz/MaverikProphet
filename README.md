# Maverik: Rolling Forecast Model using Prophet
This repository outlines the work conducted on the rolling forecast Prophet model for Maverik by the Here for the Cinnabon team. The presentation of this model won first place in the Fall 2023 Masters of Science in Business Analytics Capstone competition. This repository includes exploratory data analysis, the model proposed to Maverik and insights gained from the experience.

# Business Problem
Maverick needed an accurate daily forecast of sales KPIs for a new store's first year of sales. Achieving this allows for more effective financial planning and accurate ROI documents. This project would be a success if it is able to yield a better forecast than their current model. Stakeholders measured success by seeing if there were improvements to the benchmark RMSE. 

This is a supervised regression problem where the predictors involve both categorical and numerical variables, and the target are four sales metrics. In order to solve this problem, we conducted exploratory data analysis, trained several forecasting models, evaluated models on success metrics and generated predictions using the chosen model.

The deliverable is a predictive model that produces daily sales forecasts for each sales metric. This model considers store features, seasonality, as well as having capability to process new data and re-forecast. This project was executed by Biva Sherchan, Roman Brock, Bhoomika John Pedely and Pablo Zarate.

# [Exploratory Data Analysis](https://clavitopaz.github.io/MaverikProphet/Final_Maverik_EDA_v3.html)
We identified the following information in our initial analysis of the dataframes provided to us by Maverik:

**1. Store 21980 Sales**

On average, Store 21980 is showing extremely high diesel sales relative to other stores. We also see that store 22260 has the highest unleaded sales and store 22085 has the highest inside sales. Interestingly, food services sales is very low for all stores and relative to other sales metrics.

![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/cfbcc3df-6e55-486d-9e37-5a366fd4ae34)

**2. Sales Trends over Time**
   
In our time-series, Maverik experiences higher diesel and unleaded sales in the Summer. Total food service sales do not experience any seasonality trend - sales remain flat throughout the time series. The sales by month and sales by week charts show similar trends.

![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/9c1dd2cf-e4bc-405f-8abd-bc44e2f65027)


Looking at average sales by day of week, Maverik experiences higher sales on weekdays compared to weekends. Total inside sales and unleaded sales follow an upward trend throughout the week and peak on Friday. Diesel and food service sales remain steady throughout the weekday.

![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/dc1f97bc-6aad-4e65-9a3f-7a0e4092323f)

Looking at holidays, Diesel sales experience very low sales on Thanksgiving, Christmas Eve, Christmas and New Years holidays. Besides that, there is not much variability in holiday vs non-holiday sales metrics.

![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/5a64999e-135a-44bc-9684-f0d9ac46afed)


**3. Sales Correlations**
   
Within the EDA file, there are four correlation plots for each sales metric showing its highest correlation to seven variables in the datasets.

Correlations to highlight: Stores selling pizza to inside sales:

![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/7054bb21-6581-41c0-89bb-6df00ad2e186)

Overall, every sales metric is positively correlated to each other. The highest correlation is between inside sales and food service sales.

**4. Removing Features**
   
These features can be removed as they only have one data value: Front door, god_father_pizza, Diesel, non-24-hr, self_checkout, car_wash, and ev_charging.

The first column for each dataset has a 'count' for each row in the datasets. This can be removed.

Store 21980 will also be removed for modeling due to its exceptionally high diesel sales.

# [Team Modeling Assignment - Initial Prophet Model](https://clavitopaz.github.io/MaverikProphet/Team_Model_Final-1.html)
Our team created four separate models to see if it could beat Maverik's RMSE benchmarks. After running an Auto-Arima and Naive Bayes model, I pivoted to Prophet.

Developed by Meta, Prophet is an open-source forecasting tool that specializes in time-series data. It's tailored specifically to capture seasonality, shifts in trend and special events. We applied Maverik's day of the week, seasons and holiday seasonality to the model, as well as the target variable's most highly correlated store features.

Prophet performed better on average compared to the other models.  

![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/115f7dab-cbd6-4827-b165-9c09d02b5877)

# Adding National Unleaded & Diesel Data
While working on this model, the biggest challenges our team had was in trying to differentiate what we would propose to Maverik from other candidate. Theoretically, every model proposed to Maverik should work showing a prediction for store sales. However, without having specific store data, we would all be challenged in identifying data that is predictive in forecasting store sales.

When analyzing the unleaded sales trend, we see a ramp up in sales starting January 2023 with a huge ramp up at the end of the dataframe. Was this due to bad data? Or is there something to explain this?

![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/3300044e-2297-46f3-b7da-0185c4ba10f0)

When comparing this information to national unleaded prices, we do see that there was a steady increase in gas prices starting in January 2023.

![image](https://clavitopaz.github.io/MaverikProphet/unleadedimage.png)

So I added national weekly Unleaded & Diesel prices to the model. The results were very positive, cutting RMSE by 50%.

![image](https://clavitopaz.github.io/MaverikProphet/rmsecomparison.png)

# [Rolling Forecast Model](https://github.com/clavitopaz/MaverikProphet/blob/main/prophet_msba_final.ipynb) 
[Download Rolling Forecast Notebook here](https://clavitopaz.github.io/MaverikProphet/prophet_msba_final.ipynb)

After deciding that Prophet was the best performing model, and including national diesel and unleaded prices which reduced Unleaded and Diesel RMSE by 50%, the next step involved engineering the rolling forecast.

The major areas to highlight for the rolling forecast are as follows:
- Splitting the Data: Whereas in the original model, we split by 30 stores to train and 6 stores to test, for the rolling forecast we are now splitting the data from the beginning of the time series to Day 0 of the 'new store'.
- Training on New Data: For each loop in the code, we are taking in new daily sales data that is used to re-train the model, creating a less erroneous rolling window of predictions as more data comes in.
- Computationally Intensive: We are now running a Prophet model on four dataframes (the four sales metrics) and doing so every day for 365 days. This results in a computationally intensive model. On a 16GB Macbook, it took 2140 minutes (almost 21 hours).

The results are a model that produces daily forecasts for each sales metric for a given horizon (365 days) that updates every day on new sales information.

![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/bcfb0576-dce8-4883-9d5a-fdbc67964bdd)
![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/2272e47d-5520-4616-bd2b-318c79ad5a02)
![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/afaea07b-3f9c-46f6-8c98-92b1acae23d1)
![image](https://github.com/clavitopaz/MaverikProphet/assets/122945935/4a6075c3-ed8b-474e-ba2b-f52c263b0506)

# [Presentation of Results](https://clavitopaz.github.io/MaverikProphet/MaverikSlides.pdf) 
We can see that the Prophet Rolling Forecast model beats Maverik's benchmark by an average 73% reduction in prediction error. Adopting this model would result in more accurate predictions, improved supply chain management and more accurate initial ROI estimates.

![image](https://clavitopaz.github.io/MaverikProphet/results.png)

# Conclusion
This was an incredible project for the following:
- Using various modeling techniques to find a solution: Besides Prophet, I also built ARIMA and Naive Bayes model. The rest of the team created ETS, SARIMA and XGBoost models.
- Research industry trends: For Unleaded and Diesel sales metrics, understanding the market's pricing of prices per gallon was very useful in building a more predictive model.
- Learning to work in a team: Biva, Roman and Bhoomika are all intelligent, genuine colleagues who have dedicated many hours to ensure deliverables are accurate, of high quality and of material use for Maverik.
- Coding: Building the rolling forecast involved building dictionaries, creating loops, defining functions and more. I'm very thankful for the coding classes provided to us in the MSBA program and W3 Schools.

I hope you have enjoyed reading this GitHub about the Prophet Rolling Forecast model, the first place winner of the 2023 MSBA Capstone competition. If you have any questions, please feel free to reach out to me at pzarate.c14@gmail.com.
