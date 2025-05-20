# Medicare Part B Walkthrough

## Creating Tables

> We will be importing each individual excel file into their own seperate table within our database.
```sql
CREATE TABLE medicare_part_b_spending (
    id SERIAL PRIMARY KEY, -- Unique row identifier
    year INT, -- Year of the spending record (2018–2022)
    state VARCHAR(2), -- 2-letter state abbreviation
    hcpcs_code VARCHAR(10), -- Healthcare Common Procedure Coding System code
    hcpcs_description TEXT, -- Description of the HCPCS item
    number_of_suppliers INT, -- Count of suppliers for the item
    number_of_services INT, -- Number of services provided
    total_submitted_charges NUMERIC(15,2), -- Total amount suppliers charged
    total_allowed_amount NUMERIC(15,2), -- Medicare-approved total amount
    total_medicare_payment_amount NUMERIC(15,2), -- Total paid by Medicare
    average_supplier_payment NUMERIC(15,2), -- Avg payment per supplier
    average_medicare_payment NUMERIC(15,2) -- Avg Medicare payment per service
);
```
