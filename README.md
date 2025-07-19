# Medicare Part B: State-Level Spending Patterns (2018–2022)

## Table of Contents

[Background & Overview](#background-&-overview)

[Data Structure](#data-structure)

[Executive Summary](#executive-summary)





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

An analysis of state-level DMEPOS spending reveals that just a few high-cost items account for a disproportionate share of total Medicare expenditures. For example, home ventilators (E0466), oxygen concentrators (E1390), and glucose monitoring systems (K0553) consistently ranked among the top 3 most costly services across many states and have contributed to over 16.1% of total Medicare Part B spending between 2018 and 2022 despite there being over 1,481 other services available.

To better understand cost dynamics over time for the most financially significant equipment types, the chart below tracks spending trends for these top 3 items across five years:

<img width="1409" height="717" alt="image" src="https://github.com/user-attachments/assets/beb69d47-6493-4b7e-b748-9f3c854cbf14" />

While all three saw significant growth from 2018 to 2020, only glucose monitoring equipment has shown continued acceleration and doubling between 2020 and 2022, reaching nearly $1.73 billion in 2022 alone. In contrast, ventilator and oxygen-related costs have plateaued or slightly declined in the past two years which could suggests stabilized utilization or pricing.

## Recommendations:

Based on the insights and findings above, we would recommend CMS to consider the following: 

### **Conduct a Root-Cause Audit of the 2019 Spike:**
The 107% spike in spending between 2018 and 2019 is a major red flag. This sharp increase demands further investigation to determine whether it was driven by a policy change, broader eligibility criteria, changes in reporting practices, or potential supplier behavior. While spending has remained relatively stable post-2019, it has done so at this elevated level, which raises concerns about long-term sustainability. CMS must ensure that this jump was justified and is not a early warning signal of another unsustainable increase in the future.

### **Invest in Prevention & Alternative Care:**
Over 16% of total DMEPOS spending from 2018 to 2022 is concentrated in just two categories: diabetic monitoring supplies and oxygen-related services. These items reflect chronic conditions that can often be mitigated or better managed through early intervention, education, and preventative care programs.
By investing upstream in initiatives such as diabetes prevention programs, smoking cessation efforts, and improved access to primary care and nutrition counseling, CMS may reduce the number of beneficiaries who progress to needing these high cost and demand DMEPOS supplies. Not only could this ease financial strain on the program, but it would also support better health outcomes and improved quality of life for patients.

### **Pilot Cost-Containment Programs in High-Spending States:**
Given that Texas, Florida, and California consistently lead in spending, these states could serve as ideal pilot regions for scalable cost-containment initiatives focused on fair supplier pricing, access equity, and efficiency.

  
## Assumptions and Caveats:

Throughout the analysis, multiple assumptions were made to manage challenges with the data. These assumptions and caveats are noted below:

- The analysis was limited to the 50 U.S. states, excluding territories and outlying regions present in the raw data. This decision was made to ensure comparability across states and focus on regions with the largest share of Medicare Part B spending.
- All cost figures used were the standardized Medicare allowed amounts to remove geographic payment variations and allow for fair comparison across different states and years. This approach prioritizes trends and relative growth rather than raw spending totals.
- The analysis concentrated on the top three most frequently reimbursed DMEPOS supply categories: diabetic monitoring supplies, oxygen services, and CPAP-related equipment. While these account for a substantial portion of overall spending and are representative of broader trends, it is possible that other categories experienced equal or greater fluctuations that were not captured in this summary.
- No imputation or correction was applied to handle potential data reporting errors, particularly regarding the sudden spike in 2019. The analysis assumes that the data reflects accurate reporting and reimbursement figures from CMS.
- The analysis did not normalize spending based on changes in population size or aging demographics across states. Increases in spending could be partially due to growing Medicare enrollment or rising chronic condition prevalence rather than cost growth alone.
- The spending values across years were compared directly without adjusting for inflation. As a result, some of the growth may be due to natural increases in cost of goods and services rather than a true change in utilization or policy.
