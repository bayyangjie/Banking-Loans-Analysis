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

## Calculated Functions
SUM: <br>
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

## Calculated Columns
Engagement Days - The period that a customer has been with the bank
```dax
Engagement Days = DATEDIFF('banking_case customer'[Joined Bank], TODAY(), DAY)
```

Investment Advisor - Map the names of the investment advisors to the respective advisor ID in the "investment-advisiors.csv" dataset.




