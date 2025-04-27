# Problem Statement
This project aims to develop a basic understanding of risk analytics in banking and understand how data is used to minimize risk to the bank while lending out money to customers. 

# Objective 
The dashboard aims to help a company determine the likelihood of an applicant being able to repay the loan before deciding on the loan approval.

# About the dataset 
The dataset contains information about bank details, various client details which consist of multiple tables interlinked through keys like primary and foreign keys.

The tables are namely Banking Relationship, Banking_Clients, Gender, Investment_Advisors and Banking. 

# Data Cleaning
Creating a new column "Engagement Days" which tracks the number of days the client have stayed with the bank from the join date to present day.

Creating a new column "Engagement Timeframe" which returns the timeline of the client's stay with the bank.

Creating bins for Estimated Income as 'Low' for <100000, 'Mid' for <300000 and 'High' for the remaining income levels, and assigning 'Income Band' as the new binned column to represent the categories.

Creating a new column named 'Processing Fees' to categorize the different levels of processing fees as 'Low' - 0.01, 'Mid' - 0.03, 'High' - 0.05.

