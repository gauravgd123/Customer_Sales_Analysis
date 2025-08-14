

# Customer Loan Analysis
### Dashboard Link : https://app.powerbi.com/links/GjfK-T_B3W?ctid=6716165f-2d41-437c-b927-752275b2a814&pbi_source=linkShare
## Problem Statement

To analyze customer loan data by examining demographic factors (such as age, education, and income) alongside loan attributes (loan amount, interest rate, and tenure) in order to identify patterns, trends, and potential risk factors. This analysis aims to support data-driven decision-making for loan approvals, risk assessment, and targeted financial services.


### Steps followed 

- Step 1 : Gathering the Data files required to perform analysis.

- Step 2 : Importing data tables into database and perform necessary queries.

- Step 3 : Connected PowerBI to Microsoft SQL Server to import data from the database.
           
- Step 4 : Data Preparation: Performed Data cleaning and transforming data using Power Query Editor. The Process included:

        - Configured data types according to the data in each column.
        - Filtered rows and created additional columns to support data.
        - Created Measures Table and important measures to support analysis.
        - Removed null and duplicate records in Power Query.

- Step 5 : Created new columns for the Customer Loan Analysis to asist the analysis.

        1 - Year = Year('Loan_default'[Loan_Date_DD_MM_YYYY].[Date]) 

        2 - Credit Score Bins = 

        IF(Loan_default[CreditScore] < 400, "Very Low",
        IF(Loan_default[CreditScore]<450,"Low",
                IF(Loan_default[CreditScore]<650,"Medium",
                "High" )))

        3 - Age Group = 
        IF('Loan_default'[Age]<=19,"Teen",
        IF('Loan_default'[Age]<= 39,"Adults",
                IF('Loan_default'[Age]<=59,"Middle Age Adults",
                "Senior Citizens")))

        4 - Income Grade = 

        SWITCH(
        TRUE(),
        Loan_default[Income]<30000,"Low",
        Loan_default[Income]>=30000 && Loan_default[Income]<60000,"Medium",
        Loan_default[Income]>=60000 && Loan_default[Income]<100000,"High",
        Loan_default[Income]>=100000,"V.High")

- Step 6 : DAX Measures created for analysis:

        1 - YOY Loan Amount Change = 

        DIVIDE(CALCULATE(SUM('Loan_default'[LoanAmount]),
                'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))) - 

                CALCULATE(SUM('Loan_default'[LoanAmount]),
                        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))-1),

                CALCULATE(SUM(Loan_default[LoanAmount]),
                        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))-1),0) * 100

        2 - Avg Loan Amount(High Credit) = 

        AVERAGEX(FILTER('Loan_default','Loan_default'[Credit Score Bins] = "High"),
                        'Loan_default'[LoanAmount])

        3 - Default Rate By Year = 

        var TotalLoans = CALCULATE(COUNTROWS('Loan_default'),
                        ALLEXCEPT('Loan_default','Loan_default'[Year]))

        var default = CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default] = True())),
                ALLEXCEPT('Loan_default','Loan_default'[Year]))

        
        RETURN

        DIVIDE(default,TotalLoans) * 100

        4 - Total Loan (Credit Bins) = 

        CALCULATE(SUM('Loan_default'[LoanAmount]),'Loan_default'[Age Group] = "Adults",
                        ALLEXCEPT('Loan_default','Loan_default'[Age],
                        'Loan_default'[Age Group],'Loan_default'[CreditScore],
                                'Loan_default'[Credit Score Bins]))

        5 - Loan Amount by Purpose = 

        SUMX(FILTER('Loan_default', NOT(ISBLANK('Loan_default'[LoanAmount]))),'Loan_default'[LoanAmount])

- Step 7 : Final Dashboard:

        Page 1: Loan Defaultand Overview :

![Snap_1](https://github.com/user-attachments/assets/7aa88f3e-c5a3-4c76-adbd-6b097d9e44a8)



        Page 2 : Application Demographics and Financial Profile :

![Snap_1](https://github.com/user-attachments/assets/97ebece4-fdf4-4f1b-a955-09241fd8f865)

# Insights :

Page 1: Loan Default and Overview

        - Loan Amount by Purpose.
        - Average Loan by Age Group
        - Average income by Employment type
        - Default Rate by Employment type
        - Credit Score by Income Grade
        - Default Rate by Year


Page 2: Application Demographics and Financial Profile

        - YoY Loan Amount Change
        - Number of Loans by Education Type
        - Total loans by credit Score Bins
        - Average Loan amount by Age group and Marital status

• Total Loan Amount: 33 bn

• Total Income: 21 bn

• Average Interest rate: 13.49

• Adults have highest average loan and Teen have the lowest.

• Bachelor’s took higher no. of loans while Master’s and PHD students are lowest.

• Home Loans are higher and Auto loans and others are lower.

• Defaulting of loan rate is highest in unemployed customers.

• Customer with less credit score has higher median loan.



## Suggestions:


• More schemes such as subsidy and better interest could be provided to increase the loan number in automobile sector 

• Loss by defaulting loan can be prevented by providing less loan and on higher interest on the basis of employment and it type.

• Education related schemes can be introduced for education loan and to increase the participation of teen sector.

• Providing better interest and higher loans to customers with higher credit score. 


