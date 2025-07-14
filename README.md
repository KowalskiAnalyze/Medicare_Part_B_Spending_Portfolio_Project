# Medicare Part B: State-Level Spending Patterns (2018–2022)

## Background & Overview
Established in 1965, Medicare is a vital federal program that provides access to affordable medical services for over 67 million beneficiaries. It is primarily funded through payroll taxes, general federal revenues, and enrollee premiums. Because it’s publicly funded, every dollar spent must be justified to maintain a balance between patient access, cost-efficiency, and long-term sustainability.

Medicare is divided into four parts. This project focuses on Part B, which covers outpatient services, including physician visits, preventive care, and durable medical equipment, prosthetics, orthotics, and supplies (DMEPOS).

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

Between 2018 and 2022, Medicare Part B spending on DMEPOS totaled over **$75 billion** with standardized payments revealing consistently higher costs in populous states with higher aging and retiree communities such as **Texas**, **Flordia**, and **California**. There was a sharp **107.2% increase** in spending from 2018 to 2019, likely driven by reporting or policy changes, followed by relatively stable totals through 2022. Over **16%** of Medicare Part B spending is concentrated in just three categories: **home ventilators**, **oxygen concentrators**, and **glucose monitoring supplies**. On average, Medicare covered **76.4%** of the allowed costs and saving beneficiaries approximately **$325** per service compared to the full supplier charge.

![Medicare Part B Snapshot](https://github.com/user-attachments/assets/f56e8fde-ea92-4bb2-98c3-c1af366a497b)

An interactive Tableau dashboard can be found [here](https://public.tableau.com/views/MedicarePartBDashboard2018-2022/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link).


## Insights Deep Dive

### **Regional Comparisons:**

![image](https://github.com/user-attachments/assets/8ca50402-be25-4c8e-bdfa-b13b1fa5b70c)

This line chart tracks Medicare Part B standardized costs in California, Texas, and Florida from 2018 to 2022, the three states with the highest overall spending. All three show a dramatic spike in 2019, with costs more than doubling compared to 2018 with California rising from $351,948 to $779,627, Texas from $385,439 to $763,838, and Florida from $375,800 to $724,861. This pattern is seen through out every state in 2019 and may be attributed to a combination of reporting changes, expanded eligibility, or increased demand for durable medical equipment. After 2019, spending levels plateaued and remained relatively stable through 2022, with only minor year-to-year fluctuations. Notably, Florida experienced a visible dip in 2021, potentially signaling policy adjustments, supplier reductions, or regional disruptions. Overall, the data suggests that while these states drive a large portion of Medicare spending due to their size and aging populations, their cost trends have stabilized following the initial 2019 surge. This offers a valuable lens for assessing the long-term sustainability of Medicare expenditures in densely populated regions.

### **Yearly Spending Trends:**

![image](https://github.com/user-attachments/assets/3e98138b-9764-4ff0-b0c9-8a18d57b113f)

Between 2018 and 2019, Medicare Part B spending experienced a dramatic 107% increase, jumping from $10.2 million to $21.2 million. This abrupt spike could stem from a combination of factors such as policy changes, expanded access to coverage, or improved reporting standards that began capturing a more complete picture of service utilization. From 2019 onward, spending remained relatively stable year-over-year, hovering around $21 million and suggests that the surge was not part of a continuing upward trend but rather a correction or one-time adjustment.

The stability that followed offers a window of opportunity for policymakers and healthcare analysts to explore cost-containment strategies. With spending now on a predictable plateau, targeted interventions such as optimizing supplier pricing, encouraging preventive care, or reassessing reimbursement for high-cost equipment could lead to sustainable cost reductions. Additionally, investigating the root causes of the 2019 spike could uncover areas of overspending, fraud, or inefficiency that may still be present but now embedded in the baseline. Understanding this sharp inflection point is key to identifying both the risks and opportunities in long-term Medicare budgeting.


### **Supplier Behavior and Pricing:**

![image](https://github.com/user-attachments/assets/a887e7ea-bc59-4a9f-829d-e8f011ba218c)

This bar chart displays the average payment gap by state from 2018 to 2022, with the national average ($325.04) marked as a reference line. States on the left (in dark blue) exceed the national average which suggests greater discrepancies between supplier charges and Medicare-approved payments. This may point to potential overbilling, aggressive pricing, or weaker cost controls in those states. Conversely, states on the right (in gray) fall below the average which represent states with more aligned and possibly efficient pricing practices. These variations raise important questions about regional supplier behavior, market competition, and pricing regulations. Stakeholders might explore targeted audits or reimbursement policy reviews in high-gap states to curb excessive spending and improve consistency across regions.

![image](https://github.com/user-attachments/assets/57d9795e-1f1a-4e29-8d5c-feda84346fa8)

This chart illustrates the average percentage of costs covered by Medicare from 2018 to 2022 across all 50 states. The national average between 2018 and 2022 sits at 76.3%, with states like Florida, New Jersey, and Arizona leading the way as Medicare covers a larger share of costs in these areas. On the opposite end, states like Vermont, Hawaii, and West Virginia fall below the national average which indicates a higher financial burden on patients. This insight is crucial because it helps identify where patients may be more exposed to out-of-pocket expenses and signals opportunities for improving coverage equity. Stakeholders can use this data to evaluate whether policy adjustments or support programs are needed in low-coverage states to ease patient costs and improve healthcare access.



### **High-Cost Item Concentration:**

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
