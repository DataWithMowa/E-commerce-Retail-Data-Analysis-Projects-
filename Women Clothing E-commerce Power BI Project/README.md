# Women Clothing E-commerce Review Dashboard

<img width="943" height="465" alt="Dashboard" src="https://github.com/user-attachments/assets/9772e2e2-306e-4fe6-a422-0c331ffa4461" />

### 📊 Project Overview

This project analyzes 23,486 customer reviews from a women's clothing e-commerce platform, covering ratings, recommendations, and feedback across 4 product divisions, 7 departments, and 21 product classes.

### The Approach

Using Power BI, I cleaned the raw review data, built a set of DAX measures around rating, recommendation, and customer feedback, and created an interactive dashboard to explore how satisfaction differs by product category and customer age group.

### 🕹️ Interactive Dashboard Demo

See the dashboard filters, dynamic metrics, and charts in action below (60-second walkthrough): 

https://github.com/user-attachments/assets/8b955902-e36e-412c-8ba5-9c6af328e7dd

### To see the full live analysis: [Click here](https://lnkd.in/p/eeC4Rews)

### 🔍 Key Insights & Discoveries

* **Overall satisfaction is strong.** The average rating across all 23,486 reviews is 4.20 out of 5, and 82.24% of customers recommend what they bought.
* **One category is a clear outlier:** Trend. Both the Trend department and the Trend product class post the lowest average rating (3.82) and the lowest recommendation rate (73.95%) of any category in the dataset — a meaningful gap below every other group.
* **Bottoms and Intimates are the best-performing departments.** Bottoms leads with a 4.29 average rating and 85.13% recommendation rate, closely followed by Intimate at 4.28 and 85.01% — both ahead of the bigger departments, Tops and Dresses.
* **Older customers are the happiest customers.** Satisfaction rises steadily with age: the Under 25 group rates products highest (4.33, 86.66% recommend), while the 25–34 group is the least satisfied (4.14, 80.02% recommend) — the opposite of what you might expect.
* **Tops and Dresses drive the most reviews but not the most satisfaction.** Together they make up over 70% of all reviews, yet both sit below the dataset's overall average rating — the biggest departments aren't the best-loved ones.

### 🛠️ Power BI Skills & Dashboard Setup

To turn 23,486 raw customer reviews from one Excel file into clear business insights, I used Power BI's data cleaning, DAX, and dashboard features:

* **Data Cleaning & Preparation (Power Query):** Loaded the `Womens Clothing E-Commerce Review.xlsx` file and cleaned it before building anything.
  * Removed blank, auto-generated index columns left over from the source file
  * Renamed `Class Name` to **Product Class Name**, for consistency with the other product columns (`Product Division Name`, `Product Department Name`)
  * Converted `Recommended IND` from a 1/0 flag into readable **Yes/No** text, and renamed it to **Recommended**

* **Calculated Column:** Added an **Age Group** column, bucketing customers into Under 25, 25–34, 35–44, 45–54, and 55+, so age could be analyzed as a category instead of one age at a time.

<img width="450" height="125" alt="image" src="https://github.com/user-attachments/assets/8cdfb3c8-5cd3-4a11-a47d-0ffb87f94c16" />

* **Data Modeling:** This project came from a single flat table of reviews (`Womens Clothing E-Commerce Revi`), with no repeating customer or product records to split out, so one clean table was the right model — no separate fact/dimension tables were needed.

<img width="414" height="260" alt="image" src="https://github.com/user-attachments/assets/d661e411-e99f-459f-9b10-ca8dba5d85c6" />

* **DAX Measures:** Kept all measures in one dedicated `Measures (2)` table so they are easy to find:

| Measure | DAX |
|---|---|
| **Total Reviews** | `COUNTROWS('Womens Clothing E-Commerce Revi')` |
| **Average Rating** | `AVERAGE('Womens Clothing E-Commerce Revi'[Rating])` |
| **Recommendation Rate** | `DIVIDE(CALCULATE([Total Reviews], Recommended = "Yes"), [Total Reviews])` |
| **Recommended Reviews** | `CALCULATE([Total Reviews], Recommended = "Yes")` |
| **Not Recommended Reviews** | `CALCULATE([Total Reviews], Recommended = "No")` |
| **Negative Recommendation Rate** | `DIVIDE([Not Recommended Reviews], [Total Reviews])` |
| **Average Customer Age** | `AVERAGE('Womens Clothing E-Commerce Revi'[Age])` |
| **Average Positive Feedback** | `AVERAGE('Womens Clothing E-Commerce Revi'[Positive Feedback Count])` |
| **Total Positive Feedback** | `SUM('Womens Clothing E-Commerce Revi'[Positive Feedback Count])` |
| **Products Classes** | `DISTINCTCOUNT('Womens Clothing E-Commerce Revi'[Product Class Name])` |
| **Department** | `DISTINCTCOUNT('Womens Clothing E-Commerce Revi'[Product Department Name])` |
| **Division** | `DISTINCTCOUNT('Womens Clothing E-Commerce Revi'[Product Division Name])` |

<img width="230" height="248" alt="image" src="https://github.com/user-attachments/assets/f62da7c8-f13f-451b-8869-b0bdfa1d1a91" />

* **Key Metric Tracking:** Created KPI cards to highlight the main numbers, including **Total Reviews (23,486), Average Rating (4.20), Recommendation Rate (82.24%), Average Customer Age (43), Total Positive Feedback (59,559), and Average Positive Feedback per Review (2.54).**

<img width="593" height="52" alt="image" src="https://github.com/user-attachments/assets/6457b6ba-0197-4432-843e-c2eeb7ac470f" />

* **Chart Analysis:** Built visuals to compare average rating and recommendation rate across product division, department, and class, and to break down reviews by the Age Group column, to see which products and customer segments are most and least satisfied.

* **Interactive Slicers:** Added slicers for **Age Group, Product Division, Product Department, and Recommended**, letting users filter the whole dashboard down to specific customer segments or product lines.

<img width="100" height="120" alt="image" src="https://github.com/user-attachments/assets/609d72da-6d80-4e40-8f23-e30502d2a78f" />

### ✅ Strategic Recommendations

* **Investigate the Trend category before expanding it.** Its rating and recommendation rate trail every other group by a wide margin — find out whether this is a sizing, quality, or expectation-setting issue before investing further in it.
* **Study what Bottoms and Intimates are doing right.** Both outperform the rest of the catalog — their sizing, descriptions, or quality standards could be a model for improving Tops and Dresses.
* **Pay attention to the 25–34 customer segment.** They're the least satisfied age group despite not being the youngest or oldest — worth a closer look at what this group expects that isn't being met.
* **Don't assume volume means satisfaction.** Tops and Dresses generate the most reviews, but that reflects traffic, not happiness — resist treating review count alone as a success metric.

## 📂 How to Open and Explore the Dashboard

1. You can download the full file here: [Womens_Clothing_E-Commerce_Dashboard.pbix](https://github.com/DataWithMowa/E-commerce-Retail-Data-Analysis-Projects-/tree/main/Women%20Clothing%20E-commerce%20Power%20BI%20Project/Full%20Project)
2. Open the file locally using **Power BI Desktop**.
3. Go to the **Dashboard** page.
4. Use the **Age Group, Product Class,** and **Recommended** slicers to filter the dashboard by specific customer segments or product lines.
5. Hover over any chart to see the exact rating, recommendation rate, or review count behind it.

