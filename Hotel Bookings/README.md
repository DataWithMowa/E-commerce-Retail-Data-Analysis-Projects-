# Hotel Bookings Dashboard

<img width="943" height="465" alt="Red Dashboard" src="https://github.com/user-attachments/assets/3166469d-5609-4cac-9369-ba3632aee690" />

### 📊 Project Overview

**What really happens after someone clicks "Book Now"?**

This project analyzes over 117,000 hotel bookings from two hotels in Portugal, covering 2015 to 2017. The goal was simple: find out where the revenue is coming from, where it's being lost, and what can be done better.

#### The Approach

Using Power BI, I cleaned the raw booking data, built KPI measures for revenue, cancellations, and customer behavior, and put together an interactive dashboard to explore how bookings, cancellations, and revenue move across months, customer types, and market segments.

### 🕹️ Interactive Dashboard Demo

See the dashboard filters, dynamic metrics, and charts in action below (60-second walkthrough): 

https://github.com/user-attachments/assets/0b11f55b-bb9d-4dd1-9df1-63cc6f2a7006

### To see the full live analysis: [Click here](https://lnkd.in/p/eHB7Detb)

### 💡 Key Data Insights & Discoveries

* Revenue peaks in summer and continues to grow.
* 37.49% of bookings are cancelled — more than 1 in 3.
* Group bookings have the highest cancellation rate at 61.73%.

### 🛠️ Power BI Skills & Dashboard Setup

To turn 119,390 raw hotel booking rows from one Excel file into clear business insights, I used Power BI's data cleaning, DAX, and dashboard features:

* **Data Cleaning & Preparation (Power Query):** Loaded the `Hotel bookings.xlsx` file and cleaned it before building anything.
  * Removed bad rows: bookings with 0 adults, 0 children, and 0 babies at once; `adr` (rate) of 0 or less; and 0 total nights stayed — left with **117,399 clean rows**
  * The personal info columns (`name`, `email`, `phone-number`, `credit_card`) in the file are artificial/fake data, so the real guest data was never exposed
  * Added a **Month Number** calculated column, since `arrival_date_month` was stored as text (e.g. "July"), and set it as the sort-by column so charts show months in calendar order instead of alphabetical order

* **Data Modeling:** Unlike a multi-entity dataset, this project came from a single flat table (`hotel_booking`) with 37 columns and no repeating dimensions like product or customer records, so there was no need to split it into a fact/dimension star schema — one clean table was the right model here. Power BI's built-in date table handles the calendar hierarchy automatically.

<img width="458" height="313" alt="image" src="https://github.com/user-attachments/assets/13e3379a-2591-4d0a-aba8-e34f588fea1f" />

### DAX Measures

Kept all measures in one dedicated `Measures (2)` table so they are easy to find:

| Measure | What it does | DAX |
|---|---|---|
| **Total Bookings** | Counts every row in the table | `COUNTROWS(hotel_booking)` |
| **Total Travelers** | Counts adults, children, and babies for bookings that were not cancelled | `CALCULATE(SUMX(hotel_booking, hotel_booking[adults] + hotel_booking[children] + hotel_booking[babies]), hotel_booking[is_canceled] = 0)` |
| **Total Revenue** | Room rate × nights stayed, for bookings that were not cancelled | `CALCULATE(SUMX(hotel_booking, hotel_booking[adr] * (hotel_booking[stays_in_weekend_nights] + hotel_booking[stays_in_week_nights])), hotel_booking[is_canceled] = 0)` |
| **Average Price (Night)** | Average nightly room rate | `ROUND(AVERAGE(hotel_booking[adr]), 2)` |
| **Average Nights** | Average weekend + weeknight stay length, for bookings that were not cancelled | `ROUND(CALCULATE(AVERAGEX(hotel_booking, hotel_booking[stays_in_weekend_nights] + hotel_booking[stays_in_week_nights]), hotel_booking[is_canceled] = 0), 2)` |
| **Cancellation Rate** | Share of bookings that were cancelled | `DIVIDE(CALCULATE([Total Bookings], hotel_booking[is_canceled] = 1), [Total Bookings])` |
| **Countries Tracked** | Number of distinct countries in the data | `DISTINCTCOUNT(hotel_booking[country])` |
| **Top Country** | Ranks countries by traveler count, returns the top one | `VAR CountryTable = SUMMARIZE(hotel_booking, hotel_booking[country], "TravelerCount", [Total Travelers]) VAR TopRow = TOPN(1, CountryTable, [TravelerCount], DESC) RETURN MAXX(TopRow, hotel_booking[country])` |
| **Top Room Type** | Ranks room types by booking count, returns the top one | `VAR RoomTable = SUMMARIZE(hotel_booking, hotel_booking[reserved_room_type], "RoomCount", [Total Bookings]) VAR TopRow = TOPN(1, RoomTable, [RoomCount], DESC) RETURN MAXX(TopRow, hotel_booking[reserved_room_type])` |
| **Top Month-Year (by Bookings)** | Ranks month-year combos by booking count, returns the top one | `VAR MonthYearTable = SUMMARIZE(hotel_booking, hotel_booking[arrival_date_year], hotel_booking[arrival_date_month], "BookingCount", [Total Bookings]) VAR TopRow = TOPN(1, MonthYearTable, [BookingCount], DESC) RETURN CONCATENATEX(TopRow, hotel_booking[arrival_date_month] & " " & hotel_booking[arrival_date_year])` |
| **Top Month-Year (by Travelers)** | Ranks month-year combos by traveler count, returns the top one | `VAR MonthYearTable = SUMMARIZE(hotel_booking, hotel_booking[arrival_date_year], hotel_booking[arrival_date_month], "TravelerCount", [Total Travelers]) VAR TopRow = TOPN(1, MonthYearTable, [TravelerCount], DESC) RETURN CONCATENATEX(TopRow, hotel_booking[arrival_date_month] & " " & hotel_booking[arrival_date_year])` |
| **Best Customer Type - Most Bookings** | Ranks customer types by booking count, returns the top one | `VAR CustTable = SUMMARIZE(hotel_booking, hotel_booking[customer_type], "Cnt", [Total Bookings]) VAR TopRow = TOPN(1, CustTable, [Cnt], DESC) RETURN MAXX(TopRow, hotel_booking[customer_type])` |
| **Best Customer Type - Most Reliable** | Ranks customer types by cancellation rate (ascending), returns the lowest | `VAR CustTable = SUMMARIZE(hotel_booking, hotel_booking[customer_type], "CancelRate", DIVIDE(CALCULATE([Total Bookings], hotel_booking[is_canceled] = 1), [Total Bookings])) VAR TopRow = TOPN(1, CustTable, [CancelRate], ASC) RETURN MAXX(TopRow, hotel_booking[customer_type])` |
| **Best Customer Type - Pays Most Per Night** | Ranks customer types by average `adr`, returns the top one | `VAR CustTable = SUMMARIZE(hotel_booking, hotel_booking[customer_type], "AvgADR", AVERAGE(hotel_booking[adr])) VAR TopRow = TOPN(1, CustTable, [AvgADR], DESC) RETURN MAXX(TopRow, hotel_booking[customer_type])` |
| **Largest Families** | Ranks customer types by combined children + babies, returns the top one | `VAR CustTable = SUMMARIZE(hotel_booking, hotel_booking[customer_type], "KidsAndBabies", SUMX(hotel_booking, hotel_booking[children] + hotel_booking[babies])) VAR TopRow = TOPN(1, CustTable, [KidsAndBabies], DESC) RETURN MAXX(TopRow, hotel_booking[customer_type])` |
| **Market Segment With Highest Cancel Rate** | Ranks market segments (min. 50 bookings) by cancellation rate, returns the highest | `VAR SegmentTable = FILTER(SUMMARIZE(hotel_booking, hotel_booking[market_segment], "CancelRate", DIVIDE(CALCULATE([Total Bookings], hotel_booking[is_canceled] = 1), [Total Bookings]), "Bookings", [Total Bookings]), [Bookings] >= 50) VAR TopRow = TOPN(1, SegmentTable, [CancelRate], DESC) RETURN MAXX(TopRow, hotel_booking[market_segment])` |

<img width="300" height="311" alt="image" src="https://github.com/user-attachments/assets/cf4b8a12-2491-492e-8481-7ec17b7dab4e" />

* **Key Metric Tracking:** Created KPI cards to highlight the main business numbers, including **Total Bookings (117,399), Total Travelers (143,374), Total Revenue (€25.99M), Average Price Per Night (€103.54), Countries Tracked (178), and Cancellation Rate (37.49%).**

<img width="487" height="65" alt="image" src="https://github.com/user-attachments/assets/4b0dfd70-d53c-425c-952c-367afdff1079" />

* **Chart Analysis:** Built visuals to show revenue by month (revealing the summer peak), cancellation rate by market segment (surfacing Groups as the highest-risk segment at 61.73%), and booking volume by customer type and room type.

*It's all in the dashboard image*

* **Interactive Slicers:** Added slicers so users can filter the dashboard by hotel type, arrival year, and month, to explore specific periods and compare the two hotels directly.

<img width="200" height="100" alt="image" src="https://github.com/user-attachments/assets/d4b26bcb-c49c-40cf-acc5-7ac039f6f364" />

### 📈 Strategic Recommendations & Next Steps

* Reduce cancellations in the Group segment first, since it carries the highest cancellation rate at 61.73% — the single biggest source of lost revenue in the dataset.
* Increase direct bookings to reduce dependency on channels that bring in less reliable reservations, since cancellation rate varies sharply by market segment.
* Study what makes Resort Hotel bookings more valuable than City Hotel bookings, so that pattern can be applied across both properties.
* Use the summer revenue peak to plan staffing and inventory ahead of time, rather than reacting to it once it starts.

### 📂 How to Open and Explore the Dashboard

1. You can download the full file here: [Hotel_Bookings_Dashboard.pbix](https://github.com/DataWithMowa/E-commerce-Retail-Data-Analysis-Projects-/tree/main/Hotel%20Bookings/Full%20Project)
2. Open the file locally using Power BI Desktop.
3. Go to the Dashboard page.
4. Use the Hotel Type, Year, and Month slicers to filter all the charts by a specific hotel, arrival year, or arrival month.
5. Hover over the revenue and cancellation charts to compare Resort Hotel and City Hotel side by side.

### 🤝 Connect & Support

Thank you for taking the time to go through this project! If you have any questions or feedback, please reach out directly:

* 💼 **LinkedIn:** [Mowaninuola Umarudeen](https://www.linkedin.com/in/mowaninuolaumarudeen/)
* 📧 **Email:** [mowatheanalyst@gmail.com](mailto:mowatheanalyst@gmail.com)

*📈 **Did you find this useful?** Consider giving this repository a ⭐ **Star** if it helped you!*
