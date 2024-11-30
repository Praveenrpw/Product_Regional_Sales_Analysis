### Product Performance Regional Sales Analysis

Dataset Description
This dataset contains detailed order information for a retail business. Below are the key details based on the structure:

	Total Records: 9,994 entries
    Columns (16 in total):
        Order Information: Order Id, Order Date, Ship Mode
        Customer Demographics: Segment, Country, City, State, Postal Code, Region
        Product Details: Category, Sub Category, Product Id
        Financial Information: cost price, List Price, Quantity, Discount Percent

This dataset appears to track orders, shipping details, and product financials. It is suitable for projects like sales performance analysis, customer behavior insights, or discount impact analysis.


### Data Analysis Using Numpy, Pandas
1. Data Loading: 
Loading the Dataset: 
Used Pandas to load the dataset from a .csv file into a DataFrame (pd.read_csv).

-- Handled missing values by replacing entries such as 'Not Available' and 'unknown' with NaN using the na_values parameter.
-- Verified column data types (dtypes) to ensure the correctness of the data structure.

2. Data Exploration:
Exploring the Data:
Used DataFrame.shape to determine the size of the dataset (rows and columns).

Used DataFrame.info() and DataFrame.describe() to summarize the dataset:
-- Identified numerical and categorical columns.
-- Obtained statistical summaries (e.g., mean, max, min, etc.).
-- Explored unique values in specific columns (e.g., Ship_Mode.unique()).

3. Data Cleaning:
Handling Missing Values:
Identified missing or incorrect data in columns like Ship_Mode and replaced them as needed (na_values during load or post-load imputation).

Changing Data Types:
Converted the Order_Date column from object to datetime format for better manipulation (pd.to_datetime).
Validated data type consistency for numerical calculations (e.g., Quantity, Unit_Selling_Price, etc.).

4. Data Transformation:
Derived New Columns:
Created new calculated fields like:
-- Total_Sales = Quantity * Unit_Selling_Price.
-- Profit_Per_Unit = Unit_Selling_Price - Cost_Price.
-- Growth_Diff for year-over-year comparison (e.g., 2023 vs. 2022).
Used vectorized operations in Pandas for these transformations.

Aggregations:
Grouped data by categories (Category, Region, etc.) using groupby to calculate metrics such as:
-- Sum of Total_Sales and Total_Profit.
-- Average sales per Category and Month.
-- Sorting and Filtering:
-- Sorted data for top-performing entities (e.g., products, regions) using sort_values.
-- Filtered data for specific conditions (e.g., only products in Technology or regions like South).

5. Business Insights Derived
Year-over-Year Comparisons:
-- Calculated and compared total sales for 2022 and 2023 grouped by Category or Sub_Category.
-- Derived percentage growth for each category or region.

Top Performers:
-- Identified the highest-grossing subcategories, products, and regions.
-- Determined months with the highest sales for each category (Furniture, Technology, etc.).

Trends:
-- Grouped data by Month_Year to analyze monthly sales trends over two years.
-- Highlighted months with significant growth or decline.


### Data Analysis Using SQL
### Schema name (sales)
### Table name (orders)

Data analysis using SQL. 
Project Overview:
This project involves the analysis of sales orders for a company, using SQL queries to extract insights into various business metrics 
like sales, profits, product performance, regional sales, and year-over-year growth.

The queries cover a wide range of tasks that are commonly performed in SQL-based data analysis projects:
1. Data Exploration: 
	You are retrieving distinct data points, aggregating data, and filtering for specific conditions (e.g., total sales, 
	highest revenue products, etc.).

2. Data Aggregation: 
	Queries like calculating total sales, profits, and revenue for products or regions are important in business analytics.

3. Window Functions: 
	Using functions like ROW_NUMBER(), RANK(), and DENSE_RANK() shows advanced use of SQL for generating ranked lists 
	and performing comparative analysis.

4. Growth and Comparison: 
	You're using SQL to compute month-over-month growth and category-based comparisons across multiple years 
	(2022 and 2023). This is important in understanding business trends and making data-driven decisions.

5. Category-Specific Insights: 
	Analyzing the highest-selling products or sub-categories, regional insights, and other category-based 
	analyses is useful for understanding the business at a granular level.

Column Context:
1.  Order_Id: Unique identifier for each order placed in the system.
2.  Order_Date: The date when the order was placed.
3.  Ship_Mode: Mode of shipping (e.g., Second Class, Standard Class).
4.  Segment: Customer segment (e.g., Consumer, Corporate).
5.  Country: Country where the order was shipped.
6.  City: City of the shipping address.
7.  State: State of the shipping address.
8.  Postal_Code: Postal code of the shipping address.
9.  Region: The geographical region (e.g., South, West).
10. Category: The category of the product (e.g., Furniture, Office Supplies).
11. Sub_Category: The sub-category of the product (e.g., Chairs, Tables).
12. Product_Id: Unique identifier for each product.
13. Quantity: The number of units of the product ordered.
14. Unit_Selling_Price: The price per unit of the product.
15. Unit_Profit: Profit per unit sold.
16. Total_Profit: Total profit for the order.
17. Total_Sales: Total sales value for the order.
18. Month: Month of the order date (e.g., January, February).
19. Order_Year: The year in which the order was placed.
20. Quarter: The fiscal quarter of the order (e.g., Q1, Q2).
21. Month_Year: Combined Month and Year (e.g., 2023-03 for March 2023).

Tasks Performed:
1. Data Exploration & Filtering:
	Extracted distinct cities, regions, categories, and other relevant fields.
	Filtered orders by specific criteria (e.g., category = 'Technology', ship mode = 'Second Class').

2. Aggregation and Calculation:
	Aggregated data for total sales, total profits, and quantity sold by product, region, and category.
	Calculated average order value and total sales per order.

3. Rankings & Comparisons:
	Used SQL window functions (ROW_NUMBER(), RANK(), DENSE_RANK()) to rank products, regions, and cities based on revenue or profit.
	Identified top-performing products by revenue and profit.

4. Year-over-Year Growth Analysis:
	Compared sales and profits between 2022 and 2023 for products, regions, and categories.
	Computed growth percentage month-over-month for each year.

5. Detailed Regional and Product Insights:
	Analyzed sales trends and rankings by region.
	Compared the sales and performance of products within each region, using window functions.

Results and Insights:

-- Highest Revenue Generating Products: List of the top 10 products based on total revenue.
-- Top Performing Regions: Identification of regions with the highest sales and profits.
-- Month-over-Month Growth: Insights into which months in 2022 and 2023 had the highest sales growth.
-- Category Performance: Sub-categories with the highest sales growth in 2023 compared to 2022.


# Database Connection and Integration
Establish a connection between Python and MySQL Workbench for data transfer and querying.

1. Establishing the Connection:
Used SQLAlchemy to connect Python with the MySQL Workbench database.

2. Uploading Data to MySQL:
Used Pandas to_sql() function to transfer a cleaned DataFrame to the MySQL database

3. Fetching Data from MySQL:
Executed SQL queries directly on the MySQL table using SQLAlchemy.
Fetching filtered rows directly into Python for further analysis or manipulation.

# Two-Way Data Flow:
-- Uploaded transformed data from Python to MySQL for structured storage.
-- Retrieved specific subsets of data from MySQL back into Python for additional analysis.
