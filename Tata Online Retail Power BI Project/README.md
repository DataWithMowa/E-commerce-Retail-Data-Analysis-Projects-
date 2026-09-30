# Tata Online Retail Store Dashboard

<img width="943" height="456" alt="Dashboard " src="https://github.com/user-attachments/assets/60dc8d54-4dcb-4783-9611-46d010f00337" />

### 📊 Project Overview

**What does a year of online retail transactions actually reveal about a business?**

This project analyzes 539,387 transaction line items from a UK-based online retailer, covering December 2010 to December 2011. Using Power BI, I cleaned the raw data, calculated key revenue and customer metrics, and segmented customers by purchase behavior to understand not just what sold, but who was buying, how often, and what that means for the business going forward.

### The Approach

Using Power BI, I cleaned invalid rows, flagged cancelled orders, built a proper date table for time intelligence, and ran an RFM (Recency, Frequency, Monetary) analysis to segment customers into groups based on real buying behavior — not just guesses.

---

### 🕹️ Interactive Dashboard Demo

See the dashboard filters, dynamic metrics, and charts in action below (29-second walkthrough): 

https://github.com/user-attachments/assets/155dc255-35ef-40fd-a134-7345468b8b37

### To see the full live analysis: [Click here](https://lnkd.in/p/ez-czvd2)

## 🔍 Key Data Insights & Discoveries

* Total revenue across the dataset is **£9.77M**, from **23,795 orders** and **4,372 customers**.
* **The United Kingdom drives 84% of total revenue** (£8.21M) — this is overwhelmingly a domestic business, with the Netherlands, Ireland, Germany, France, and Australia as distant secondary markets.
* **Revenue builds steadily toward the holiday season**, climbing from under £700K a month for most of the year to **£1.46M in November 2011** — the highest month in the dataset — before the data cuts off partway through December.
* **16.1% of all orders are cancelled** (3,835 of 23,795) — a meaningful share of transactions never convert into completed revenue.
* Customer segmentation (RFM) shows just **39.8% of identified customers are "High Value,"** yet that group alone accounts for **71.5% of total revenue** (£6.99M) — a small core of repeat, high-spending customers is carrying the business.
* **"One-Time" customers make up over a third of the identified customer base but generate only 5.3% of revenue** — a large group that never comes back.
* Transactions with no customer ID attached ("Unidentified") still account for **15% of revenue** — a sizeable chunk of sales isn't tied to any traceable customer record.
* Top-selling products by revenue include the **Regency Cakestand 3 Tier**, **White Hanging Heart T-Light Holder**, **Party Bunting**, and **Jumbo Bag Red Retrospot** — small, giftable, décor-style items, consistent with a gift/homeware retail business.

---

## 🛠️ Power BI Skills & Dashboard Setup

To turn 539,387 raw transaction rows into clear business insights, I used Power BI's data cleaning, DAX, and dashboard features:

* **Data Cleaning & Preparation (Power Query):** Loaded the `TATA Online Retail Dataset.xlsx` file and cleaned it before building anything.
  * Replaced blank `Description` values with "Unknown" instead of leaving nulls
  * Removed rows with a `UnitPrice` of £0.01 or less (invalid/free entries)
  * Added an **Order Status** column, flagging any invoice starting with "C" as "Cancelled" and everything else as "Completed"
  * Duplicated `InvoiceDate` into a clean **InvoiceDateOnly** date column, separate from the full timestamp, for cleaner date-based filtering

* **Data Modeling:** Built a custom **Calendar** table (`CALENDAR(DATE(2010,12,1), DATE(2011,12,31))`) with Year, Month, Month Year, and YearMonthNo columns, so time-based visuals sort correctly and support proper time intelligence — rather than relying only on Power BI's auto-generated date hierarchy.

<img width="682" height="342" alt="image" src="https://github.com/user-attachments/assets/5192765c-7854-4b21-bf9d-58963838e144" />

* **Customer RFM Segmentation:** Built a dedicated **Customer RFM** table using a calculated table expression that computes, per customer: Recency (days since last purchase, relative to Dec 9, 2011), Frequency (distinct completed orders), and Monetary (total spend). Customers with no ID are grouped into a separate "Unidentified" row so they're not lost from the analysis.

<img width="868" height="344" alt="image" src="https://github.com/user-attachments/assets/34c8beb7-eeac-40eb-869e-fad5823a9f2c" />

* **Segment Logic:** Added a **Segment** calculated column on top of the RFM table, classifying every customer into one of four groups:

```DAX
Segment = 
IF(
    'Customer RFM'[CustomerID] = -1,
    "Unidentified",
    IF(
        'Customer RFM'[Frequency] = 1,
        "One-Time",
        IF(
            'Customer RFM'[Monetary] >= 648.55 && 'Customer RFM'[Recency (Days)] <= 90,
            "High Value",
            "Regular"
        )
    )
)
```

* **DAX Measures:** Kept all measures in one dedicated `Measures (2)` table (plus one measure on the RFM table) so they are easy to find:

**Total Revenue**
```DAX
Total Revenue = SUMX('Online Retail', 'Online Retail'[UnitPrice] * 'Online Retail'[Quantity])
```

**Total Orders**
```DAX
Total Orders = DISTINCTCOUNT('Online Retail'[InvoiceNo])
```

**Total Customers**
```DAX
Total Customers = DISTINCTCOUNT('Online Retail'[CustomerID])
```

**Avg Order Value**
```DAX
Avg Order Value = DIVIDE([Total Revenue], [Total Orders])
```

**Cancelled Orders Count** — counts distinct invoices flagged as Cancelled
```DAX
Cancelled Orders Count = 
CALCULATE(
    DISTINCTCOUNT('Online Retail'[InvoiceNo]),
    'Online Retail'[Order Status] = "Cancelled"
)
```

**Segment Revenue** — total spend rolled up by RFM segment
```DAX
Segment Revenue = SUM('Customer RFM'[Monetary])
```

* **Key Metric Tracking:** Created KPI cards to highlight the main business numbers, including **Total Revenue (£9.77M), Total Orders (23,795), Total Customers (4,372), Average Order Value (£410.59), and Cancelled Orders (3,835).**

<img width="395" height="37" alt="image" src="https://github.com/user-attachments/assets/23f0815a-3a1f-4bb4-96a5-3e93a535a023" />

* **Chart Analysis:** Built visuals to show revenue trend by month (revealing the pre-holiday build-up), revenue by country (surfacing the UK's dominance), and revenue and customer count by RFM segment (surfacing the small High Value group as the true revenue driver).

* **Interactive Slicers:** Added slicers for Country and Month Year, letting users filter the whole dashboard down to specific markets and time periods

<img width="274" height="48" alt="image" src="https://github.com/user-attachments/assets/20220a77-87bb-4625-bf56-f9e82f39787c" />

---

## 📈 Strategic Recommendations & Next Steps

* Protect and grow the High Value segment first — it's 39.8% of identified customers but 71.5% of revenue, making it the single most important group to retain.
* Investigate why over a third of customers only ever buy once, and test a follow-up offer or email aimed specifically at converting One-Time buyers into Regular ones.
* Reduce the 16.1% order cancellation rate by reviewing the most-cancelled products and whether stock, pricing, or listing accuracy is driving it.
* Treat international markets as a growth opportunity, not an afterthought — with 84% of revenue from the UK alone, there's clear room to build out the Netherlands, Ireland, Germany, France, and Australia further.
* Use the Unidentified 15% of revenue as a prompt to improve checkout data capture, so more transactions can be tied to a real customer for future segmentation.

---

## 📂 How to Open and Explore the Dashboard

1. You can download the full file here: [Tata_Online_Retail_Dashboard.pbix](https://github.com/DataWithMowa/E-commerce-Retail-Data-Analysis-Projects-/tree/main/Tata%20Online%20Retail%20Power%20BI%20Project/Full%20Project)
2. Open the file locally using **Power BI Desktop**.
3. Go to the **Dashboard** page.
4. Use the **Country** and **Month Year** slicers to filter all the charts by market, completed vs. cancelled orders, or a specific time period.
5. Hover over the RFM segment chart to compare how much revenue and how many customers sit in each of the High Value, Regular, One-Time, and Unidentified groups.

### 🤝 Connect & Support

Thank you for taking the time to go through this project! If you have any questions or feedback, please reach out directly:

* 💼 **LinkedIn:** [Mowaninuola Umarudeen](https://www.linkedin.com/in/mowaninuolaumarudeen/)
* 📧 **Email:** [mowatheanalyst@gmail.com](mailto:mowatheanalyst@gmail.com)

*📈 **Did you find this useful?** Consider giving this repository a ⭐ **Star** if it helped you!*
