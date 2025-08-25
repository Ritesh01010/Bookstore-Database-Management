
# Bookstore Database Management System

## 📖 Introduction

This project is a **relational database system** designed to manage a bookstore's inventory, customer information, and sales orders. The system provides a centralized way to handle data, allowing for efficient querying to gain business insights, track sales, and manage stock levels.

-----

## ⚙️ Database Design

The database is built around three main tables: `Books`, `Customers`, and `Orders`. This structure is normalized to reduce data redundancy and maintain data integrity.

### Database Schema

  * **Books**: Contains all details about the book inventory.
      * `Book_ID` (Primary Key)
      * `Title`
      * `Author`
      * `Genre`
      * `Published_Year`
      * `Price`
      * `Stock`
  * **Customers**: Stores information about each customer.
      * `Customer_ID` (Primary Key)
      * `Name`
      * `Email`
      * `Phone`
      * `City`
      * `Country`
  * **Orders**: A transactional table that links books and customers for each sale.
      * `Order_ID` (Primary Key)
      * `Customer_ID` (Foreign Key)
      * `Book_ID` (Foreign Key)
      * `Order_Date`
      * `Quantity`
      * `Total_Amount`

-----

## 🚀 Getting Started

To get this database up and running on your local machine, follow these steps.

### Prerequisites

You need a SQL environment to execute the script, such as PostgreSQL.

### Installation

1.  **Create the Tables**: Run the DDL statements in the `Bookstore Database Management.sql` file to create the `Books`, `Customers`, and `Orders` tables.

2.  **Import Data**: The SQL script includes `COPY` commands to import data from CSV files. You will need to **update the file paths** in the script to point to the location of your `Books.csv`, `Customers.csv`, and `Orders.csv` files.

    ```sql
    -- Example of the command to update
    COPY Books(Book_ID, Title, Author, Genre, Published_Year, Price, Stock)
    FROM 'YOUR_FILE_PATH\Books.csv'
    CSV HEADER;
    ```

-----

## 📊 Example Queries

The SQL script includes several queries to analyze the bookstore's data. Here are a few examples:

### Find the Most Frequently Ordered Book

This query helps identify the best-selling book.

```sql
SELECT b.title, COUNT(o.order_id) AS ORDER_COUNT
FROM orders o
JOIN books b ON o.book_id = b.book_id
GROUP BY b.title
ORDER BY ORDER_COUNT DESC
LIMIT 1;
```

### Find the Customer Who Spent the Most

This helps identify the most valuable customer.

```sql
SELECT c.name, SUM(o.total_amount) AS Total_Spent
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
GROUP BY c.name
ORDER BY Total_spent DESC
LIMIT 1;
```

### Calculate Remaining Stock

A crucial query for inventory management.

```sql
SELECT
    b.title,
    b.stock,
    COALESCE(SUM(o.quantity), 0) AS Order_quantity,
    b.stock - COALESCE(SUM(o.quantity), 0) AS Remaining_Quantity
FROM books b
LEFT JOIN orders o ON b.book_id = o.book_id
GROUP BY b.book_id
ORDER BY b.book_id;
```
