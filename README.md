## Data Cleaning & ETL Automation using BigQuery (SQL) & Sheets
Campaign report - data streamlining &amp; automation project 
# Problem Statement - 
The existing Client Campaign Data process was broken. It was prone to errors, missing data points, and too many manual steps, and it was time-consuming and inefficient. There were multiple data sources and for every report, the team needed to fetch data from various sources and create a new Excel sheet every time resulting in duplicate data, redundant efforts and errors. Bottomline - The process was missing the "Single Source of Truth"

# Issues in the existing data management & reporting system - 
- The leads coming in from the client were not recorded as per their LeadID or any primary key
- The older leads are not getting updated after a point of time. Let’s say if a lead from June is disqualified in August or converts into an opportunity later, it's may not get updated in our systems
- Leads/opportunities from Inbound calls, chatbot, Calendly l don’t have campaign (UTM) tags every time they come into the sheet. The client has to add the tags manually in the sheets a few times a month, but the same leads may not have any tags in the subsequent sheets
In short, the data is not dynamically updated currently.

# As a result, data discrepancies creep in. For example - 

- Missing Leads -
  - Leads (all sources) in the CRM sheets (Jan- 17 Sep 2023) - 3061
  - Leads (all sources) in the most recent sheet (Jan- 17 Sep 2023)  - 2978
- Missed Opportunities -
  - ~25% error in the existing system.

## To fix these issues and create an error-proof unified system, automation was needed - A continuously updated & living "Source of Truth"
# A system where each lead is stored in a unified sheet, all previous leads are updated on the relevant attributes, and there is no data loss.

Step 1 - Creating the mastersheet table
```sql
Create Table
  `.....Sheets.MasterSheet`
(Lead_ID STRING,
Created_Date DATE,
Account_Engagement_Created_Date STRING,
Entry_Status STRING,
Lead_Status STRING,
Company___Account STRING,
City STRING,
State_Province STRING,
Zip_Postal_Code STRING,
utm_source STRING,
utm_medium STRING,
utm_campaign STRING,
Google_Analytics_Source STRING,
Google_Analytics_Medium STRING,
Google_Analytics_Campaign STRING,
Google_Analytics_Content STRING,
Google_Analytics_Term STRING,
Disqualified_Reason STRING,
Primary_Source STRING,
Stage STRING,
Total_Lines INTEGER,
Handset_Qty INTEGER,
IoT_Qty INTEGER,
Connected_Device_Qty INTEGER,
No__of_Employees INTEGER)
```
  

Step 2 - Populating Master sheet (with the first sheet)

```sql
INSERT INTO
  `.....Sheets.MasterSheet`
SELECT
  Lead_ID,
  Created_Date,
  Account_Engagement_Created_Date,
  Entry_Status,
  Lead_Status,
  Company___Account,
  City,
  State_Province,
  Zip_Postal_Code,
  utm_source,
  utm_medium,
  utm_campaign,
  Google_Analytics_Source,
  Google_Analytics_Medium,
  Google_Analytics_Campaign,
  Google_Analytics_Content,
  Google_Analytics_Term,
  Disqualified_Reason,
  Primary_Source,
  Stage,
  Total_Lines,
  Handset_Qty,
  IoT_Qty,
  Connected_Device_Qty,
  No__of_Employees
FROM
  `....Sheets.May_17_2023`
```

Step 3 - Updating Mastersheet with New CRM data sheet (recurring)

```sql
UPDATE
`dataanalytics-2023-394903.USCC_Leads_CRM_Sheets.MasterSheet` AS M
SET
M.Lead_Status = N.Lead_Status,
M.Disqualified_Reason = N.Disqualified_Reason,
M.Stage = N.Stage,
M.Total_Lines = N.Total_Lines,
M.Handset_Qty = N.Handset_Qty,
M.IoT_Qty = N.IoT_Qty,
M.utm_source = IFNULL(M.utm_source,N.utm_source),
M.utm_medium = IFNULL(M.utm_medium,N.utm_medium),
M.utm_campaign = IFNULL(M.utm_campaign,N.utm_campaign),
M.Connected_Device_Qty = N.Connected_Device_Qty
FROM
`dataanalytics-2023-394903.USCC_Leads_CRM_Sheets.Sep_27_2023` AS N
WHERE
M.Lead_ID = left(N.Lead_ID,15)
```
