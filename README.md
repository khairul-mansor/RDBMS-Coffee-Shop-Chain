# RDBMS-Coffee-Shop-Chain

## Project : Database Design and Implementation

In this project, a New York based coffee shop chain is looking to expand nationally by opening a number of franchise locations. As part of their expansion process, they want to streamline operations and revamp their data infrastructure.

My objective is to design their relational database systems for improved operational efficiencies and to make it easier for their executives to make data driven decisions.

Currently their data resides in several different systems: accounting software, suppliers’ databases, point of sales (POS) systems, and even spreadsheets. I reviewed the data in all of these systems and designed a central database to house all of the data. I created the database objects and loaded them with source data. Finally, I created subsets of data that the business partners require, exported them, and then loaded them into staging databases that use different RDBMS.

## Software used in this project
In this project, I used PostgreSQL Database, IBM Db2 Database, and MySQL. These are all relational database management systems (RDBMS) designed to efficiently store, manipulate, and retrieve the data.

  

## Data used in this project
In this project, I worked with a subset of data from the Coffee shop sample data (https://community.ibm.com/community/user/businessanalytics/blogs/steven-macko/2019/07/12/beanie-coffee-1113?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDB0110ENSkillsNetwork24601058-2022-01-01).

I used a modified version of the data for the project.

I worked with data from the following sources:

    Staff information held in a spreadsheet at HQ
    Sales outlet information held in a spreadsheet at HQ
    Sales data output as a CSV file from the POS system in the sales outlets
    Customer data output as a CSV file from a bespoke customer relationship management system
    Product information maintained in a spreadsheet exported from your supplier's database

## Objectives

    Identify entities.
    Identity attributes.
    Create an entity relationship diagram (ERD) using the pgAdmin ERD Tool.
    Normalize tables.
    Define keys and relationships.
    Create database objects by generating and running the SQL script from the ERD Tool.
    Create a view and export the data.
    Create a materialized view and export the data.
    Import data into a Db2 database.
    Import data into a MySQL database.


### Identify entities
The first step when designing a new database is to review any existing data and identify the entities for the new system.

The following image shows sample data from each of the data sources that I worked with to design the new central database. I reviewed the image and identified the entities that I planned to create.


![existing_data](https://user-images.githubusercontent.com/63607240/180668418-e921e488-29e4-4004-86f5-9a001902dbe0.png)


### Identify attributes

I identified the attributes for each of the entities that I planned to create.


### Create an ERD

Now that I have defined some of the attributes and entities, I can determine the tables and columns for them and create an ERD.

Open a new terminal.

Use the start_postgres command to start a PostgreSQL service session.

Use the pgAdmin weblink to open pgAdmin in a new tab in the browser.

Create a new database named COFFEE, view the schemas in the new COFFEE database, and then start a new ERD project.

Add a table to the ERD for the sale transactions entity using the information above. Consider what naming convention to use so that the colleagues will be able to understand the data and to ensure that the names are valid in other RDBMS. And use the sample data to determine appropriate data types for each column.


Add a table to the ERD for the product entity using the information above. Consider what naming convention to use so that the colleagues will be able to understand the data and to ensure that the names are valid in other RDBMS. And use the sample data to determine appropriate data types for each column.



### Normalize tables

When reviewing the ERD I noticed that it does not conform to second normal form. I normalized some of the tables within the database.

Review the data in the sales transaction table. Note that the transaction id column does not contain unique values because some transactions include multiple products.

Determine which columns should be stored in a separate table to remove the repeating rows and to put this table into second normal form.

Add a new table named sales_detail to the ERD, define the columns in the new table, and delete the moved columns from the sales transaction table, leaving a matching column in each of two tables to later create a relationship between them.

Review the data in the product table. Note that the product category and product type columns contain redundant data.

Determine which columns should be stored in a separate table to reduce redundant data and to put this table into second normal form.

Add a new table named product_type to the ERD, define the columns in the new table, and delete the moved columns from the product table, , leaving a matching column in each of two tables to later create a relationship between them.

### Define keys and relationships

After normalizing the tables, I defined their primary keys and defined relationships between the tables in the ERD.

Identify an appropriate column in each table to be a primary key and create the primary keys in the tables in the ERD.


Identify the relationships between the following pairs of tables and then create the relationships in the ERD:

    sales_detail to sales_transaction
    sales_detail to product
    product to product_type


### Create database objects by generating and running the SQL script from the ERD Tool

Now that the design is complete, I generated an SQL script from the ERD which I can use to create the database schema. 

Use the Generate SQL functionality in the ERD Tool to create an SQL script from the ERD.

Download the GeneratedScript.sql file to the local computer storage.

In pgAdmin, open the Query Tool, upload and open the GeneratedScript.sql file from the local computer storage, and then execute the script to create the tables defined in the ERD. Verify that the tables now exist in the public schema of the COFFEE database.

Download the CoffeeData.sql file to your local computer storage.

In pgAdmin, open another instance of the Query Tool, upload and open the CoffeeData.sql file from the local computer storage, and then execute the script to populate the tables that has just been created.

In pgAdmin, view the first 100 rows of the sales_detail table.

### Create a view and export the data
The external payroll company have requested a list of employees and the locations at which they work. This should not include the CEO or CFO who own the company. I created a view in the PostgreSQL database that returns this information and export the results to a CSV file.

In the COFFEE database, create a new view named staff_locations_view using the following SQL:

    SELECT staff.staff_id,
    staff.first_name,
    staff.last_name,
    staff.location
    FROM staff
    WHERE "position" NOT IN ('CEO', 'CFO');

View all the rows returned from the view.

### Create a materialized view and export the data

A marketing consultant requires access to the product data in their MySQL database for a marketing campaign. I created a materialized view in the PostgreSQL database that returns this information and export the results to a CSV file.

In the COFFEE database, create a new materialized view named product_info_m-view using the following SQL:

    SELECT product.product_name, product.description, product_type.product_category
    FROM product
    JOIN product_type
    ON product.product_type_id = product_type.product_type_id;

Refresh the materialized view with data.

View all the rows returned from the view.

Save the results of the query to a file named product_info_m-view.csv on the local computer storage.


### Import data into a Db2 database

The external payroll company have asked to upload the staff location information to their Db2 database.

In a new browser tab, go to https://cloud.ibm.com/login, log in using the credentials, and then open a console for the Db2 on Cloud instance

Use the Load Data feature to load a new table named STAFF_LOCATIONS with the staff location information saved in the staff_locations_view.csv file

Explore the new table and then view the data in it.


### Import data into a MySQL database

The marketing consultant has asked to upload the product information to their MySQL database.

In the terminal, use the start_mysql command to start a MySQL service session.

Use the browser weblink to open phpMyAdmin in a new tab in the browser.

In phpMyAdmin, create a new database named coffee_shop_products, and then import the product information saved in the product_info_m-view.csv file from the materialized view into a new table in the coffee_shop_products database.

Browse the contents of the new table.
