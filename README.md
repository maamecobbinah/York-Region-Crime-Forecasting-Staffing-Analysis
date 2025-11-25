# York Region Crime Forecasting & Staffing Analysis
##  Problem Statement
The Deputy Chief of the Community Safety Branch is seeking insights into future crime trends and front-line staffing requirements across York Region. This project aims to support strategic decision-making by forecasting crime and calls-for-service patterns and identifying high-risk districts.
#### The project is divided into two major components:
#### A. Crime Forecasting for York Region
Forecast total crime trends for York Region and for one selected Police District.
Identify the district most at risk of a future increase in crime and explain why.
Integrate one external public dataset (not provided) into the forecasting model—representing external factors that may influence crime (e.g., weather, unemployment, population growth).

#### B. Staffing & Scheduling Forecast for 4 District
Forecast future calls-for-service demand specifically for 4 District (Vaughan).
Analyze historical scheduling exceptions (e.g., sick days, vacations, stat holidays) from 2022–2024.
Identify:
Days of the year where officer availability is lowest, and
Whether these coincide with days of high call demand.
Use the results to highlight operational risks and staffing challenges.

##  Approach
This project follows a structured analytical workflow:
Data → Feature Engineering → EDA → Modeling → Evaluation → Insights
#### 2.1 Feature Engineering
Planned feature engineering includes:
A. Calls-for-Service Dataset
Convert date integers (YYYYMMDD) into proper datetime objects.
Extract:
Year, Month, Day, Weekday, Season
Hour-of-day (from Call Time)
Aggregate crime counts at:
Daily level (primary granularity)
District & Sector level
Create crime type groupings (e.g., property crime, violent crime).
Merge external datasets (example):
Weather patterns
Population growth
Socioeconomic indicators
Create lag features:
7-day lag, 30-day lag, 365-day lag
Rolling averages (7, 14, 30 days)
B. 4 District Staffing Exceptions
Parse exception_date into datetime.
Aggregate by:
Day (total hours of exceptions)
Exception type (Sick, Vacation, Stat Holiday, etc.)
Platoon
Create features that indicate:
Daily officer availability
“High absence” vs “Normal absence” thresholds

#### 2.2 Forecasting Modeling Techniques
To ensure robust forecasting, I will train multiple models and compare their performance using metrics such as RMSE, MAE, and MAPE.
Models to be Evaluated
Holt-Winters (Triple Exponential Smoothing)
Captures trend + seasonality
Strong baseline for time series with clear patterns
SARIMA (Seasonal ARIMA)
Handles seasonality, autocorrelation, and differencing
Well-suited for daily or monthly crime patterns
VAR (Vector Auto Regression)
Allows multi-variate forecasting
Useful when integrating external features (e.g., weather, unemployment)
Deep Learning Model (Transformer-based Time Series Forecasting)
Temporal Fusion Transformer (TFT)

##### I will select one Transformer-style model to compare against the statistical baselines.
Model Selection Strategy
Train all four models on the crime time series.
Evaluate performance on a validation period.
Select the best-performing model for:
York Region total crime forecast
Selected district forecast
4 District call-for-service demand forecast
