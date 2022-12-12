---
layout: default
title: Removing Short CVE Timelines
parent: Home
nav_order: 5
---


 * Prune away any CVEs (i.e., their associated records from the dataset) having insufficient representation in our dataset for discovering casual relationships specifically for that CVE.
    * If a CVE’s remediation timeline is too short then key underlying causal relationships cannot be discovered or any that are discovered will not be as credible (more likely to be false positives). Within our dataset, seven CVEs are only represented by seven time periods or less (six, after deleting the last row of each CVE timeline as a last step of appending variables from the next time period). Discovering a structured causal model from such a small sample that faithfully represents the underlying direct causal relationships (relative to the variables collected) might be difficult. Thus, these CVEs with only seven time periods are a good candidate for deletion. 
    * On the other hand, we want to be careful not to delete too many CVEs or what we learn from the remaining subset of CVEs might not adequately represent the OpenSSL Project in general.
    * Also, recollect that we deleted about 17% of the records, but these were for CVEs whose remediation timelines started before 2007. If their timeline continued well past 2007, then we’ll have plenty of time periods to analyze and for more recent time periods; but if not, then we don’t really have many time periods anyway (and none too recent) to draw useful (and timely) conclusions about the underlying causal relationships of interest; and thus the latter are also hardly bad candidates for deletion.
    * Beyond these seven CVEs, there are 114 CVEs with longer remediation timelines represented in our dataset. The shortest of these timelines includes 18 time periods (17 after the appending variables from next time period step), which may be more than sufficient to discovering any significant causal relationships (significant enough to have an observable change in outcomes of interest).
    * Thus 7 or fewer cases seems like a reasonable threshold for guiding us in which CVEs to prune from the dataset that will still leave us with a large majority of the dataset for analysis but with each retained CVE well represented in terms of the number of time periods available for analysis (at least 18).
    * Therefore, we deleted the 7 CVEs (their associated rows) with 7 or fewer time periods. Their deletion leaves us with a total of 35 fewer cases (rows). (Actually, we delete 28 rows if we do the deletion after appending variables from the next time period and deleting the last row from each CVE remediation timeline.)
    * 	Detail: specify headers for two new columns: CountTPs (count of Time Periods) and LastTPisLT8 (length of last time period is less than 8). CountTPs has value 1 in row 2 and otherwise value “=IF([next-period cve_id]<>[current cve_id], 1, [previous-row’s CountTPs]+1)”; and LastTPisLT8 has value “=IF([next-row CountTPs]=1,IF([current-row CountTPs]<8,1,0),0)”. Then filtering on the latter for non-zero values, we find these cve_ids for CVEs having rather short available remediation timelines (with parentheses hyphen inserted before the last four digits because recall that we converted the original cve_id by concatenating only the last two digits of the year with the last four digits of the CVE id number): 
10(-)0742 (6 TPs), 10(-)1633 (6 TPs), 16(-)6307 (3 TPs), 16(-)6305 (4 TPs), 16(-)6309 (3 TPs), 16(-)7054 (2 TPs), 19(-)1543 (4 TPs). 
And then it’s easy to simply delete the associated (28) rows from the dataset.

The resulting dataset has 5414 rows and 26 columns; covering 114 CVEs (121-7).

The result of deleting rows associated with CVEs having few time periods should look like this:

```diff
+ openssl_social_smells_timeline..renameVariables..resolveMD..deleteLastRecordEachCVE..deleteShortCVEs.csv
```
