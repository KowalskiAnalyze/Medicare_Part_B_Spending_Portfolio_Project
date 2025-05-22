# Medicare Part B SQL Walkthrough

## Importing CSV Files to SQL

After creating our MedicarePartB database, we will create a table with the correct table schema to serve as a template for importing CSV data for each corresponding year:
```sql
-- Creates 2018 table and table schema template for importing CSV files into.
CREATE TABLE "2018" (
    RFRG_PRVDR_GEO_LVL VARCHAR(50) NOT NULL,
    RFRG_PRVDR_GEO_CD VARCHAR(2),
    RFRG_PRVDR_GEO_DESC VARCHAR(100),
    RBCS_LVL VARCHAR(50) NOT NULL,
    RBCS_ID CHAR(6) NOT NULL,
    RBCS_DESC TEXT NOT NULL,
    HCPCS_CD CHAR(5) NOT NULL,
    HCPCS_DESC TEXT NOT NULL,
    SUPLR_RENTL_IND CHAR(1) NOT NULL,
    TOT_RFRG_PRVDRS INT NOT NULL,
    TOT_SUPLRS INT NOT NULL,
    TOT_SUPLR_BENES INT,
    TOT_SUPLR_CLMS INT NOT NULL,
    TOT_SUPLR_SRVCS INT NOT NULL,
    AVG_SUPLR_SBMTD_CHRG NUMERIC(10, 2) NOT NULL,
    AVG_SUPLR_MDCR_ALOWD_AMT NUMERIC(10, 2) NOT NULL,
    AVG_SUPLR_MDCR_PYMT_AMT NUMERIC(10, 2) NOT NULL,
    AVG_SUPLR_MDCR_STDZD_AMT NUMERIC(10, 2) NOT NULL,
    YEAR INT DEFAULT 2018 NOT NULL -- Make sure this value matches the name of the table.
);
```
> **NOTE**: We added a custom YEAR column that does not exist in the original CSV file. This allows us to differentiate records by year when we later perform a UNION ALL across multiple year tables.

Now that we have our **"2018"** table to use as a template, we can save time and create the other table years for our analysis:

> Right-click on Tables (1) and select "*Create* > *Table...*".

![image](https://github.com/user-attachments/assets/f875bf33-4187-4122-b91a-b429b0ab96d3)

> Enter the year of the next CSV file to import in the **Name** section.

![image](https://github.com/user-attachments/assets/c9a276bb-afd7-437d-915b-7ff3331cbdbf)

> Go to the **Columns** tab, dropdown the *"Select to inherit from..."* and select **public."2018"** to copy the schema from the 2018 table.
> 
> (If this option does not appear, try refreshing the database tree in the left panel.) 

![image](https://github.com/user-attachments/assets/1539967c-5644-4caa-bd8b-ffbfe83a5c17)

> All the columns names should have copied over from the 2018 table.
> Once everything looks correct, click the blue **"Save"** button.

![image](https://github.com/user-attachments/assets/a1357511-df9d-4a2e-9400-737fefa5837f)

A new empty table labeled **"2019"** is now available for us. Before we import the corresponding CSV file, we need to make sure to change the default value for the year column. This is important because when we eventually combined all the records, we need to know which record belongs to which year.

> Right click the **"2019"** table and click *"Properties..."*. Go to the **Columns** tab and update the default value for the **year** column to **2019**.
>
> Click the blue **"Save"** button once the correct year is updated.

![image](https://github.com/user-attachments/assets/e6b08ce0-cf05-42fd-ae7d-e24492887f59)

> Now we're ready to import the **2019.csv** file into SQL. Right-click on the **2019** table, select *"Import/Export Data..."* and find the corresponding CSV file.

![image](https://github.com/user-attachments/assets/180d233e-69fa-4f9e-867b-84e16c7ee9bc)

> Go to the **Options** tab and make sure **"Header"** is checked on.

![image](https://github.com/user-attachments/assets/94e45231-2dcc-4b6f-82d8-dbd3dc079a04)

> Lastly, go to the **Columns** tab and remove **"year"** in the *"Columns to import"* list since that is an additional custom column we created which is not in the original CSV file.
>
> After **year** is removed from the list, click the blue **OK**.

![image](https://github.com/user-attachments/assets/5c95e85f-00d3-4504-993c-4ee92e81896e)

Be sure to check each table to verify the year column values match the name of the table: 
```sql
-- Checks the year value for the year column for table 2019 along with all the other columns.
SELECT
    YEAR,
    *
FROM  "2019";
```

If you forgot to update the default value in the table's properties before importing, you can always manually set these values with a query:

```sql
-- Updates the correct value by matching the year value to the correct table year.
UPDATE "2019"
SET
    YEAR='2019';
```
Make sure to repeat these steps until you have all 5 tables imported for each year.

## Combining Tables

Now that we have all 5 tables for each year, we will create a common table expression using the WITH and UNION ALL functions. 

```sql
-- Creates a temporary table for all the records between the years 2018-2022.
WITH
    COMBINED_DATA AS (
        SELECT
            *
        FROM
            "2018"
        UNION ALL
        SELECT
            *
        FROM
            "2019"
        UNION ALL
        SELECT
            *
        FROM
            "2020"
        UNION ALL
        SELECT
            *
        FROM
            "2021"
        UNION ALL
        SELECT
            *
        FROM
            "2022"
    )
SELECT
    *
FROM
    COMBINED_DATA;
```
This should create a new dataset which has a total of 367781 records.

> **NOTE**: For readability and to save space, we’ll omit the full CTE definition in the remaining queries. Assume that the CTE has been declared above each future query in this walkthrough and is available for use.

You can always double-check by verifying that all the years are present:

```sql
-- Selects one of each year from the year column in the combined table for verfication.
SELECT DISTINCT
    YEAR
FROM
    COMBINED_DATA
ORDER BY
    YEAR;
```
> **Result:**
> |year |
> |:--- |
> |2018 |
> |2019 |
> |2020 |
> |2021 |
> |2022 |

## Filtering to State-Level

If we take a look at the **"rfrg_prvdr_geo_desc"** column, we can notice that this dataset includes national data:

```sql
-- Showcases the different regions included in the datasets.
SELECT DISTINCT
    RFRG_PRVDR_GEO_DESC
FROM
    COMBINED_DATA;
```
This analysis focuses on Medicare Part B data from the 50 U.S. states. Regions such as U.S. territories, military postal codes, and ambiguous entries (e.g., ‘Unknown’, ‘National’) will be excluded to maintain a consistent state-level comparison. We will be using a WHERE function to filter out these regions:

```sql
SELECT
    *
FROM
    COMBINED_DATA
WHERE
    RFRG_PRVDR_GEO_DESC NOT IN (
        'Puerto Rico',
        'Guam',
        'Virgin Islands',
        'Armed Forces Europe',
        'Armed Forces Pacific',
        'Foreign Country',
        'District of Columbia',
        'National',
        'Unknown',
        'Northern Mariana Islands',
        'Armed Forces Central/South America'
    )
    AND RFRG_PRVDR_GEO_DESC IS NOT NULL;
```
We can verify our filter by counting the number of distinct states grouped by year. If done correctly, each year should only have 50 states:

```sql
SELECT
    YEAR,
    COUNT(DISTINCT RFRG_PRVDR_GEO_DESC) AS NUMBER_OF_STATES
FROM
    COMBINED_DATA
WHERE
    RFRG_PRVDR_GEO_DESC NOT IN (
        'Puerto Rico',
        'Guam',
        'Virgin Islands',
        'Armed Forces Europe',
        'Armed Forces Pacific',
        'Foreign Country',
        'District of Columbia',
        'National',
        'Unknown',
        'Northern Mariana Islands',
        'Armed Forces Central/South America'
    )
    AND RFRG_PRVDR_GEO_DESC IS NOT NULL
GROUP BY
    YEAR
ORDER BY
    YEAR;
```
> **Result:**
> |year |number_of_states | 
> |:--- |---:| 
> |2018 | 50|
> |2019 | 50|
> |2020 | 50|
> |2021 | 50|
> |2022 | 50|







