# Walmart Sales Prediction (Time Series Analysis)

Sales prediction is a crucial task for businesses as it allows corporations to set clear goals and to make better business decisions. Specifically in retail industries, sales forecast helps to efficiently allocate resources for future growth and to set up appropriate inventory and service levels, reducing unnecessary waste. Time series models are a widely used method for for sales prediction, as they are well-suited for predicting future values of a series of past sales data that are measured at regular intervals. Therefore, we would like to explore various time series models to make sales prediction of Walmart, the world’s largest retail company by revenue. We also aim to evaluate the models based on their accuracy, robustness, and computational efficiency.

There are two main time series models we are using: **ARIMA/SARIMA** and **Gaussian Processes**. We use drill-down analysis to analyze aggregate weekly sales per category and per state. For each of per category and per state analysis, We compare ARIMA/SARIMA and GP models’ performance on our test data (last 10-week sales) using RMSE as metric to determine which model we would like to use. We find that ARIMA/SARIMA works better both in per category and per state analysis, with normalized RMSE of 0.123 and 0.0829 respectively, smaller than 0.243 and 0.269 for GP models.

Here is the graph of our drill-down analysis method:

<p align="center">
<img width="800" alt="Screenshot 2023-05-31 at 13 03 00" src="https://github.com/MeiyuLiT/Time-Series-Analysis-Walmart-Sales-Prediction/assets/75913591/59d58006-c9c7-436e-a0df-ee572dfb8d2e">
</p>

## Dataset
For our project, we use a hierarchical sales data from Walmart Daily Sales ranging from Jan 28th, 2011 to Jun 18th, 2016, containing sales information of products in three categories (Food, Hobby, Household) sold in three states (California, Texas, Wisconsin). To better capture patterns and diminish the noise, we aggregrate the daily sales to weekly sales. Thus, our input dataset is Walmart Weekly Sales in three categories sold in three states.

## Code and some Results/Graphs
The code is divided into 3 parts: Data Preprocessing, ARIMA/SARIMA, Gaussian Process model. Here are some brief description and results. Please refer to _Report.pdf_ for further details.

- **Data Preprocessing**: we remove the sales at first time point as outliers becuase the sales at the begining time is not a full week which results in relatively low sales (outlier).

<p align="center">
<img width="800" alt="Screenshot 2023-05-31 at 13 00 59" src="https://github.com/MeiyuLiT/Time-Series-Analysis-Walmart-Sales-Prediction/assets/75913591/bda6bc44-8a72-45b8-a053-c22c3d94d2ea">
</p>

- **ARIMA/SARIMA**: for ARIMA, we conducted the ADF test in Figure 4 to see if we should difference or seasonlize our dataset by comparing the P-values. As we can see all states and categories should be 1st order differencing because it has the P-values smaller than 0.001 and Hobby does not follow the seasonal pattern. Thus, for Hobby category, we look at the ACF and PACF in Figure 3 and decide to choose ARIMA(4, 1, 2). For SARIMA, as shown in Figure 5, we construct Grid Search of a range of 256 parameter groups for each state (CA, TX, WI) and each category (Food and Household) and choose the one with the smallest AIC per state and per category.

<p align="center">
<img width="550" alt="Screenshot 2023-05-31 at 13 22 44" src="https://github.com/MeiyuLiT/Time-Series-Analysis-Walmart-Sales-Prediction/assets/75913591/2ca219fe-0163-487b-b86b-6a3b7cd0afbb">
<img width="550" alt="Screenshot 2023-05-31 at 13 23 08" src="https://github.com/MeiyuLiT/Time-Series-Analysis-Walmart-Sales-Prediction/assets/75913591/60ef0dab-9cc9-46a2-97be-2aabb25eb5d7">
</p>

- **Gaussian Process Model**: we construct our kernels and provide initial estimates of hyper-parameters l and p. Notice that we treat length scale as free parameter and treat periodicity as fixed to ensure the interpretability of our kernels and models. To implement this, we use scikit-learn library in Python, and in its implementation, the "fit" method maximizes the marginal log likelihood of the GP model, which is of great help for hyper-parameter tuning.

<p align="center">
<img width="550" alt="Screenshot 2023-05-31 at 13 25 41" src="https://github.com/MeiyuLiT/Time-Series-Analysis-Walmart-Sales-Prediction/assets/75913591/dccf0d0e-4a69-4b52-ab2a-4979b9651af4">
</p>

Here is the RMSE table:
<p align="center">
<img width="631" alt="Screenshot 2023-05-31 at 13 27 22" src="https://github.com/MeiyuLiT/Time-Series-Analysis-Walmart-Sales-Prediction/assets/75913591/08917a21-0948-4717-b293-083200a0430f">
</p>

Here is the prediction plot by using ARIMA/SARIMA and Gaussian Process:
<p align="center">
<img width="1096" alt="Screenshot 2023-05-31 at 13 28 14" src="https://github.com/MeiyuLiT/Time-Series-Analysis-Walmart-Sales-Prediction/assets/75913591/4090983c-d4b6-452b-aab9-ca51a8a697cc">
  <img width="977" alt="Screenshot 2023-05-31 at 13 29 11" src="https://github.com/MeiyuLiT/Time-Series-Analysis-Walmart-Sales-Prediction/assets/75913591/9b53631d-7ee4-485d-98f2-55f72c181500">
</p>


