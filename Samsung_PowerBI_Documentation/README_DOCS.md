# Samsung Global Performance — Project Documentation

Documentation for the Samsung-inspired Power BI portfolio project.

> **Data disclaimer:** This is an independent educational project using synthetic practice data. It is not affiliated with or endorsed by Samsung Electronics. Revenue, campaign, demand, and market-share values are not official Samsung results.

## Included documentation

- `docs/DATA_DICTIONARY.md` — tables, fields, definitions, and suggested data types.
- `docs/RELATIONSHIP_DIAGRAM.md` — recommended model relationships and Mermaid diagram.
- `docs/DAX_MEASURES.md` — reusable DAX measures for sales, product, market, and marketing analysis.

## Suggested repository layout

```text
samsung-global-performance/
├── README.md
├── LICENSE
├── Samsung_Global_Performance.pbix
├── Data/
│   ├── Samsung_Products.csv
│   ├── Samsung_Countries.csv
│   ├── Samsung_Sales.csv
│   ├── Samsung_Marketing.csv
│   └── Samsung_Country_Market_Monthly.csv
├── Screenshot/
└── Samsung_PowerBI_Documentation/
    ├── README_DOCS.md
    └── docs/
        ├── DATA_DICTIONARY.md
        ├── RELATIONSHIP_DIAGRAM.md
        └── DAX_MEASURES.md
```

**Before using the formulas:** confirm the exact column names in your CSV headers and Power BI model. The documentation assumes the monthly table is named `Samsung_Country_Market_Monthly` and the marketing date field is `CampaignDate`; adjust these names if yours differ.
