# Amazon E-commerce Sales Dashboard

<img width="942" height="500" alt="Amazon ecommerce Sales " src="https://github.com/user-attachments/assets/bb21ed4e-c6e6-4463-84dd-de45ec31a6c3" />

### 📊 Project Overview

This project was built to understand **how price, ratings, reviews, discounts, and revenue connect on Amazon**, and to turn raw sales data into clear insights that show what really drives sales and customer trust.

Using Power BI, I cleaned and analyzed the data, calculated key metrics, and built an interactive dashboard to explore **product pricing, customer ratings, review counts, discounts, and revenue across categories**.

The analysis also explored:

* Which products bring in the most revenue, and whether premium products lead.
* Whether a higher price means a higher rating.
* Why the categories with the most reviews are not always the best rated.
* How discounts differ across categories, and what that may say about competition and pricing.

I built the dashboard completely in **Power BI**, using data to look beyond "What sells?" and understand what makes customers trust a product.

### 🕹️ Interactive Dashboard Demo

See the dashboard filters, dynamic metrics, and charts in action below (31-second walkthrough): 

https://github.com/user-attachments/assets/81530ff8-2165-4f2b-b1e0-98b5c2c75394

### To see the full live analysis: [Click here](https://lnkd.in/p/ehQUjN4E)

### 💡 Key Data Insights & Discoveries

1. **Premium Products Drive Most of the Revenue:** **Higher-priced products brought in the majority of total revenue**, showing that expensive items carry a big share of sales.
2. **Higher Price Does Not Mean Higher Rating:** **Products with higher prices did not automatically get better ratings**, so paying more does not always mean customers are happier.
3. **Simple, Everyday Products Earned Some of the Best Ratings:** Some **basic, low-cost products received some of the strongest ratings**, showing that customers value products that do their job well.
4. **Most Reviewed Does Not Mean Best Rated:** **Categories with the most reviews were not always the highest rated**, so a lot of reviews does not always mean a lot of satisfaction.
5. **Electronics Led in Total Discounts:** **Electronics offered the highest total discounts**, which may point to strong competition and its effect on pricing.

### 🛠️ Power BI Skills & Dashboard Setup

To turn 1,465 raw Amazon product rows from one flat CSV file into clear business insights, I used Power BI's data cleaning, data modeling, DAX, and dashboard features:

* **Data Cleaning & Preparation (Power Query):** Loaded the single `Amazon Sales.csv` file and cleaned it before building anything.
  * Removed the **₹** symbol from `discounted_price` and `actual_price` and changed both to currency
  * Removed the **%** sign from `discount_percentage`, divided it by 100, and set it as a percentage
  * Replaced error values in `rating` and `rating_count` with 0 and fixed the data types
  * Removed duplicate rows using `product_id`, `user_id`, and `review_id`
  * Split the long `category` column (it used `|` between levels) into **Product Category** and **Sub Category**

* **Data Modeling (Star Schema):** The whole project came from only one dataset. I kept the raw file as a staging table (`Amazon_Sales Real`) and split it into one fact table and three dimension tables so the model is clean and easy to filter.

| Table | Type | What it holds |
|---|---|---|
| **Fact Sales** | Fact | `product_id`, `user_id`, `review_id`, `discounted_price`, `actual_price`, `discount_percentage`, `rating`, and a calculated `Price Tier` column |
| **Dim_Product** | Dimension | `product_id`, `product_name`, `about_product`, `product_link`, `img_link`, `Product Category`, `Sub Category` |
| **Dim_User** | Dimension | `user_id`, `user_name` |
| **Dim_Review** | Dimension | `review_id`, `review_title`, `review_content`, `rating_count` |

<img width="863" height="367" alt="image" src="https://github.com/user-attachments/assets/a0bb93c4-483c-4b95-a8ca-6e88c777ebad" />

* **Relationships:** Connected the fact table to each dimension with **one-to-many** relationships: `Fact Sales[product_id]` → `Dim_Product`, `Fact Sales[user_id]` → `Dim_User`, and `Fact Sales[review_id]` → `Dim_Review`.

* **Price Tier Column:** Created a calculated column that groups every product into **Budget (<500), Mid-Range (500–2K), or Premium (>2K)**. This powers the Price Tier slicer and the Pricing Tier Segmentation donut.

<img width="682" height="59" alt="image" src="https://github.com/user-attachments/assets/f1487819-fc04-4ca3-aa9d-5e70b9f1eff4" />

* **DAX Measures:** Kept all measures in one dedicated `Measures` table so they are easy to find:

<img width="244" height="244" alt="image" src="https://github.com/user-attachments/assets/dfdadd4f-6d6d-404c-8fc1-078c4700173d" />

| Measure | DAX |
|---|---|
| **Total Revenue** | `SUM('Fact Sales'[discounted_price])` |
| **Total Actual Price** | `SUM('Fact Sales'[actual_price])` |
| **Total Discount** | `[Total Actual Price] - [Total Revenue]` |
| **Average Order Value** | `AVERAGE('Fact Sales'[discounted_price])` |
| **Average Actual Price** | `AVERAGE('Fact Sales'[actual_price])` |
| **Average Discounted Price** | `AVERAGE('Fact Sales'[discounted_price])` |
| **Average Rating** | `AVERAGE('Fact Sales'[rating])` |
| **Total Reviews** | `COUNT('Fact Sales'[review_id])` |
| **Total Reviews Volume** | `SUM(Dim_Review[rating_count])` |
| **Average Reviews Per Product** | `AVERAGE(Dim_Review[rating_count])` |
| **User Engagement Count** | `DISTINCTCOUNT(Dim_Review[review_id])` |

* **Key Metric Tracking:** Created KPI cards to highlight the main business numbers, including **Total Revenue (₹5M), Total Actual Price (₹8M), Average Order Value (₹3K), Average Rating (4.1), Total Reviews (1K), and Total Discount (₹3.40M).**

You can find the image on the dashboard

* **Chart Analysis:** Used a **scatter chart** to test if higher prices really mean better ratings, a **category performance table** to compare review volume and rating by product category, and a **donut chart** to show how revenue splits across Budget, Mid-Range, and Premium products.

* **Interactive Toggle Button (Bookmarks):** Added **Product / Category** buttons on the Total Discount visual, so one space can switch between a discount-by-category bar chart and a **Product Image Table**. For the table, I set `img_link` to **Image URL** and `product_link` to **Web URL**, so the product pictures and clickable links show inside the dashboard.

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/5537bc57-0358-48dd-8ce2-61272ac140af" />

* **Interactive Slicer:** Added a **Price Tier** slicer, letting users filter the whole dashboard by Budget, Mid-Range, or Premium products.

<img width="150" height="100" alt="image" src="https://github.com/user-attachments/assets/48e7ceb9-1b97-4ee1-a076-bca86976925c" />

### 📈 Strategic Recommendations & Next Steps

* **Don't put all the eggs in the Premium basket:** Almost all the money (90.84%) comes from Premium products, but they're not rated any better than cheap ones. Push Budget and Mid-Range products more instead of only relying on high prices.

* **Watch the discounts on Electronics:** Electronics gets the most reviews, but it also gets the most discounts. Check if all that discounting is really needed, or if it's just cutting into profit for no reason.

* **Look into why Car & Motorbike has low ratings:** This category has the lowest rating (3.8) even though it sells well. Find out what customers are unhappy about before it starts hurting the whole store's reputation.

* **Add dates to the data:** Right now there's no way to see trends over time. Adding order or review dates would show if the Premium tier is growing or shrinking, instead of just one snapshot.

### 📂 How to Open and Explore the Workbook

1. You can download the full file here: [Retails_Sales_Dashboard.xlsx](https://github.com/DataWithMowa/E-commerce-Retail-Data-Analysis-Projects-/tree/main/Retail%20Sales%20%26%20Customer%20Demographic%20Excel%20Project/Full%20Project)
2. Open the file locally using **Power BI desktop**.
3. 3. Go to the **Dashboard** page.
4. Use the Price Tier slicer on the top right side of the dashboard layout to filter the charts dynamically.

### 📂 How to Open and Explore the Dashboard

1. You can download the full file here: [Amazon_Ecommerce_Sales_Dashboard.pbix](https://github.com/DataWithMowa/E-commerce-Retail-Data-Analysis-Projects-/tree/main/Amazon%20E-Commerce%20Sales%20Power%20BI%20Project/Full%20Project)
2. Open the file locally using Power BI Desktop.
3. Go to the Dashboard page.
4. Use the Price Tier slicer on the top right side of the dashboard to filter all the charts by Budget, Mid-Range, or Premium.
5. Click the Product / Category buttons on the Total Discount visual to switch between the discount-by-category chart and the Product Image Table.

### 🤝 Connect & Support

Thank you for taking the time to go through this project! If you have any questions or feedback, please reach out directly:

* 💼 **LinkedIn:** [Mowaninuola Umarudeen](https://www.linkedin.com/in/mowaninuolaumarudeen/)
* 📧 **Email:** [mowatheanalyst@gmail.com](mailto:mowatheanalyst@gmail.com)

*📈 **Did you find this useful?** Consider giving this repository a ⭐ **Star** if it helped you!*


