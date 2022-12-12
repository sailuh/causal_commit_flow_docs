---
layout: default
title: Binarize CVE IDs
parent: Home
nav_order: 7
---

(This is sometimes referred to as “hot encoding” in the data science literature.)

If we want to understand the impact of sociotechnical behaviors from one time period on the next within the context of a CVE remediation timeline, we will want to treat each CVE remediation as its own recurring situation that we want to apply causal discovery to. Different CVEs involve different problems to solve, which may require different solution approaches and different individuals interacting differently to do the remediation. But within a CVE remediation timeline, we’re likely to have a more shared context for sociotechnical behaviors from one time period to impact the work activities from that and the next time periods.
There are several approaches that we can take; but here are the main three: 

1. Split the dataset by CVE and apply causal discovery to each of the 114 resulting smaller CVE timeline-focused datasets to obtain a structural causal model for each; and then employ some kind of aggregation scheme to identify common causal structures.
2. Utilize one of the multi-dataset causal algorithms (e.g., IMaGES) that employs aggregation during causal discovery thereby creating a single structural causal model that represents the causal relationships that are shared across the CVE timelines.
3. For each distinct CVE, append an indicator variable, and then after doing that for all CVEs, apply causal discovery to the resulting single dataset (with 114 new columns) and make note of which variables V have a CVE-related indicator variable pointing into them and for which CVEs: these variables V have a local causal structure (i.e., “parents,” variables with a causal edge coming out of them into V) that differs specifically for those CVEs; otherwise, the variable V has a local causal structure with its parents that is shared across many (perhaps even all) CVEs; and thus can be considered possibly typical of the OpenSSL project as a whole during the years spanned by the dataset.

The challenge with the first approach is to determine which aggregation scheme to use. The challenge with the second approach is that at the time of this research, the multi-dataset algorithm used seemed to not work reliably in the particular way we were using it. Thus, circumstances pushed us towards the third approach.

Detail of how we implemented the third approach: the basic idea is to replace the cve_id column with one indicator variable for each distinct cve_id value. We call this “binarizing cve_id” because we are taking a categorical variable (cve_id) with multiple distinct values (114 distinct values) and replacing it with 114 indicator variables, one per distinct CVE id value. 

A couple technical comments before proceeding: 

1. Note that we really should drop one of the 114 indicator variables because its values are inferable from the values of the other 113 indicator variables. (In other words, the sum of all 114 indicator variables for any row in the dataset must be exactly 1, and thus this imposes a constraint on the 114 indicator variables.) But as it takes 114 variables to recognize the determinism, it’s really not an issue for our use of causal discovery; and more to the point, while determinisms creating “aliasing” relationships among the variables where one combination of variables can validly substitute for another, we’ll shortly drop some of these 114 variables anyway and the determinism will be gone. 
2. A reminder that any causal relationship between two variables discovered by pursuing this approach (or one of the other two approaches) might be replaced by either a chain of causal edges (if we were to add variables in the dataset that included mediators) or a fork—an inverted “V” with a third variable playing the role of apex with two causal edges coming into our two variables (if we were to add potential confounders to our dataset). Thus, any causal edges discovered might actually reflect a more complicated causal structure between source and target variables. Incidentally, similar can be said about any type of regression analysis (regarding inclusion of variables), but here we make this overriding assumption/caution explicit.

To facilitate binarization, we wrote a Python program that takes each distinct value (cve_id_number) in the “cve_id” column of a dataset (dataset.csv) and appends a new column “b_cve_id_number” to the dataset. The “b_cve_id_number” column (the prefix “b_” is short for “[Binary] indicator”) is simply an indicator variable that identifies which rows of the dataset area associated with the remediation of that particular CVE (and were assigned the same CVE id number in the cve_id column). Then the “cve_id” column is deleted. The program is run using Python (version 3.9) with command-line arguments as follows (shortening the name of the CSV file to just “dataset.csv” for clarity):

```diff
+ python binarize-or-split-first-col.v020article.py dataset.csv B
```

Calling arguments used above: (1) The first argument after the name of the program is the name of the CSV file to be analyzed (here shortened to “dataset.csv”). (2) The second argument must indicate either "B" for binarize; or "S" for split. By specifying a "B" here, this program will output a CSV file bearing the same name but prefixed by "bin-". The output file includes the original variables found in the input file, but after them the indicators b_J appear, in the same order as the distinct values J in column A. These new variables are binary variables (thus the "b_" prefix) with b_J equaling 1 if and only if the associated case has value J in column A. Otherwise it has value 0.

Here is the program listing: 

```diff
+ binarize-or-split-first-col.v020article.py
```

The resulting dataset, whose name is as before but extended with the prefix “bin” has 5414 rows and 133 columns; covering 114 CVEs and 19 other variables, which include Start and two sets of 9 sociotechnical variables plus outcomes (one set of 9 variables each for the current time period and next time period).

The resulting file should look like this:

```diff
+ bin-openssl_social_smells_timeline..renameVariables..resolveMD..deleteLastRecordEachCVE..deleteShortCVEs..delDmsmHighCorr.csv
```

