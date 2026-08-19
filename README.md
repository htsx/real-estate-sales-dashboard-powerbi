# Real Estate Sales Dashboard: Power BI

An interactive 3 page Power BI dashboard analyzing over 22,000 property sales from a King County, WA real estate dataset.

## Data Source

This data comes from [REAL ESTATE, King County, WA, USA](https://www.kaggle.com/datasets/mayankgautam47/mg-house-data-usa) on Kaggle, created by Mayank Gautam, and it's licensed under CC BY NC 4.0.

## Dashboard Pages

### 1. Executive Overview

This page gives a quick, high level snapshot of the housing market.

- KPI cards showing total properties (22K), total sales value ($12bn), average price ($540.10K), median price ($450.00K), and average price per square foot ($259.67)
- A line chart of average sale price over time (May 2014 through May 2015), showing prices dip a bit in late fall and winter, then pick back up heading into spring 2015
- A histogram of price distribution, showing most sales sit under $1M with a handful of much more expensive outliers pulling the tail out
- A bubble map showing average price by ZIP code across King County
- A bar chart of the top 10 ZIP codes by average price, led by 98039 in Medina
- A slicer panel that lets you filter everything by ZIP code, sale year, bedrooms, waterfront status, or grade

### 2. Property Analysis

This page digs into which physical features actually move the price.

- A scatter plot of living area versus sale price, with bubble size showing lot size and color showing grade. Bigger homes clearly sell for more, and higher grade homes cluster toward the top
- Bar charts breaking out average price by bedrooms, bathrooms, and grade individually
- A detailed table listing every property's ID, ZIP code, price, living area, lot size, bedrooms, bathrooms, grade, and condition
- A price decomposition tree you can drill through by ZIP code, then grade, then waterfront status, then bedrooms
- A donut chart showing property count by condition rating, most homes land in the middle at a "3"

### 3. Market Insights

This page pulls together broader trends across the market.

- Waterfront homes sell for roughly 3.1 times what non waterfront homes go for
- Homes with basements sell for a bit more than homes without
- Renovated homes sell for a bit more than homes that haven't been renovated
- A histogram showing how housing stock is spread out by age
- Average construction and design grade by ZIP code
- A bar chart comparing standard homes to luxury grade homes (grade 10 and up)
- A donut chart showing that about 63% of properties sell below the county wide average
- Average condition rating by ZIP code
- A table comparing average price by ZIP code, shown next to a card with the true county wide average for easy comparison

## How to Use

1. Download `Real Estate Sales.pbix` from this repository
2. Open it in [Power BI Desktop](https://www.microsoft.com/en-us/power-platform/products/power-bi/downloads), it's free and you don't need an account to view or explore it locally
3. Use the slicer panel on the Executive Overview page to filter by ZIP code, sale year, bedrooms, waterfront status, or grade
4. Click into the decomposition tree on the Property Analysis page to drill through ZIP code, grade, waterfront status, and bedrooms

## Key Insights and Findings

Waterfront properties sell for roughly 3.1 times the price of non waterfront properties.

Most sales fall below $1M, but a long tail of high end outliers pulls the average price ($540K) noticeably above the median ($450K).

ZIP code 98039 in Medina has the highest average sale price in the county, while several ZIP codes sell for $250K to $300K below the county wide average.

About 63% of homes sell below the overall market average, which makes sense once you realize a smaller number of high value sales are the ones pulling the average up.

Only a small share of properties qualify as luxury grade (grade 10 and up), but they carry an outsized influence on the countywide average price.

Living area is the clearest visual driver of price. The scatter plot shows a strong, consistent trend where bigger homes sell for more, and higher grade homes tend to cluster toward the upper end.

Most properties, about 65%, are rated Condition 3, so the overall housing stock skews solidly toward average condition rather than excellent or poor.

Renovation adds a modest price bump, but it's nowhere near as large as the waterfront premium. Location and water access matter more to price than whether a home's been renovated.

Basements carry a smaller premium too, again nowhere near as strong as waterfront or living area.

House age is spread out pretty broadly, with the biggest single cluster of properties falling in the 5 to 10 year range, suggesting a wave of construction not long before this data was collected.

## How the Dashboard Is Built

Most of the measures behind this dashboard are simple, direct calculations, averages, sums, counts, and a few basic comparisons like house age and lot size differences.

For the Market Insights charts specifically (waterfront, basement, renovation, luxury status, and the above/below average comparison), I originally built these using more advanced filtering logic, but ended up simplifying them. Instead, I added a few small columns that just label each property with a plain category (like "Waterfront" or "Not Waterfront"), and let Power BI's built in chart aggregation handle the rest. Same numbers, same findings, just an easier model to read and follow.

The Neighborhood Price Comparison table works the same way. Instead of a formula calculating the difference from average, the table just shows each ZIP code's average price directly, with a separate card next to it showing the true county wide average so you can compare them side by side.

## Data Quality Fixes

A handful of issues came up while building this, and I want to be upfront about what I found and fixed.

- One record had a typo listing 33 bedrooms for a single home, clearly meant to be 3
- Some records had malformed or future dated transactions that were throwing off the sales trend, so I filtered those out
- I found duplicate property ID entries in the source data
- House age was originally being calculated off today's date instead of the actual sale year, which meant the numbers would quietly drift every time the file was reopened. I fixed this so age is now calculated relative to the sale itself
- The Sales Trend chart had a partial leading month (May 2014, just 1 property) and two single day outliers (October 11, 2014 and May 27, 2015) that were distorting the chart because it was built at a daily level. I fixed this by aggregating to monthly instead

## Tools

Power BI Desktop, Power Query, DAX

## Screenshots
### Executive Overview (Page 1)
<img width="1269" height="719" alt="Screenshot 2026-08-18 181936" src="https://github.com/user-attachments/assets/c082d38a-cb1d-4211-94f6-08563d918ae7" />

### Property Analysis (Page 2)
<img width="1272" height="717" alt="Screenshot 2026-08-18 181943" src="https://github.com/user-attachments/assets/96dcfe98-4f24-4e12-b8b1-5ca373c271e3" />

### Market Insights (Page 3)
<img width="1269" height="719" alt="Screenshot 2026-08-18 181953" src="https://github.com/user-attachments/assets/74bdff4d-6266-499f-9db2-b76b6d41b924" />
