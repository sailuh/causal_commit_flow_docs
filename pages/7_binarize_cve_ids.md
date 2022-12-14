---
layout: default
title: Binarize CVE IDs
parent: Home
nav_order: 7
---

## 7. Causal Discovery within CVE Timeline Context

If we want to understand the impact of sociotechnical behaviors from one time period on the next within the context of a CVE timeline, we will want to treat each CVE timeline as its own recurring situation that we want to apply causal discovery to. Different CVEs involve different problems to solve, which may require different solution approaches and different individuals interacting differently to do the remediation. But within a CVE remediation timeline, we’re likely to have a more shared context for sociotechnical behaviors from one time period to impact the work activities from that and the next time periods.

There are several approaches that we can take; but here are the main three: 

1. Split the dataset by CVE and apply causal discovery to each of the 114 resulting smaller CVE timeline-focused datasets to obtain a structural causal model for each; and then employ some kind of aggregation scheme to identify common causal structures.
2. Utilize one of the multi-dataset causal algorithms (e.g., IMaGES) that employs aggregation during causal discovery thereby creating a single structural causal model that represents the causal relationships that are shared across the CVE timelines.
3. For each distinct CVE, append an indicator feature, and then after doing that for all CVEs, apply causal discovery to the resulting single dataset (with 114 new columns) and make note of which features F have a CVE-related indicator feature pointing into them and for which CVEs: these features F have a local causal structure (i.e., “parents,” features with a causal edge coming out of them into V) that differs specifically for those CVEs; otherwise, the feature F has a local causal structure with its parents that is shared across many (perhaps even all) CVEs; and thus can be considered possibly typical of the OpenSSL project as a whole during the years spanned by the dataset.

The challenge with the first approach is to determine which aggregation scheme to use. The challenge with the second approach is that at the time of this research, the multi-dataset algorithm used seemed to not work reliably in the particular way we were using it. Thus, circumstances pushed us towards the third approach.

### 7.1 Binarize CVE ID (Hot Encoding) 

To implement this approach, we replace the cve_id column with one indicator feature for each distinct cve_id value. We call this “binarizing cve_id” because we are taking a categorical feature (cve_id) with multiple distinct values (114 distinct values) and replacing it with 114 indicator features, one per distinct CVE id value. 

In addition to the generated 114 indicator features, we drop one of the 114 indicator features because its values are inferable from the values of the other 113 indicator features. In other words, the sum of all 114 indicator features for any row in the dataset must be exactly 1, and thus this imposes a constraint on the 114 indicator features. As it takes 114 features to recognize the determinism, it is not an issue for our use of causal discovery; while determinisms creating “aliasing” relationships among the features where one combination of features can validly substitute for another, we’ll shortly drop some of the 114 features, removing the determinism. 

Note that any causal relationship between two features discovered by pursuing this approach (or one of the other two approaches) might be replaced by either a chain of causal edges (if we were to add features in the dataset that included mediators) or a fork—an inverted “F” with a third feature playing the role of apex with two causal edges coming into our two features (if we were to add potential confounders to our dataset). Thus, any causal edges discovered might actually reflect a more complicated causal structure between source and target features. Incidentally, similar can be said about any type of regression analysis (regarding inclusion of features), but here we make this overriding assumption/caution explicit.

To facilitate binarization, we wrote a Python program that takes each distinct value (cve_id_number) in the “cve_id” column of a dataset (dataset.csv) and appends a new column “b_cve_id_number” to the dataset. The “b_cve_id_number” column (the prefix “b_” is short for “[Binary] indicator”) is simply an indicator feature that identifies which rows of the dataset area associated with the remediation of that particular CVE (and were assigned the same CVE id number in the cve_id column). Then the “cve_id” column is deleted. The program is run using Python (version 3.9) with command-line arguments as follows (shortening the name of the CSV file to just “dataset.csv” for clarity):

```diff
+ python binarize-or-split-first-col.v020article.py dataset.csv B
```

Calling arguments used above: (1) The first argument after the name of the program is the name of the CSV file to be analyzed (here shortened to “dataset.csv”). (2) The second argument must indicate either "B" for binarize; or "S" for split. By specifying a "B" here, this program will output a CSV file bearing the same name but prefixed by "bin-". The output file includes the original features found in the input file, but after them the indicators b_J appear, in the same order as the distinct values J in column A. These new features are binary features (thus the "b_" prefix) with b_J equaling 1 if and only if the associated case has value J in column A. Otherwise it has value 0.

Here is the program listing: 

```diff
+ binarize-or-split-first-col.v020article.py
```

The resulting dataset, whose name is as before but extended with the prefix “bin” has 5414 rows and 133 columns; covering 114 CVEs and 19 other features, which include Start and two sets of 9 sociotechnical features plus outcomes (one set of 9 features each for the current time period and next time period).

The resulting file should look like this:

```diff
+ bin-openssl_social_smells_timeline..renameVariables..resolveMD..deleteLastRecordEachCVE..deleteShortCVEs..delDmsmHighCorr.csv
```

