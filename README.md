# ETL Project using SSIS

This project involves creating an ETL (Extract, Transform, Load) process using SQL Server Integration Services (SSIS). The goal is to extract data from source systems, transform it as necessary, and load it into a Data Warehouse.

## Project Overview

The ETL process is designed to load data into the following tables:
- **Dimension Tables:**
  - `DimCustomer`: Contains customer information.
  - `DimProduct`: Contains product details.
  - `DimTime`: Contains time-related data.
- **Fact Table:**
  - `FactSales`: Stores sales transaction data.

## Prerequisites

- **SQL Server**: Ensure that SQL Server is installed and running.
- **SSIS**: Install SQL Server Data Tools (SSDT) to create and manage SSIS packages.
- **Database Connection**: Make sure you have a valid OLE DB connection to the SQL Server instance where your data warehouse is hosted.

## Steps to Create ETL Packages

### 1. Extract Data

- **Data Flow Task**: Add a Data Flow Task to your SSIS package.
- **OLE DB Source**: Use an OLE DB Source to connect to the source database.
  - Configure the connection and SQL query to select the required data.
  - Example query:
    ```sql
SELECT 
    fc.CustomerID, 
    fc.CustomerName, 
    fp.ProductID, 
    fp.ProductName, 
    fs.QuantitySold, 
    fp.UnitPrice,
    ft.Date
FROM 
    salesDWH.FactSales fs 
INNER JOIN salesDWH.DimCustomer fc ON fs.CustomerID = fc.CustomerID
INNER JOIN salesDWH.DimProduct fp ON fs.ProductID = fp.ProductID
INNER JOIN salesDWH.DimTime ft ON fs.TimeID = ft.TimeID;
    ```

### 2. Transform Data

- **Transformation Components**: Add necessary transformation components in the Data Flow Task.
  - **Data Conversion**: Convert data types if needed.
  - **Lookup**: Use Lookup transformations to enrich data with information from other tables.
  - **Derived Column**: Create new columns or transform existing ones using expressions.

### 3. Load Data

- **OLE DB Destination**: Use an OLE DB Destination to load data into the Data Warehouse.
  - Map the transformed data to the corresponding destination tables (e.g., `DimCustomer`, `DimProduct`, `FactSales`,`DimTime`).
