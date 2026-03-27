# Sales Performance Dashboard – Power BI

## Overview

This project presents an interactive Power BI dashboard designed to analyze sales performance using Year-to-Date (YTD) and Previous Year-to-Date (PYTD) comparisons.

The dashboard enables users to explore trends, identify underperforming regions, and evaluate profitability across products and customer accounts.

This project was completed as part of a guided Power BI tutorial to strengthen my data modeling, DAX, and dashboard design skills.

## Objectives

1. Analyze sales performance over time
2. Compare current performance with the previous year
3. Identify trends and underperforming regions
4. Evaluate profitability using Gross Profit %

## Methodology
### Data: The dataset consists of three main tables;

1. Fact_Sales: Transaction-level sales data including revenue, quantity, cost, and dates
2. Dim_Accounts: Customer and location information
3. Dim_Product: Product hierarchy and classification

🧱 Data Modeling

A star schema was implemented:
Fact table: Fact_Sales
Dimension tables: Dim_Product, Dim_Accounts, Dim_Date

A custom date table was created using DAX:

Dim_Date = CALENDAR(DATE(2022,01,01), DATE(2024,12,31))
📐 Key Measures (DAX)
Sales = SUM(Fact_Sales[Sales_USD])
Quantity = SUM(Fact_Sales[Quantity])
COGs = SUM(Fact_Sales[COGS_USD])
Gross Profit = [Sales] - [COGs]

GP% = DIVIDE([Gross Profit], [Sales])
Time Intelligence:
YTD_Sales = TOTALYTD([Sales], Dim_Date[Date])
PYTD_Sales = CALCULATE([Sales], SAMEPERIODLASTYEAR(Dim_Date[Date]))
Dynamic Measure Selection:
S_YTD =
VAR selected_value = SELECTEDVALUE(Slc_Values[Values])
RETURN
SWITCH(
    selected_value,
    "Sales", [YTD_Sales],
    "Quantity", [YTD_Quantity],
    "Gross Profit", [YTD_GrossProfit]
)
📊 Dashboard Features
🔹 KPI Cards

YTD Sales

PYTD Sales

YTD vs PYTD

Gross Profit %

🔹 Interactive Slicers

Metric selector (Sales, Quantity, Gross Profit)

Year filter

🔹 Visualizations

Treemap showing bottom-performing countries

Waterfall chart for performance breakdown

Line & column chart for trend comparison

Scatter plot analyzing profitability vs revenue

🎨 Conditional Formatting

Positive values highlighted in blue

Negative values highlighted in red

Improves quick identification of performance trends

🧠 Key Insights

Some countries consistently show negative growth compared to the previous year

Sales trends vary across product types and time periods

Certain accounts generate high revenue but lower profit margins

💡 Business Recommendations

Focus on improving performance in underperforming regions

Optimize product strategy based on profitability trends

Target high-revenue, low-profit accounts for cost improvements

⚙️ Tools Used

Power BI

DAX (Data Analysis Expressions)

Data Modeling (Star Schema)

📸 Dashboard Preview

(Add your screenshots here)

📚 What I Learned

Building a star schema data model

Creating time-intelligence measures (YTD, PYTD)

Using SWITCH for dynamic measure selection

Designing interactive dashboards with slicers

Applying conditional formatting for better insights
