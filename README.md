# 🏢 European Real Estate Market Analytics — Power BI Report

<div align="center">

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Data Modelling](https://img.shields.io/badge/Data%20Modelling-Star%20Schema-0D9488?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Production%20Ready-16A34A?style=for-the-badge)

**A production-grade, investor-ready Power BI report analysing 5,000 property listings across 10 EU cities — built with a full star schema, 30+ DAX measures, 4 report page tooltips, and a drill-through property deep-dive layer.**

[Report Overview](#-report-overview) · [Data Model](#-data-model) · [Report Pages](#-report-pages) · [DAX Measures](#-dax-measures) · [Tooltips](#-report-page-tooltips) · [Key Insights](#-key-insights) · [Setup](#-setup--installation)

</div>

---

## 📌 Project Summary

This end-to-end Power BI report was built to simulate a real-world real estate analytics platform — the kind used by property investors, market analysts, and agency product teams to make data-driven decisions on pricing strategy, market entry, and portfolio optimisation across Europe.

The report covers the complete analytics lifecycle: raw data → star schema data model → DAX measure suite → interactive dashboards → drill-through property profiles → report page tooltips with contextual investor intelligence.

| Dimension | Detail |
|-----------|--------|
| Dataset | 5,000 EU property listings |
| Countries | 10 (Austria, Belgium, Czech Republic, France, Germany, Italy, Netherlands, Poland, Portugal, Spain) |
| Cities | 10 major EU cities |
| Report Pages | 3 main pages + 1 drill-through page + 4 tooltip pages |
| DAX Measures | 30+ calculated measures |
| Calculated Columns | 5 (Bedroom Bucket, Building Decade, DOM Bucket, DOM Velocity Category, Price Band) |
| Parameters | 4 What-If parameters |
| Data Model | Star schema — 1 fact table, 5 dimension tables |

---

## 🖥️ Report Overview

### Page 1 — Market Overview & Geographic Analysis
> *"How are listings priced, distributed and absorbed across the EU — and what does the selected metric reveal at each level of time and property detail?"*

The entry page of the report. A **KPI parameter slicer** drives every visual on the page — selecting any of the 6 KPI cards (Total Listings, Avg Sale Price, Avg Price/m², Avg Days on Market, Price Appreciation, Avg Rent/Month) dynamically updates all charts to plot that metric.

<img width="1459" height="821" alt="Screenshot_10" src="https://github.com/user-attachments/assets/4ed4319f-dccd-4c2c-9014-1cc8421c7c83" />

**Key visuals:**
- **ZoomCharts timeline series** — Selected parameter trending by Year → Quarter → Month → Day with full drill hierarchy
- **Scatter plot** — Total Listings vs Avg Sale Price by Country (bubble size = DOM, cross-filter enabled)
- **Bar chart** — Selected parameter by furnishing status with DOM Velocity drill-down
- **Column chart** — Selected parameter by energy rating (A–G) with Bedroom Bucket drill-down
- **Donut chart** — Sale vs Rental listing type split with drill-through to property deep-dive page

**KPI reference labels** are dynamic single-sentence measures that update based on filter context, showing YoY changes and EU-relative benchmarks inline beneath each card value.

---

### Page 2 — Property Characteristics & Pricing Drivers
> *"What structural and physical features actually drive property prices — and where does the premium plateau?"*

The analytical engine of the report. Every visual answers a specific pricing question about property size, age, floor level, bedroom count, and amenity configuration.

<img width="1459" height="822" alt="Screenshot_15" src="https://github.com/user-attachments/assets/1a1d102a-99bb-4d33-aa90-c5c3dafaba96" />

**Key visuals:**
- **Clustered column + line combo** — Avg Sale Price and Total Listings by Building Decade with drill-down to year_built
- **Scatter plot** — Avg Sale Price vs Avg Square Metres by Country, legend: Property Type (with report page tooltip)
- **Bar chart** — Avg Sale Price by Bedroom Bucket (0–1 / 2 / 3 / 4 / 5+) showing plateau point
- **Scatter plot** — Avg Price/m² vs Avg Floor Number by Country with drill-down to Property Type
- **Waterfall chart** — Amenity price premium uplift: Base → Parking → Elevator → Gym → Pool → Premium Total

---

### Page 3 — Investor Intelligence & Market Opportunity
> *"Where does risk-adjusted opportunity lie — and which markets should investors prioritise?"*

The strategic layer of the report. Built specifically for property investors and fund analysts, this page surfaces yield signals, absorption velocity, stale listing risk, and opportunity scoring in a single ranked view.

<img width="1458" height="821" alt="Screenshot_9" src="https://github.com/user-attachments/assets/1103e5eb-8bf7-453e-969b-88bf2f587156" />

**Key visuals:**
- **Clustered column + line combo** — Price Appreciation % (bar) and Opportunity Score (line) by Country-City, sorted by score
- **Bar chart** — Total Listings and Avg Sale Price by DOM Velocity Category (Fast <30d / Medium 30–60d / Slow 60–90d / Stale >90d)
- **Matrix table** — 11 KPIs across all 10 cities: listings, price, price/m², DOM, building age, sqm, monthly rent, floor number, opportunity score, % parking, amenity index — with per-column conditional formatting and coloured opportunity score badges
- **Bar chart** — Avg Days on Market by Property Type (ranked, colour-coded by velocity)

---

### Drill-Through Page — Property Deep Dive
> *"Show me the full individual property profile — right-click any listing type to drill through."*

Triggered by right-clicking any visual containing `listing_type` and selecting Drill-Through. The page renders a property-level matrix showing all individual listings filtered to the selected type (Sale or Rental) with full column detail, amenity icons, and a report page tooltip on every row.

<img width="1460" height="821" alt="Screenshot_16" src="https://github.com/user-attachments/assets/9f71ec61-2ce7-4dcc-a8cf-2411d2b7090e" />

**Matrix columns:** Property ID · Property Type · Listing Date · Rent/Sales price · Property Size · Price/m² · Building Age · Days on Market · No of Floors · No of Bedrooms · No of Bathrooms · Amenity Index (with trend arrows)

**Amenity Index** column uses conditional formatting icons (↑ green / ↓ red / → neutral) based on index score relative to the page average.

---

## 🗃️ Data Model

The report uses a **star schema** with one central fact table and five dimension/reference tables, following Power BI modelling best practices with single-directional relationships and a dedicated isolated measure table.

```
                    ┌─────────────┐
                    │  Parameter  │  (What-If: Page KPI slicer)
                    └─────────────┘
                    ┌──────────────┐
                    │ KPI Parameter│  (What-If: KPI card selector)
                    └──────────────┘
┌─────────────┐     ┌──────────────────────────┐     ┌──────────────┐
│  DimDate    │────►│  FactEURealEstateDataset  │◄────│ DimLocation  │
│             │     │                          │     │              │
│ listing_date│     │ property_id              │     │ address      │
│ Day Name    │     │ address                  │     │ city         │
│ Month       │     │ bathrooms                │     │ country      │
│ Month Name  │     │ bedrooms                 │     │ Country_City │
│ Quarter     │     │ Bedroom Bucket (col)     │     │ latitude     │
│ Year        │     │ Building Decade (col)    │     │ longitude    │
└─────────────┘     │ Building Decade Category │     └──────────────┘
                    │ days_on_market           │
┌─────────────┐     │ DOM Bucket (col)         │     ┌──────────────┐
│DimPropImage │────►│ DOM Velocity Category    │     │Amenity Table │
│             │     │ elevator                 │     │              │
│ property_id │     │ energy_rating            │     │ Amenity      │
│Property_image│    │ floor_number             │     │ Sequence     │
│ property_type│    │ furnishing_status        │     └──────────────┘
└─────────────┘     │ gym                      │
                    │ last_sold_price_eur       │     ┌──────────────┐
                    │ listing_date             │     │    Summary   │
                    │ listing_type             │     │  (reference) │
                    │ monthly_rent_eur         │     └──────────────┘
                    │ parking_spots            │
                    │ price_per_sqm            │     ┌──────────────┐
                    │ property_id              │     │ Measure Table│
                    │ property_type            │     │  (isolated)  │
                    │ PropertyType ID          │     │ 30+ measures │
                    │ sale_price_eur           │     └──────────────┘
                    │ square_meters            │
                    │ swimming_pool            │
                    │ year_built               │
                    └──────────────────────────┘
```

### Relationship Summary

| From | To | Key | Cardinality | Direction | Active |
|------|----|-----|-------------|-----------|--------|
| DimDate | FactEURealEstateDataset | listing_date | 1:Many | → | ✅ Yes |
| DimLocation | FactEURealEstateDataset | city / country | 1:Many | → | ✅ Yes |
| DimPropImage | FactEURealEstateDataset | property_id | 1:Many | → | ✅ Yes |

> All measures live in the **isolated `Measure Table`** with no rows — following the single-table measure isolation pattern for clean Fields pane organisation.

---

## 📐 DAX Measures

All measures are organised into display folders within the Measure Table. Key measures are documented below.

### Core measures

```dax
Total Listings =
COUNTROWS('FactEURealEstateDataset')

Avg Sale Price EUR =
AVERAGEX(
    FILTER('FactEURealEstateDataset', [listing_type] = "Sale"),
    [sale_price_eur]
)

Avg Monthly Rent EUR =
AVERAGEX(
    FILTER('FactEURealEstateDataset', [listing_type] = "Rent"),
    [monthly_rent_eur]
)

Avg Price Per SQM =
AVERAGE('FactEURealEstateDataset'[price_per_sqm])

Avg Days on Market =
AVERAGE('FactEURealEstateDataset'[days_on_market])

Avg Building Age =
AVERAGEX('FactEURealEstateDataset', YEAR(TODAY()) - [year_built])
```

### Investor intelligence measures

```dax
Market Blended Yield =
VAR AvgRent =
    CALCULATE(
        AVERAGE('FactEURealEstateDataset'[monthly_rent_eur]),
        'FactEURealEstateDataset'[listing_type] = "Rent",
        NOT ISBLANK('FactEURealEstateDataset'[monthly_rent_eur])
    )
VAR AvgSalePrice =
    CALCULATE(
        AVERAGE('FactEURealEstateDataset'[sale_price_eur]),
        'FactEURealEstateDataset'[listing_type] = "Sale",
        NOT ISBLANK('FactEURealEstateDataset'[sale_price_eur])
    )
RETURN
    DIVIDE(AvgRent * 12, AvgSalePrice, BLANK())

-- ─────────────────────────────────────────────────────────────

Opportunity Score =
VAR YieldScore  = DIVIDE([Market Blended Yield], 0.10, 0) * 0.5
VAR DOMScore    = (1 - DIVIDE([Avg Days on Market], 180, 0)) * 0.3
VAR AmenityScore = [Amenity Index] * 0.2
RETURN
    DIVIDE(YieldScore + DOMScore + AmenityScore, 2.2, 0)

-- ─────────────────────────────────────────────────────────────

Price Appreciation % =
DIVIDE(
    [Avg Sale Price EUR] - [Avg Last Sold Price EUR],
    [Avg Last Sold Price EUR],
    BLANK()
)

-- ─────────────────────────────────────────────────────────────

High Value Slow Count =
COUNTROWS(
    FILTER(
        'FactEURealEstateDataset',
        [sale_price_eur] > [EU Avg Sale Price]
            && [days_on_market] > [EU Avg DOM]
    )
)
```

### Amenity measures

```dax
Amenity Index =
AVERAGEX(
    'FactEURealEstateDataset',
    (   IF([gym] = "Yes", 1, 0)
      + IF([swimming_pool] = "Yes", 1, 0)
      + IF([elevator] = "Yes", 1, 0)
      + IF([parking_spots] > 0, 1, 0)
    ) / 4
)

-- ─────────────────────────────────────────────────────────────

Amenity Waterfall Change =
-- Marginal € uplift per amenity vs. base (no amenities)
CALCULATE([Avg Sale Price EUR], [gym]="Yes")
    - CALCULATE([Avg Sale Price EUR], [gym]="No")
-- (replicated per feature: Parking, Elevator, Gym, Pool)
```

### Dynamic reference label measures (KPI card subtitles)

```dax
-- Example: Avg Sale Price reference label
Avg Sale Price Label =
VAR _val     = [Avg Sale Price EUR]
VAR _eu      = CALCULATE([Avg Sale Price EUR], ALL('FactEURealEstateDataset'))
VAR _delta   = DIVIDE(_val - _eu, _eu)
VAR _dir     = IF(_delta >= 0, "▲", "▼")
RETURN
    "Average prices show a YoY increase of " &
    FORMAT(_delta, "0.00%") & ", reflecting market value shifts"
```

### Tooltip title measure

```dax
Tooltip Property Insight =
VAR _propType =
    IF(
        HASONEVALUE(DimPropertyType[property_type]),
        SELECTEDVALUE(DimPropertyType[property_type]),
        CALCULATE(
            SELECTEDVALUE(DimPropertyType[property_type]),
            TOPN(1, VALUES(DimPropertyType[property_type]), [Total Listings], DESC)
        )
    )
VAR _country     = SELECTEDVALUE(DimLocation[country])
VAR _avgSize     = [Avg Square Meters]
VAR _priceSQM    = [Avg Price Per SQM]
VAR _countryBase =
    CALCULATE(
        [Avg Price Per SQM],
        REMOVEFILTERS(DimPropertyType[property_type])  -- country-level baseline
    )
VAR _diffPct     = DIVIDE(_priceSQM - _countryBase, _countryBase)
VAR _direction   = IF(_diffPct >= 0, "above", "below")
VAR _typeLabel   = IF(ISBLANK(_propType), "Properties", _propType & "s")
RETURN
    _typeLabel & " in " & _country &
    " average " & FORMAT(_avgSize, "#,##0") & "m² at €" &
    FORMAT(_priceSQM, "#,##0") & "/m² — " &
    _direction & " the " & _country & " all-type average by " &
    FORMAT(ABS(_diffPct), "0.0%")
```

> **Note on the tooltip measure:** `REMOVEFILTERS(DimPropertyType[property_type])` is used (not `DimLocation[country]`) to correctly compute the within-country baseline across all types — removing the country filter would return the EU-wide average, not the country-level benchmark.

---

## 💡 Calculated Columns

Five calculated columns are added directly to the fact table to enable categorical grouping across all visuals.

```dax
-- Bedroom Bucket
Bedroom Bucket =
SWITCH(TRUE(),
    'FactEURealEstateDataset'[bedrooms] <= 1, "0–1 bed",
    'FactEURealEstateDataset'[bedrooms] = 2,  "2 bed",
    'FactEURealEstateDataset'[bedrooms] = 3,  "3 bed",
    'FactEURealEstateDataset'[bedrooms] = 4,  "4 bed",
    "5+ bed"
)

-- Building Decade
Building Decade =
SWITCH(TRUE(),
    'FactEURealEstateDataset'[year_built] < 1960, "Pre-1960",
    'FactEURealEstateDataset'[year_built] < 1970, "1960s",
    'FactEURealEstateDataset'[year_built] < 1980, "1970s",
    'FactEURealEstateDataset'[year_built] < 1990, "1980s",
    'FactEURealEstateDataset'[year_built] < 2000, "1990s",
    'FactEURealEstateDataset'[year_built] < 2010, "2000s",
    'FactEURealEstateDataset'[year_built] < 2020, "2010s",
    "2020s"
)

-- DOM Velocity Category
DOM Velocity Category =
SWITCH(TRUE(),
    'FactEURealEstateDataset'[days_on_market] <= 30,  "Fast < 30D",
    'FactEURealEstateDataset'[days_on_market] <= 60,  "Medium 30D–60D",
    'FactEURealEstateDataset'[days_on_market] <= 90,  "Slow 60D–90D",
    "Stale > 90D"
)
```

---

## 🔍 Report Page Tooltips

Four dedicated tooltip pages (canvas size: 400×340px, Page type: Tooltip) surface contextual intelligence when hovering any visual data point. Each tooltip is bound to specific visuals via **Format → Tooltip → Type → Report Page**.

### TT\_Market\_Pulse — Market snapshot tooltip
**Bound to:** Country scatter plot (Page 1)

Displays a full country market profile on hover:
- City and country header with dynamic title
- 6 KPI mini-cards: Avg Sale Price · Avg Price/m² · Total Listings · Avg DOM · Price Appreciation % · Avg Property Size
- Total Listings by Property Type bar chart (filtered to hovered country)
- Dynamic insight sentence

*Screenshot: Netherlands – Amsterdam showing €1.65M avg price, €4.75K/m², 592 total listings, 183 days DOM, 58.99% price appreciation*

---

### TT\_Property\_DNA — Property type profile tooltip
**Bound to:** Size vs price scatter plot (Page 2)

Displays the country × property type breakdown on hover:
- Dynamic title: `[PropertyType]s in [Country] average [size]m² at €[price]/m² — X% above/below country avg`
- 8 metric cards: Avg Sale Price · Price/m² · Avg Bedrooms · Avg Bathrooms · Avg Floor · Building Age · DOM · Amenity Index
- Amenity premium bar chart (parking, elevator, gym, pool uplift)

*Key fix: Uses `HASONEVALUE()` + `TOPN()` fallback to ensure property type always resolves even when the legend field isn't propagated directly into tooltip filter context by Power BI.*

---

### TT\_Investor\_Edge — Investor brief tooltip
**Bound to:** Country-City bar chart and Matrix (Page 3)

Surfaces the full opportunity score decomposition on hover:
- Country-City dynamic title with opportunity tier badge
- Opportunity score breakdown: Yield score (50%) · Speed score (30%) · Amenity score (20%)
- 6 market signal cards: Avg DOM · Avg DOM Stale · High Value Slow count · Market Blended Yield · Price Appreciation · Opportunity Score
- Opportunity Score by DOM Velocity mini bar chart
- Top Opportunity Driver measure — identifies the primary score driver (yield / speed / amenity)

*Screenshot: Netherlands–Amsterdam scoring 21.04% — Yield score 20.11%, Speed score –0.57%, Amenity score 7.95%*

---

### TT\_Property\_Insight — Property-level deep dive tooltip
**Bound to:** Matrix rows on the drill-through page

Renders a full individual property card on row hover:
- Property title: `[Furnishing Status] [Property ID] in [Address]`
- Full narrative: property type · city · country · size · price/m² · days listed · opportunity score · investment recommendation
- Feature grid: Sale price · Price Appreciation · EPC Rating · Year Built · Elevator · Pool · Gym · Parking
- DOM velocity badge (Fast 30D–60D / Medium / Slow / Stale)

*Screenshot: PROP-00017 — Semi-Furnished Apartment, Spain–Madrid, 135m², €2,719/m², 33 days, Score 39.39%, "Monitor for pricing adjustment"*

---

## 🎛️ What-If Parameters

| Parameter | Range | Step | Powers |
|-----------|-------|------|--------|
| Page KPI Selector | 1–6 | 1 | Drives entire Page 1 visual suite |
| KPI Parameter | 1–6 | 1 | Drives KPI card selection |
| DOM Stale Threshold | 30–120 days | 15 | Redefines `[Avg DOM Stale]` and velocity buckets |
| Price Threshold | €100K–€2M | €50K | Powers budget scenario KPIs on Page 3 |

---

## 📊 Key Insights

| Finding | Detail |
|---------|--------|
| **Highest avg sale price** | France – Paris at **€1.71M** (€5,201/m²) |
| **Best market blended yield** | Poland – Warsaw at **4.17%** EU average |
| **Fastest market** | Germany – Berlin: **164 days** avg DOM |
| **Most stale market** | Netherlands – Amsterdam: **183 days** avg DOM |
| **Top appreciation** | Netherlands – Amsterdam at **58.99%** vs last sold |
| **Highest amenity index** | Spain – Madrid at **41.02%** |
| **Pool premium** | Adds the largest single-amenity uplift vs base price |
| **Bedroom plateau** | Price uplift peaks at the 2→3 bed transition; diminishes beyond 4 beds |
| **EPC paradox** | Band E carries the highest listing count (1,230) despite low efficiency rating |
| **Stale risk** | 2,297 of 5,000 listings are high-value slow — potential negotiation leverage |

---

## 🏗️ Setup & Installation

### Prerequisites

- Microsoft Power BI Desktop (June 2024 release or later)
- ZoomCharts Drill Down Visuals PRO (AppSource — free tier sufficient for timeline)
- Dataset: `EU_Real_Estate_Dataset.xlsx` (5,000 rows, 25 columns)

### Data source columns

```
property_id | listing_date | property_type | listing_type | address | city |
country | bedrooms | bathrooms | square_meters | year_built | sale_price_eur |
monthly_rent_eur | price_per_sqm | days_on_market | latitude | longitude |
last_sold_price_eur | parking_spots | gym | swimming_pool | elevator |
furnishing_status | energy_rating | floor_number
```

### Installation steps

```bash
# 1. Clone this repository
git clone https://github.com/yourusername/eu-real-estate-analytics.git
cd eu-real-estate-analytics

# 2. Open the report
# Double-click: Eu_real_estate_market_analytics.pbix

# 3. Update the data source path
# Home → Transform Data → Data Source Settings
# Update file path to your local copy of EU_Real_Estate_Dataset.xlsx

# 4. Refresh data
# Home → Refresh

# 5. Verify relationships
# Model view → confirm all 4 relationships are active
```

### Folder structure

```
eu-real-estate-analytics/
├── Eu_real_estate_market_analytics.pbix   # Main Power BI report file
├── data/
│   └── EU_Real_Estate_Dataset.xlsx        # Source dataset (5,000 rows)
├── assets/
│   ├── page1_overview.png                 # Report screenshots
│   ├── page2_drivers.png
│   ├── page3_intelligence.png
│   ├── drillthrough_deepdive.png
│   └── data_model.png
├── docs/
│   ├── dax_measures.md                    # Full measure documentation
│   ├── data_dictionary.md                 # Column definitions
│   └── tooltip_specs.md                   # Tooltip page configurations
└── README.md
```

---

## 🧰 Technical Stack

| Tool | Version | Purpose |
|------|---------|---------|
| Power BI Desktop | June 2024+ | Report authoring |
| DAX | — | Measures, calculated columns, parameters |
| Power Query (M) | — | Data transformation and type enforcement |
| ZoomCharts Drill Down Visuals | Free tier | Timeline series on Page 1 |
| Azure Maps (native) | Built-in | Geographic scatter visualisation |
| PptxGenJS | 4.0.1 | Report blueprint slide deck generation |

---

## 🎨 Design System

The report uses a consistent dark navy design language across all 3 pages and 4 tooltip pages:

| Element | Value |
|---------|-------|
| Background | `#0A1628` (deep navy) |
| Card surface | `#142548` |
| Page 1 accent | `#C9A84C` (gold) |
| Page 2 accent | `#0D9488` (teal) |
| Page 3 accent | `#7C3AED` (purple) |
| Fast DOM | `#16A34A` (green) |
| Medium DOM | `#B45309` (amber) |
| Stale DOM | `#DC2626` (red) |
| Primary font | Calibri / Trebuchet MS |
| Border | `#1E3A6E` |

---

## 📁 Report Pages Summary

| Page | Type | Accent | Slicers | Visuals | Tooltip |
|------|------|--------|---------|---------|---------|
| Page 1: Overview | Main | Gold | 5 | 5 + 6 KPIs | TT_Market_Pulse |
| Page 2: Drivers | Main | Teal | 5 | 5 + 6 KPIs | TT_Property_DNA |
| Page 3: Intelligence | Main | Purple | 5 | 4 + 6 KPIs | TT_Investor_Edge |
| Drill-Through: Deep Dive | Drill | Coral | — | Matrix + KPIs | TT_Property_Insight |
| TT_Market_Pulse | Tooltip | Navy | — | 6 KPIs + bar | — |
| TT_Property_DNA | Tooltip | Navy | — | 8 KPIs + bar | — |
| TT_Investor_Edge | Tooltip | Navy | — | Score decomp + bar | — |
| TT_Property_Insight | Tooltip | Navy | — | Property card | — |

---

## 🤝 Contributing

Contributions and suggestions are welcome. If you spot a measure that could be optimised, a visual that could be improved, or a new analytical angle worth exploring:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/measure-optimisation`
3. Commit your changes: `git commit -m 'Improve opportunity score weighting'`
4. Push to the branch: `git push origin feature/measure-optimisation`
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**[Your Name]**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/yourprofile)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/yourusername)
[![Portfolio](https://img.shields.io/badge/Portfolio-FF5722?style=for-the-badge&logo=google-chrome&logoColor=white)](https://yourportfolio.com)

---

<div align="center">

*Built with precision. Designed for decisions.*

⭐ If this report helped you, please consider starring the repository.

</div>
