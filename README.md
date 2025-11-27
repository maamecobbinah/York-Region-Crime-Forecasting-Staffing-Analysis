# York Region Crime Forecasting & Staffing Analysis
##  Problem Statement
The Deputy Chief of the Community Safety Branch is seeking insights into future crime trends and front-line staffing requirements across York Region. This project aims to support strategic decision-making by forecasting crime and calls-for-service patterns and identifying high-risk districts.
#### The project is divided into two major components:
#### A. Crime Forecasting for York Region
Forecast total crime trends for York Region and for one selected Police District.
Identify the district most at risk of a future increase in crime and explain why.
identify  external factors that may influence crime (e.g., weather, unemployment, population growth).

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
- **Grid Search SARIMA:** Allows multi-variate forecasting  , handles seasonality, autocorrelation 
  - Useful when integrating external features (e.g., unemployment and event data)
- **Gradient Boosting Regression Forecaster:** 


##### Model Selection Strategy
Train all four models on the crime time series.

Evaluate performance on a validation period.

Select the best-performing model for:

York Region total crime forecast

Selected district forecast

4 District call-for-service demand forecast


## Data Description
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


## Exploratory Data Analysis (EDA)
During EDA we found the following 
The Calls-for-Service for crime shows steady growth in police demand across York Region from 2022 to 2024. While the region saw a strong increase from 2022 to 2023, growth slowed significantly in the 2023–2024 period.
#### Key Insights (High-Level)
- **Total calls increased by +11% from 2022→2023**, showing strong growth.
- **Growth slowed to only +1% from 2023→2024**, indicating call volumes are starting to stabilize after a large jump the previous year.
- **District 1 is driving the most growth overall**, with a **+20% increase from 2022→2024** — the largest in the region.
- **District 5 also shows strong multi-year growth** (+15% over two years).
- **District 4 has the highest total call volume**, even though its YoY growth flattened in 2024 (–2% from 2023→2024).
- **Districts 2 and 3 show moderate, stable increases** with low-risk growth patterns.
- The **“~” category is inconsistent** and represents a very small portion of the data.
  
| District | 2022 Total Calls | 2023 Total Calls | 2024 Total Calls | Overall Total | 2022→2023 YoY | 2022→2024 YoY | 2023→2024 YoY |
|---------|------------------|------------------|------------------|----------------|----------------|----------------|----------------|
| 1       | 32,735           | 37,701           | 39,343           | 109,779        | **15%**        | **20%**        | 4%             |
| 2       | 32,180           | 35,268           | 35,011           | 102,459        | 10%            | 9%             | -1%            |
| 3       | 9,034            | 9,837            | 9,939            | 28,810         | 9%             | 10%            | 1%             |
| 4       | 43,688           | 48,177           | 47,358           | 139,223        | 10%            | 8%             | -2%            |
| 5       | 29,478           | 32,678           | 33,902           | 96,058         | 11%            | **15%**        | 4%             |
| ~       | 1,227            | 1,042            | 1,263            | 3,532          | -15%           | 3%             | 21%            |
| **Total** | **148,342**      | **164,703**      | **166,816**      | **479,861**    | **11%**        | **12%**        | **1%**         |


#### Additional Trends Observed
Along with district-level growth, several seasonal, weekly, and hourly patterns appear in the Calls-for-Service data.
##### Monthly Trends
- **May to October are the peak months for calls.**
- **August receives the highest volume, averaging ~43,434 calls.**
- **There is a noticeable dip in September, followed by a rebound in October.**
<img width="1035" height="493" alt="image" src="https://github.com/user-attachments/assets/f327a28c-65f4-43a6-87c0-3fa66e847fbe" />

##### Day-of-Week Trends
- **Thursday and Friday have the highest call volumes, each exceeding 72,000 calls.**
- **Friday is the single highest day, with approximately 73,000 calls.**
<img width="1015" height="503" alt="image" src="https://github.com/user-attachments/assets/d548f220-524e-42a0-b1c0-582c84bb870c" />

##### Time-of-Day Trends
- **Calls gradually rise through the morning and peak between 12 PM and 5 PM.**
- **3 PM is the busiest hour of the day, showing the highest call activity.**
<img width="1012" height="501" alt="image" src="https://github.com/user-attachments/assets/ecb2a7d4-7b66-479a-92e5-d65b03ee08bc" />

##### Sector Trends
- **Sector 22 and Sector 42 drive the most calls region-wide, each averaging over 36,000 calls.**
These sectors represent key operational hotspots.
<img width="997" height="722" alt="image" src="https://github.com/user-attachments/assets/bdadf9a6-ea2a-491c-a716-f83f779cac64" />


##### Top Call Types
**The top 5 most common call categories are:**
- **Welfare Checks**
- **Self-Reported Accidents**
- **Assisting a Police Officer or Other Agency**
- **Motor Vehicle Injury (MVC Injury)**
- **Ambulance Assistance**
These call types dominate activity levels and reflect where frontline demand is highest.
<img width="1006" height="772" alt="image" src="https://github.com/user-attachments/assets/fc12e7a9-fa86-4d6a-b1f4-9dc8b0a4e851" />


##  Results


##  Conclusion 
