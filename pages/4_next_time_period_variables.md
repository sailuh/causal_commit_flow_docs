---
layout: default
title: Next Time Period Variables
parent: Home
nav_order: 4
---


## 4. Appending Variables Representing the Next Time Period 

Having addressed missing values, we now expand our set of features by including their respective next time period.

### 4.1 Next Time Period Features

Consider Section 2 features, now with missing data addressed. We now use them to derive the next time step features. More specifically, these features repeat the 11 features assigned columns E-O of Section 2 but instead pertain to the next time period (90 days) in a CVE’s timeline. Their names match the 11 features of Sextion 2 but have a suffix “2” appended to their names. Their derivation is simple: Each of these next-time period-related features are derived as follows: Make a copy of the corresponding feature and shift all of its values up by one row. Of course, the top value of each original-variable column is simply unused; and the bottom value of the new column is blank.  

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

As there were 5563 rows and there are 121 CVEs represented in the dataset, this should leave 5442 rows (after the header row). And instead of 15 columns, there should now be 26.

The result of this appending operation should look like this:

```diff
+ openssl_social_smells_timeline..renameVariables..resolveMD..deleteLastRecordEachCVE.csv
```
