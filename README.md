Relational Database Management Systems: 
- Relational databases consist of data stored in  tables.
- Relational database management systems (RDBMS) serves as an interface to access and manage relational databases, using SQL.
- RDBMS administrators can grant access to users and set specific roles and permissions.

-- Creating a Schema
CREATE SCHEMA mythirdcodeschema DEFAULT CHARACTER SET utf8MB4;

-- Creating Table using UI

PK: Primary Key (values in that column can never repeat because they need to distinctly identify a specific record).
NN: Non-null (any record that will get inserted into this database must contain a value) 

-- Accessing myfirsttable in myfirstschema
SELECT * FROM myfirstschema.myfirsttable;

-- creating table with column names
CREATE TABLE myfirstcodetable (
    myfirstcodetable_id BIGINT NOT NULL,
    my_character_field VARCHAR(50),
    my_text_field TEXT,
    my_created_at TIMESTAMP,
    PRIMARY KEY (myfirstcodetable_id)
);

-- view the entire table
SELECT * FROM myfirstcodetable;

-- view a part of the table (some columns)
SELECT 
    my_text_field,
    my_created_at
FROM myfirstcodetable;














-- MySQL Data Types

Visit: https://www.w3schools.com/sql/sql_datatypes.asp

Assignment 1: 




















-- Create Schema
CREATE SCHEMA toms_marketing_stuff DEFAULT CHARSET utf8mb4;

USE toms_marketing_stuff;

-- Adding a table with 2 columns
CREATE TABLE publishers (
	publisher_id INT,
    publisher_name VARCHAR(65),
    PRIMARY KEY (publisher_id)
);

-- Adding another table with 3 columns
CREATE TABLE publisher_spend (
	publisher_spend_id VARCHAR(45),
	publisher_id INT,  -- cannot repeat primary key on multiple records (can have same publishers spending) 
    month DATE,
    spend DECIMAL(10,2),
    PRIMARY KEY (publisher_spend_id)
);

SELECT * FROM publishers;
SELECT * FROM publisher_spend;


-- Dropping Columns
ALTER TABLE customer_purchases
DROP COLUMN customer_id;

SELECT * FROM customer_purchases;

-- Insert Columns
ALTER TABLE customer_purchases
ADD COLUMN purchase_amount DECIMAL(10,2) AFTER customer_purchase_id;

SELECT * FROM customer_purchases;

Note: Decimal(10,2) means 10 values and 2 decimal places.
Note: Whenever you add tables, must specify data types.

Assignment 2





















-- Select Schema
USE candystore;

-- Remove hourly_wage column
ALTER TABLE employees
DROP COLUMN hourly_wage;

-- Add new column 
ALTER TABLE employees
ADD COLUMN avg_customer_rating DECIMAL(10,1);

SELECT * FROM employees;


use myfirstcodeschema; 

-- Dropping Table
DROP TABLE myfirstcodetable;

-- Dropping Schema
DROP SCHEMA mysecondcodeschema;

Assignment 3





















use candystore;

-- Dropping Table
DROP TABLE candy_products;

-- Dropping Schema
DROP SCHEMA candystore_old;

SELECT * FROM inventory;

-- Inserting 1 Record into the "inventory" table
INSERT INTO inventory VALUES
(10,'wolf skin hat',1);

-- Inserting Multiple Records
INSERT INTO inventory VALUES
(11,'fur fox skin',1),
(12,'plaid button up shirt',8),
(13,'flannel zebra jammies',6);

-- Inserting Specific Records
INSERT INTO inventory(inventory_id, item_name) VALUES
(14,'wolf skin sneakers');

Updating Records
- Update values of records using an UPDATE statement.
- The UPDATE statement will always require a SET clause, which tells the server which values to set.
- In most cases, will also use a WHERE clause (optional) to specify which records to update. If we do not use a WHERE clause, the UPDATE will process on all records.

-- Update table "inventory"
UPDATE inventory
SET number_in_stock = 0    -- we sold out
WHERE inventory_id = 9

-- Update multiple records in table
UPDATE inventory
SET number_in_stock = 0    -- we sold out
WHERE inventory_id IN (1,9)

Note: better to update ‘keys’ in the where clause. E.g., “item_name” was not the primay key so it might repeat again in the database.

















Assignment 4

























use candystore;

SELECT * FROM employees;

-- Adding records to the employees table
INSERT INTO employees VALUES
(7, 'Charles', 'Munger', '2020-03-15', 'Clerk', NULL),
(8, 'William', 'Gates', '2020-03-15', 'Clerk', NULL);

-- Adding customer ratings
UPDATE employees
SET avg_customer_rating = 4.8
WHERE employee_id = 1;

UPDATE employees
SET avg_customer_rating = 4.6
WHERE employee_id = 2;

UPDATE employees
SET avg_customer_rating = 4.75
WHERE employee_id = 3;

UPDATE employees
SET avg_customer_rating = 4.9
WHERE employee_id = 4;

UPDATE employees
SET avg_customer_rating = 4.75
WHERE employee_id = 5;

UPDATE employees
SET avg_customer_rating = 4.2
WHERE employee_id = 6;

-- Deleting a record (if no WHERE clause, removes all records)
DELETE FROM inventory
WHERE inventory_id = 7;

Autocommit
SELECT @@autocommit;
SET autocommit = OFF;  -- or 0

-- Deleting a record 
DELETE FROM inventory
WHERE inventory_id = 7;

-- undo the deletion because autocommit = 0;
ROLLBACK;

-- now rollback is no longer useful, it has been committed
COMMIT;

Truncate
- If we want to remove all records from a table but preserve the table structure, we can do that using TRUNCATE TABLE.
- The data is removed but the column names, data types, column order, and any constraints placed on the table are all preserved.
- TRUNCATE TABLE is very similar to using DELETE without a WHERE clause (but there are differences).
- DELETE can be rollback, TRUNCATE cannot be rollback.

DDL, DML, DQL, DCL, DTL
Data Definition Language (DDL)
Used to create or modify the structure of a database. 
E.g., CREATE, ALTER, DROP, TRUNCATE
Data Manipulation Language (DML)
Used to add, modify, or delete data records. 
E.g., INSERT, UPDATE, DELETE
Data Query Language (DQL) 
Used to query data (often considered part of DML). 
E.g., SELECT, SHOW, HELP
Data Control Language (DCL) 
Used to grand and revoke permissions.
E.g., GRANT, REVOKE
Data Transaction Language (DTL)
Used to manage transactions
E.g., START TRANSACTION, COMMIT, ROLLBACK

use thriftshop;

SELECT * FROM customers;

SELECT @@autocommit;
SET autocommit = OFF;

-- can be rollback
DELETE FROM customers
WHERE customer_id BETWEEN 1 AND 6;

ROLLBACK;

-- cannot be rollback
TRUNCATE TABLE customers;

Assignment 5





















use candystore;

SELECT * FROM employees;

-- SELECT @@autocommit;
-- SET autocommit = OFF;

-- Delete a record from the 'employees' table
DELETE FROM employees
WHERE employee_id = 4;

-- ROLLBACK;
-- COMMIT;

-- Remove all data from the customer reviews table but preserve structure
DELETE FROM customer_reviews
WHERE customer_review_id BETWEEN 1 AND 33;

TRUNCATE TABLE customer_reviews;

SELECT * FROM customer_reviews;


Table Relationships & Cardinality
- Cardinality refers to the uniqueness of values in a column (or attribute) of a table and is commonly used to describe how 2 tables relate (one-to-one, one-to-many, many-to-many).
- Foreign key: can have repeated values, so there may be many instances of each foreign key value in a column. 
- Primary key: only one unique value, so there is only one instance of each primary key value in a column. 
- One-to-many: connecting a foreign key in one table to a primary key in another. 

One-To-Many Relationship

































Assignment 6




















use onlinelearningschool;

-- FK (course_id, course_name)
-- PK (rating_id)
SELECT * FROM course_ratings;

-- FK (course_id)
SELECT * FROM course_ratings_summaries;

-- PK (course_id)
SELECT * FROM courses;




















 Database Normalization: 
- Normalization is the process of structuring the tables and columns in a relational database to minimize redundancy and preserve data integrity. Benefits of normalization include: eliminating duplicate data (this makes storage and query processing more efficient) and reducing errors & anomalies (restrictions around data structure help to prevent human errors).

- In practice, normalization involves breaking out data from a single merged table into multiple related tables. Instead of storing redundant information about each store and film in a single table (like the one on the left), we create new tables containing a single record for each unique value, and link to those tables using a simple id.


use mavenfuzzyfactorymini;

SELECT * FROM website_pageviews_non_normalized
WHERE website_session_id = 20;

-- Create Table with pageviews normalized, FK (website_session_id), PK (website_pageview_id) 
CREATE TABLE website_pageviews_normalized
SELECT 
	website_pageview_id,
    created_at,
    website_session_id, 
    pageview_url
FROM website_pageviews_non_normalized;

SELECT * FROM website_pageviews_normalized;

-- There are duplicate rows. Thus, need to use Keyword 'DISTINCT' to obtain a unique row.
CREATE TABLE website_session_normalized
SELECT DISTINCT
	website_session_id,
    session_created_at, 
    user_id,
    is_repeat_session,
    utm_source, 
    utm_campaign,
    utm_content,
    device_type,
    http_referer
FROM website_pageviews_non_normalized;

SELECT * FROM website_session_normalized;

Assignment 7
























USE onlinelearningschool;

SELECT * FROM courses;

SELECT * FROM course_ratings;

SELECT * FROM course_ratings_summaries;

CREATE TABLE course_ratings_normalized
SELECT 
	rating_id,
    course_id,
    star_rating
FROM course_ratings;

-- or can alter and drop column in course_ratings.





Enhanced Entity Relationship (EER) Models

As we design our databases, creating EER diagrams help to visually model the relationships between tables and the contraints within those tables.

EER diagrams can help us map out things like: 
- which tables are in the database
- which columns exist in each table
- the data types of the various columns
- primary and foreign keys within tables
- relationship carindality between tables
- constraints on columns (i.e., Non-NULL)








































Assignment 7













































Note: As a best practice, every table in a database should contain at least 1 primary key and at least 1 foreign key. 




Mid-Term Project



use mavenmoviesmini;

SELECT * FROM inventory_non_normalized;

CREATE SCHEMA mavenmoviesmini_normalized;
USE mavenmoviesmini_normalized;

CREATE TABLE inventory (
	inventory_id INT NOT NULL,
    film_id INT NOT NULL,
    store_id INT NOT NULL,
	PRIMARY KEY (inventory_id)
    );
    
CREATE TABLE film (
	film_id INT NOT NULL,
    title VARCHAR(255) NOT NULL,
    description VARCHAR(255) NOT NULL,
    release_year INT NOT NULL,
    rental_rate DECIMAL(6,2) NOT NULL,
    rating VARCHAR(45) NOT NULL,
    PRIMARY KEY (film_id)
    );
    
CREATE TABLE store (
	store_id INT NOT NULL,
    store_manager_first_name VARCHAR(45) NOT NULL,
    store_manager_last_name VARCHAR(45) NOT NULL,
    store_address VARCHAR(45) NOT NULL,
    store_city VARCHAR(45) NOT NULL,
    store_district VARCHAR(45) NOT NULL,
    PRIMARY KEY (store_id)
);

INSERT INTO inventory (inventory_id, film_id, store_id)
SELECT DISTINCT inventory_id, film_id, store_id
FROM mavenmoviesmini.inventory_non_normalized;

SELECT * FROM inventory;

INSERT INTO film (film_id, title, description, release_year, rental_rate, rating)
SELECT DISTINCT film_id, title, description, release_year, rental_rate, rating
FROM mavenmoviesmini.inventory_non_normalized;

SELECT * FROM film;

INSERT INTO store (store_id, store_manager_first_name, store_manager_last_name, store_address, store_city, store_district)
SELECT DISTINCT store_id, store_manager_first_name, store_manager_last_name, store_address, store_city, store_district
FROM mavenmoviesmini.inventory_non_normalized;

SELECT * FROM store;






















Indexes: 
























-- Add Index
ALTER TABLE `candystore`.`customer_reviews` 
ADD INDEX `emplyee_id` (`employee_id` ASC) VISIBLE;
;

-- Drop Index
ALTER TABLE `candystore`.`customer_reviews` 
DROP INDEX `employee_id` ;
;





Unique Constraint
- Can prescribe which columns in our tables we allow to have repeating values, and which columns must be unique.
- Including a unique constraint on columns that should not repeat is a good way to ensure data integrity, and is a good best practice.

By UI


By SQL Code:

use thriftshop;

SELECT * FROM inventory;

-- Code for Unique Constraint
ALTER TABLE thriftshop.inventory
ADD UNIQUE INDEX item_name_UNIQUE (item_name ASC) VISIBLE;

-- This will generate an error
INSERT INTO inventory VALUES
(15, 'fur fox skin',1);
  
Error Code: 1062. Duplicate entry 'fur fox skin' for key 'inventory.item_name_UNIQUE'


Not Null
- Want certain columns to be populated with a value for every single record that ever gets written.
- Apply  a NON-NULL constraint to a certain column, which requires all records to have a value in that column.
- If the NON-NULL constraint is on a column, and someone tries to INSERT a record without including a value for the column, the INSERT will fail. 

use thriftshop;

SELECT * FROM inventory;

INSERT INTO inventory(inventory_id, item_name) VALUES (14, 'hot new product');

DELETE FROM inventory WHERE inventory_id = 14;

ALTER TABLE `thriftshop`.`inventory` 
CHANGE COLUMN `number_in_stock` `number_in_stock` BIGINT NOT NULL ;

Error Code: 1364. Field 'number_in_stock' doesn't have a default value















Assignment: 
























use sloppyjoes;

SELECT * FROM customer_orders;
-- Index (order_id), NOT NULL (order_total)

SELECT * FROM menu_items;
-- Index (menu_item_id), UNIQUE (item_name), NOT NULL (price, item_name)

SELECT * FROM staff;
-- Index (staff_id), NOT NULL (first_name, last_name, orders_served)

UPDATE customer_orders
SET order_total = 0
WHERE order_id IN (3,8,12,16,19);













Stored Procedure
- A stored procedure is a prepared SQL code that you can save, so the code can be reused over and over again.
- Delimiter: separates statements and executes each statement separately.

- First, change the default delimiter to //
- Second, use (;) in the body of the stored procedure and // after the END keyword to end the stored procedure.
- Third, change the default delimiter back to a semicolon (;)

use thriftshop;

SELECT * FROM inventory;

-- changing the delimiter
DELIMITER // 
-- creating the procedure
CREATE PROCEDURE sp_selectAllInventory()
BEGIN
	SELECT * FROM inventory;
END //
-- changing the delimiter back to the default
DELIMITER ;

-- Calling the procedure that we have created
CALL sp_selectAllInventory();

-- if we later want to DROP the procedure, we can use the this...
DROP PROCEDURE IF EXISTS sp_selectAllInventory;

Assignment: Stored Procedures





















use sloppyjoes;

DELIMITER //
CREATE PROCEDURE sp_staffOrdersServed() -- start here
BEGIN
	SELECT
		staff_id,
        COUNT(order_id) AS orders_served
	FROM customer_orders
    GROUP BY staff_id;
END //

DELIMITER ;

CALL sp_staffOrdersServed();

DROP PROCEDURE IF EXISTS sp_staffOrdersServed;

CALL sp_staffOrdersServed(); -- should have error coz it does not exist anymore


Triggers
- Create Triggers where we can prescribe certain actions on a table to trigger one or more other actions to occur.
- May prescribe that our triggered action occurs either BEFORE or AFTER an INSERT, UPDATE, or a DELETE.
- Triggers are a common way to make sure related tables remain in sync as they are updated over time. 

use thriftshop;

SELECT * FROM inventory;
SELECT * FROM customer_purchases;

-- Whenever customer_purchases table is updated, the inventory table gets updated. 
CREATE TRIGGER purchaseUpdateInventory
AFTER INSERT ON customer_purchases
FOR EACH ROW 
	UPDATE inventory
		-- subtracting an item for each purchase
        SET number_in_stock = number_in_stock - 1
	WHERE inventory_id = NEW.inventory_id;
    
INSERT INTO customer_purchases VALUES
(13,NULL,3,NULL), -- inventory_id = 3, velour jumpsuit
(14,NULL,4,NULL);


use thriftshop;

SELECT * FROM inventory;

-- Code for Unique Constraint
ALTER TABLE thriftshop.inventory
ADD UNIQUE INDEX item_name_UNIQUE (item_name ASC) VISIBLE;

-- This will generate an error
INSERT INTO inventory VALUES
(15, 'fur fox skin',1);
  
Error Code: 1062. Duplicate entry 'fur fox skin' for key 'inventory.item_name_UNIQUE'

Assignment: Triggers





















use sloppyjoes;

SELECT * FROM staff;
SELECT * FROM customer_orders;

CREATE TRIGGER updateOrder
AFTER INSERT ON customer_orders
FOR EACH ROW 
	UPDATE staff
    SET orders_served = orders_served + 1
    WHERE staff.staff_id = NEW.staff_id;
    
INSERT INTO customer_orders VALUES
(21, 1, 10.98),
(22, 2, 5.99),
(23, 2, 7.99),
(24, 2, 12.97);

SELECT * FROM staff

-- to drop trigger
-- DROP TRIGGER IF EXISTS updateOrder


Server Management
- The Administration tab gives up options to monitor and control the status of the MySQL Server we are connected to.
- The Server Status view gives us a quick view of our Server.
- The Startup / Shutdown tool allows us to Start or Stop our database instance.
- Can restart the server

User Management
- Workbench makes it easy for us to add Users and to manage their Privileges.
- Users can be granted varying levels of permissions. At one end of the spectrum, you could create a User than can only SELECT, and at the other end of the spectrum is the DBA role, which carries all privileges.


Final Assignment



text






use nodelogin;

CREATE TABLE IF NOT EXISTS accounts (
username varchar(50) not null, 
password varchar(255) not null,
email varchar(100) null, 
groupName varchar(100) null, 
role varchar(100) not null, 
primary key(username),
foreign key(groupName) references userGroup (groupName)
);

CREATE TABLE IF NOT EXISTS userGroup (
username varchar(50) not null, 
groupName varchar(100) not null, 
role varchar(100) not null, 
primary key (groupName, username)
);






