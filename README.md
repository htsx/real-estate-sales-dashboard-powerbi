# Real Estate Sales Dashboard — Power BI

An interactive 3-page Power BI dashboard analyzing 22,000+ property sales 
from a King County, WA real estate dataset.

## Data Source
[REAL ESTATE — King County, WA, USA](https://www.kaggle.com/datasets/mayankgautam47/mg-house-data-usa) 
by Mayank Gautam, licensed under [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/)

## Dashboard Pages

### 1. Executive Overview
A macro-level snapshot of the housing market for quick, high-level decision-making.
- **KPI Cards** — Total Properties (22K), Total Sales Value ($12bn), Average Price ($540.10K), Median Price ($450.00K), Average Price per SqFt ($259.67)
- **Sales Trend (Line Chart)** — Average sale price over time (May 2014–May 2015), revealing seasonal volatility and pricing spikes
- **Price Distribution (Histogram)** — Right-skewed distribution of sale prices, showing the market is concentrated in the sub-$1M range with a long tail of high-end outliers
- **Average Price by ZIP Code (Bubble Map)** — Geographic view of King County sales, sized/positioned by ZIP-level average price
- **Top 10 ZIP Codes by Average Price (Horizontal Bar Chart)** — Highest-value ZIP codes ranked, led by 98039 (Medina)
- **Slicer Panel** — Independent filters for ZIP Code, Sale Year, Bedrooms, Waterfront, and Grade

### 2. Property Analysis
A property-level deep dive into what physical characteristics drive price.
- **Living Area vs. Sale Price (Scatter Plot)** — Individual property-level plot; X = living area, Y = price, bubble size = lot size, color = grade — reveals a strong positive correlation between square footage and price, with grade clustering visible at the high end
- **Average Price by Bedrooms / Bathrooms / Grade (Bar Charts)** — Isolates the price impact of each individual feature
- **Property Detail Table** — Row-level listing of Property ID, ZIP Code, Price, Living Area, Lot Size, Bedrooms, Bathrooms, Grade, and Condition for granular inspection
- **Price Decomposition Tree** — Interactive drill-down of total sales price through ZIP Code → Grade → Waterfront → Bedrooms
- **Property Count by Condition (Donut Chart)** — Distribution of properties across condition ratings (majority rated "3")

### 3. Market Insights
Comparative and segment-level trends across the broader market.
- **Average Price by Waterfront (Bar Chart)** — Waterfront properties sell for roughly 3x the price of non-waterfront properties
- **Average Price by Basement Status (Bar Chart)** — Homes with basements command a modest price premium
- **Average Price by Renovation Status (Bar Chart)** — Renovated homes sell above non-renovated homes on average
- **Count of Properties by House Age (Histogram)** — Distribution of housing stock by age in years
- **Average Grade by ZIP Code (Bar Chart)** — Highest construction/design quality ratings by ZIP
- **Total Properties by Luxury Status (Bar Chart)** — Standard vs. luxury-grade (grade ≥ 10) property split
- **Homes Above vs. Below Market Average (Donut Chart)** — 63.36% of properties sell below the county-wide average price
- **Average Condition by ZIP Code (Bar Chart)** — Property condition ratings by geography
- **Neighborhood Price Comparison (Table)** — Per-ZIP price difference from the overall market average, highlighting over/under-valued neighborhoods

## How to Use
1. Download `_______` from this repository
2. Open it in [Power BI Desktop](https://www.microsoft.com/en-us/power-platform/products/power-bi/downloads) (free — no account required to view/explore locally)
3. Use the slicer panel on the Executive Overview page to filter by ZIP Code, Sale Year, Bedrooms, Waterfront, or Grade
4. Click into the Decomposition Tree on the Property Analysis page to drill through ZIP → Grade → Waterfront → Bedrooms

## Key Insights & Findings


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
- `Average Monthly Sales = AVERAGEX(VALUES(mg_house_data[date]), CALCULATE(COUNT(mg_house_data[id])))`

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
- `Average House Age = AVERAGEX(mg_house_data, YEAR(TODAY()) - mg_house_data[yr_built])`
- `Average Neighborhood Living Area = AVERAGE(mg_house_data[sqft_living15])`
- `Newest House = MAX(mg_house_data[yr_built])`
- `Oldest House = MIN(mg_house_data[yr_built])`

### Waterfront & Views
- `Waterfront Houses = CALCULATE(COUNTROWS(mg_house_data), mg_house_data[waterfront] = 1)`
- `Average Waterfront Price = CALCULATE(AVERAGE(mg_house_data[price]), mg_house_data[waterfront] = 1)`
- `Average Price (Non-Waterfront) = CALCULATE(AVERAGE(mg_house_data[price]), mg_house_data[waterfront] = 0)`
- `Waterfront Price Premium = [Average Waterfront Price] - [Average Price (Non-Waterfront)]`
- `Properties with View = CALCULATE(COUNTROWS(mg_house_data), mg_house_data[view] > 0)`

### Renovation & Basement
- `Properties Renovated = CALCULATE(COUNTROWS(mg_house_data), mg_house_data[yr_renovated] > 0)`
- `Renovation Rate = DIVIDE([Properties Renovated], [Total Properties])`
- `Average Renovation Age = AVERAGEX(FILTER(mg_house_data, mg_house_data[yr_renovated] > 0), YEAR(TODAY()) - mg_house_data[yr_renovated])`
- `Homes with Basement = CALCULATE(COUNTROWS(mg_house_data), mg_house_data[sqft_basement] > 0)`
- `Basement Percentage = DIVIDE([Homes with Basement], [Total Properties])`

### Market Segmentation
- `Luxury Homes = CALCULATE(COUNTROWS(mg_house_data), mg_house_data[grade] >= 10)`
- `Luxury Home Percentage = DIVIDE([Luxury Homes], [Total Properties])`
- `Homes Above Average Price = CALCULATE(COUNTROWS(mg_house_data), FILTER(mg_house_data, mg_house_data[price] > [Average Price]))`

### Neighborhood & Comparison Analytics
- `Neighborhood Average Price = CALCULATE(AVERAGE(mg_house_data[price]), ALLEXCEPT(mg_house_data, mg_house_data[zipcode]))`
- `Price Difference = AVERAGE(mg_house_data[price]) - [Neighborhood Average Price]`
- `Difference from Neighborhood Avg = AVERAGE(mg_house_data[price]) - CALCULATE(AVERAGE(mg_house_data[price]), ALL(mg_house_data[zipcode]))`
- `Lot Size Difference = AVERAGE(mg_house_data[sqft_lot]) - AVERAGE(mg_house_data[sqft_lot15])`

## Data Quality Fixes
- Corrected a mistyped 33-bedroom outlier record (should have been 3)
- Filtered malformed/future-dated transaction records skewing the sales trend
- Identified duplicate property ID entries in the source data

## Tools
Power BI Desktop, Power Query, DAX

## Screenshots
# Executive Overview (Page 1)
<img width="1375" height="744" alt="executive-overview png" src="https://github.com/user-attachments/assets/9bfe4e50-453a-441e-ae3a-1cc04287059f" />

# Property Analysis (Page 2)
<img width="1373" height="746" alt="property-analysis png" src="https://github.com/user-attachments/assets/bb8e3a2f-9795-4a0a-8cbb-93580faae0b0" />

# Market Insights (Page 3)
<img width="1371" height="747" alt="market-insights png" src="https://github.com/user-attachments/assets/47a802e4-0b34-49d3-aa66-3ceb4b6480c5" />
