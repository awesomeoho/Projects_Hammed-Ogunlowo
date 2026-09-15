# St Louis Online Computer Devices & Accessories Dashboard w/ Power BI
![Dashboard Page 1](/St%20Louis%20PowerBI%20project/images/Powerbi_visualization.png)
📊 View interactive dashboard here on the Power BI Service

## Introduction
This dashboard was created for Cruz Adams, Marketing Manager at St. Louis Online Computer Devices & Accessories, to solve a critical business question: **do customers shop the site on mobile devices more than desktops at specific times of the day?** Using e-commerce performance data categorized by day periods, this project provides a clear visual interface to analyze device preferences across time segments and optimize marketing campaigns accordingly.

## Dashboard File
You can find the file for the dashboard here: [St_Louis_Sales_Dashboard](/St%20Louis%20PowerBI%20project/reports/St%20Loius.pbix)

## Skills Showcased
This project was a journey through key Power BI features. Here's a look at what we mastered:

**⚙️ Data Transformation (ETL) with Power Query:** Cleaned and structured sales metrics, standardizing device categories (Desktop, Mobile, Tablet) and time segments (Morning, Afternoon, Evening, Night, Midnight).

**🧮 DAX & Calculated Measures:** Created custom aggregation measures to sum revenue streams across device types and day periods (Sum of Desktop Sales, Sum of Mobile Sales, Sum of Tablet Sales).

**📊 Core & Combo Charts:** Built Stacked Column Charts, Pie/Donut Charts, and Multi-Line Trend Charts to contrast sales performance by period and platform.

**🔢 KPI Indicators & Data Grids:** Configured Card Visuals for high-level revenue KPIs and structured Matrix/Table Visuals to present granular hourly breakdowns.

**🎨 User-Centric Dashboard Design:** Formatted visual hierarchy, device color-coding, and custom contrast highlights to draw immediate attention to key revenue periods.

## 🖱️ Interactive Reporting:

**Slicers:** Implemented DayPeriod top-level dropdown filtering.

**Buttons:** Built a single-click "Clear Selections" reset button.

**Cross-Filtering:** Enabled interactive chart cross-highlighting across device breakdowns.

## Dashboard Overview
This report provides both a high-level revenue summary and explicit device-by-time analyses to test marketing hypotheses.

## Visualizing the Hypothesis
To prove or disprove Cruz's hypothesis, the dashboard breaks down device performance across five core time periods:

**KPI Summary Cards:** Highlights overall site revenue ($1.9M total) alongside device totals ($1.3M Desktop, $487.3K Mobile, $132.3K Tablet), immediately showing Desktop dominance overall.

**Category Sales Bar Chart:** A stacked column visual comparing sales volumes across Morning, Night, Afternoon, Evening, and Midnight to isolate high-traffic windows.

**Sales Category Trend Chart:** A multi-line chart tracking Desktop, Mobile, and Tablet trajectories side-by-side throughout the day. It reveals that while Desktop leads in overall dollar volume, Mobile maintains distinct peak activity windows relative to its baseline in the Morning and Night periods.

**Granular Matrix Table:** Provides exact numerical figures per time slot ($163,000 Mobile sales in the Morning vs. $134,500 at Night) to support precise ad-budget reallocations.

## Conclusion
This dashboard disproves the idea that Mobile surpasses Desktop in absolute dollar value at any point, as Desktop leads in every period. However, it successfully validates that Mobile sales peak during the Morning ($163K) and Night ($134.5K) periods, giving Cruz the exact data needed to schedule targeted mobile marketing programs during these peak engagement windows.