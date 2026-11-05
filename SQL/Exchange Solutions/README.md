# Exchange Solutions: Retail SQL Analysis

## Project Overview

This project uses a retail order database to answer business questions with SQL Server. The formal brief in [`Project Question/Exchange Solutions.pdf`](Project%20Question/Exchange%20Solutions.pdf) asks for state-level order volume and the five highest-selling product categories in each city with more than 100 consumers. The SQL work shown in the evidence also explores product pricing, customer activity, order value, payment status, and sales rankings.

The goal is to turn related transaction tables into useful summaries while making the meaning and grain of each metric clear. The screenshots are query-and-result evidence from SQL Server Management Studio; they are not a substitute for a saved, rerunnable `.sql` script.

## Questions and Findings

### 1. Which five states generated the most orders?

The saved query groups customers by state and sums `order_item_quantity`. That measures units ordered by state


### 2. What are the top five categories by sales in cities with more than 100 consumers?

This analysis first identifies qualifying cities, totals `order_item_subtotal` by city and category, then ranks each city's categories by sales. 

The saved screenshot shows 40 result rows, arranged as five categories for each of eight qualifying cities. Examples visible in the results include Fishing as the top category in Bronx, Brooklyn, Caguas, and Chicago. The screenshot's Caguas Fishing sales total is approximately 2,563,871.80.

## Data and Relationships

The `data/` folder contains these tables as CSV files:

| Table | Role | Key fields used in the analysis |
| --- | --- | --- |
| `customers` | Customer attributes and location | `customer_id`, `customer_city`, `customer_state` |
| `orders` | Order header, date, and status | `order_id`, `order_customer_id`, `order_date`, `order_status` |
| `order_items` | Product lines within each order | `order_item_order_id`, `order_item_product_id`, `order_item_quantity`, `order_item_subtotal`, `order_item_product_price` |
| `products` | Product names, categories, and prices | `product_id`, `product_category_id`, `product_name`, `product_price` |
| `categories` | Category names and department IDs | `category_id`, `category_department_id`, `category_name` |
| `departments` | Department names | `department_id`, `department_name` |

The main join path is `customers -> orders -> order_items -> products -> categories -> departments`. A customer can have many orders; an order can have many line items. Consequently, joining order headers to order items changes the result grain from one row per order to one row per item.


## Approach

1. Inspect the provided files and identify the table keys and relationships.
2. Load the tables into SQL Server and qualify table names with the `dbo` schema.
3. Join tables along their key relationships to bring together customer, order, product, and category context.
4. Choose the correct metric for each question: count orders, sum units, or sum line-item sales; these are not interchangeable.
5. Aggregate at the requested grain, filter groups with `HAVING`, and use ranking/window functions for per-city or per-category comparisons.
6. Review both query text and result grids. Confirm that the row count and grain match the business question before interpreting the output.

The screenshots show this progression from straightforward filtering and joins through grouped analysis, common table expressions, and window functions.

## Additional SQL Exercises in the Evidence

The screenshots document these additional queries and their demonstrated techniques:

| Analysis | SQL approach and result shown | Interpretation note |
| --- | --- | --- |
| Products ordered by price | Join `order_items` to `products`, then sort `order_item_product_price` descending. | Returns transaction lines, so the same product can appear repeatedly. Use the `products` table for a distinct product catalog price list. |
| Product names containing “Football” | `WHERE product_name LIKE '%Football%'`; 75 rows shown. | `%` matches any sequence before or after the search term. |
| Categories and departments | Inner join `categories` to `departments`; 48 rows shown. | Displays category-to-department mapping; an inner join excludes unmatched departments. |
| Customer purchase quantity | Group joined customer/order-item rows and use `HAVING SUM(order_item_quantity) > 1`; 5,721 rows shown. | This is units purchased, not a count of orders. Grouping by name can combine different customers with the same name; group by customer ID for customer-level results. |
| Order ID, date, and customer | Select order fields with customer names after joining to line items; 172,198 rows shown. | Repeated order IDs are expected because the join produces one row per item. Join `orders` directly to `customers` for one row per order. |
| Subtotal per order | Sum `order_item_subtotal` and group by order ID; 57,431 rows shown. | A useful order-value measure, but it is a subtotal rather than necessarily the final amount paid. |
| Pending payment orders | Count orders where `order_status = 'PENDING_PAYMENT'`; result: 15,030. | A point-in-time operational count from the supplied data. |
| Products above average price | Compare each product price with a scalar `AVG(product_price)` subquery; 442 rows shown. | Identifies above-average-priced products, not necessarily best-selling or most profitable products. |
| Departments with categories | Left join departments to categories; 48 rows shown. | A left join preserves departments even when there is no matching category. |
| Cumulative sales | Aggregate line subtotals by order and date, then apply `SUM(...) OVER (ORDER BY ...)` with an unbounded preceding frame; 57,431 rows shown. | The window frame defines a running total over the ordered rows. Add a stable tie-breaker to the ordering when dates can tie. |
| Most expensive products per category | `ROW_NUMBER() OVER (PARTITION BY category ORDER BY product_price DESC)`; the displayed filter uses `<= 5` and returns 255 rows. | The screenshot comment requests the top three but the filter returns up to five per category. Change the filter to `<= 3` to meet that request. |
| Current and previous customer order subtotal | Aggregate each customer's order subtotal, then use `LAG` partitioned by customer and ordered by date/order ID. | Enables comparison with the customer's preceding order; the first order per customer correctly has no prior value (`NULL`). |
| Top spending customers per city | Sum item subtotals per customer and city, then use `DENSE_RANK` partitioned by city; 2,736 rows shown. | `DENSE_RANK` includes ties, so a city can have more than five returned customers. Use `ROW_NUMBER` for exactly five. |
| Spending quartiles by state | Sum customer spend and apply `NTILE(4)` partitioned by state; 12,331 rows shown. | The quartiles are relative within each state. Ordering spend ascending labels the lowest spenders as quartile 1. |

## Skills Demonstrated

- **Relational joins:** combine normalized entities using primary/foreign-key relationships. This is foundational for reconciling data across sales, customer, product, and finance systems.
- **Filtering and pattern matching:** use `WHERE`, `LIKE`, and status predicates to select relevant records, a common requirement for operational reports.
- **Aggregation:** use `COUNT`, `SUM`, and `AVG` with `GROUP BY` and `HAVING` to convert row-level records into business metrics.
- **Common table expressions (CTEs):** break multi-stage analysis into readable steps, which helps maintain and audit complex reporting queries.
- **Window functions:** use `ROW_NUMBER`, `DENSE_RANK`, `NTILE`, `LAG`, and cumulative `SUM` to compare records within groups without losing row-level context. These patterns support leaderboards, segmentation, period-over-period analysis, and cumulative KPIs.
- **Grain and metric selection:** distinguish an order from an order line, an order count from units sold, and sales revenue from profit. Getting this right is essential because a syntactically valid query can still answer the wrong business question.

## Business Inferences and Follow-up

- The saved state ranking is heavily led by Puerto Rico (PR) on ordered-unit volume, with California (CA) second. This suggests a strong concentration of recorded demand, but the result needs validation using distinct order counts before making an order-volume claim.
- Category sales vary by city, making city-level rankings useful for campaign assortment and localized merchandising. High sales can reflect customer count and order volume, so compare per-customer sales and category mix before treating a city as having a stronger preference.
- Pending-payment volume (15,030 orders in the screenshot) can guide payment-funnel monitoring. To assess risk, compare it with all orders and examine how long orders remain pending; this dataset snapshot alone does not establish a failure rate.
- The price, subtotal, and customer-spend analyses can inform product tiers and customer segmentation. They do not measure profitability or lifetime value because cost, returns, discounts, and a defined customer tenure are not included in the fields used here.
- The next useful analyses would compare order count, units, and sales side by side; normalize category sales by active customers; check status/date trends; and confirm data completeness and duplicate handling before using the findings for planning.

## Evidence

All screenshots supplied in `images/` are embedded below. They show the SQL Server query text and the corresponding result grids.

<details>
<summary>State and city category rankings</summary>

**Top states by ordered quantity**

<img src="images/Screenshot%202026-09-16%20165729.png" alt="SQL query and result for top states by ordered quantity" width="100%">

**Top categories by city sales**

<img src="images/Screenshot%202026-09-16%20165810.png" alt="SQL query and result for top categories by sales in qualifying cities" width="100%">

</details>

<details>
<summary>Product, category, and customer queries</summary>

**Products sorted by ordered product price**

<img src="images/Screenshot%202026-09-16%20170416.png" alt="Product names and ordered product prices sorted descending" width="100%">

**Products matching Football**

<img src="images/Screenshot%202026-09-16%20170459.png" alt="Products filtered by a Football name match" width="100%">

**Categories and departments**

<img src="images/Screenshot%202026-09-16%20170559.png" alt="Category and department inner join results" width="100%">

**Customer quantities over one**

<img src="images/Screenshot%202026-09-16%20170709.png" alt="Customer purchase quantity query and results" width="100%">

**Customer quantity results, reformatted**

<img src="images/Screenshot%202026-09-16%20170814.png" alt="Customer purchase quantity results with formatted customer names" width="100%">

**Order details with customer names**

<img src="images/Screenshot%202026-09-16%20170913.png" alt="Order IDs, dates, and associated customer names" width="100%">

</details>

<details>
<summary>Order totals, status, and product pricing</summary>

**Subtotal by order**

<img src="images/Screenshot%202026-09-16%20171003.png" alt="Subtotal aggregated by order ID" width="100%">

**Pending-payment order count**

<img src="images/Screenshot%202026-09-16%20171036.png" alt="Count of pending-payment orders" width="100%">

**Products priced above the average**

<img src="images/Screenshot%202026-09-16%20171110.png" alt="Products with prices above the average product price" width="100%">

**Departments including categories**

<img src="images/Screenshot%202026-09-16%20171245.png" alt="Department-to-category left join results" width="100%">

**Cumulative sales by order date**

<img src="images/Screenshot%202026-09-16%20171310.png" alt="Running cumulative sales query and results" width="100%">

**Top-priced products within categories**

<img src="images/Screenshot%202026-09-16%20171331.png" alt="Product ranking by price within each category" width="100%">

</details>

<details>
<summary>Customer comparisons and segmentation</summary>

**Compare a customer's order with the previous order**

<img src="images/Screenshot%202026-09-16%20173056.png" alt="Customer order subtotal compared with the previous order" width="100%">

**Top spending customers in each city**

<img src="images/Screenshot%202026-09-16%20173303.png" alt="Top spending customers ranked within each city" width="100%">

**Customer spending quartiles by state**

<img src="images/Screenshot%202026-09-16%20173340.png" alt="Customer spend grouped into quartiles within each state" width="100%">

</details>