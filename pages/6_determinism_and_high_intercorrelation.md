---
layout: default
title: Determinism and High Intercorrelation Variables
parent: Home
nav_order: 6
---

## 6. Addressing Determinism and High Intercorrelation Among Variables

When the same or similar feature is measured more than one way within a time period, it can create variables that are highly correlated to each other. Prior to applying causal discovery, we will want to combine or remove such variables from the dataset. When a pair of variables are measuring pretty much the same feature of a situation, the causal discovery algorithm we use will tend to “reward” only one of the two variables with a causal relationship with a third variable. In other words, the set of causal relationships between those two highly-correlated variables and any other variable in the dataset will be split between the two variables, making interpretation of the results of causal discovery more challenging. Specific examples, some of which lead to a change in the dataset include:

 * Three variables “activity_0,” “activity_2,” and “commit” all gauge the amount of commit activity that occurs within a time period. Which of these three variables should we keep, or should we combine them into a new composite variable that better represents the amount of commit activity within a time period? In this case, the solution is very simple: “activity_0” and “activity_2” are fully determined by the number of commits (“commit”) and thus we can simply delete the less informative versions of “commit” from the dataset. In other words, we simply delete “activity_0” and “activity_2.”
 * The ‘End’ feature follows the ’Start’ feature by exactly 90 days, which is a deterministic relationship, and so ’End’ was removed (in an earlier step).
 * Also, we found the following very high inter-correlations among the features already defined, and so we removed the second member of each pair listed:
    * 1) mis_link and org silo (0.960)
    * 2) congruence and communicate (0.995)
    * 3) mis_link2 and org silo2 (0.962)
    * 4) congruence2 and communicate2 (0.995)
 * For a close-but-take-no-action example, consider the two variables “mail-dev” and “thread.” Both represent the amount of mail activity during a time period. Are they both measuring the same thing? Not quite. The correlation between “mail-dev” and “thread” is about 0.8, which sounds high but is still pretty far from the threshold of 0.9 [or -0.9] that we have been using in our work for determining when two variables are so highly correlated that one of them should be removed from the dataset to reduce the risk of splitting discussed above.

The above 6 column deletions (activity_0, activity_2, org_silo, org_silo2, communicate, communicate2) leave us with 5442 rows (still) but now 20 columns (cve_id, Start [capitalizing the initial], the 9 variables for the time period and 9 for the next following the deletions of org_silo, communicate, etc.).

The resulting file should look like this:

```diff
+ [openssl_social_smells_timeline..renameVariables..resolveMD..deleteLastRecordEachCVE..deleteShortCVEs..delDmsmHighCorr.csv
```
