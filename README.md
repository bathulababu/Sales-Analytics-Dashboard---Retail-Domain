# Sales-Analytics-Dashboard---Retail-Domain

This project focuses on analyzing sales data using SQL and Power BI to gain valuable business insights. It includes setting up a local MySQL database, performing SQL queries to extract and analyze data, and utilizing Power BI for data visualization and deeper analysis.

## Project Structure

The project contains the following:

* **`db_dump.sql`**:  A SQL dump file containing the database schema and data for the sales analysis.

## Setup Instructions

### 1.  Install MySQL

    * Follow the instructions in this video to install MySQL on your local computer: [https://www.youtube.com/watch?v=WuBcTJnIuzo](https://www.youtube.com/watch?v=WuBcTJnIuzo)

### 2.  Import the Database

    * Download the `db_dump.sql` file.
    * Import the SQL dump into your local MySQL database using the instructions provided in the installation video.

## Data Analysis Using SQL

The following SQL queries were used to analyze the sales data:

1.  **Show all customer records:**

    ```sql
    SELECT * FROM customers;
    ```

2.  **Show total number of customers:**

    ```sql
    SELECT count(*) FROM customers;
    ```

3.  **Show transactions for Chennai market (market code for Chennai is 'Mark001'):**

    ```sql
    SELECT * FROM transactions WHERE market_code = 'Mark001';
    ```

4.  **Show distinct product codes that were sold in Chennai:**

    ```sql
    SELECT DISTINCT product_code FROM transactions WHERE market_code = 'Mark001';
    ```

5.  **Show transactions where currency is US dollars:**

    ```sql
    SELECT * FROM transactions WHERE currency = "USD";
    ```

6.  **Show transactions in 2020 joined with the date table:**

    ```sql
    SELECT transactions.*, date.* FROM transactions INNER JOIN date ON transactions.order_date = date.date WHERE date.year = 2020;
    ```

7.  **Show total revenue in the year 2020:**

    ```sql
    SELECT SUM(transactions.sales_amount)
    FROM transactions
    INNER JOIN date ON transactions.order_date = date.date
    WHERE date.year = 2020
    AND (transactions.currency = "INR\r" OR transactions.currency = "USD\r");
    ```

8.  **Show total revenue in the year 2020, January Month:**

    ```sql
    SELECT SUM(transactions.sales_amount)
    FROM transactions
    INNER JOIN date ON transactions.order_date = date.date
    WHERE date.year = 2020
    AND date.month_name = "January"
    AND (transactions.currency = "INR\r" OR transactions.currency = "USD\r");
    ```

9.  **Show total revenue in the year 2020 in Chennai:**

    ```sql
    SELECT SUM(transactions.sales_amount)
    FROM transactions
    INNER JOIN date ON transactions.order_date = date.date
    WHERE date.year = 2020
    AND transactions.market_code = "Mark001";
    ```

## Data Analysis Using Power BI

The following Power BI formula was used to create a calculated column:

1.  **Formula to create `norm_amount` column (to normalize amounts based on currency conversion):**

    ```powerbi
    norm_amount =
        Table.AddColumn (
            #"Filtered Rows",
            "norm_amount",
            each if [currency] = "USD"
                      or [currency] = "USD#(cr)" then [sales_amount] * 75
                 else [sales_amount],
            type any
        )
    ```

    * This formula converts USD amounts to INR using an exchange rate of 75.

## Data Dictionary (Based on SQL Queries)

Based on the SQL queries, here's a brief description of the tables and columns:

* **`customers`**:
    * `customer_code` (VARCHAR): Unique code for each customer. (Primary Key)
    * `custmer_name` (VARCHAR): Name of the customer.
    * `customer_type` (VARCHAR): Type of customer (e.g., "Brick & Mortar", "E-Commerce").

* **`transactions`**:
    * (Columns inferred from queries, a full `DESCRIBE transactions;` would be more precise)
    * `order_date` (DATE): Date of the transaction.
    * `market_code` (VARCHAR): Code identifying the market.
    * `product_code` (VARCHAR): Code identifying the product.
    * `currency` (VARCHAR): Currency of the transaction.
    * `sales_amount` (Numeric): Amount of the sales transaction.

* **`date`**:
    * `date` (DATE): Date. (Primary Key)
    * `year` (INT): Year of the date.
    * `month_name` (VARCHAR): Name of the month.

## Key Insights

The SQL queries and Power BI formula demonstrate the following types of analysis:

* Customer demographics and types.
* Sales analysis by market (e.g., Chennai).
* Currency conversion and revenue calculation.
* Time-based analysis of sales revenue (e.g., yearly, monthly).
