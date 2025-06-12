# Medicare Part B: State-Level Spending Patterns (2018–2022)

## Background & Overview
Established in 1965, Medicare is a vital federal program that provides access to affordable medical services for over 67 million beneficiaries. It is primarily funded through payroll taxes, general federal revenues, and enrollee premiums. Because it’s publicly funded, every dollar spent must be justified to maintain a balance between patient access, cost-efficiency, and long-term sustainability.

Medicare is divided into four parts—this project focuses on Part B, which covers outpatient services, including physician visits, preventive care, and durable medical equipment, prosthetics, orthotics, and supplies (DMEPOS).

This analysis uses publicly available datasets from the Centers for Medicare & Medicaid Services (CMS), detailing Medicare Part B spending on DMEPOS from 2018 to 2022, broken down by state and HCPCS code.

The goal is to evaluate state-level spending behavior using the following key performance indicators (KPIs):

- **Medicare Spending per HCPCS Code**
- **Total Yearly State-Level Medicare Part B Spending**
- **Total Number of Suppliers per Region**
- **Average Supplier Medicare Payment Amount**
- **Supplier Vs Medicare Charge Comparison**
- **Medicare Payment Coverage Rate**
  
Analysis and recommendations are provided on the following key areas:

- **Regional Comparisons:** *Evaluate which states drive the highest Medicare spending and identify regional outliers.*
- **Yearly Spending Trends:** *Track how total Medicare Part B state-level spending has changed year-over-year to highlight rising costs or improved efficiencies.*
- **Supplier Behavior and Pricing:** *Examine gaps between supplier-submitted charges, Medicare-approved rates, and actual Medicare payments to flag irregular pricing practices.*
- **High-Cost Item Concentration:** *Evaluate which DMEPOS account for the largest share of state-level spending.*

An interactive Tableau dashboard can be found [here](https://public.tableau.com/views/MedicarePartBDashboard2018-2022/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link).

A SQL query walkthrough showcasing the combining and cleaning of data can be found [here](sql_walkthrough.md). 

## Data Structure

The core database consists of Medicare Part B records from 2018 to 2022, with each year stored in a separate table. All tables share a consistent column structure, allowing them to be seamlessly combined into a single dataset containing 367,781 records.

Below is a breakdown of the structure and contents of each table:

- **2018** - Contains 42,397 rows for calendar year 2018
- **2019** - Contains 42,198 rows for calendar year 2019
- **2020** - Contains 41,098 rows for calendar year 2020
- **2021** - Contains 40,267 rows for calendar year 2021
- **2022** - Contains 39,129 rows for calendar year 2022

| **Field Name**                | **Term Name**                            | **Description**                                                                                                                                                      |
|-------------------------------|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Rfrg_Prvdr_Geo_Lvl`          | Geography Level                          | Identifies the level of geography the data is aggregated at. 'State' means state-level aggregation; 'National' means across all states for a given HCPCS code.       |
| `Rfrg_Prvdr_Geo_Cd`           | Referring Provider Geography Code        | FIPS code of the referring provider’s state. Blank when reported at the national level.                                                                              |
| `Rfrg_Prvdr_Geo_Desc`         | Referring Provider Geography Description | State or region name. Can include 50 states, D.C., U.S. territories, Armed Forces areas, Unknown, Foreign Country, or 'National'.                                    |
| `RBCS_Lvl`                    | RBCS Level                               | High-level grouping of Restructured BETOS Classification System: Durable Medical Equipment, Prosthetic/Orthotic Devices, or Drugs/Nutritional Products.              |
| `RBCS_Id`                     | RBCS Identifier                          | 6-character RBCS ID, breaking down category, subcategory, family, and major procedure.                                                                               |
| `RBCS_Desc`                   | RBCS Description                         | Concatenated description of the RBCS category and subcategory.                                                                                                       |
| `HCPCS_Cd`                    | HCPCS Code                               | HCPCS code for the specific DMEPOS product/service.                                                                                                                  |
| `HCPCS_Desc`                  | HCPCS Description                        | Description of the corresponding HCPCS code.                                                                                                                         |
| `Suplr_Rentl_Ind`             | Supplier Rental Indicator                | Indicates whether the DMEPOS item is a rental.                                                                                                                       |
| `Tot_Rfrg_Prvdrs`             | Number of Referring Providers            | Number of referring providers ordering DMEPOS products/services.                                                                                                     |
| `Tot_Suplrs`                  | Number of Suppliers                      | Number of suppliers rendering DMEPOS products/services.                                                                                                              |
| `Tot_Suplr_Benes`             | Number of Supplier Beneficiaries         | Total unique beneficiaries associated with supplier claims. Suppressed if fewer than 11 to protect privacy.                                                          |
| `Tot_Suplr_Clms`              | Number of Supplier Claims                | Total DMEPOS claims submitted by suppliers.                                                                                                                          |
| `Tot_Suplr_Srvcs`             | Number of Supplier Services              | Number of DMEPOS services rendered by suppliers.                                                                                                                     |
| `Avg_Suplr_Sbmtd_Chrg`        | Average Supplier Submitted Charges       | Average amount charged by suppliers before Medicare reductions.                                                                                                      |
| `Avg_Suplr_Mdcr_Alowd_Amt`    | Average Supplier Medicare Allowed Amount | Average allowed amount (includes Medicare payments, deductibles, coinsurance, and third-party contributions).                                                        |
| `Avg_Suplr_Mdcr_Pymt_Amt`     | Average Supplier Medicare Payment Amount | Average amount Medicare actually paid, post-deductibles and coinsurance.                                                                                             |
| `Avg_Suplr_Mdcr_Stdzd_Amt`    | Average Supplier Medicare Standard Amount| Standardized average Medicare payment (adjusts for geographic differences in payment rates).                                                                         |

## Executive Summary

### Overview of Findings

Explain the overarching findings, trends, and themes in 2-3 sentences here. This section should address the question: "If a stakeholder were to take away 3 main insights from your project, what are the most important things they should know?" You can put yourself in the shoes of a specific stakeholder - for example, a marketing manager or finance director - to think creatively about this section.

[Visualization, including a graph of overall trends or snapshot of a dashboard]



## Insights Deep Dive
### Category 1:

* **Main insight 1.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 2.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 3.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 4.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.

[Visualization specific to category 1]


### Category 2:

* **Main insight 1.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 2.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 3.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 4.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.

[Visualization specific to category 2]


### Category 3:

* **Main insight 1.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 2.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 3.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 4.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.

[Visualization specific to category 3]


### Category 4:

* **Main insight 1.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 2.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 3.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 4.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.

[Visualization specific to category 4]



## Recommendations:

Based on the insights and findings above, we would recommend the [stakeholder team] to consider the following: 

* Specific observation that is related to a recommended action. **Recommendation or general guidance based on this observation.**
  
* Specific observation that is related to a recommended action. **Recommendation or general guidance based on this observation.**
  
* Specific observation that is related to a recommended action. **Recommendation or general guidance based on this observation.**
  
* Specific observation that is related to a recommended action. **Recommendation or general guidance based on this observation.**
  
* Specific observation that is related to a recommended action. **Recommendation or general guidance based on this observation.**
  


## Assumptions and Caveats:

Throughout the analysis, multiple assumptions were made to manage challenges with the data. These assumptions and caveats are noted below:

* Assumption 1 (ex: missing country records were for customers based in the US, and were re-coded to be US citizens)
  
* Assumption 1 (ex: data for December 2021 was missing - this was imputed using a combination of historical trends and December 2020 data)
  
* Assumption 1 (ex: because 3% of the refund date column contained non-sensical dates, these were excluded from the analysis)
