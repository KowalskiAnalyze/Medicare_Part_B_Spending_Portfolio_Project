# Medicare Part B SQL Walkthrough

## Creating Tables

After creating our MedicarePartB database, we will create a table with the correct table schema to serve as a template for importing CSV data for each corresponding year.
```sql
-- Creates 2018 table and table schema template for importing CSV into.
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
    YEAR INT DEFAULT 2018
);
```
>Note: We added a custom YEAR column that was not present in the original CSV file. This allows us to differentiate records by year when we later perform a UNION ALL across multiple year tables.


