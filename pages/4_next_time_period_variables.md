---
layout: default
title: Next Time Period Variables
parent: Home
nav_order: 4
---


## 4. Appending Variables Representing the Next Time Period 

Append to the dataset these additional 11 variables representing the impact on the next time period: org_silo2, mis_link2, silence2, congruence2, communicate2, code_dev2, file2, mail_dev2, thread2, commit2, churn2.

* These additional variables and how they are created is explained in the data dictionary near the beginning of this section. 
* Further detail: each of these 11 variables ending in “2” was created in Excel by first creating a new column having the appropriate column header and then in the column of the corresponding original variable (i.e., pertaining to the current time period), copy the values starting in row 3 through row 5563 into positions row 2 through row 5562 of that new column. 
* Of course, when performing this copy and paste, the values in row 2 for the original variables are ignored/lost; while for the corresponding next time-period variables, the values associated with the last time period of any CVE’s remediation no longer make sense (or are blank in the case of the very last row) and should be deleted.
* To accomplish this deletion of the last time-period record for each CVE within Excel, create an indicator variable that indicates when a case represents the last time period for a CVE’s remediation. Detail: in row 2, insert a formula in a new column: “=IF([next-period cve_id]=[current cve_id],0,1)”, copy that cell and paste the formula onto the rest of the column; then copy-paste-value-only the entire column so that the formulas are replaced by their values. Then as we explained elsewhere, put a filter on that column, select only the rows with 1s (the rows that are the last cases for a CVE’s remediation) in the pull-down menu, and delete the resulting rows (i.e., the rows appearing under the header row). Then remove the filter and delete the indicator variable column.
* As there were 5563 rows and there are 121 CVEs represented in the dataset, this should leave 5442 rows (after the header row). And instead of 15 columns, there should now be 26.

The result of this appending operation should look like this:

```diff
+ openssl_social_smells_timeline..renameVariables..resolveMD..deleteLastRecordEachCVE.csv
```
