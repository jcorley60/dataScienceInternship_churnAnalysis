# Data Science Internship - Credit Union Churn Analysis

Project code was created during my Summer 2023 data science internship with Open Technology Solutions (OTS) which lasted just over 2-months.
Open and closed accounts at Bellco Credit Union for the lookback period 1/1/2022 - 8/1/2022 are examined. 
NOTE: Jupyter kernel cells (and project files) have been reviewed to ensure sensitive information from members (Personally Identifiable Information) and/or the financial institution has not been disclosed.


## Project Hypothesis

Alternative hypothesis, H_a:
It is possible to predict if an account will be closed within the next 30-days using at minimum the following feature set:
{
    prior 30-transactions,
    is this a jointly owned account?,
    age of account owner
}

## PROJECT FILES

1. README.md
	1. Establishes project goals and inventories project files, bringing to light the purpose of each project file

2. closed accounts_2.0_written_to_table.ipynb
	1. finds closed accounts w/in Snowflake-Snowpark data warehouse, using functional programming, for lookback period
		- Logic for closed accounts is determined and implemented
	2. establishes Snowflake connection via YAML config file & web-browser based authentication
	3. DataFrame testing for max aggregation choice variable to determine most recent account status (open, closed, etc.)
	4. writes closed accounts to a new Snowflake data warehouse for ease of review
		- uses SQL code to query newly created data warehouse as a check

3. open accounts_written_to_table.ipynb
	1. finds open accounts w/in Snowflake-Snowpark data warehouse, using functional programming, for lookback period
	2. the set difference between closed accounts and active accounts with at least 30-processing transactions for the given lookback period is taken
	3. establishes Snowflake connection via YAML config file & web-browser based authentication
	4. writes open accounts to a new Snowflake data warehouse for ease of review
		- uses SQL code to query newly created data warehouse as a check

4. table_view_exploration_feature_engineering.ipynb
	1. represents a cursory exploration of 1,500+ database tables held within OTS’ Snowflake instance including preliminary feature mapping heuristics 
		- Code blocks containing PII have been curated to remove PII per request.
	2. instrumental in feature engineering
	3. file witheld from GitHub
	4. e.g. Account Lockout Notifications Table Mapping:
		acct.acctnbr <--> 
		acctlockout.acctnbr | acctlockout.lockoutflagcd <-->
		lockoutflag.lockoutflagcd | lockoutflag.lockoutflagdesc
		- explanation: 
			1. column 'acctnbr' [account number] from the 'acct' table is joined on the 'acctnbr' column from the 'lockoutflag' table
			2. under the table 'acctlockout' we find another column 'lockoutflagcd' and use this column to join to table 'lockoutflag' via it's column 'lockoutflagcd'
			3. we arrive at account lockout flag descriptions for those accounts which have them, revealing info on accounts which have been locked out

5. account_metadata.ipynb
	1. This is a fine-tuned feature mapping which culminates in a final feature set. The final feature set is created as a Pandas DataFrame and is pickled (withheld from GitHub). Code blocks containing PII have been removed per request.
		- feature engineering of variables found in underlying SQL tables is performed
	2. code to grab 2 tables from 2 different Snowflake databases using a Snowflake SQL join
	3. functional programming used extensively
	4. visualization of raw underlying database columns in aggregate, where needed
	5. utilization of [Snowflake] distributed computing aggregate functions
	6. Windowing is used to determine the latest zipcode associated w/ an account as Credit Union members can/have moved at any time, sometimes frequently

6. model_and_evaluation.ipynb
	1. Churn analysis modeling & model evaluation
		1. Random Forest Classifier
		2. covariance among variables is examined
		3. one-hot encoding of variables for subsequent modeling
		4. splitting of data for train-test-split (train:test --> 70:30)
		5. Confusion matrix evaluation of model, classification report (precision, recall, F1-score, accuracy), ROC curve
	2. Feature importance is examined from RandomForestClassifier model to determine which factors contributed the most to members closing their Bellco credit union accounts
		1. if a member had a consumer loan they are more likely to close this account (paid off a loan or refinanced it potentially, likely indicating Bellco was primarily used for a loan)
		2. how many transactions a member processed leading up to their account closing is important; the more transactions in a given period contributes to ultimate account closure
			- this can be explored further, for example: how many transactions were processed recently as compared to a member's average number of transactions?
		3. if a member is using ATMs for cash withdrawals factors-in when determining if they might close out their account and leave Bellco
			- this can be explored further: is there a threshold for the number of ATM withdrawals during any given period, or does it matter if a member is primarily using ATMs for withdrawals, only (as a percent let's say)?

7. MemberChurnAnalysis_InternshipProject_Presentation.pptx
	1. PowerPoint presentation given to executive management
	2. High-level presentation for business exec/CEO audience with minimal code
	
8. CS3904 Internship_Executive Summary.pdf
	1. Executive Summary of project in 2 pages

