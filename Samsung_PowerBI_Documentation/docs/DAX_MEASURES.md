# DAX Measures Reference

Create measures with **Modeling → New measure**. Verify table and column names against the Power BI model before pasting. The examples assume rates in the market table are stored as decimals between 0 and 1.

## Sales and product measures

```dax
Total Revenue =
SUM(Samsung_Sales[RevenueUSD])
```

```dax
Units Sold =
SUM(Samsung_Sales[UnitsSold])
```

```dax
Average Selling Price =
DIVIDE([Total Revenue], [Units Sold], 0)
```

```dax
Countries with Sales =
DISTINCTCOUNT(Samsung_Sales[CountryID])
```

```dax
Total Products =
DISTINCTCOUNT(Samsung_Products[ProductID])
```

```dax
Products Sold =
DISTINCTCOUNT(Samsung_Sales[ProductID])
```

```dax
Revenue Share % =
DIVIDE(
    [Total Revenue],
    CALCULATE(
        [Total Revenue],
        REMOVEFILTERS(Samsung_Products[ProductCategory])
    ),
    0
)
```

```dax
Units Share % =
DIVIDE(
    [Units Sold],
    CALCULATE(
        [Units Sold],
        REMOVEFILTERS(Samsung_Products[ProductCategory])
    ),
    0
)
```

## Calendar table and time intelligence

Create a Calendar table with **Modeling → New table**:

```dax
Calendar =
ADDCOLUMNS(
    CALENDAR(
        MIN(Samsung_Sales[OrderDate]),
        MAX(Samsung_Sales[OrderDate])
    ),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month", FORMAT([Date], "MMM"),
    "Year Month", FORMAT([Date], "YYYY-MM"),
    "Month Start", DATE(YEAR([Date]), MONTH([Date]), 1)
)
```

Mark `Calendar` as a date table using `Calendar[Date]`. Sort `Calendar[Month]` by `Calendar[Month Number]`. For multiple years, use `Month Start` or `Year Month` on the axis.

```dax
Revenue Previous Year =
CALCULATE(
    [Total Revenue],
    SAMEPERIODLASTYEAR('Calendar'[Date])
)
```

```dax
Revenue YoY Growth % =
DIVIDE(
    [Total Revenue] - [Revenue Previous Year],
    [Revenue Previous Year],
    0
)
```

## Marketing measures

```dax
Total Marketing Spend =
SUM(Samsung_Marketing[MarketingSpendUSD])
```

```dax
Total Impressions =
SUM(Samsung_Marketing[Impressions])
```

```dax
Total Clicks =
SUM(Samsung_Marketing[Clicks])
```

```dax
Total Conversions =
SUM(Samsung_Marketing[Conversions])
```

```dax
Total Campaigns =
DISTINCTCOUNT(Samsung_Marketing[CampaignID])
```

```dax
Click-Through Rate % =
DIVIDE([Total Clicks], [Total Impressions], 0)
```

```dax
Conversion Rate % =
DIVIDE([Total Conversions], [Total Clicks], 0)
```

```dax
Cost per Click =
DIVIDE([Total Marketing Spend], [Total Clicks], 0)
```

```dax
Cost per Conversion =
DIVIDE([Total Marketing Spend], [Total Conversions], 0)
```

```dax
Average Spend per Campaign =
DIVIDE([Total Marketing Spend], [Total Campaigns], 0)
```

## Market estimates

Assumes table name `Samsung_Country_Market_Monthly`. Adjust if the imported table has a different name.

```dax
Estimated Category Demand =
SUM(Samsung_Country_Market_Monthly[EstimatedCategoryDemandUnits])
```

```dax
Estimated Samsung Demand =
SUMX(
    Samsung_Country_Market_Monthly,
    Samsung_Country_Market_Monthly[EstimatedCategoryDemandUnits]
        * Samsung_Country_Market_Monthly[SamsungShareEstimate]
)
```

```dax
Estimated Market Share % =
DIVIDE(
    [Estimated Samsung Demand],
    [Estimated Category Demand],
    0
)
```

```dax
Average Online Sales Share % =
AVERAGE(
    Samsung_Country_Market_Monthly[OnlineSalesShareEstimate]
)
```

## Suggested formats

| Measure | Format |
|---|---|
| Total Revenue, Average Selling Price, Marketing Spend | Currency (USD) |
| Units Sold, Impressions, Clicks, Conversions | Whole number |
| Total Campaigns, Total Products, Countries with Sales | Whole number |
| Revenue Share, Units Share, CTR, Conversion Rate, YoY Growth, Market Share | Percentage |
| Cost per Click, Cost per Conversion, Average Spend per Campaign | Currency (USD) |
| Estimated Category Demand, Estimated Samsung Demand | Whole number (units) |

## Interpretation and caveats

- Average Selling Price is revenue divided by units sold, a weighted realized price—not the average of list prices.
- Revenue Share % removes the ProductCategory filter but retains other filters such as date and country.
- Total Campaigns counts distinct campaign IDs in the current filter context, not campaign rows.
- Conversion Rate is conversions divided by clicks; confirm that this matches the source's conversion definition.
- Market-share measures are synthetic estimates, not official Samsung statistics.
- Do not calculate marketing ROI/ROAS from separate Sales and Marketing fact tables without campaign-attributed revenue or profit.
- A final `0` in `DIVIDE` returns zero when the denominator is zero. Replace it with `BLANK()` if an undefined rate should display as blank.
