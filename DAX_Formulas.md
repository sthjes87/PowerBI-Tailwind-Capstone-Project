# DAX (Data Analysis Expressions) for PowerBI Project

This file contain the main DAX formulas used in this analysis

## Create tables

### 1. Configure the Calendar Table

```DAX
CalendarTable =
ADDCOLUMNS(
    CALENDAR(DATE(2020, 1, 1), DATE(2023, 12, 31)),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month", FORMAT([Date], "MMMM"),
    "Quarter", QUARTER([Date]),
    "Weekday", WEEKDAY([Date]),
    "Day", DAY([Date])
)

```

## 2. Create a Sales in USD Calculated Table

```DAX
Sales in USD =
ADDCOLUMNS(
    Sales,
    "Country Name", RELATED(Countries[Country]),
    "Exchange Rate", RELATED('Exchange Data'[Exchange Rate]),
    "Exchange Currency", RELATED('Exchange Data'[Exchange Currency]),
    "Gross Revenue USD", [Gross Revenue] * RELATED('Exchange Data'[Exchange Rate]),
    "Net Revenue USD", [Net Revenue] * RELATED('Exchange Data'[Exchange Rate]),
    "Total Tax USD", [Total Tax] * RELATED('Exchange Data'[Exchange Rate])
)
```
## Configure aggregations using DAX

### 1. Calculate Yearly Profit margin

```DAX
Yearly Profit Margin = 
DIVIDE(
    SUM('Sales in USD'[Net Revenue USD]),
    sum('Sales in USD'[Gross Revenue USD]),
    0)
```
### 2. Calculate Quarterly Profit

```DAX
Quarterly Profit = 
CALCULATE(
    SUMX('Sales in USD','Sales in USD'[Gross Revenue USD]-'Sales in USD'[Net Revenue USD]),
    DATESQTD('Calendar Table'[Date])
)
```
### 3. Calculate Year-to-Date Profit

```DAX
Year-to-Date Profit = 
TOTALYTD(
    SUMX('Sales in USD',
    [Gross Revenue USD]-[Net Revenue USD]),
    'Calendar Table'[Date]
)
```

### 4. Calculate Median Sales

```DAX
Median Sales =
(
    MEDIAN = ([Gross Revenue USD])
)
```