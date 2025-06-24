# Bank-Customers-Churn-Analysis
A detailed Churn Analysis and Visualization of Bank Customers' Behaviour

## Project Overview
The sole aim of this project is to gain insight into the churn dataset, in-order to understand customers' behaviour across three country

## The Raw Data
A csv file which consists of two messy tables

### About  Data
###### Raw Columns
1.	Customers Surname: The customer's last name
2.	Geography:  The country where the customer resides (France, Spain or Germany)
3.	Gender: The customer's gender (Male or Female)
4.	Tenure: The number of years the customer has been with the bank
5.	Balance: The customer's account balance
6.	Number Of Product: The number of bank products the customer uses (e.g., savings account, credit card)
7.	 Has Cr Card: Whether the customer has a credit card (1 = yes, 0 = no)
8.	Is Active Member: Whether the customer is an active member (1 = yes, 0 = no)
9.	Estimated Salary: The estimated salary of the customer
10.	Exited: The estimated salary of the customer


## Tools and Steps
•	EXCEL; Used for:
1. Data loading and inspection
2. Data manipulations
3. Handling missing values,
4. Cleaning and
5. Formatting

•  SQL; Steps took involved:
1. Loading the refined datasets into the Database
2. Feature Engineering
3. Analysis

• Power BI; Steps took involved:
1. Transformation
2. Feature Engineering
3. Analysis
4. Visualizations

## Challenges
•	Challenge  1: Several improper casing of alphabets, a lot of misspellings, duplicates columns and rows, values errors, nulls and missing values were observed in the raw dataset.
      o	Solution: I use MS EXCEL to perform data manipulations, transformations, cleaning and formatting

•	Challenge 2: Another hurdle was loading the dataset to SQL server base, this was as a result of incorrect formatting of the CSV file to correspond with the SQL data type format
      o	Solution: This was also handled using Excel and SQL

• Challenge 3: After successive analysis in Sql, Creating successful relationships between exported files was herculian task in Power BI
      o	Solution: This was done by noting the type of relationship between the export files and applying them
      
## Feature Engineering
Feature engineering in SQL for data analysis involves strategically crafting new, informative features from existing data to enhance model performance and extract deeper insights.
Here are lists of Feature Engineering:

1.	Cr _Card_Satus from Has Credit Card
2.	Membership_Satus from Is_Active_Member
3.	Exit_Status From Exited
4.	Age Bracket: Using Globacom age bracket categorization
• 18-24: Young Adult
• 25-35: Mid Adult
• 36-65: Old Adult
• 65 and Above: Elder`
5.	Credit Score group from Credit Score:
• 350-550: Poor
• 551-650: Fair
• 651-750: Good
• 751 and Above: Excellent
6.	Salary Class from Estimate Salary
7.	Acc Bal Class form Balance




## DataBase Creation AND Importation Process

-- ---------------------------------- Octoplus Database Creation -------------------------------------
```sql

CREATE DATABASE IF NOT EXISTS octoplus;
USE octoplus;
CREATE TABLE IF NOT EXISTS bankchurn(
									Customer_ID varchar(255),
                                    Surname varchar(255),
                                    Credit_Score int,
                                    Geography varchar(255),
                                    Gender varchar(255),
                                    Age int,
                                    Tenure int,
                                    Balance decimal(20,2),
                                    Num_Of_Product int,
                                    Has_Cr_Card varchar(255),
                                    Is_Active_Member varchar(255),
                                    Estimated_Salary decimal(20,2),
                                    Exited varchar(255)
);

```

#### Feature Engineering CODES

-- --------------------------------------------------------------------------------------
-- --------------------------------------------------------------------------------------
-- ----------------FEATURE ENGINEERING-----------------------

-- Cr_Card_Satus from Has Credit Card ------------------

```sql

SELECT
		Has_Cr_Card,
        (CASE
				WHEN `Has_Cr_Card` = 1 THEN "Yes"
		   ELSE "No"
		  END
		) AS Cr_Card_Satus
FROM bankchurn;

ALTER TABLE bankchurn ADD COLUMN Cr_Card_Status VARCHAR (25);

UPDATE bankchurn
SET Cr_Card_Status = (CASE
				WHEN `Has_Cr_Card` = 1 THEN "Yes"
		   ELSE "No"
		  END
		);
        
```sql        
-- -------------------------------------------------------------------------------------------


-- -------------------------------------------------------------------------------------------

-- Membership_Satus from Is_Active_Member ------------------

```sql

SELECT
		Is_Active_Member,
        (CASE
				WHEN `Is_Active_Member` = 1 THEN "Active"
		   ELSE "Not Active"
		  END
		) AS Membership_Satus
FROM bankchurn;

ALTER TABLE bankchurn ADD COLUMN Membership_Satus VARCHAR (25);

UPDATE bankchurn
SET Membership_Satus = (CASE
				WHEN `Is_Active_Member` = 1 THEN "Active"
		   ELSE "Not Active"
		  END
		);

```

-- -----------------------------------------------------------------------------------------


-- ------------------------------------------------------------------------------------------
-- Exit_Status From Exited --------------------------------------------------------------

```sql

SELECT
		Exited,
        (CASE
				WHEN `Exited` = 1 THEN "Churned"
		   ELSE "Not Churned"
		  END
		) AS Exit_Satus
FROM bankchurn;

ALTER TABLE bankchurn ADD COLUMN Exit_Status VARCHAR (25);

UPDATE bankchurn
SET Exit_Satus = (CASE
				WHEN `Exited` = 1 THEN "Churned"
		   ELSE "Not Churned"
		  END
		);

```
       
 -- -------------------------------------------------------------------------------- 
 
 
-- --------------------------------------------------------------------------------
-- Age Bracket: Using Globacom age bracket categorization -------------------------

```sql

SELECT
	Age,
    (CASE
			WHEN `Age` BETWEEN 18 AND 24 THEN "Young Adult"
			WHEN `Age` BETWEEN 25 AND 35 THEN "Mid Adult"
			WHEN `Age` BETWEEN 36 AND 65 THEN "Old Adult"
		ELSE "Elder"
	END
    ) AS Age_Bracket
FROM bankchurn;


ALTER TABLE bankchurn ADD COLUMN Age_Bracket VARCHAR(25);

UPDATE bankchurn
SET Age_Bracket = (CASE
			WHEN `Age` BETWEEN 18 AND 24 THEN "Young Adult"
			WHEN `Age` BETWEEN 25 AND 35 THEN "Mid Adult"
			WHEN `Age` BETWEEN 36 AND 65 THEN "Old Adult"
		ELSE "Elder"
	END
	);

```
   
-- --------------------------------------------------------------------------------


-- -------------------------------------------------------------------------------

-- Credit Score group from Credit Score -----------------------------------------

```sql

SELECT
	Credit_Score,
    (CASE
			WHEN `Credit_Score` BETWEEN 350 AND 550 THEN "Poor Credit"
			WHEN `Credit_Score` BETWEEN 551 AND 650 THEN "Fair Credit"
			WHEN `Credit_Score` BETWEEN 651 AND 750 THEN "Good Credit"
		ELSE "Excellent"
	END
    ) AS CreditScoreGroup
FROM bankchurn;


ALTER TABLE bankchurn ADD COLUMN CreditScoreGroup VARCHAR(25);

UPDATE bankchurn
SET CreditScoreGroup  = (CASE
			WHEN `Credit_Score` BETWEEN 350 AND 550 THEN "Poor Credit"
			WHEN `Credit_Score` BETWEEN 551 AND 650 THEN "Fair Credit"
			WHEN `Credit_Score` BETWEEN 651 AND 750 THEN "Good Credit"
		ELSE "Excellent"
	END
	);

```
   
-- ----------------------------------------------------------------------------

-- ---------------------------------------------------------------------------------
-- Salary Class from Estimate Salary ------------------------------------------------
 
```sql

 SELECT
	Estimated_Salary,
    (CASE
			WHEN `Estimated_Salary` BETWEEN 0 AND 1000 THEN "0-1k"
			WHEN `Estimated_Salary` BETWEEN 1001 AND 10000 THEN "1k-100k"
			WHEN `Estimated_Salary` BETWEEN 10001 AND 100000 THEN "10k-100k"
            WHEN `Estimated_Salary` BETWEEN 100001 AND 150000 THEN "100k-150k"
		ELSE "Above 150k"
	END
    ) AS SalaryClass
FROM bankchurn;

ALTER TABLE bankchurn ADD COLUMN SalaryClass VARCHAR(25);

UPDATE bankchurn
SET SalaryClass = (CASE
			WHEN `Estimated_Salary` BETWEEN 0 AND 1000 THEN "0-1k"
			WHEN `Estimated_Salary` BETWEEN 1001 AND 10000 THEN "1k-100k"
			WHEN `Estimated_Salary` BETWEEN 10001 AND 100000 THEN "10k-100k"
            WHEN `Estimated_Salary` BETWEEN 100001 AND 150000 THEN "100k-150k"
		ELSE "Above 150k"
	END
	);

```
  
-- -------------------------------------------------------------------------------------

-- ----------------------------------------------------------------------------------------
-- Acc Bal Class form Balance -----------------------------------------------------------------

```sql

SELECT
	Balance,
    (CASE
			WHEN `Balance` BETWEEN 0 AND 1000 THEN "0-1k"
			WHEN `Balance` BETWEEN 1001 AND 10000 THEN "1k-10k"
			WHEN `Balance` BETWEEN 10001 AND 100000 THEN "10k-100k"
            WHEN `Balance` BETWEEN 100001 AND 200000 THEN "100k-200k"
		ELSE "Above 200k"
	END
    ) AS Acc_Bal_Class
FROM bankchurn;

ALTER TABLE bankchurn ADD COLUMN Acc_Bal_Class VARCHAR(25);

UPDATE bankchurn
SET Acc_Bal_Class = (CASE
			WHEN `Balance` BETWEEN 0 AND 1000 THEN "0-1k"
			WHEN `Balance` BETWEEN 1001 AND 10000 THEN "1k-10k"
			WHEN `Balance` BETWEEN 10001 AND 100000 THEN "10k-100k"
            WHEN `Balance` BETWEEN 100001 AND 200000 THEN "100k-200k"
		ELSE "Above 200k"
	END
	);

```


-- ------------------------------------ ANALYSIS ---------------------------------------------------------------------
#### The Following questions and Tasked are to be answered and completed respectively by the below codes
1. Build a relational database using MySQL with the provided dataset.

 

2. Write SQL queries to answer the following:

 

i. Identify the most churned category across all segments

 

ii. Calculate the total number of churned customers

 

iii. Determine the overall number of customers

 

3.Create a Power BI report for the analysis.
-- --------------- ----------------------------------------------- --------------------------------------------------

-- ------------------------------------ -----------------------------------------------------------------------------

-- --------------- OVERVIEW --------------------------------------------------

```sql

SELECT *
FROM bankchurn;

```

-- --------------- (2. iii)  Determine the overall number of customers ----------------------------------
-- Total Distinct Customers -----------------------------------------

``` sql

SELECT 
COUNT(DISTINCT (Customer_ID)) AS Total_Custommer
FROM bankchurn;

```

-- ---------------------------------------------------------------------------------

-- ------------------------------------------------------------------------------

-- 2 (ii) Calculate the total number of churned customers ------------------------------------

```sql

SELECT 
Exit_Satus,
COUNT(DISTINCT (Customer_ID)) AS Total_Custommer
FROM bankchurn
WHERE Exit_Satus = "Churned";

```

-- ---------------------------------------------------------------------------

-- ----------------------- -----------------------------------------------------------------------------
-- ------------ (2 iii) Write SQL queries to answer the following ----------------------------------------------------------
-- --------------------------------- Identify the most churned category across all segments ----------------------

-- Most Churned Age Bracket -------------------------------------------

```sql

SELECT
Age_Bracket,
COUNT(DISTINCT (Customer_ID)) AS Total_Custommer
FROM Bankchurn
WHERE Exit_Satus = "Churned"
GROUP BY Age_Bracket
ORDER BY Total_Custommer DESC
Limit 1;

```

-- ------------------------------------------------------------------

-- ------------------------------------------------------------------
-- ------------------- Most Churned Gender -----------------------------------------------------

```sql

SELECT
Gender,
COUNT(DISTINCT (Customer_ID)) AS Total_Custommer
FROM Bankchurn
WHERE Exit_Satus = "Churned"
GROUP BY Gender
ORDER BY Total_Custommer DESC
LIMIT 1;

```

-- -------------------------------------------------------------------------

-- Most churned Tenure -----------------------------------------------------

``` sql

SELECT
Tenure AS YearsOFTenure,
COUNT(DISTINCT (Customer_ID)) AS Total_Custommer
FROM Bankchurn
WHERE Exit_Satus = "Churned"
GROUP BY YearsOFTenure
ORDER BY Total_Custommer DESC
LIMIT 1;

```


-- --------------------------------------------------------------------------

-- Moost Churn Geography ------------------------------------

```sql

SELECT
Geography AS Country,
COUNT(DISTINCT (Customer_ID)) AS Total_Custommer
FROM Bankchurn
WHERE Exit_Satus = "Churned"
GROUP BY Country
ORDER BY Total_Custommer DESC
LIMIT 1;

```

-- ----------------------------------------------------------------

-- ------------------------------------------------------------------
-- Most Churn base on wether the customer has Credit card or not--------


```sql

SELECT
Cr_Card_Status,
COUNT(DISTINCT (Customer_ID)) AS Total_Custommer
FROM Bankchurn
WHERE Exit_Satus = "Churned"
GROUP BY Cr_Card_Status
ORDER BY Total_Custommer DESC
LIMIT 1;

```


-- ----------------------------------------------------------------------------


-- --------------------------------------------------------------------------------
-- Most Churn Based on Num Of Product

```sql

SELECT
Num_Of_Product,
COUNT(DISTINCT (Customer_ID)) AS Total_Custommer
FROM Bankchurn
WHERE Exit_Satus = "Churned"
GROUP BY Num_Of_Product
ORDER BY Total_Custommer DESC
LIMIT 1;

```

-- ------------------------------------------------------------------------

-- -----------------------------------------------------------------------------------
-- Credit Score Group from Credit score: using standard credit score models like FICO and Vanage Score --------------------------------------------

-- Most Churn Based on Credit Score Group------------------------------------- 


```sql

SELECT
CreditScoreGroup,
COUNT(DISTINCT (Customer_ID)) AS Total_Custommer
FROM Bankchurn
WHERE Exit_Satus = "Churned"
GROUP BY CreditScoreGroup
ORDER BY Total_Custommer DESC
LIMIT 1;

```

-- ------------------------------------------------------------------------------------

-- -------------------------------------------------------------------------------------
-- Salary Class from Estmated Salary ----------------------------------------------------------------

-- Most Churned based on Salary Class ------------------------------------------------------------------------------


```sql

SELECT
SalaryClass,
COUNT(DISTINCT (Customer_ID)) AS Total_Custommer
FROM Bankchurn
WHERE Exit_Satus = "Churned"
GROUP BY SalaryClass
ORDER BY Total_Custommer DESC
LIMIT 1;


```

-- -------------------------------------------------------------------------------------------------

-- --------------------------------------------------------------------------------------------------
-- Most Churn Based on Account Balance class ------------------------------------------

```sql

SELECT
Acc_Bal_Class,
COUNT(DISTINCT (Customer_ID)) AS Total_Custommer
FROM Bankchurn
WHERE Exit_Satus = "Churned"
GROUP BY Acc_Bal_Class
ORDER BY Total_Custommer DESC
LIMIT 1;

```
-- ------------------------------------------------------------------------------

-- ------------------------------------------------------------------------------
-- Most Churned based on Membership Status ------------------------------------

``` sql

SELECT
Membership_Satus,
COUNT(DISTINCT (Customer_ID)) AS Total_Custommer
FROM Bankchurn
WHERE Exit_Satus = "Churned"
GROUP BY Membership_Satus
ORDER BY Total_Custommer DESC
LIMIT 1;

```


## Results/Findings
##### Data Insight Generation for Churn Analysis

1.	The Dataset consist of 10,000 Distinct customers
2.	2,037 customers churned
3.	Churn rate of 20.37% was observed which is above the 15% of the normal average churned rate as proposed by Prof, F. Fiordelisi (Essex Business School of University of Essex)
4.	Old Adult of Age range 36-65, happens to be the most churned, accounting for 1,655 individuals, approximately 81% of the Churn Population
5.	Females happens to churned more, as they account for 1,113 individuals, approximately 56% of the Churn Population
6.	Customers who just spent 1year in the service churned most in the tenure series as they 232, representing 11.4% of the total population. Further research is required on their experience and satisfaction rate. 
7.	Most churned Country is Germany with a staggering population of 814, representing 40% of the Churn Population
8.	Customer who has Credit Card churned more with an alarming population of 1424, representing 70% of the Churn Population. Further research is required on their experience and satisfaction rate.
9.	Base on number of product patronage, Customers who uses only one of our products churned the most with a shocking population of 1409, representing 69.1% of the Churn Population
10.	Customer with fair credit score (551-650) has the highest churn with Population of 689 which representing 34% of the total population
11.	Customers with Salary range of 10,000 to 100,000 has the highest churn with Population of 892 which representing 44% of the total population
12.	Customers whose account Balance fall between 100k-200k has the highest churn with Population of 1192 which representing 59% of the total population



## Recommendations
1. Further investigation is warranted to understand the factors contributing to the highest churn rate observed within the 36-65 age demographic. This trend is particularly concerning given that this age group typically represents a period of significant stability and development
2. Additional research is needed to identify the root causes of customer churn in Germany
3. Comprehensive research is also necessary to determine why customers with credit accounts are exhibiting a high volume of churn
4. Further investigation is necessary to understand why new customers are exhibiting a higher propensity to churn
