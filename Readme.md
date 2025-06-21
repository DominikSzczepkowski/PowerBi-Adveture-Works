
# <span style="color:#ffffff; font-size: 1%;">AdvetureWorks Power Bi Project</span>
<div style=" border-bottom: 8px solid #FFD700; overflow: hidden; border-radius: 10px; height: 45px; width: 100%; display: flex;">
  <div style="height: 100%; width: 65%; background-color:rgb(147, 4, 73); float: left; text-align: center; display: flex; justify-content: center; align-items: center; font-size: 25px; ">
    <b><span style="color: #FFFFFF; padding: 20px 20px;">AdvetureWorks Power Bi Project</span></b>
  </div>
  <div style="height: 100%; width: 35%; background-image: url('https://images4.alphacoders.com/869/869425.jpg'); background-size: cover; background-position: center; float: left; border-top-right-radius: 10px; border-bottom-right-radius: 4px;">
  </div>
</div>

**Project Title**: AdvetureWorks
**Author**: Dominik Szczepkowski  
**Database**: `https://www.kaggle.com/datasets/algorismus/adventure-works-in-excel-tables/data

<div style="background-color: #fff8dc; padding: 16px; border-radius: 8px; border-left: 6px solid #fdd835; font-family: Arial, sans-serif;">
  <p style="margin: 0 0 12px 0; font-size: 16px; text-align: justify;"> 
    The project consists of 4 visualizations, plus an additional filter panel. The created Dashboard aims to present gross sales, based on the target and previous years. The bar chart located in the bottom left corner displays the percentage net profit from individual categories. The table is intended to provide general information about the dataset and consists of 8 columns. Below is a screenshot of the created dashboard, along with a description of the individual visualizations and the measures used.
  </p>
</div>
<a href="https://ibb.co/LDLPDRt7"><img src="https://i.ibb.co/cKGvK3Fn/Adveture-Works-Dashboard.png" alt="Adveture-Works-Dashboard" border="0" /></a>

## Table Overview

Table use following column:

- **Order_date**: Date of the order.
- **CountryFlagURL**: URL to the country flag icon.
- **Salesperson**: Name of the salesperson.
- **Category**: Product category (e.g., Accessories, Bikes, Clothing, Components).
- **CategoryIconURL**: URL to the category icon.
- **Quantity**: Number of items sold.
- **Gross Sales**: Total revenue before deductions.
- **Margin %**: Profit margin percentage (original dataset column name).

### Measure: 

The `CategoryIconURL` measure assigns an icon URL based on the product category using the following DAX code:

Code:
```dax
CategoryIconURL =
SWITCH(
    TRUE(),
    DIM_Product[Category] = "Accessories", "https://img.icons8.com/ios-filled/500/ffffff/shopping-bag.png",
    DIM_Product[Category] = "Bikes", "https://img.icons8.com/ios-filled/500/ffffff/bicycle.png",
    DIM_Product[Category] = "Clothing", "https://img.icons8.com/ios-filled/500/ffffff/t-shirt.png",
    DIM_Product[Category] = "Components", "https://img.icons8.com/ios-filled/500/ffffff/settings.png"
)
```
The `CountryFlagURL` measure assigns an icon URL based on the Country using the following DAX code:
```dax
CountryFlagURL = 
SWITCH(
    TRUE(),
    DIM_Region[Country] = "United States","https://img.freeflagicons.com/thumb/round_icon/united_states_of_america/united_states_of_america_640.png",
    DIM_Region[Country] = "Canada", "https://img.freeflagicons.com/thumb/round_icon/canada/canada_640.png",
    DIM_Region[Country] = "France", "https://img.freeflagicons.com/thumb/round_icon/france/france_640.png",
    DIM_Region[Country] = "Germany", "https://img.freeflagicons.com/thumb/round_icon/germany/germany_640.png",
    DIM_Region[Country] = "Australia", "https://img.freeflagicons.com/thumb/round_icon/australia/australia_640.png",
    DIM_Region[Country] = "United Kingdom", "https://img.freeflagicons.com/thumb/round_icon/united_kingdom/united_kingdom_640.png"

)
```
## Line Chart 
Created to present changes in the gross sales variable based on the previous year, the most important measures used to create the chart include endpoints and min, max values ​​with labels.

### Measure: 
Endpoint
```dax
Gross Sales | Endpoints = 
VAR _first_month = 1
VAR _first = CALCULATE([Gross Sales], DIM_Date[Month Number]=_first_month)

VAR _last_month = [Last Month]
VAR _last = CALCULATE([Gross Sales], DIM_Date[Month Number] =_last_month)

RETURN
 _first+_last
```
Max value of Gross Sales in visible period
```dax
Gross Sales | Max Month = 
VAR _sum_table=
SUMMARIZE(ALLSELECTED(DIM_Date), DIM_Date[Year Month Number], "Value", [Gross Sales]) 
VAR _treshold = MAXX(_sum_table, [Value])
VAR _current = [Gross Sales]
VAR _result = IF(_current = _treshold, [Gross Sales])

RETURN
_result
```
Min value of Gross Sales in visible period
```dax
Gross Sales | Min Month = 
VAR _current = [Gross Sales]
VAR sum_table = SUMMARIZE(ALLSELECTED(DIM_Date), DIM_Date[Year Month Number], "Value", [Gross Sales])
VAR _treshold = MINX(sum_table, [Value])
VAR _result = IF(_current= _treshold, [Gross Sales])

RETURN
_result
```

## Card
This visual is designed to provide stakeholders with a clear and immediate understanding of current sales performance relative to targets. It helps identify:

- Whether the sales strategy is on track,

- Which categories are performing well or underperforming,

- How much more effort is needed to reach the final goal.

This makes it a valuable component in executive dashboards or sales performance reports.
### Measure
Gross Sales
```dax
Gross Sales = SUMX(FACT_Sales, FACT_Sales[Quantity] * FACT_Sales[Unit Price])
```
Measure for bar chart (bakground)
```dax
Gross Sales (for Bar Chart) = 
SWITCH(
    TRUE(),
    [Gross Sales] < 0, [Target],
    [Target] - [Gross Sales]
)
```
Text symbol
```dax
Gross Sales % to target Text Symbol = 

SWITCH(
    TRUE(),
    [Gross Sales % to Target] = BLANK(), "-",
    [Gross Sales % to Target] < 1 && [Gross Sales % to Target] > 0.4, " ⚠️",
    [Gross Sales % to Target] < 0.4, " 🛑",
    "✔️"
)
```
## Bar Horizontal
This visual displays the Net Profit contribution of each product category, allowing stakeholders to understand profitability distribution and identify areas of concern or opportunity.
This visualization helps business decision-makers:

- Track profit performance across product categories.

- Identify loss-making areas (e.g., Bikes).

- Understand how each category contributes to the overall profit structure.

- Prioritize focus areas for cost optimization or sales strategy adjustments.

It is especially valuable in financial performance dashboards and category profitability reports.

### Measure
Category label
```dax
Category_ = SELECTEDVALUE(DIM_Product[Category])
```
Header (Category)
```dax
Header = 0
```
Subheader - value (gross Sales)
```dax
Subheader = 0
```
Bar - 1 
```dax
Total bar = 1
```