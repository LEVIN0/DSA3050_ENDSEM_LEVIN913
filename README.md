# DSA3050A-Advanced-PowerBI-Exam

## CaixaBank Financial Transactions Intelligence Dashboard

---

## Information

| Field | Details |
|---|---|
| **Student Name** | Levin Ekuam |
| **Admission Number** | 669913 |
| **Course Code** | DSA 3050A |
| **Semester** | Spring Semester 2026 |

---



# Project Overview

This project is my end semester examination submission for DSA 3050A Advanced 
Business Intelligence at USIU-Africa. The aim of the project was to build a 
complete and professional Power BI analytical solution using a real world banking 
dataset, covering the full BI pipeline from raw data acquisition and cleaning all 
the way through to data modeling, DAX calculations, interactive dashboard design, 
and business insight generation.

The dataset I chose is the CaixaBank Financial Transactions Dataset which was 
originally created by CaixaBank Tech for their 2024 AI Hackathon. CaixaBank is 
one of Spain's largest retail banks and the dataset contains real transaction 
records, customer information, and card data spanning the 2010s decade. The 
dataset was sourced from Kaggle and contains four related CSV files that naturally 
form a star schema when modeled in Power BI.

The final solution consists of three interactive report pages covering an executive 
summary, detailed customer and risk analysis, and performance segmentation. It 
includes interactive slicers, cross-filtering between visuals, and over ten DAX 
measures that support business decision making across the banking portfolio.

---

# Problem Statement

Banks generate millions of transactions every day and the challenge is turning 
that raw data into actionable intelligence that stakeholders can actually use to 
make decisions. The specific business problem this project is trying to address is:

> *How can a bank's business intelligence team monitor transaction performance, 
> understand customer spending behaviour across different merchant categories and 
> demographics, identify patterns in potentially risky transactions, and track 
> the card portfolio health over time in order to support better decisions on 
> risk management, customer strategy, and operational efficiency?*

The dashboard I built is designed to answer this question by giving stakeholders 
a clear and interactive view of transaction volumes and amounts, customer 
segmentation by income, age, credit score and tenure, spending patterns by time 
of day and transaction channel, and how these metrics change across different 
time periods.

---

# Dataset Description

| Detail | Information |
|---|---|
| **Dataset Name** | Financial Transactions Dataset: Analytics |
| **Source** | Kaggle |
| **Creator** | CaixaBank Tech (2024 AI Hackathon) |
| **URL** | https://www.kaggle.com/datasets/computingvictor/transactions-fraud-datasets |
| **Nature of Data** | Real-world banking institution data |
| **Time Period** | 2010s decade |
| **Total Transactions** | 1,000,000+ rows |
| **Number of Files** | 4 CSV files |

![alt text](<Screenshots/Data source.png>)

### File 1 — transactions_data.csv (Fact Table)
This is the main table in the dataset and the one with the most rows. It contains 
every individual transaction record including the transaction ID, the date and time 
the transaction happened, the client who made the transaction, which card was used, 
the transaction amount, the merchant details including city and state, the MCC code 
identifying the type of merchant, whether the transaction was done via chip swipe 
or online, any errors that occurred during processing, and whether the transaction 
was flagged as fraudulent.

### File 2 — cards_data.csv (Dimension Table)
This table contains information about each payment card in the portfolio including 
the card ID, which client it belongs to, the card brand such as Visa Mastercard 
Amex or Discover, the card type which is either Credit Debit or Debit Prepaid, 
the credit limit assigned to the card, the account opening date, whether the card 
has a chip, and the number of cards that have been issued to that particular client.

### File 3 — users_data.csv (Dimension Table)
This table contains customer demographic and financial information including the 
client ID, current age, gender, home address, city, state, yearly income, total 
outstanding debt, credit score, and the number of credit cards the customer holds. 
This table was very important for the customer segmentation analysis I built in 
the dashboard.

### File 4 — mcc_codes.csv (Lookup Dimension Table)
This is a small lookup table that maps each four digit Merchant Category Code to 
a human readable description of the type of merchant, for example Grocery Stores, 
Gas Stations, Restaurants, and so on. I merged this table into the transactions 
table during Power Query transformation so that every transaction would show a 
readable merchant category name.

---

# Tools Used

| Tool | Purpose |
|---|---|
| **Power BI Desktop** | Main tool for data modeling, DAX, and dashboard design |
| **Power Query (M Language)** | Data cleaning and transformation of all four tables |
| **DAX (Data Analysis Expressions)** | Writing calculated columns and measures |
| **Microsoft Excel** | Initial inspection of raw CSV files before importing |
| **Kaggle** | Source platform for downloading the dataset |
| **GitHub** | Version control, file storage, and project documentation |

---

#  Steps Followed

### Step 1 — Data Acquisition
I downloaded all four CSV files from Kaggle. Before doing anything in Power BI 
I opened each file in Excel first just to get a feel for the structure, check 
what columns were available, see what the data looked like, and spot any obvious 
problems like missing values or wrong formatting.
l cannot add the files themselves to the repository because they are very large files 

### Step 2 — Importing Data into Power BI
I opened Power BI Desktop and used Get Data then Text/CSV to import all four 
files one by one. Each file was loaded as a separate query in Power Query and 
I renamed them clearly as Fact_Transactions, Dim_Cards, Dim_Users, and Dim_MCC 
so it would be easy to identify each table throughout the project.

### Step 3 — Data Cleaning and Transformation in Power Query
This was one of the most important steps. I performed ten transformation steps 
across the tables to make sure the data was clean and ready for modeling. These 
steps included fixing data types so that dates were date/time, amounts were 
decimal numbers, and IDs were text, removing duplicates on primary key columns, 
replacing null values in the errors column with the text No Error, replacing 
null values in merchant_state and zip with Unknown, removing currency symbols 
from the amount and credit_limit columns before changing the type to decimal, 
standardizing inconsistent text values in the gender and card type columns, 
creating a conditional column called Transaction Channel based on the use_chip 
field, extracting year month and hour parts from the date column, creating a 
numeric Fraud_Flag column with 1 for fraud and 0 for non-fraud, and merging 
the Dim_MCC table into Fact_Transactions to bring in readable merchant category 
descriptions.

### Step 4 — Data Modeling
After cleaning all four tables I went to the Model View in Power BI and set up 
the relationships to form a star schema. Fact_Transactions sits at the centre 
connected to Dim_Cards on card_id, Dim_Users on client_id, and Dim_MCC on mcc. 
I also created a Date Table using DAX and connected it to the transactions date 
column. All relationships were set to Many-to-One with single cross-filter 
direction. I added calculated columns to the Date Table for year, month name, 
quarter, and day of week.

### Step 5 — Creating DAX Measures
I created a dedicated empty table called DAX Measures to store all my measures 
in one place and keep the model tidy. I wrote ten measures in total covering 
total transaction amount, total transactions count, active users, average 
transaction amount, fraud amount, fraud rate percentage, year-to-date amount, 
previous year amount, year-over-year growth percentage, and merchant category 
ranking.

### Step 6 — Creating Calculated Columns
I created thirteen calculated columns across Fact_Transactions, Dim_Users, and 
Dim_Cards. These columns group raw numerical values into meaningful business 
categories like age bands, income bands, credit score bands, DTI bands, 
transaction size categories, time of day groups, and card generation 
classification. These columns made the dashboard much more useful for slicing 
and filtering the data.

### Step 7 — Dashboard Design
I designed three report pages using a consistent light theme with clear titles 
and properly labelled visuals. Page 1 is the Executive Summary with KPI cards 
and high level charts. Page 2 is the Detailed Analysis with scatter charts, 
matrices, and a treemap. Page 3 is the Performance and Segmentation page with 
channel analysis, income band distribution, and credit limit comparisons. I 
added slicers on each page to make the dashboard fully interactive.

### Step 8 — Generating Insights and Writing Recommendations
After exploring the completed dashboard I identified five key insights based on 
what I could see in the data and wrote three actionable business recommendations 
that the bank could consider acting on.

### Step 9 — Documentation and GitHub Upload
I took screenshots of all major stages including the raw data, Power Query 
transformations, model view, DAX formulas, and all three dashboard pages. I 
compiled a full PDF report and uploaded everything to this GitHub repository.

---

# Dashboard Features

### Page 1 — Executive Summary
![alt text](<Screenshots/Executive Summary.png>)
- Four KPI cards at the top showing Active Users, Total Amount, Total 
  Transactions, and Average Transaction Amount giving an immediate portfolio 
  health check
- Line chart showing how Total Amount has trended over time broken down by 
  Year Quarter Month and Day
- Donut chart showing the split of Total Amount across the four card brands 
  Visa Mastercard Amex and Discover
- Bar chart showing Total Amount by Merchant State to identify the top 
  performing geographic markets
- Four interactive slicers for Card Brand, Card Type, Gender, and Age Band 
  that cross-filter all visuals on the page simultaneously

### Page 2 — Detailed Analysis
![alt text](<Screenshots/Second Page.png>)
- Scatter chart plotting Total Transactions against Credit Score coloured by 
  Age Band to show how transaction behaviour varies across customer risk profiles
- Clustered bar chart showing Total Transactions by Time of Day and Card Type 
  revealing peak transaction hours and preferred card types
- Matrix table showing Total Customers in DTI Bands cross-tabulated by Age Band 
  with conditional formatting to highlight high debt concentration
- Matrix table showing Total Amount by Card Brand and Customer Tenure to 
  identify which loyalty segments drive the most value
- Treemap showing Year-over-Year Growth percentage by Merchant City to spot 
  the fastest growing geographic markets

### Page 3 — Performance and Segmentation
![alt text](<Screenshots/Third Page.png>)
- Bar chart comparing Total Amount by Transaction Channel showing the split 
  between Swipe and Online transactions
- Pie chart showing Total Transactions by Income Band to understand the 
  financial profile of the customer base
- 100% stacked bar chart showing card type usage broken down by Age Band to 
  reveal how different generations prefer to pay
- Horizontal bar chart showing Average Transaction Amount by Credit Limit Band 
  demonstrating the relationship between credit access and spending behaviour
- Interactive slicers for Quarter and Customer Tenure enabling time-based and 
  loyalty-based filtering across all visuals on the page

---

#  Key DAX Measures

| Measure | Formula | What It Does |
|---|---|---|
| **Total Amount** | `SUM('Fact_Transactions'[amount])` | Adds up all transaction amounts across the portfolio |
| **Total Transactions** | `COUNTROWS('Fact_Transactions')` | Counts the total number of transaction records |
| **Active Users** | `DISTINCTCOUNT('Fact_Transactions'[client_id])` | Counts unique customers who made at least one transaction |
| **Avg Transaction Amount** | `AVERAGE('Fact_Transactions'[amount])` | Calculates the average spend per transaction |
| **YTD Transaction Value** | `TOTALYTD(SUM('Fact_Transactions'[amount]), 'DimDate'[Date])` | Cumulative transaction amount from the start of the year to the current date |
| **Previous Year Transaction Value** | `CALCULATE([Total Amount], SAMEPERIODLASTYEAR('DimDate'[Date]))` | Total amount for the same period in the previous year |
| **YoY Growth %** | `DIVIDE([Total Amount] - [Previous Year Transaction Value], [Previous Year Transaction Value], 0) * 100` | Percentage change in total amount compared to the previous year |
| **Merchant Rank** | `RANKX(ALL('Dim_MCC'[edited_description]), [Total Amount],, DESC, DENSE)` | Ranks each merchant category from highest to lowest by total transaction amount |

---

# Key Insights

**Insight 1 — Swipe Transactions Dominate and Create a Security Risk**
Swipe transactions account for over 80% of the total transaction amount at 
approximately $38M out of $45.90M total. This is concerning because swipe 
technology is significantly more vulnerable to card skimming fraud compared 
to chip transactions. The bank should prioritise migrating both merchants and 
customers to chip-enabled transactions as soon as possible.

**Insight 2 — The Low Income Segment Drives the Majority of Transactions**
The Poor income band accounts for 68.7% of all transactions which is about 
720,000 transactions. This means the bank's core customer base is predominantly 
low income individuals who based on the DTI Band data may also be carrying 
significant debt loads relative to their income. This creates a compounding 
credit and repayment risk that the bank needs to monitor closely.

**Insight 3 — Afternoon Is the Peak Transaction Period**
The Time of Day analysis shows that most transactions happen in the Afternoon 
between 12pm and 6pm followed by the Morning slot. Night-time transactions 
between 12am and 6am are very low in volume but are typically higher risk 
in banking because monitoring is reduced during those hours and fraudulent 
activity can go undetected for longer.

**Insight 4 — Visa Dominates the Portfolio Creating Network Concentration Risk**
Visa accounts for 50.2% of total transaction value and Mastercard adds another 
37.95% meaning two card networks handle nearly 88% of all transaction value. 
This level of concentration means that any disruption to either network would 
have an immediate major impact on the bank's operations. The Amex segment is 
also underutilised despite premium cardholders typically spending more per 
transaction.

**Insight 5 — Veteran Customers Generate the Most Value But Youth Is Underrepresented**
Veteran customers with 30 or more years of tenure generate $658,925 in total 
transaction value which is more than all shorter tenure groups combined. This 
confirms that long term customer retention is the most valuable strategy for 
the bank. However the Youth age band is severely underrepresented in the data 
which is a warning sign for future portfolio growth as the older customer 
segments eventually become less active.

---

# Challenges Encountered


**Challenge 1 — Currency Symbols Preventing Correct Data Types**
The amount and credit_limit columns imported with dollar signs and commas which 
caused Power BI to read them as text instead of numbers. I had to go into Power 
Query and use Replace Values to remove the dollar sign and commas before I could 
change the data type to Decimal Number.


**Challenge 2— Slow Performance With Large Dataset**
With over one million rows in the transactions table Power BI was becoming very 
slow every time I made a change during development. I fixed this by going into 
Power Query options and disabling the automatic refresh so that queries only ran 
when I manually clicked Refresh. This made the development process much smoother.

---

# Conclusion

Overall this project was a very useful and practical experience that helped me 
understand how the different components of a Power BI solution fit together. 
Starting from raw CSV files and ending up with a fully interactive three page 
dashboard with DAX measures and a proper star schema gave me a much better 
appreciation for what business intelligence work actually involves in a real 
organisation.

The CaixaBank dataset was a good choice for this project because having four 
related tables that naturally form a star schema made the modeling process very 
clean and logical. The fact that it is real banking data also made the analysis 
feel more meaningful because the insights I found, like the dominance of swipe 
transactions and the concentration of low income customers, are the kinds of 
things that a real bank analyst would actually care about.

The most challenging parts of the project for me were getting the calculated 
columns to work correctly without the DAX row context errors, and figuring out 
why the KPI cards were blank. Both issues took some time to debug but solving 
them helped me understand Power BI more deeply than I would have if everything 
had worked perfectly from the start.

If I were to continue this project I would add a dedicated fraud analysis page 
with a map visual showing where fraud is concentrated geographically, and I would 
also explore adding a what-if parameter to simulate how changes in fraud rate 
would affect the bank's total revenue. I would also look into connecting the 
dashboard to a live data source rather than static CSV files to make it more 
suitable for real operational use.

---



