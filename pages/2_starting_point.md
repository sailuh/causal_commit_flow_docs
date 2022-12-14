---
layout: default
title: Starting Point
parent: Home
nav_order: 2
---


## 2. Starting point

The starting point for causal discovery is this dataset (CSV file) produced by [Kaiaulu's Issue Commit Flow Notebook](https://github.com/sailuh/kaiaulu/blob/master/vignettes/issue_social_smell_showcase.Rmd) ; henceforth called Kaiaulu: 

```diff
+ openssl_social_smells_timeline.csv
```

The following table provides a Data Dictionary explaining the variables in the dataset. The first column defines Kaiaulu's output. The second column defines derived variables for input to causal discovery, and hereafter to disambiguate they are defined as features. Since missing values in the original table reflect in meaning zero commits, they were replaced with zeros (MD Reason 2 further below). 

The Data Dictionary is separated in 4 tables for clarity, however it represents a single data table. 

### 2.1 Descriptive Variables

| Variable| Definition |	Derived Variable (i.e. Feature) |
|-------------------|------------|----------------------------------------------------------------|
|cve_id (A)| Identifies a software vulnerability in OpenSSL| Converted from String to Integer due to Tetrad data type limitations. <br /> <br /> Concatenate the last two digits of the year with the last four digits of the cve_id and convert into an integer. (E.g. 2006 and CVE ID XXX4339 becomes 06339). <br /> <br />   Rationale: The CVE ID feature is converted into a set of dichotomous indicator features, one per CVE ID. This conversion allows searching the entire dataset as a single CSV file (rather than one per CVE ID). <br /> <br /> Limitations: While making the application of causal discovery simpler, the transformation presumes a single underlying causal model relating sociotechnical measures to outcomes within consecutive time periods for CVE remediation. However, the extent to which such a common-across-CVEs structural causal model exists is also concurrently tested by this inclusion of the dichotomous CVE indicators in the dataset being searched; and seeing whether and how many edges form between a CVE dichotomous indicator. So, in what follows, we pay particular attention to how many such edges form between particular CVEs and any sociotechnical or outcome variables. |
| commit_interval (activity_0: B) (activity_2: C)	| Concatenation of the hashes of the first and last instances of CVE-related activity (especially commits) during the time period. If there is only one commit within the 3-month period associated with a given CVE remediation, then the hashes will be identical. If there are none, then the field will just be blank. | Replaced with indicator features: <br /> <br /> •	activity_0 (equals 1 if and only if zero hashes appear) and <br /> •	activity_2 (equals 1 if and only if two hashes appear) <br /> <br /> Later, both indicator features were removed following their use in preparation of the dataset for causal discovery, given their redundancy. <br /> <br /> |
| start_day (D) | 	First day of the time period |	Renamed “Start” for brevity. <br /> <br /> Replaced with date ordinal value due to Tetrad not supporting datetime datatype.|
| end_day | 	90 days after the start_day	| Deleted due to deterministic relationship with start_day |


### 2.2 Social Smell Variables

The next three variables (org_silo, missing_links, and radio_silence) represent “social smells”, as defined in Kaiaulu's Notebook. 

| Variable| Definition |	Derived Variable (i.e. Feature) |
|-------------------|------------|----------------------------------------------------------------|
| org_silo (E) | 	The number of a pair of developer file changes within a time period which at least one developer is not subscribed to the mailing list	| No change to variable name | 
| missing_links (F)	| The number of a pair of developer file changes within a time period which the pair of developers do not reply to the same e-mail thread in the mailing list. | Renamed “mis_link” for brevity | 
| radio_silence (G)	| Count of delays due to lack of direct communication between two sub-communities engaged in a CVE’s remediation. | Renamed “silence” for brevity | 
| primma_donna |	(Not yet fully operationalized at the time of our analysis and thus dropped from further consideration.)	| Deleted | 
| st_congruence (H) |	Short for “Socio-technical congruence.” A single continuous measure of overall similarity between collaboration graph and the file graph. <br /> <br /> Scale from 0..1 (1 means perfect congruence). |	Renamed “congruence” for brevity | 
| Communicability (I) |	A continuous measure of the diffusion of information. How likely is it that an architectural decision is known by the set of developers that need to know about it. <br /> <br /> Scale from 0..1 (1 means perfect communicability). Is the inverse of incommunicability. |	Renamed “communicate” for brevity | 

### 2.3 Statistics Variables 

| Variable| Definition |	Derived Variable (i.e. Feature) |
|-------------------|------------|----------------------------------------------------------------|
| code_only_devs (J) |	Count of developers who made commits to the implicated CVE-related files during the time period. (For the CVE identified in column A.) |	Renamed “code_dev” for brevity | 
| code_files (K)	| Count of files involved in CVE’s remediation. |	Renamed “file” for brevity | 
| ml_only_devs (L)	| Count of people who sent at least one item to mailing list during time period. Not necessarily on topic. |	Renamed “mail_dev” for brevity | 
| ml_threads (M) | Count of email threads in ML during that time window on any topic. | Renamed “thread” for brevity | 
| n_commits (N) |	Count of commits (there can be multiple commits for multiple files) during time period | 	Renamed “commit” for brevity |
| sum_churn (O)	| Count of Lines of Code (LOC) committed to the code file counted in code_files; summed over the commits in n_commits.	| Renamed “churn” for brevity | 

### 2.4 Next Time Period Features

The following features are defined from the aforementioned derived variables (i.e. features) after addressing missing data in Section 4. "Appending Variables Representing the Next Time Period. Since their definition column is the same as the equivalent in the aforementioned tables, and hence omitted here.

These features repeat the 11 features assigned columns E-O above but instead pertain to the next time period (90 days) in a CVE’s timeline. Their names match the 11 features above but have a suffix “2” appended to their names. Their derivation is simple: Each of these next-time period-related features are derived as follows: Make a copy of the corresponding feature and shift all of its values up by one row. Of course, the top value of each original-variable column is simply unused; and the bottom value of the new column is blank.  

Finally, for each CVE id, the record representing the last time period for a CVE’s remediation is then deleted. This is because it had gotten either the values of the first time period of the next CVE, or blanks, if it was the last row of the entire dataset. 

The features appear in the same order as the corresponding original 11 features above: org_silo2, mis_link2, silence2, congruence2, communicate2, code_dev2, file2, mail_dev2, thread2, commit2, churn2.


| Variable|	Derived Variable (i.e. Feature) |
|----------------|----------------------------------------------------------------|
| org_silo2 (P) |	Same count as “org_silo” but applied to the next time period |
| mis_link2 (Q)	| Same count as “mis_link” but applied to the next time period |
| silence2 (R)	| Same count as “silence” but applied to the next time period |
| congruence2 (S)	| Same 0..1 measure as “congruence” but applied to the next time period |
| communicate2 (T) | Same 0..1 measure as “communicate” but applied to the next time period |
| code_dev2 (U)	| Same count as “code_dev” but applied to the next time period |
| file2 (V)	| Same count as “file” but applied to the next time period |
| mail_dev2 (W)	| Same count as “mail_dev” but applied to the next time period |
| thread2 (X)	| Same count as “thread” but applied to the next time period |
| commit2 (Y)	| Same count as “commit” but applied to the next time period |
| churn2 (Z)	| Same count as “churn” but applied to the next time period |

The results of this initial conversion appear here: 

```diff
+ openssl_social_smells_timeline..renameVariables.csv
```

Note that we keep the original filename but append a brief phrase (“...renameVariables”) to indicate what action we just took on the dataset. The original dataset and this name-converted dataset both have 6697 rows.

Also, we assume that all the rows dealing with the same CVE are contiguous to each other and also that for each CVE, rows appear in time period order (for the same CVE).
