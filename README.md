# Sales Performance Dashboard – Power BI
<img width="624" height="270" alt="Title" src="https://github.com/user-attachments/assets/0f4433d5-040a-4928-9767-395d9b127323" />

## Overview

This project presents an interactive Power BI dashboard designed to analyze sales performance using Year-to-Date (YTD) and Previous Year-to-Date (PYTD) comparisons.

The dashboard enables users to explore trends, identify underperforming regions, and evaluate profitability across products and customer accounts.

This project was completed as part of a guided Power BI tutorial to strengthen my data modeling, DAX, and dashboard design skills.

## Objectives

1. Analyze sales performance over time
2. Compare current performance (YTD) with the previous year (PYTD)
3. Identify trends and underperforming regions
4. Evaluate profitability using Gross Profit %

## Methodology
### Data
The dataset consists of three main tables;

1. Fact_Sales: Transaction-level sales data including revenue, quantity, cost, and dates
2. Dim_Accounts: Customer and location information
3. Dim_Product: Product hierarchy and classification

### Data Modeling

A star schema was implemented:

1. Fact table: Fact_Sales
2. Dimension tables: Dim_Product, Dim_Accounts, Dim_Date (custom date table created using DAX)

```
Dim_Date = CALENDAR(DATE(2022,01,01), DATE(2024,12,31))
```

Key Measures DAX
```
Sales = SUM(Fact_Sales[Sales_USD])
Quantity = SUM(Fact_Sales[Quantity])
COGs = SUM(Fact_Sales[COGS_USD])
Gross Profit = [Sales] - [COGs]

GP% = DIVIDE([Gross Profit], [Sales])

Time Intelligence for Sales:
YTD_Sales = TOTALYTD([Sales],Fact_Sales[Date_Time])

PYTD_Sales = 
CALCULATE(
    [Sales],
    SAMEPERIODLASTYEAR(Dim_Date[Date]),
    Dim_Date[Inpast] = TRUE
)
```

Dynamic Measure Selection:
```
S_YTD = 
VAR selected_value = SELECTEDVALUE(Slc_Values[Values])
VAR result = SWITCH(selected_value,
    "Sales", [YTD_Sales],
    "Quantity", [YTD_Quantity],
    "Gross Profit", [YTD_GrossProfit],
    BLANK()
)
RETURN
result

S_PYTD = 
VAR selected_value = SELECTEDVALUE(Slc_Values[Values])
VAR result = SWITCH(selected_value,
    "Sales", [PYTD_Sales],
    "Quantity", [PYTD_Quantity],
    "Gross Profit", [PYTD_GrossProfit],
    BLANK()
)
RETURN
result
```

### Dashboard Features and Visualization
1. Year filter
2. SWITCH Metric selector (Sales, Quantity, Gross Profit)
3. KPI Cards for PYTD Sales, YTD Sales, YTD vs PYTD, Gross Profit %
4. Treemap showing bottom 10 performing countries 
6. Waterfall chart for performance breakdown
7. Line & column chart for trend comparison
8. Scatter plot analyzing profitability vs revenue

### Conditional Formatting for YTD vs PYTD Values

A positive value would be highlighted in green while a negative value would be highlighted in red

## Result

<img width="2560" height="1440" alt="Dashboard" src="https://github.com/user-attachments/assets/022b70e2-5934-48aa-a5c4-311b2bfa42b0" />

### Key Insights

1. Some countries consistently show negative growth compared to the previous year
2. Sales trends vary across product types and time periods
3. Certain accounts generate high revenue but lower profit margins

### Business Recommendations

1. Focus on improving performance in underperforming regions
2. Optimize product strategy based on profitability trends
3. Target high-revenue, low-profit accounts for cost improvements

## Conclusion
### What I Learned

1. Building a star schema data model
2. Creating time-intelligence measures (YTD, PYTD)
3. Using SWITCH for dynamic measure selection
4. Designing interactive dashboards with slicers
5. Applying conditional formatting for better insights
