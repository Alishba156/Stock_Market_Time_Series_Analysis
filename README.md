Predicting Yahoo Daily Adjusted Closing Rate
1. Introduction
This project addresses a critical challenge faced by financial analyst David and his client Janice, who experienced significant investment losses due to inaccurate Yahoo stock price forecasts. The objective is to leverage advanced analytics to develop a more reliable method for predicting stock prices, specifically focusing on Yahoo's Adjusted Closing Rate.

2. Problem Statement
The core challenge is to identify the most effective modeling technique to minimize residual errors in stock forecasting. David needs to address two specific questions:

Methodology Selection: Should he use standard Linear Regression (which treats variables independently) or Time Series Analysis (where time is the continuous exploratory variable)?
Predictive Accuracy: How can he model the Adjusted Closing Rate of Yahoo stock to ensure future forecasts align with actual market performance?
3. Feature Description
Feature Name	Description
Date	Date of the given day of trading (The time index).
Year	Year of the given day of trading (Extracted for annual trends).
Month	Month of the given day of trading (Extracted for seasonality).
Day	Day of the month of the given day of trading.
D	Floating point representation of the day (Used for mathematical trend modeling).
Open	The price at which the stock first traded when the exchange opened.
Close	The final price at which a stock traded during the regular trading session.
High	The highest price at which a stock traded during the day.
Low	The lowest price at which a stock traded during the day.
Volume	The total number of shares that were traded during the day.
adjusted_close	Target Variable: The closing price that accounts for all corporate actions (dividends/splits).
4. Methodology
The project follows a comprehensive Time Series Analysis approach:

4.1. Data Loading and Splitting
Stock data for Yahoo (ticker ^GSPC) from 2016-01-01 to 2016-12-31 was fetched using yfinance. The data was then split into training (Jan-Sept) and testing (Oct-Dec) sets.

4.2. Stationarity Testing
To ensure the reliability of Time Series models, the data's stationarity was rigorously tested using:

Exploratory Data Analysis (Rolling Statistics): Visual inspection of rolling mean and standard deviation.
Augmented Dickey-Fuller (ADF) Test: A statistical test to confirm the null hypothesis of non-stationarity.
4.3. Data Transformations
Various transformations were applied to achieve stationarity:

Log Transformation: Applied to stabilize variance.
Square Root & Cube Transformations: Explored but found insufficient.
Exponentially Weighted Moving Average (EWMA): Used to remove trend, showing improved but not perfectly stationary results.
First-Order Differencing: The most effective method,  Valuet−Valuet−1 , successfully removed trend and seasonality, yielding highly stationary data.
Decomposition: The time series was decomposed into Trend, Seasonality, and Residual components. The residuals were then tested for stationarity.
4.4. Autocorrelation Analysis
Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF) plots, along with the Durbin-Watson statistic, were used to identify the presence and nature of autocorrelation in the differenced series. This analysis was crucial for determining the optimal parameters for the ARIMA model.

4.5. ARIMA Model Development
An ARIMA (AutoRegressive Integrated Moving Average) model was constructed. The parameters (p, d, q) were chosen based on the ACF and PACF plots:

p (Auto-Regressive): Derived from the PACF plot (e.g., p=1 or p=2).
d (Differencing): Set to 1 due to first-order differencing.
q (Moving Average): Derived from the ACF plot (e.g., q=1).
The full ARIMA(1,1,1), as well as AR(2,1,0) and MA(0,1,1) sub-models, were implemented and compared based on their Residual Sum of Squares (RSS).

4.6. Model Validation and Reversal of Transformations
Accuracy Tests: Mean Forecast Error (MFE), Mean Absolute Error (MAE), Residual Sum of Squares (RSS), and Root Mean Squared Error (RMSE) were calculated to evaluate model performance.
AIC (Akaike Information Criterion): Used as a quality check; a lower AIC (large negative value) indicates a better model.
Reversing Transformations: The log transformation and differencing were reversed (cumulative sum, then exponential) to convert the predictions back into the original USD price scale.
5. Results
The ARIMA(1,1,1) model demonstrated strong performance in fitting the differenced log data. The final forecast was obtained by reversing the transformations, yielding predicted stock prices in USD. The model achieved a final RMSE of 3266952.52 and MAE of 3266952.52 after reversing transformations on the full dataset, indicating that the model, while effective on stationary data, still requires careful consideration of scaling when compared directly to original prices.
