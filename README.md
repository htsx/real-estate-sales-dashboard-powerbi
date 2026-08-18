# Real Estate Sales Dashboard — Power BI

An interactive 3-page Power BI dashboard analyzing 22,000+ property sales 
from a King County, WA real estate dataset.

## Data Source
[REAL ESTATE — King County, WA, USA](https://www.kaggle.com/datasets/mayankgautam47/mg-house-data-usa) 
by Mayank Gautam, licensed under [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/)

## Dashboard Pages

### 1. Executive Overview
A macro-level snapshot of the housing market for quick, high-level decision-making.
- **KPI Cards**: Total Properties (22K), Total Sales Value ($12bn), Average Price ($540.10K), Median Price ($450.00K), Average Price per SqFt ($259.67)
- **Sales Trend (Line Chart)**: Average sale price over time (May 2014 – May 2015), showing a seasonal dip in late fall/winter and a recovery into spring 2015
- **Price Distribution (Histogram)**: Right-skewed distribution of sale prices, showing the market is concentrated in the sub-$1M range with a long tail of high-end outliers
- **Average Price by ZIP Code (Bubble Map)**: Geographic view of King County sales, sized/positioned by ZIP-level average price
- **Top 10 ZIP Codes by Average Price (Horizontal Bar Chart)**: Highest-value ZIP codes ranked, led by 98039 (Medina)
- **Slicer Panel**: Independent filters for ZIP Code, Sale Year, Bedrooms, Waterfront, and Grade

### 2. Property Analysis
A property-level deep dive into what physical characteristics drive price.
- **Living Area vs. Sale Price (Scatter Plot)**: An individual property-level plot (X = living area, Y = price, bubble size = lot size, color = grade) revealing a strong positive correlation between square footage and price, with high-grade properties clustering toward the upper end
- **Average Price by Bedrooms / Bathrooms / Grade (Bar Charts)**: Isolates the price impact of each individual feature
- **Property Detail Table**: Row-level listing of Property ID, ZIP Code, Price, Living Area, Lot Size, Bedrooms, Bathrooms, Grade, and Condition for granular inspection
- **Price Decomposition Tree**: Interactive drill-down of total sales price through ZIP Code → Grade → Waterfront → Bedrooms
- **Property Count by Condition (Donut Chart)**: Distribution of properties across condition ratings (majority rated "3")

### 3. Market Insights
Comparative and segment-level trends across the broader market.
- **Average Price by Waterfront Status (Bar Chart)**: Waterfront properties sell for roughly 3.1x the price of non-waterfront properties
- **Average Price by Basement Status (Bar Chart)**: Homes with basements command a modest price premium
- **Average Price by Renovation Status (Bar Chart)**: Renovated homes sell above non-renovated homes on average
- **Count of Properties by House Age (Histogram)**: Distribution of housing stock by age in years
- **Average Grade by ZIP Code (Bar Chart)**: Highest construction/design quality ratings by ZIP
- **Total Properties by Luxury Status (Bar Chart)**: Standard vs. luxury-grade (grade ≥ 10) property split
- **Homes Above vs. Below Market Average (Donut Chart)**: 63.36% of properties sell below the county-wide average price
- **Average Condition by ZIP Code (Bar Chart)**: Property condition ratings by geography
- **Neighborhood Price Comparison (Table)**: Average price per ZIP code, shown alongside a standalone overall average price card for easy comparison

## How to Use
1. Download `Real Estate Sales.pbix` from this repository
2. Open it in [Power BI Desktop](https://www.microsoft.com/en-us/power-platform/products/power-bi/downloads) (free — no account required to view/explore locally)
3. Use the slicer panel on the Executive Overview page to filter by ZIP Code, Sale Year, Bedrooms, Waterfront, or Grade
4. Click into the Decomposition Tree on the Property Analysis page to drill through ZIP → Grade → Waterfront → Bedrooms

## Key Insights & Findings
- **Waterfront premium**: Waterfront properties sell for roughly 3.1x the price of non-waterfront properties
- **Market concentration**: Most sales fall below $1M, with a long tail of high-end outliers pulling the average price ($540K) well above the median ($450K)
- **Geographic disparity**: ZIP 98039 (Medina) has the highest average sale price in King County, while several ZIP codes sell $250K–$300K below the county-wide average
- **Below-average is the norm**: 63.36% of homes sell below the overall market average, meaning a smaller number of high-value sales pull the average upward
- **Luxury is a small segment**: Only a small share of properties qualify as luxury-grade (grade ≥ 10), yet they carry disproportionate weight on countywide average pricing
- **Living area is the strongest visual price driver**: The Living Area vs. Sale Price scatter plot shows a clear, consistent upward trend, larger homes command higher prices, with high-grade properties (7+) clustering toward the upper end of both axes
- **Condition ratings skew heavily toward "average"**: 64.92% of properties are rated Condition 3, meaning the housing stock is overwhelmingly in standard/average condition rather than excellent or poor
- **Renovation adds a modest, not dramatic, price bump**: Renovated homes sell higher than non-renovated homes on average, but the gap is proportionally smaller than the waterfront premium, location/water access matters more to price than renovation status
- **Basements carry a smaller premium than waterfront access**: Homes with basements sell for modestly more than those without, though the effect is far less pronounced than waterfront or living area
- **House age is broadly distributed with a concentration in newer builds**: The largest single cluster of properties falls in the 5–10 year age range, suggesting a wave of construction/development shortly before the sales data was recorded.

## Key DAX Measures

### Executive KPIs
- `Total Properties = COUNTROWS(mg_house_data)`
- `Total Sales Value = SUM(mg_house_data[price])`
- `Average Price = AVERAGE(mg_house_data[price])`
- `Median Price = MEDIAN(mg_house_data[price])`
- `Average Price per SqFt = DIVIDE(SUM(mg_house_data[price]), SUM(mg_house_data[sqft_living]))`
- `Maximum Price = MAX(mg_house_data[price])`
- `Minimum Price = MIN(mg_house_data[price])`
- `Sales = COUNT(mg_house_data[id])`

### Property Characteristics
- `Average Bedrooms = AVERAGE(mg_house_data[bedrooms])`
- `Average Bathrooms = AVERAGE(mg_house_data[bathrooms])`
- `Average Living Area = AVERAGE(mg_house_data[sqft_living])`
- `Average Lot Size = AVERAGE(mg_house_data[sqft_lot])`
- `Average Basement Size = AVERAGE(mg_house_data[sqft_basement])`
- `Average Floors = AVERAGE(mg_house_data[floors])`
- `Average Grade = AVERAGE(mg_house_data[grade])`
- `Average Condition = AVERAGE(mg_house_data[condition])`
- `Average View Rating = AVERAGE(mg_house_data[view])`
- `Average House Age = AVERAGEX(mg_house_data, 2015 - mg_house_data[yr_built])`
- `Average Renovation Age = AVERAGEX(FILTER(mg_house_data, mg_house_data[yr_renovated] > 0), 2015 - mg_house_data[yr_renovated])`
- `Average Neighborhood Living Area = AVERAGE(mg_house_data[sqft_living15])`
- `Newest House = MAX(mg_house_data[yr_built])`
- `Oldest House = MIN(mg_house_data[yr_built])`
- `Lot Size Difference = AVERAGE(mg_house_data[sqft_lot]) - AVERAGE(mg_house_data[sqft_lot15])`

### Neighborhood & Comparison Analytics
- `Overall Average Price = AVERAGE(mg_house_data[price])`, shown in a standalone card so it always reflects the true countywide average, unaffected by whatever ZIP code filter context is active elsewhere on the page
- **Neighborhood Average Price**: shown directly in the comparison table by placing `zipcode` and `price` in the table and setting the aggregation to Average, no separate measure required

## Calculated Columns

A handful of simple `IF` based columns power the Market Insights charts, translating raw numeric fields into readable category labels for each chart's axis:

- `Waterfront Label = IF(mg_house_data[waterfront] = 1, "Yes", "No")`
- `Basement Label = IF(mg_house_data[sqft_basement] > 0, "Basement", "No Basement")`
- `Renovation Label = IF(mg_house_data[yr_renovated] > 0, "Renovated", "Not Renovated")`
- `Luxury Label = IF(mg_house_data[grade] >= 10, "Luxury", "Standard")`
- `Above Average Label = IF(mg_house_data[price] > AVERAGE(mg_house_data[price]), "Above Average", "Below Average")`

Each column feeds a chart axis directly, letting Power BI's built in visual aggregation (Average or Count) do the calculation, rather than a measure filtering the data manually. This keeps the model's logic transparent and easy to follow at a glance.

## Data Quality Fixes
- Corrected a mistyped 33-bedroom outlier record (should have been 3)
- Filtered malformed/future-dated transaction records skewing the sales trend
- Identified duplicate property ID entries in the source data
- Corrected house age calculations that were anchored to the current date rather than the sale year, causing age figures to drift over time
- Removed a partial leading month (May 2014, 1 property) and identified two single-day outliers (Oct 11 2014, May 27 2015) that were distorting the Sales Trend chart due to daily-level granularity; resolved by aggregating to monthly
- Simplified the Market Insights page's underlying DAX: replaced `CALCULATE`, `ALLEXCEPT`, and `ALL` based measures (used for the Waterfront, Basement, Renovation, Luxury, and Above/Below Average charts, plus the Neighborhood Price Comparison table) with simple calculated columns and built in visual aggregation, keeping the same verified figures with a more approachable underlying model

## Tools
Power BI Desktop, Power Query, DAX

## Screenshots
### Executive Overview (Page 1)
<img width="1269" height="719" alt="Screenshot 2026-08-18 181936" src="https://github.com/user-attachments/assets/c082d38a-cb1d-4211-94f6-08563d918ae7" />

### Property Analysis (Page 2)
<img width="1272" height="717" alt="Screenshot 2026-08-18 181943" src="https://github.com/user-attachments/assets/96dcfe98-4f24-4e12-b8b1-5ca373c271e3" />

### Market Insights (Page 3)
<img width="1269" height="719" alt="Screenshot 2026-08-18 181953" src="https://github.com/user-attachments/assets/74bdff4d-6266-499f-9db2-b76b6d41b924" />
