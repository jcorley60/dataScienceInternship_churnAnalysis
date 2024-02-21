#! START_HERE.txt

############
# OVERVIEW #
############
Project code was created during my Summer 2023 data science internship with Open Technology Solutions (OTS) which lasted just over 2-months.  
Open and closed accounts at Bellco Credit Union for the lookback period 1/1/2022 - 8/1/2022 are examined. 
NOTE: Jupyter kernel cells (and project files) have been reviewed to ensure sensitive information from members (Personally Identifiable Information) and/or the financial institution has not been disclosed.


######################
# Project Hypothesis #
######################
Alternative hypothesis, H_a:
It is possible to predict if an account will be closed within the next 30-days using at minimum the following feature set:
{
    prior 30-transactions,
    is this a jointly owned account?,
    age of account owner
}


#################
# PROJECT FILES #
#################
1. START_HERE.txt
	A. Project file inventory which documents project goals as well as the purpose of each project file

2. closed accounts_2.0_written_to_table.ipynb
	A. finds closed accounts w/in Snowflake-Snowpark data warehouse, using functional programming, for lookback period
		i. Logic for closed accounts is determined and implemented
	B. establishes Snowflake connection via YAML config file & web-browser based authentication
	C. DataFrame testing for max aggregation choice variable to determine most recent account status (open, closed, etc.)
	D. writes closed accounts to a new Snowflake data warehouse for ease of review
		i. uses SQL code to query newly created data warehouse as a check

3. open accounts_written_to_table.ipynb
	A. finds open accounts w/in Snowflake-Snowpark data warehouse, using functional programming, for lookback period
	B. the set difference between closed accounts and active accounts with at least 30-processing transactions for the given lookback period is taken
	C. establishes Snowflake connection via YAML config file & web-browser based authentication
	D. writes open accounts to a new Snowflake data warehouse for ease of review
		i. uses SQL code to query newly created data warehouse as a check

4. table_view_exploration_feature_engineering.ipynb
	A. represents a cursory exploration of 1,500+ database tables held within OTS’ Snowflake instance including preliminary feature mapping heuristics 
		i. Code blocks containing PII have been curated to remove PII per request.
	B. instrumental in feature engineering
	C. file witheld from GitHub
	D. e.g. Account Lockout Notifications Table Mapping:
		acct.acctnbr <--> 
		acctlockout.acctnbr | acctlockout.lockoutflagcd <-->
		lockoutflag.lockoutflagcd | lockoutflag.lockoutflagdesc
		i. explanation: 
			a. column 'acctnbr' [account number] from the 'acct' table is joined on the 'acctnbr' column from the 'lockoutflag' table
			b. under the table 'acctlockout' we find another column 'lockoutflagcd' and use this column to join to table 'lockoutflag' via it's column 'lockoutflagcd'
			c. we arrive at account lockout flag descriptions for those accounts which have them, revealing info on accounts which have been locked out

5. account_metadata.ipynb
	A. This is a fine-tuned feature mapping which culminates in a final feature set. The final feature set is created as a Pandas DataFrame and is pickled (withheld from GitHub). Code blocks containing PII have been removed per request.
		i. feature engineering of variables found in underlying SQL tables is performed
	B. code to grab 2 tables from 2 different Snowflake databases using a Snowflake SQL join
	C. functional programming used extensively
	D. visualization of raw underlying database columns in aggregate, where needed
	E. utilization of [Snowflake] distributed computing aggregate functions
	F. Windowing is used to determine the latest zipcode associated w/ an account as Credit Union members can/have moved at any time, sometimes frequently

6. model_and_evaluation.ipynb
	A. Churn analysis modeling & model evaluation
		i. Random Forest Classifier
		ii. covariance among variables is examined
		iii. one-hot encoding of variables for subsequent modeling
		iv. splitting of data for train-test-split (train:test --> 70:30)
		v. Confusion matrix evaluation of model, classification report (precision, recall, F1-score, accuracy), ROC curve
	B. Feature importance is examined from RandomForestClassifier model to determine which factors contributed the most to members closing their Bellco credit union accounts
		i. if a member had a consumer loan they are more likely to close this account (paid off a loan or refinanced it potentially, likely indicating Bellco was primarily used for a loan)
		ii. how many transactions a member processed leading up to their account closing is important; the more transactions in a given period contributes to ultimate account closure
			a. this can be explored further, for example: how many transactions were processed recently as compared to a member's average number of transactions?
		iii. if a member is using ATMs for cash withdrawals factors-in when determining if they might close out their account and leave Bellco
			a. this can be explored further: is there a threshold for the number of ATM withdrawals during any given period, or does it matter if a member is primarily using ATMs for withdrawals, only (as a percent let's say)?

7. MemberChurnAnalysis_InternshipProject_Presentation.pptx
	A. PowerPoint presentation given to executive management
	B. High-level presentation for business exec/CEO audience with minimal code
	
8. CS3904 Internship_Executive Summary.pdf
	A. Executive Summary of project in 2 pages

