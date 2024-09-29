# Lux Tech Academy & Data Science East Africa Bootcamp

### Data Analysis and Analytics Project Based Learning Bootcamp

#### This file contains week 1 project assignment for the Bootcamp training from September 23<sup>rd</sup> to October 11<sup>th</sup>, 2024

#### Project assignment

- You are provided with a table called Sales, which contains the following columns:
  - SaleID: A unique ID for each sale.
  - ProductID: The ID of the product sold.
  - CustomerID: The ID of the customer who made the purchase.
  - SaleDate: The date the sale occurred.
  - Amount: The amount of the sale.
- Questions- Write SQL scripts that:
  - Finds the total sales for each product.
  - Calculates the total sales for each month.
  - Identifys customers who made more than 5 purchases.

#### Solution

    + This Sequel query demonstrates how to create the given table while maintaining the integrity of the data and following the correct naming rules and conventions.
    + In this project, the SQL statements are specific to PostgreSQL which is my DBMS of choice.

#### Create sales table

    ```
    CREATE TABLE sales (
        sales_id bigserial UNIQUE
        product_id bigserial
        customer_id bigserial
        sale_date timestamp WITH time zone
        amount numeric(15,2)
    )
    ```

#### Breakdown

    + This table design follows the given rules as
        - Uses the **CREATE TABLE** to create a table with the name **sales**
        - Indicates the column names the table contains with their correct data types and necessary constraints to define the kind of data each column should accept.
        - Note that for the first three columns for the ID's I use the bigserial integer type that is auto-incrementing.

    + The next step I presume would be to import the data from your source and populate the table to work with in the database.
    + The following would be the Import statement(I assume the data exists in a CSV file)

#### Import table

    ```
    COPY sales
    FROM 'C:\YourDirectory\sales.csv'
    WITH (FORMAT CSV, HEADER);
    ```

#### Breakdown

    + The **COPY** statement indicates that we to import the data into the sales table
    + The **FROM** statement indicates the exact location address of your CSV file in your local machine(in this case we assume we are working in a Windows machine)
    + The **WITH** keyword specifies the nature of the file, in this case it is a CSV file and also uses the **HEADER** keyword to speccify that we should exclude the files header rows

##### So far with the **CREATE TABLE** and **COPY** scripts, we have a working table with all the columns and rows we need to get our hands dirty on

##### The following scripts will answer the questions that we have

- Question 1: Find the total sale for each product
- Solution:

  ```
  SELECT
  product_id,
  sum(amount) AS total_sale
  FROM sales;
  ```

- Calculate the total sales for each month.
- Solution:

  ```
  SELECT
  DATE_TRUNC('month',sale_date) AS month,
  sum(amount) AS monthly_total_sale
  FROM sales
  GROUP BY month
  ORDER BY month;
  ```

- Identify customers who made more than 5 purchases.
- Solution:

  ```
  SELECT
  customer_id,
  COUNT(*) AS total_purchases
  FROM sales
  GROUP BY customer_id
  HAVING COUNT(*)>5;
  ```
