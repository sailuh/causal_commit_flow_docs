---
layout: default
title: Determinism and High Intercorrelation Variables
parent: Home
nav_order: 6
---

## 6. Addressing Determinism and High Intercorrelation Among Features

When the same or similar aspect is measured more than one way within a time period, it can create features that are highly correlated to each other. Prior to applying causal discovery, we want to combine or remove such features from the dataset. When a pair of features are measuring the same aspect of a situation, the causal discovery algorithm we use will tend to “reward” only one of the two features with a causal relationship with a third feature. In other words, the set of causal relationships between those two highly-correlated features and any other feature in the dataset will be split between the two features, making interpretation of the results of causal discovery more challenging. 

### 6.1 Removed High Intercorrelation Features

Consider for example the features “activity_0,” “activity_2,” and “commit”. All three gauge the amount of commit activity that occurs within a time period. Which of these three features should we keep, or should we combine them into a new composite feature that better represents the amount of commit activity within a time period? In this case, the solution is simple: “activity_0” and “activity_2” are fully determined by the number of commits (“commit”) and thus we can simply delete the less informative versions of “commit” from the dataset. In other words, we simply delete “activity_0” and “activity_2.”

Next, we consider the Start and End features. The ‘End’ feature follows the ’Start’ feature by exactly 90 days, which is a deterministic relationship, and so ’End’ was removed (in an earlier step).

Less obvious are inter-correlation among other features. We found the following very high inter-correlations among the features already defined, and so we removed the second member of each pair listed:
    1. mis_link and org silo (0.960)
    2. congruence and communicate (0.995)
    3. mis_link2 and org silo2 (0.962)
    4. congruence2 and communicate2 (0.995)
    

### 6.2 Special Cases 

For a close-but-take-no-action example, consider the two variables “mail-dev” and “thread.” Both represent the amount of mail activity during a time period, however they do not measure the same aspect. The correlation between “mail-dev” and “thread” is about 0.8, which although apparently high is still below the threshold of 0.9 [or -0.9] that we have chosen for determining when two features are so highly correlated that one of them should be removed from the dataset to reduce the risk of splitting discussed above.

The above 6 feature deletions (activity_0, activity_2, org_silo, org_silo2, communicate, communicate2) leave us with 5442 rows but now 20 columns (cve_id, Start, the 9 variables for the time period and 9 for the next following the deletions of org_silo, communicate, etc.).

The resulting file is now:

```diff
+ [openssl_social_smells_timeline..renameVariables..resolveMD..deleteLastRecordEachCVE..deleteShortCVEs..delDmsmHighCorr.csv
```
