---
layout: default
title: Starting Point
parent: Home
nav_order: 2
---


## 2. Starting point

The starting point for causal discovery is this dataset (CSV file) produced by the issue-tracking and mailing-list scraping tools we were using; henceforth called the “scraping tool”: 

```diff
+ openssl_social_smells_timeline.csv
```

Here is a Data Dictionary explaining the variables in the dataset; both those variables (1) included in the initial dataset produced by the scraping tool; or (2) newly derived variables for input to causal discovery. While the second column, Definition, duplicates material already covered better elsewhere, note that for any time periods when there were zero commits, these definitions imply that many of the measures should be zero; and thus it is safe to impute zero for this particular source of missing values (what is called MD Reason 2 further below). (Don’t do any of these changes yet—we’ll address them below the table.)

| Variable (column)	| Definition |	How treated in preparing for and conducting causal discovery |
|-------------------|------------|----------------------------------------------------------------|
|cve_id (A)| The identifier of the CVE being remediated within the OpenSSL project, which maintains the Secure Socket Layer code critical to the Chromium Project; as described in our article. Figure 1 depicts overall timeline for a single CVE’s remediation. | In the original dataset, cve_id was represented as a string. Converted to an integer due to a limitation in what datatypes are supported by the software packages we were using (including Tetrad [we’ll say more about “Tetrad” later in this document]). <br /> <br /> Detail: the earliest cve is from year 2006, and so the “20” is redundant, so concatenate the last two digits of the year with the last four digits of the cve_id and convert into an integer. The specific Excel formula used was `=NUMBERVALUE(CONCAT(LEFT(RIGHT(cve_id,7),2),RIGHT(cve_id,4)))` without the quotes. (Further detail: create a new column B, select B2, paste the above formula into it, and replace both instances of “cve_id” with the cell address (here, A2) and then that cell formula is copied down to the bottom of the spreadsheet, the 2nd column selected, copied, and paste values before deleting the original cve_id column.)  The resulting conversion is an injection: records with the same unique cve_id are mapped to the same unique integer. <br /> <br />   How used: later, cve_id is converted into a set of dichotomous indicator variables, one per cve_id; and the variable itself was deleted. This conversion allows searching the entire dataset as a single CSV file (rather than one per cve_id). While making the application of causal discovery much simpler, there is a downside to this approach, in that it presumes that there is a single underlying causal model relating sociotechnical measures to outcomes within consecutive time periods for CVE remediation. However, the extent to which such a common-across-CVEs structural causal model exists is also concurrently tested by this inclusion of the dichotomous CVE indicators in the dataset being searched; and seeing whether and how many edges form between a CVE dichotomous indicator. So, in what follows, we pay particular attention to how many such edges form between particular CVEs and any sociotechnical or outcome variables. |
| commit_interval (activity_0: B) (activity_2: C)	| Concatenation of the hashes of the first and last instances of CVE-related activity (especially commits) during the time period. If there is only one commit within the 3-month period associated with a given CVE remediation, then the hashes will be identical. If there are none, then the field will just be blank. | Replaced with indicator variables: <br /> <br /> •	activity_0 (equals 1 if and only if zero hashes appear) and <br /> •	activity_2 (equals 1 if and only if two hashes appear) <br /> Later, both indicator variables were removed following their use in preparation of the dataset for causal discovery, given their redundancy. <br /> <br /> Detail: variable commit_interval is the concatenation of two 40-character hashes separated by a hyphen; or blank if there are no commits to form hashes from. Here are the Excel formulas used to derive the two activity indicator variables: <br /> <br /> •	activity_0 - used `=IF(commit_interval="",1,0)` <br /> •	activity_2 - used `=IF(AND(NOT(activity_0),LEFT(commit_interval,40)<> RIGHT(commit_interval,40)),1,0)` (Less detail this time: create 2 new columns, edit 2nd row cells with the above formula but replacing “activity_0” and “commit_interval” with the correct cell addresses, etc.)
| start_day (D) | 	First day of the time period |	Initially, renamed “Start” for brevity. <br /> <br /> Replaced with date ordinal value because our causal discovery tool doesn’t support the datetime datatype. The conversion was achieved by using an Excel formula: `=datevalue(left(start_day,10))`
| end_day | 	90 days after the start_day	| Deleted due to deterministic relationship with start_day |
|(3 Communication smells-related variables)	| | The next three variables (org_silo, missing_links, and radio_silence) represent “communication smells” and are derived in a similar way: the scraping tool we used builds two graphs: Graph A for who posts and who responds, Graph B for relationships between code units, and then maps Graph A into Graph B (developer D worked on code C). All three communication smells are counts of particular features of the connectivity among these graphs. (The main text of the article provides a more precise explanation for radio_silence [silence].) | 
| org_silo (E) | 	Count of “the number of single collaborations between different developers in which at least one of them does not participate in the [appropriate] communication channel” for the current time period	| No change to variable name | 
| missing_links (F)	| Count of instances where two people were working on same file within the time period, but there’s no evidence of communication from looking at issue tracker and mailing list. | Renamed “mis_link” for brevity | 
| radio_silence (G)	| Count of delays due to lack of direct communication between two sub-communities engaged in a CVE’s remediation. | Renamed “silence” for brevity | 
| primma_donna |	(Not yet fully operationalized at the time of our analysis and thus dropped from further consideration.)	| Deleted | 
| st_congruence (H) |	Short for “Socio-technical congruence.” A single continuous measure of overall similarity between collaboration graph and the file graph. Scale from 0..1 (1 means perfect congruence). |	Renamed “congruence” for brevity | 
| Communicability (I)	A continuous measure of the diffusion of information. How likely is it that an architectural decision is known by the set of developers that need to know about it. |  Scale from 0..1 (1 means perfect communicability). Is the inverse of incommunicability. |	Renamed “communicate” for brevity | 
| code_only_devs (J) |	Count of coders who made commits to the implicated CVE-related files during the time period. (For the CVE identified in column A.) |	Renamed “code_dev” for brevity | 
| code_files (K)	| Count of files involved in CVE’s remediation. |	Renamed “file” for brevity | 
| ml_only_devs (L)	| Count of people who sent at least one item to mailing list during time period. Not necessarily on topic. |	Renamed “mail_dev” for brevity | 
| ml_threads (M) | Count of email threads in ML during that time window on any topic. | Renamed “thread” for brevity | 
| n_commits (N) |	Count of commits (there can be multiple commits for multiple files) during time period | 	Renamed “commit” for brevity |
| sum_churn (O)	| Count of Lines of Code (LOC) committed to the code file counted in code_files; summed over the commits in n_commits.	| Renamed “churn” for brevity | 
| (next time period-related variables) | | DON’T DO THIS STEP YET. This step is best done after addressing missing data. Wait until the subsection “Appending Variables Representing the Next Time Period” to accomplish this step. | <br /> <br /> The final few variables in this table are a repeat of the 11 variables assigned columns E-O above but instead pertain to the next time period (90 days) in a CVE’s remediation; and their names match the 11 variables above but have a suffix “2” appended to their names. <br /> <br /> These 11 new variables represent what happened in the next time period that could (in principle) be partly attributed to what happened in the current time period for the CVE identified in column A, or to idiosyncrasies of the CVE itself (e.g., whose resolution requires a skillset not present in coders involved in its remediation). <br /> <br /> In terms of manipulating the dataset, each of these next-time period-related variables is derived in this way: make a copy of the corresponding original variable and shift all of its values up by one row. Of course, the top value of each original-variable column is simply unused; and the bottom value of the new column is blank.  <br /> <br /> Finally, for each CVE id, the record representing the last time period for a CVE’s remediation is then deleted. Of course, it had gotten either the values of the first time period of the next CVE, or blanks, if it was the last row of the entire dataset. For more detail on how to accomplish this step, see the subsection below titled “Appending Variables Representing the Next Time Period.” <br /> <br /> The variables appear in the same order as the corresponding original 11 variables above: org_silo2, mis_link2, silence2, congruence2, communicate2, code_dev2, file2, mail_dev2, thread2, commit2, churn2. | 
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

Note the details of how the conversions mentioned in the third column above are achieved won’t matter as long as certain properties of the variables are retained: in particular, the cve_id conversion need only be an injection (one-to-one); the conversion of start-day to an integer can be any standard datetime conversion to integer (as long as it is linear and monotonic; as many such conversions are). 
Now we implement the data conversions and renaming of variables implied in the table above. The results of this initial conversion appear here: 

```diff
+ openssl_social_smells_timeline..renameVariables.csv
```

Note that we keep the original filename but append a brief phrase (“...renameVariables”) to indicate what action we just took on the dataset. The original dataset and this name-converted dataset both have 6697 rows.

Also, we assume that all the rows dealing with the same CVE are contiguous to each other and also that for each CVE, rows appear in time period order (for the same CVE). It’s straightforward to check these conditions in Microsoft Excel, for example by adding a formula(s) in a new column(s) to check this out. (We’ll give a couple examples later of similar manipulations in Excel which have additional explanation.)
