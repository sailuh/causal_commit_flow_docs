---
layout: default
title: Removing Short CVE Timelines
parent: Home
nav_order: 5
---

## 5. Remove CVEs Whose Timeline is Too Short

If a CVE’s timeline is too short then key underlying causal relationships cannot be discovered or any that are discovered will not be as credible (more likely to be false positives). On the other hand, we want to be careful not to delete too many CVEs or what we learn from the remaining subset of CVEs might not adequately represent the OpenSSL Project in general. Also, recollect that we deleted about 17% of the records, but these were for CVEs whose timelines started before 2007. If their timeline continued well past 2007, then we’ll have available enough time periods to analyze and for more recent time periods; but if not, then we do not have many time periods (and none too recent) to draw useful (and timely) conclusions about the underlying causal relationships of interest; and thus the latter are also hardly bad candidates for deletion.

Within our dataset, seven CVEs are only represented by seven time periods or less (six, after deleting the last row of each CVE timeline as a last step of appending variables from the next time period). Discovering a structured causal model from such a small sample that faithfully represents the underlying direct causal relationships (relative to the variables collected) might be difficult. Thus, these CVEs with only seven time periods are a good candidate for deletion. We therefore prune away any CVEs (i.e., their associated records from the dataset) having insufficient representation in our dataset for discovering casual relationships specifically for that CVE. 

Beyond these seven CVEs, there are 114 CVEs with longer timelines represented in our dataset. The shortest of these timelines includes 18 time periods (17 after the appending variables from next time period step), which may be more than sufficient to discovering any significant causal relationships (significant enough to have an observable change in outcomes of interest). Thus 7 or fewer time periods in a CVE timeline seems like a reasonable threshold for guiding us in which CVEs to prune from the dataset that will still leave us with a large majority of the dataset for analysis but with each retained CVE well represented in terms of the number of time periods available for analysis (at least 18).

Therefore, we deleted the 7 CVEs (their associated rows) with 7 or fewer time periods. Their deletion leaves us with a total of 35 fewer rows.

The resulting dataset has 5414 rows and 26 columns; covering 114 CVEs (121-7). The resulting file is:

```diff
+ openssl_social_smells_timeline..renameVariables..resolveMD..deleteLastRecordEachCVE..deleteShortCVEs.csv
```
