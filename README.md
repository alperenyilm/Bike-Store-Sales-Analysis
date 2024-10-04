# Sales Performance Analysis of Bike Stores

## Table of Contents
- [Project Description](#project-description)
- [Technologies Used](#technologies-used)
- [Dataset](#dataset)
  - [Database Structure](#database-structure)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
- [Data Analysis](#data-analysis)
- [Results/Findings](#resultsfindings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)

### Project Overview
This project aims to analyze bicycle sales between the years 2016-2018. The data includes details about customers, stores, states, and bicycle categories to provide insights into revenue distribution. An interactive dashboard was created using Tableau to visualize this data, and SQL was used to query and manipulate the dataset.

![Dashboard](https://github.com/user-attachments/assets/b357fc91-8682-4c9c-b4ae-f5212602f0e8)

### Technologies Used
- **SQL**: For querying and manipulating the dataset.
- **Tableau**: To visualize the data and create an interactive dashboard.
- **Excel**: For storing data and creating pivot tables.

### Dataset
This project utilizes the **BikeStores** sample database, which is available for download [here](https://www.sqlservertutorial.net/getting-started/load-sample-database/).

### **Database Structure**
The BikeStores database contains two schemas: `sales` and `production`, which are used to manage customer orders, products, and store information.

#### **1. sales.customers**
- This table stores customer information.
  - Columns: `customer_id`, `first_name`, `last_name`, `phone`, `email`, `street`, `city`, `state`, `zip_code`.

#### **2. sales.orders**
- Contains details of customer orders.
  - Columns: `order_id`, `customer_id`, `order_status`, `order_date`, `required_date`, `shipped_date`, `store_id`, `staff_id`.

#### **3. sales.order_items**
- Stores details of the items included in each order.
  - Columns: `order_id`, `item_id`, `product_id`, `quantity`, `list_price`, `discount`.

#### **4. production.products**
- Contains information about the products sold.
  - Columns: `product_id`, `product_name`, `brand_id`, `category_id`, `model_year`, `list_price`.

#### **5. production.brands**
- Stores product brand information.
  - Columns: `brand_id`, `brand_name`.

#### **6. production.categories**
- Contains product category details.
  - Columns: `category_id`, `category_name`.

#### **7. sales.stores**
- Provides information about the stores selling the products.
  - Columns: `store_id`, `store_name`, `phone`, `email`, `street`, `city`, `state`, `zip_code`.

#### **8. sales.staffs**
- Stores information about the staff working at the stores.
  - Columns: `staff_id`, `first_name`, `last_name`, `email`, `phone`, `active`, `store_id`, `manager_id`.

### Data Cleaning and Preparation
Although the data provided in the **BikeStores** database is generally clean and well-structured, a few adjustments were made using SQL to ensure consistency and improve data usability. These adjustments included:
- **Data Corrections**: Minor adjustments to certain fields (e.g., aligning date formats or correcting any inconsistencies in customer names or product details).
- **Sorting Data**: SQL queries were used to organize the data, such as sorting orders by date and grouping them by store or product category to make analysis easier.
- **Null Values**: There were no significant null values, but any minor missing data points were handled appropriately to avoid analysis issues.

Overall, there was no need for extensive cleaning due to the high quality of the dataset, but the above minor adjustments helped ensure accurate analysis.

### Exploratory Data Analysis (EDA)

EDA involved exploring the sales data to answer key questions, such as:

- What is the overall sales trend?
- Which products are top sellers?
- What are the peak sales periods?
- Which stores performed the best?
- How does customer preference vary across regions?

### Data Analysis

To extract relevant insights from the dataset, the following SQL query was used to retrieve data on orders, customers, products, and sales performance across stores:

```sql
SELECT
    ord.order_id,
    CONCAT(cus.first_name, ' ', cus.last_name) AS 'customers',
    cus.city,
    cus.state,
    ord.order_date,
    SUM(ite.quantity) AS 'total_units',
    SUM(ite.quantity * ite.list_price) AS 'revenue',
    pro.product_name,
    cat.category_name,
    sto.store_name,
    CONCAT(sta.first_name, ' ', sta.last_name) AS 'sales_rep'
FROM sales.orders AS ord
JOIN sales.customers AS cus
ON ord.customer_id = cus.customer_id
JOIN sales.order_items AS ite
ON ord.order_id = ite.order_id
JOIN production.products AS pro
ON ite.product_id = pro.product_id
JOIN production.categories AS cat
ON pro.category_id = cat.category_id
JOIN sales.stores AS sto
ON ord.store_id = sto.store_id
JOIN sales.staffs AS sta
ON ord.staff_id = sta.staff_id
GROUP BY 
    ord.order_id,
    CONCAT(cus.first_name, ' ', cus.last_name),
    cus.city,
    cus.state,
    ord.order_date,
    pro.product_name,
    cat.category_name,
    sto.store_name,
    CONCAT(sta.first_name, ' ', sta.last_name)

```

### Results/Findings

The analysis results are summarized as follows:

1. The company's sales increased steadily between 2016 and 2017, followed by a slight decline in 2018.
2. Mountain bikes and electric bikes were the top-performing categories in terms of sales and revenue.
3. Stores located in California and New York generated the highest revenue, indicating a regional preference for bicycles.

### Recommendations

Based on the analysis, I recommend the following actions:

- Invest in marketing and promotions during peak sales seasons to maximize revenue.
- Focus on expanding and promoting top-selling products like mountain bikes and electric bikes.
- Implement targeted marketing strategies in high-revenue states like California and New York.

### Limitations

Some limitations of the analysis include:

- The dataset only covers three years of sales data, which may not fully capture long-term trends.
- Certain external factors, such as economic conditions or seasonal weather patterns, were not accounted for in this analysis but may have impacted sales performance.
- Although there were no significant data quality issues, some minor adjustments were made, such as correcting date formats and sorting records, to ensure consistency.

