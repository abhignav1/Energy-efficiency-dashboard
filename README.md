# Electricity Sales & Utility Reporting Analytics | Power BI

### From Excel-Based Reporting to Interactive Business Intelligence

**Technologies:** Power BI | Power Query | DAX | Excel | Data Modeling | ETL | Data Quality | Business Intelligence

**Reporting Period:** 2023–2025  
**Project Type:** Energy and Utilities Analytics Portfolio Project  
**Industry:** Energy & Utilities

---

## 1. Project Overview

This project demonstrates how traditional electricity and utility reporting can be transformed into an interactive business intelligence solution using Microsoft Power BI.

The inspiration for this project came from my experience as an **Energy Efficiency Business Intelligence Intern at Unitil Corporation**, where I worked with operational and energy-related data, Excel reporting, KPI dashboards, and data quality investigations.

Through this experience, I developed a deeper understanding of the challenges involved in preparing reliable reports when information comes from different source systems and requires validation, transformation, and reconciliation.

I developed this independent portfolio project using publicly available electricity reporting data to demonstrate how historical reporting can be standardized and analyzed through a centralized Power BI data model.

The current project analyzes three reporting years—2023, 2024, and 2025—and explores electricity sales, revenue, customer trends, utility performance, and regional variations.

## 2. Business Problem

In energy and utility organizations, operational and reporting data may originate from multiple systems.

Different data sources can produce challenges such as:

- Inconsistent reporting formats across years and systems.
- Repetitive Excel-based data preparation.
- Difficulties comparing historical business performance.
- Missing, invalid, or incorrectly formatted data.
- Limited visibility into trends across utilities and geographic regions.
- Time-consuming troubleshooting when reported values don't match source records.
- Dependence on manual updates when source data changes.

These challenges can affect reporting efficiency, consistency, and confidence in business decisions.

**The objective of this project is to explore how structured data preparation and centralized Power BI reporting can improve accessibility, consistency, and analytical visibility.**

## 3. Connection to My Unitil Experience

During my Energy Efficiency Business Intelligence internship at Unitil Corporation, I worked with SQL, Excel, Power BI, operational data, and reporting activities.

My responsibilities included supporting KPI dashboards, analyzing energy consumption and operational information, improving reporting workflows, and investigating data issues.

My resume documents a **40% improvement in reporting efficiency** through dashboard development and reporting automation activities.

That experience helped me recognize the importance of reliable upstream data and the potential benefits of moving from disconnected reporting files toward standardized, repeatable analytical processes.

This portfolio project builds on those lessons by independently exploring multi-year electricity data consolidation, dimensional modeling, and interactive reporting.

**This is an independent demonstration, not a reproduction of Unitil's internal reporting systems or proprietary datasets.**

## 4. Data Sources and Reporting Scope

The project uses electricity reporting spreadsheets organized by year.

Reporting years:

- 2023
- 2024
- 2025

The dataset includes fields such as:

- Utility Number and Utility Name
- State
- Data Year
- Service Type
- Residential, Commercial, Industrial, and Transportation Sales
- Total Electricity Sales (MWh)
- Total Revenue (Thousand Dollars)
- Total Customer Count

These fields support annual comparisons, state-level reporting, and utility-level performance analysis.

The analysis is based on the available source records. Historical data completeness, reporting-category definitions, and comparability must be validated before the results are interpreted as official national totals.

## 5. My Approach to Solving the Problem

### Phase 1 — Data Ingestion and Consolidation

I imported electricity reporting spreadsheets for three different years into Power BI.

Using Power Query, I combined the datasets into a unified reporting table.

The consolidation process involved examining source structures, aligning column names, checking data types, and validating reporting-year information.

**Outcome:** A consolidated multi-year dataset suitable for comparative analysis.

### Phase 2 — Data Cleaning and Troubleshooting

One of the key technical challenges was encountering the Power Query error:

`DataFormat.Error: We couldn't convert to Number.`

Rather than modifying the DAX calculations to work around the problem, I investigated the upstream transformation process.

I reviewed the Applied Steps, identified the stage where numeric conversion failed, isolated problematic rows, and traced the issue to nonnumeric content in the source data.

After confirming that an invalid text footer row was present, I removed it and reapplied the appropriate data type.

This reinforced an important lesson:

**Reliable dashboards require understanding and resolving data quality problems at their source, not simply hiding errors in the final visualization.**

### Phase 3 — Dimensional Data Modeling

I implemented a star-schema-style analytical model in Power BI.

**Fact Table:**

`FactSales` — Contains electricity sales, revenue, customer counts, reporting years, and related identifiers.

**Dimension Tables:**

- `DimYear` — Reporting years.
- `DimState` — State identifiers.
- `DimUtility` — Utility identifiers and names.

I created one-to-many relationships with single-direction filtering to support consistent analysis across multiple visuals.

This structure improves model organization and allows measures to respond to report filters.

## DAX Measures & Business Logic

To support accurate business reporting and historical performance comparisons, I developed reusable DAX measures within the Power BI semantic model.

These measures dynamically respond to Year, State, and Utility filters, allowing users to analyze electricity performance at different levels.

### 1. Total Electricity Sales (MWh)

```dax
Total Sales MWh =
SUM(FactSales[Total Sales Megawatthours])
```

**Business Purpose:** Calculates total reported electricity sales for the selected reporting period, state, or utility.

### 2. Total Revenue

```dax
Total Revenue ($) =
SUM(FactSales[Total Revenue Thousand Dollars]) * 1000
```

**Business Purpose:** Converts reported revenue from thousands of dollars into actual dollar values, enabling consistent financial analysis.

### 3. Total Customers

```dax
Total Customers =
SUM(FactSales[Total Customer Count])
```

**Business Purpose:** Calculates the reported customer count for the selected reporting context, subject to validation of overlapping service records.

### 4. Previous Year Sales

```dax
Previous Year Sales =
VAR CurrentYear = SELECTEDVALUE(DimYear[Year])
RETURN
IF(
    NOT ISBLANK(CurrentYear),
    CALCULATE(
        [Total Sales MWh],
        REMOVEFILTERS(DimYear),
        DimYear[Year] = CurrentYear - 1
    )
)
```

**Business Purpose:** Retrieves electricity sales from the previous reporting year while preserving applicable State and Utility filters.

### 5. Year-over-Year Sales Growth

```dax
Sales Growth % =
DIVIDE(
    [Total Sales MWh] - [Previous Year Sales],
    [Previous Year Sales]
)
```

**Business Purpose:** Measures the percentage increase or decrease in electricity sales compared with the preceding year.

This helps identify increasing or declining demand across utilities and geographic regions.

### 6. Revenue per Customer

```dax
Revenue Per Customer =
DIVIDE(
    [Total Revenue ($)],
    [Total Customers]
)
```

**Business Purpose:** Measures reported revenue relative to customer count, providing an additional perspective on customer-level revenue performance.

### 7. Revenue per MWh

```dax
Revenue Per MWh =
DIVIDE(
    [Total Revenue ($)],
    [Total Sales MWh]
)
```

**Business Purpose:** Calculates realized revenue per megawatt-hour to support comparisons across utilities and reporting years.

### Data Validation Considerations

Before interpreting these metrics, the reporting data must be reviewed for duplicate totals, overlapping service classifications, inconsistent units, and missing values.

Year-over-year calculations require a single year in the evaluation context. Customer-based ratios also require consistent customer definitions and reporting coverage.

### Key Technical Takeaway

Developing these measures helped me strengthen my understanding of DAX filter context, year-over-year comparisons, data modeling, and the importance of consistent business definitions.

Rather than creating isolated calculations for each visual, I structured the measures for reuse across executive dashboards, state comparisons, and utility-level reporting.

### Phase 5 — Interactive Dashboard Development

I designed an executive dashboard to present electricity reporting information in a more accessible format.

Key dashboard components include:

**Executive KPI Reporting**

Displays electricity sales, revenue, customer totals, and annual growth measures.

**Electricity Sales Trend Analysis**

Compares historical electricity sales across reporting years.

**State-Level Sales Performance**

Ranks states by reported electricity sales and allows geographic filtering.

**Annual Revenue Distribution**

Provides an overview of revenue contribution across reporting periods.

**Revenue per Customer**

Examines the relationship between reported revenue and customer counts.

**Interactive Filtering**

Allows users to explore the data by year, state, utility, and service type.

## 6. Power BI Data Architecture

The analytical workflow follows this structure:

`Annual Excel Files → Power Query → Data Cleaning & Standardization → FactSales → Dimension Tables → DAX Measures → Interactive Dashboards`

The data model separates business metrics from filtering attributes and supports flexible reporting without repeatedly combining spreadsheets for individual analyses.

## 7. Reporting Automation and Future Enhancements

The current implementation demonstrates historical Excel data consolidation and interactive Power BI reporting.

A potential next stage would be connecting supported source systems directly to Power BI through databases, APIs, controlled file repositories, or appropriate data pipelines.

Such an extension could include:

- Scheduled dataset refreshes.
- Incremental data processing where supported.
- Standardized source-to-target mappings.
- Automated data validation and quality checks.
- Refresh monitoring and error notifications.
- Historical reporting reconciliation.
- Improved reporting access for business stakeholders.

These are **proposed enhancements**, not capabilities currently demonstrated as completed production integrations.

For live or frequently updated sources, refresh frequency would depend on source connectivity, organizational access, security requirements, and Power BI architecture.

## 8. Business Value and Analytical Outcomes

This project demonstrates how a structured BI workflow can support:

**Reporting consistency:** Combining annual reporting files under a common analytical structure.

**Reduced manual preparation:** Reusing Power Query transformations instead of repeatedly building reports from separate files.

**Data quality awareness:** Identifying conversion problems and invalid source records before business calculations are finalized.

**Historical visibility:** Comparing electricity sales, revenue, and customer metrics across multiple reporting years.

**Self-service analysis:** Allowing users to filter results by geographic region and utility.

**Scalability planning:** Establishing a foundation that could support additional data sources and automated refresh workflows.

The project does not claim a measured production efficiency improvement; the purpose is to demonstrate analytical design and a repeatable approach to solving reporting problems.

## 9. Technical Skills Demonstrated

| Area | Skills |
|---|---|
| Data Preparation | Excel, Power Query, cleaning, transformations |
| Data Integration | Multi-file consolidation, schema alignment |
| Data Modeling | Star schema, fact tables, dimensions, relationships |
| Analytical Calculations | DAX, aggregations, YoY variance, ratios |
| Reporting | Power BI, KPI dashboards, interactive filters |
| Data Quality | Error investigation, type validation, source review |
| Business Analysis | Performance comparison, stakeholder-focused reporting |
| Solution Design | Reporting modernization, refresh architecture concepts |

## 10. Key Lessons and Problem-Solving Approach

The biggest lesson from this project is that effective business intelligence starts before visualization.

Working through reporting challenges helped reinforce my approach:

1. Understand the business meaning of the data.
2. Inspect source structures and data quality.
3. Identify the root cause of errors.
4. Correct transformation logic before calculating business metrics.
5. Organize the model for maintainable reporting.
6. Validate calculations against source values.
7. Present information in a form that supports business decisions.

I see Power BI not simply as a dashboard tool, but as part of a larger process of transforming operational data into useful, trusted information.

## 11. Project Scope and Limitations

This project is an independent portfolio demonstration using publicly available electricity data.

It is inspired by reporting challenges encountered during my Unitil experience, but does not contain Unitil's confidential operational information.

The dashboard should not be treated as a validated reproduction of official Unitil performance or internal reporting.

Production deployment, automated live-source connectivity, and scheduled refresh monitoring are potential future enhancements.

---

**Tools:** Microsoft Power BI, DAX, Power Query, Microsoft Excel

**Project Focus:** Energy Analytics | Business Intelligence | Data Quality | Reporting Modernization

**Author:** Abhigna Valambatla
