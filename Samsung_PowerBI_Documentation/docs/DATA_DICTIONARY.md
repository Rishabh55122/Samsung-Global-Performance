# Data Dictionary

Field meanings and data types below are based on the fields shown in the project model and formulas. Validate them against the actual CSV headers before publishing.

## 1. `Samsung_Products`

Product dimension; ideally one row per unique product.

| Column | Suggested type | Definition |
|---|---|---|
| `ProductID` | Text | Unique product key. |
| `ProductName` | Text | Product display name. |
| `ProductCategory` | Text | Product family/category for grouping visuals. |
| `BusinessDivision` | Text | Business unit associated with the product. |
| `ListPriceUSD` | Decimal number | Simulated list price in USD. |
| `ModelYear` | Whole number | Product model year. |
| `WarrantyMonths` | Whole number | Warranty duration in months. |

## 2. `Samsung_Countries`

Country dimension; ideally one row per country.

| Column | Suggested type | Definition |
|---|---|---|
| `CountryID` | Text | Unique country key. |
| `CountryName` | Text | Country name for map visuals. |
| `CurrencyCode` | Text | Currency code associated with the country. |
| `MarketType` | Text | Market classification. |
| `Region` | Text | Region for slicers and regional comparisons. |

## 3. `Samsung_Sales`

Sales fact table; each row represents a sales record according to the source dataset's grain.

| Column | Suggested type | Definition |
|---|---|---|
| `SalesID` | Text | Sales record identifier, if present and unique. |
| `OrderDate` | Date | Date of the sales record. |
| `ProductID` | Text | Foreign key to Products. |
| `CountryID` | Text | Foreign key to Countries. |
| `CustomerSegment` | Text | Customer segment. |
| `SalesChannel` | Text | Sales channel. |
| `UnitsSold` | Whole number | Quantity sold. |
| `UnitPriceUSD` | Decimal number | Recorded unit price in USD. |
| `DiscountRate` | Decimal number | Discount rate; verify whether encoded 0–1 or 0–100. |
| `RevenueUSD` | Decimal number | Recorded revenue in USD. |

## 4. `Samsung_Marketing`

Marketing fact table. A row may represent a campaign or a campaign-country-product-date observation; verify the source grain.

| Column | Suggested type | Definition |
|---|---|---|
| `CampaignID` | Text | Campaign identifier; may repeat across rows. |
| `CampaignDate` | Date | Campaign date used for time analysis; verify exact header. |
| `CampaignType` | Text | Campaign type/tactic. |
| `CountryID` | Text | Foreign key to Countries. |
| `ProductID` | Text | Foreign key to Products, if present/populated. |
| `Impressions` | Whole number | Recorded ad impressions. |
| `Clicks` | Whole number | Recorded clicks. |
| `Conversions` | Whole number | Recorded conversion events. |
| `MarketingSpendUSD` | Decimal number | Campaign spend in USD. |

## 5. `Samsung_Country_Market_Monthly`

Monthly country-level market estimates. Expected grain: one row per country per month unless the source includes additional dimensions.

| Column | Suggested type | Definition |
|---|---|---|
| `CountryID` | Text | Foreign key to Countries. |
| `MonthStart` | Date | First day of the represented month. |
| `EstimatedCategoryDemandUnits` | Whole number | Synthetic category demand estimate in units. |
| `SamsungShareEstimate` | Decimal number | Synthetic market-share estimate; verify if 0–1 or 0–100. |
| `OnlineSalesShareEstimate` | Decimal number | Synthetic online-sales-share estimate; verify encoding. |
| `ActiveRetailPartners` | Whole number | Active retail partner count, if present. |

## Quality checks

- Set ID/key columns to **Text**, even when values look numeric.
- Set dates to **Date**, monetary values to **Decimal number**, and quantities to **Whole number**.
- Check key consistency between dimension and fact tables.
- Check for duplicate dimension keys, blank IDs, missing dates, and impossible negative values.
- If shares are stored as decimals such as `0.134`, format as percentages without multiplying by 100. If stored as `13.4`, convert to `0.134` before percentage formatting.
- Set `CountryName` data category to **Country/Region** for mapping.
- All figures are synthetic educational data, not official Samsung data.
