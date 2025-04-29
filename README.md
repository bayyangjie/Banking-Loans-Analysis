# Problem Statement
This project aims to develop a basic understanding of risk analytics in banking and understand how data is used to minimize risk to the bank while lending out money to customers. 

# Objective 
The dashboard aims to help a company determine the likelihood of an applicant being able to repay the loan before deciding on the loan approval.

# About the dataset 
The dataset contains information about bank details, various client details which consist of multiple tables interlinked through keys like primary and foreign keys.

The tables are namely Banking Relationship, Banking_Clients, Gender, Investment_Advisors and Banking. 

# Data Preparation
## Data Cleaning
Creating a new column "Engagement Days" which tracks the number of days the client have stayed with the bank from the join date to present day.

Creating a new column "Engagement Timeframe" which returns the timeline of the client's stay with the bank.

Creating bins for Estimated Income as 'Low' for <100000, 'Mid' for <300000 and 'High' for the remaining income levels, and assigning 'Income Band' as the new binned column to represent the categories.

Creating a new column named 'Processing Fees' to categorize the different levels of processing fees as 'Low' - 0.01, 'Mid' - 0.03, 'High' - 0.05.

## DAX Measures
Functions used: SUM(), SUMX(), DATEDIFF(), DISTINCTCOUNT() <br>

Engagement Length - To count the number of days a customer has been engaged with the bank.
```dax
Engagement Length = SUM('banking_case customer'[Engagement Days])
```

Total Clients - To get the total number of unique clients that are currently with the bank
```dax
Total Clients = DISTINCTCOUNT('banking_case customer'[Client ID])
```

Total Deposit - To get the total amount of deposit that a customer has put into the bank 
```dax
Total Deposit = SUM('banking_case customer'[Bank Deposits]) + SUM('banking_case customer'[Saving Accounts]) + SUM('banking_case customer'[Foreign Currency Account]) + SUM('banking_case customer'[Checking Accounts])
```

Total Loan -  Total loan (bank loan + Business lending + credit cards balance) that was given to a client.
```dax
Total Loan = SUM('banking_case customer'[Bank Loans]) + SUM('banking_case customer'[Business Lending]) + SUM('banking_case customer'[Credit Card Balance])
```

Total Fees - The total amount charged by the bank to a customer for account opening and maintenance charges.
```dax
Total Fees = SUMX('Clients - Banking' , [Total Loan] * 'Clients - Banking'[Processing Fees])
```
<!--- SUMX performs a row-by-row calculation to compute the fee for each client then adds up all the fees into a single totalled value --->

Total Credit Card Balance - The amount of short term financing that the bank has loaned out


## DAX Calculated Columns
Functions used: DATEDIFF(), YEAR(), SWITCH() <br>

Engagement Days - The period that a customer has been with the bank.
```dax
Engagement Days = DATEDIFF('banking_case customer'[Joined Bank], TODAY(), DAY)
```

Investment Advisor - Map the names of the investment advisors to the respective advisor ID in the "investment-advisiors.csv" dataset.

Year of Joining - The year that the client joined the bank, extracted from the 'Joined Bank' column which contains date values.
```dax
Year of Joining = YEAR('banking_case customer'[Joined Bank])
```

Engagement Timeframe - Binned values from 'Engagement Days' into four categories (< 5 Years, < 10 Years, < 20 Years, > 20 Years).
```dax
Engagement Timeframe = 
SWITCH(TRUE(),
'banking_case customer'[Engagement Days] < 365, "< 1 Year",
'banking_case customer'[Engagement Days] < 1825, "< 5 Years",
'banking_case customer'[Engagement Days] < 3650, "< 10 Years",
'banking_case customer'[Engagement Days] < 7300, "< 20 Years",
"> 20 Years")
```

# Visualization and Result

## Home
A total of 5 pages are created - Home, Loan Analysis, Deposit Analysis, Summary, Drill Through Report. <br>

Navigation buttons are added at the top of each page to allow easy navigation between the pages. <br>

Filters are also included to break down the data by Banking Relationship Type, Gender, Joining Year and Clent's Investment Advisor. <br>

<img src="https://github.com/bayyangjie/Banking-Loans-Analysis/blob/main/Images/homepage.png?raw=true" width="100%">

## Loan Analysis
The loan analysis shows that Europeans take the highest bank loan of $777.62M and Private Banking makes up the most amount of loans given to clients at $1.99B. 

Business Lending loans contribute to the most amount out of the different loan types.  

Majority of Bank Loan goes to clients that are from the 'Medium' Income Band category.

The most amount of loan given out are to clients who have been engaged with the bank for at least 20 years followed by those who have stayed with the bank over 20 years. This could suggest that the bank treasures loyal clients and places more trust in them, thus having the confidence to provide these groups of clients with more loans.

<img src="https://github.com/bayyangjie/Banking-Loans-Analysis/blob/main/Images/loan%20analysis.png?raw=true" width="100%">

## Deposit Analysis
The deposit analysis shows that clients from the 'Medium' income band category form the highest amount of deposits.

<img src="https://github.com/bayyangjie/Banking-Loans-Analysis/blob/main/Images/deposit%20analysis.png?raw=true" width="100%">

## Summary
<img src="https://github.com/bayyangjie/Banking-Loans-Analysis/blob/main/Images/Summary.png?raw=true" width="100%">

## Drill Through Report
The drill through report enables a user to right click on the drill-through fields (i.e Banking Relation, Engagement Timeframe, Income Band, Nationality) on the different visuals of the main pages, which leads the user to the drill-through page to obtain more details about the fields. <br>

For example, getting more information about each Income Band type like Total Loans, Total Deposits, Sum of Properties Owned, Amount in Savings Account etc; Total Deposits by clients within each Banking Relationship; Overall Engagement Length of each Nationality. <br>

<img src="https://github.com/bayyangjie/Banking-Loans-Analysis/blob/main/Images/Drill%20Through%20Latest.gif?raw=true" width="100%">

# Conclusion

