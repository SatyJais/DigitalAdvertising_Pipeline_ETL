## Data Cleaning & ETL Automation using BigQuery & Sheets
Campaign report - data streamlining &amp; automation project 
# Problem Statement - 
The existing Client Campaign Data process was broken. It was prone to errors, missing data points, and too many manual steps, and it was time-consuming and inefficient. There were multiple data sources and for every report, the team needed to fetch data from various sources and create a new Excel sheet every time resulting in duplicate data, redundant efforts and errors. Bottomline - The process was missing the "Single Source of Truth"

# Issues in the existing system - 
- The leads coming in from the client were not recorded as per their LeadID or any primary key
- The older leads are not getting updated after a point of time. Let’s say if a lead from June is disqualified in August or converts into an opportunity later, it's may not get updated in our systems
- Inbound calls, chatbot,  leads/opportunities don’t have UTM tags every time they come into the sheet. USCC has to add UTM tags manually in the sheets a few times a month, but the same leads may not have any tags in the subsequent sheets
- In short, the data is not dynamically updated currently.


