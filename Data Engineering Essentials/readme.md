# Data Engineering Essentials

# OLTP

## Definition: 

OLTP stands for Online Transaction Processing.

## Example: 

Consider the Amazon e-commerce portal. When a user searches for a product or makes a purchase, the interface sends a request to the backend server. The server then retrieves the necessary data from the database and displays it on the frontend.

## Architecture: 

This is a generic architecture for most web portals. Information is stored in databases, and front-facing systems retrieve data from these databases. Such databases are considered OLTP databases.

## Examples of OLTP Databases: 

MySQL, SQL Server, IBM DB2, PostgreSQL, etc.

# Characteristics:

- OLTP databases respond quickly.
- They primarily handle small amounts of data.
- They are optimized for fast reads and writes, but only for small pieces of data.
- They are not designed for large queries or historical analysis.
- Purpose: To process small amounts of data quickly.

# OLAP

## Example: 

External customers access the website and retrieve data from databases. Internal customers, such as data analysts, BI teams, and Power BI developers, also access these databases to generate reports (e.g., sales reports). These teams often process large amounts of data, such as quarterly reports. If these teams query the OLTP databases for historical analysis, the performance of the OLTP system may degrade, affecting the user experience.

## Solution: 

To avoid this, OLAP systems (data warehouses) are used. These systems are designed to handle large and complex queries efficiently. 

## The challenge: 

How does the Data Warehouse receive data from OLTP systems?

## The answer: 

ETL (Extract, Transform, Load) processes.

- OLAP databases are optimized for running complex queries efficiently.


---

# ETL (Extract, Transform, Load)

ETL processes move data from OLTP databases to OLAP systems (Data Warehouses) periodically (e.g., hourly, daily).

- Steps:

    - Extract: Retrieve data from OLTP databases.

    - Transform: Clean and structure the data (if needed).

    - Load: Insert transformed data into the Data Warehouse.

- Common ETL tools: Informatica, IBM DataStage, AWS Glue, Apache Airflow.
- ETL is a core part of Data Engineering.

--- 

# OLTP vs. OLAP Table Design

## OLTP:

- Normalized: Tables are designed to minimize redundancy and dependency, often resulting in more joins.
- Use Case: Suitable for small data sizes with fast read/write operations.
- Characteristics: 
    - More joins are required
    - Suitable for small-sized data.

## OLAP:

- Denormalized: Tables are designed using dimensional data modeling (a data warehousing concept).
- Use Case: Reduces the number of joins and keeps more data in fewer tables, making data retrieval faster.
- Characteristics: 
    - Reduces dependencies on multiple tables
    - Prefers storing maximum data in fewer tables
    - Requires fewer joins, eading to faster data retrieval
    - Read operations are frequent, while write operations are minimal.

---

# Dimensional Data Modeling (Used in Data Warehousing)

## Definition:

A modeling technique used in data warehousing.

## Characteristics:

- Fact Tables: Contain measurable business data (e.g., sales transactions).

- Dimension Tables: Store descriptive attributes (e.g., product details, customer information).

## Schema Types:

- Star Schema: Fact table at the center with direct connections to dimension tables.
- Snowflake Schema: Dimension tables are further normalized.

## Data Vault (Modern Data Warehousing Concept)

- Uses Link Tables and Satellite Tables to structure data efficiently.

- Provides scalability and better historical tracking.

#### Notes: 

Data modeling is an old concept but it comes with one disadvantage: data redundancy will be there when it is in denormalized form, which is completely fine. Data storage is cheap in today's world, while processor/computation power is higher. Fast data access is more important with higher power. Dimensional model data does not change frequently; read operations occur a lot, while write operations are relatively few.

---

# Data Marts

## Use Case

Large enterprises have multiple applications for different business functions (e.g., sales, marketing, customer management, operations, and HR), each with its own OLTP database. A single Data Warehouse collects data from all these OLTP systems. As businesses grow, Data Warehouses can become massive and complex with thousands of tables.

Solution: Break down the Data Warehouse into smaller, specialized Data Marts for specific business units or departments.

- Data Marts are smaller, specialized segments of a Data Warehouse.

- Example: A company with different applications (Sales, Marketing, Customer Management, Operations, Employee Management) will have separate OLTP databases for each.

- Instead of analyzing all tables within a massive Data Warehouse, Data Marts focus on specific business units.

## Advantages of Data Marts:

- Focus on specific business functions, reducing complexity (no worry about rest of the tables).
- Improves manageability by allowing department-specific access and permissions (Easier role-based access and permission management).
- Enhances performance by isolating relevant data.
- Uses structured data, including Fact and Dimension Tables.

## Types of Data Marts

- Independent Data Mart - Directly sourced from OLTP systems, bypassing the data warehouse.
- Dependent Data Mart - Receives data from a central Data Warehouse.
- Hybrid Data Mart - Combines data from both OLTP and Data Ware

---

# Dimension Tables in Data Modeling

- Dimension Tables store descriptive information and do not change frequently.
- Example: In a retail grocery store, daily orders (Fact Table) change frequently, but the Product Table (Dimension Table), which contains products like bread, does not.
- Dimension Tables tend to be wide (fewer rows, more columns).
- Fact Tables grow in size over time, whereas Dimension Tables remain relatively stable.

---

# Slowly Changing Dimensions (SCD)

- Used in Data Warehousing for Dimension Tables, which change infrequently.

- Example: A grocery store generates daily orders (Fact Table), while the Product Table (Dimension) rarely changes.

## Types of SCD

- SCD Type 0 - Constant; data never changes.
- SCD Type 1 - Overwrites existing data if the primary key exists; inserts new data otherwise.
- SCD Type 2 - Keeps historical data by flagging old records and inserting new ones. Often uses a surrogate primary key rather than primary key to avoid duplicacy.
- SCD Type 3 - Stores limited history in designated columns (e.g., keeping only the last five password changes). Solves the problem with SCD Type 2 of multiple rows
- SCD Type 4 - Uses an audit table to maintain full history while keeping the main table clean.
- SCD Type 5 & 6 - Less commonly used variations.