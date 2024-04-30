# Superstore-Analysis
• Data source: Tableau Sample Superstore

• Tools: SQL (MySQL), Tableau

## Introduction
Superstore is a U.S. based small retail business that sells furniture, office supplies, and technology products. Their customer segments are mass Consumers, Corporate, and Home Offices. The project aims to provide a thorough analysis of Superstore sales performance and trends to reveal key insights that can be used to guide data-driven decision-making. Using a detailed dataset, the project explores multiple facets of the business, including sales trends, product popularity, regional market performance, and more. This report highlights key metrics, analyzes category and product performance, examines regional sales trends, and offers strategic insights and recommendations to optimize sales and maintain a competitive edge in the industry.

## Requirements
The Superset dataset used in this project was obtained from Kaggle and converted from an XSLX file to a CSV file for easier handling. The analysis was performed using MySQL and complemented using Tableau to create an interactive dashboard for data visualization.
The data is publicly available through Kaggle under [https://www.kaggle.com/datasets/ishanshrivastava28/superstore-sales](url) .It comes with 9995 rows with 9994 rows of pure data and 1 row being column headers. It contains the data of 793 customers. The data contains 21 columns namely; Row ID, Order ID, Order Date, Ship Date, Ship Mode, Customer ID , Customer Name, Segment, Postal Code, City, State, Country, Region, Product ID, Category, Sub-Category, Product Name, Sales, Quantity, Discount and Profit.
The only limitations of our dataset that I could mention is that the most recent data point was almost 10 years ago. So our data is not current. However, our data is quite reliable, original, comprehensive and is cited.

## Business Questions
1. What is the yearly total sales and profit?
2. What is the total sales, average sales and percent of total sales of each category?
3. What are the sales by category and sub-category?
4. What are the sales by region and state?
5. Which subcategories have the highest and lowest total sales overall?
6. Which subcategories have the highest and lowest total profit overall?
7. Which region generates the highest sales and profit?
8. What are the profit margins by region?
9. What are the top and bottom states total sales and profits with their profit margins?What are the top and bottom states total sales and profits with their profit margins?
10. What are the top 10 cities total sales and profits with their profit margins?
11. What is discount vs average sales?
12. What is the total discount per product category?
13. What are the most discounted subcategories(product type)?
14. What is the highest total sales and profit per category in each region?
15. What is the highest total sales and profit per category in each state?
16. What are the top 15 most profitable products?
17. Which segment makes the most of our sales and profit?

## Data Cleaning and Preparation
### Duplicate Values 
After importing the database and values from the .csv file in the MySQL workbench, I checked for duplicate values. Duplicate values may lead to inaccurate analysis.
There appears to be 0 duplicate entries across all columns.

### Null Values
Checking for null values involves identifying any missing or undefined data in each column.
There were no null values found in the dataset.

### Update Table
Updating the schema to add an "order_year" column by extracting the year from order_date for easier filtering by year.

```
ALTER TABLE sample_superstore
ADD COLUMN order_year INT AFTER order_id;

UPDATE sample_superstore
SET order_year = YEAR(order_date);
```

## Data Analysis & Insights
1. The data below shows how the profits overr the years have steadily increased with each year being more profitable than the other.
```
SELECT order_year, SUM(sales) as total_sales, SUM(profit) as total_profit
FROM sample_superstore
GROUP BY 1
ORDER BY 1;
```
| order_year | total_sales | total_profit |
|------------|-------------|--------------|
| 2011       | 481763.83   | 49044.48     | 
| 2012       | 464426.18   | 60907.79     | 
| 2013       | 600533.77   | 80062.50     | 
| 2014       | 733215.19   | 93439.73     | 

2.	Technology is the largest category with total sales of $836.2K, accounting for about 36% of Superstore's total business. The Furniture and Office Supplies categories each account for 32% and 31% of total sales, respectively. Average sales in the Technology category are roughly 1.3x that of the Furniture category and 3.8x that of the Office Supplies category. 
```
SELECT category, SUM(sales) AS total_sales, AVG(sales) AS average_sales,
SUM(sales)/(SELECT SUM(sales) FROM sample_superstore)*100 AS percentage_of_total
FROM sample_superstore
GROUP BY 1
ORDER BY 2 desc;
```
| category        | total_sales | avg_sales  | perc_of_total |
|-----------------|-------------|------------|---------------|
| Technology      | 835900.14   | 454.540587 | 36.784093     |
| Furniture       | 733047.06   | 353.446027 | 32.258005     |
| Office Supplies | 703502.87   | 121.692245 | 30.957902     |
