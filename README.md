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
- **Holt–Winters (Triple Exponential Smoothing):** Captures trend + seasonality  
  - Strong baseline for time series with clear patterns
- **SARIMA (Seasonal ARIMA):** Handles seasonality, autocorrelation, and differencing  
  - Well-suited for daily or monthly crime patterns
- **VAR (Vector Auto Regression):** Allows multi-variate forecasting  
  - Useful when integrating external features (e.g., weather, unemployment)
- **Deep Learning Model (Transformer-based Time Series Forecasting):** Temporal Fusion Transformer (TFT)


##### Model Selection Strategy
Train all four models on the crime time series.

Evaluate performance on a validation period.

Select the best-performing model for:

York Region total crime forecast

Selected district forecast

4 District call-for-service demand forecast

### Data Description
#### 3.1 Calls-for-Service Dataset (2022–2024)
This dataset contains all recorded calls made to York Regional Police requesting assistance.
| **Column Name** | **Description** |
|-----------------|-----------------|
| **Call Date**   | Date of the call (YYYYMMDD format). |
| **Call Time**   | Time of call (HH:MM:SS). |
| **District**    | Police district (1–5 or “~”). |
| **Sector**      | Sector within district (11–52 or “~”). |
| **Call Type**   | Crime or incident type (e.g., PROPERTY DAMAGE, THEFT OF VEHICLE, FIRE). |

Use Cases:
-**Forecasting call volume**
-**Identifying crime trends**
-**District-level analysis**

#### 3.2 4 District Staffing Exceptions (2022–2024)
This dataset lists all exceptions affecting officer availability in 4 District.
| **Column Name**        | **Description** |
|------------------------|-----------------|
| **station_name**       | Platoon or unit (e.g., 4 District C Platoon). |
| **badge_number**       | Unique officer identifier. |
| **exception_date**     | Date of exception (YYYYMMDD). |
| **exception_start_time** | Time exception began. |
| **hours**              | Duration of exception. |
| **exception_group**    | Category (Sick, Stat Holiday, Vacation, etc.). |
| **exception_source**   | Origin (e.g., Voluntary Time Off, Sick). |

Use Cases:
-**Forecasting high-risk scheduling periods**
-**Matching staffing shortages with high-demand crime days**
