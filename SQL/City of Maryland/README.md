# City of Maryland SQL Project

## Overview

This SQL Server project explores bicycle and parts purchasing data. It combines transaction, product and category information to answer questions about products purchased, supplier discounts, geographic sales patterns and net purchase value. The project brief and its questions are in [`New folder/City  of Maryland.pdf`](New%20folder/City%20%20of%20Maryland.pdf).

The queries are demonstrated in the screenshots below.

## Analysis and Results

1. **Join purchase and product data**: combines purchase transactions with product details to return product names and numbers alongside order, account, supplier, quantity and price fields, using different types of JOIN.

2. **Rank products by country**: uses a common table expression `(CTE)` and `ROW_NUMBER()` partitioned by country to rank products, with category, quantity and net purchase amount in the output.

3. **Find discounted purchases by category**: joins product and category details, filters to purchases with a non-zero discount and calculates the discount amount.

4. **Summarize purchases by geography and product**: groups purchase amounts across country, state/province, city, category, class and product using `ROLLUP`, supporting hierarchical summaries.

5. **Calculate net purchase KPIs**: derives purchase amount, discount amount and net purchase amount, then assigns a Low, Medium, or High label with `CASE`.

## Skills Demonstrated

| Skill | Why it matters | When an SQL developer uses it |
| --- | --- | --- |
| Multi-table joins | Relational data is usually split across normalized tables; joins bring related facts and descriptive attributes together without duplicating source data. | Building reports, dashboards, extracts and analysis datasets from transaction and lookup tables. |
| Aggregation and grouping | `SUM`, `GROUP BY`, and `ORDER BY` turn detailed rows into useful totals and comparisons. | Revenue or purchasing summaries, category comparisons and operational metrics. |
| Window functions | `ROW_NUMBER()` ranks rows within a partition while retaining row-level detail. | Top-N reports, deduplication, sequencing events and comparing records within a group. |
| Common table expressions (CTEs) | A CTE gives a complex query a named, readable intermediate result. | Organizing multi-step transformations and making ranking or filtering logic easier to maintain. |
| Conditional logic and null handling | `CASE` creates business categories; `ISNULL` makes missing grouping values readable. | KPI bands, status labels, fallback values and presentation-ready rollups. |
| Calculated measures and numeric rounding | Arithmetic expressions derive discounts and net amounts from transaction fields; rounding makes monetary output easier to interpret. | Invoice calculations, margin analysis, discounts and financial reporting. |
| Filtering | Predicates such as non-zero discount checks reduce results to records relevant to a question. | Applying date ranges, business rules, category selections and quality checks. |
| Hierarchical rollups | `GROUP BY ROLLUP` produces subtotals across multiple levels of a hierarchy in one query. | Drill-down reporting by region, product family, department or other dimensions. |

Together, these techniques help SQL developers turn normalized operational data into reliable, understandable outputs that business users can filter, compare and use to make decisions.

## Screenshots

### Join transaction and product tables

![SQL joins returning purchase transaction and product details](images/Screenshot%202026-09-16%20175900.png)

### Rank products by country

![Country-level product ranking with category and net purchase amount](images/Screenshot%202026-09-16%20175939.png)

### Identify discounted purchases

![Supplier discount amounts by product and category](images/Screenshot%202026-09-16%20180025.png)

### Roll up purchases by geography and product

![Purchase amount grouped by geographic and product dimensions](images/Screenshot%202026-09-16%20180107.png)

### Derive purchase amount and KPI band

![Purchase, discount, net purchase amount, and KPI classification](images/Screenshot%202026-09-16%20180139.png)

## Project Contents

- `data/`: five CSV files containing query result exports.
- `images/`: SQL Server Management Studio screenshots showing the queries and their results.
- `New folder/`: the project brief PDF with the data model and analysis questions.

## Running the Queries

The screenshots show the work in SQL Server Management Studio against a database containing `PurchaseTrans`, `Product`, `Product_Category`, and `Category` tables. To reproduce the analyses, load the corresponding source tables into SQL Server and run the query logic shown in the screenshots. The CSV files are result exports, not database setup scripts or executable SQL.