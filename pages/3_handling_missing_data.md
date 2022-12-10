---
layout: default
title: Handling Missing Data
parent: Home
nav_order: 3
---


## 3. Handling missing data (MD) 

When we examine the CSV file identified above, we find a spreadsheet of 6697 rows (not counting the header row) and 16 columns; however, many of the cells in the spreadsheet are blank, that is, they have no value assigned to them—this is referred to as “missing data” (MD). Further examination reveals there is a pattern to the missingness. We can specify that pattern through a partially-manual examination of the spreadsheet using Microsoft® Excel®, which takes time, but it can be done, or we can use a tool, such as Mplus® [1], to automate that analysis more systematically and completely. Because the Mplus tool is a commercial tool that not everyone will have relatively easy access to, we briefly describe both approaches to characterizing the MD in the dataset.

**Characterizing the MD in a dataset using Excel.**  There are multiple approaches to analyzing the pattern of MD for a dataset opened with Microsoft Excel; we briefly describe one way here. Note that when a file has been opened using Excel, the bottom bar (called the “Status Bar”) displays summary information about the content of that column. (What information is displayed in that Status Bar can be set in various ways within the tool including simply bringing up the Context menu by right-clicking (on a Windows PC) on the Status Bar and selecting the information you desire be displayed.) By selecting a column in Excel, we can see a count of how many cells have some kind of content (not MD). For example, selecting the first column “cve_id”, we see from the Status Bar that there are 6698 cells with content (including the header), that is, there is no MD in the first column. Similarly, when we examine the second column “commit_interval” we see from the Status Bar that there are 4498 non-blank cells. Subtracting this from 6698 (6697 plus the header row) works out to 2200 cells that are blank. And we can likewise do this examination for the other 14 columns. There is another useful feature of Microsoft Excel and that is the column filter, which can be accessed from the ribbon on top, selecting “Data” and then “Filter.” In what follows, we assume the reader is either familiar with these Excel features, and Filter in particular, or is willing to come up to speed on these features by googling for a description of how this can be accomplished. For this dataset, we can pursue a “divide and conquer” approach to characterizing MD: systematically, one column at a time (until we’re done), filter on the column twice: once on the non-MD, and then on the MD, each time characterizing which columns (of the filtered spreadsheet) have cells with MD. In this way, we can discern if there is a systematic pattern to the MD. For example, if we select the second column “commit_interval” we can set up a filter on just the non-MD (by scrolling to the bottom of the filter pull-down menu and unclicking “(Blanks)”), we find that the first column with any MD is the fifth column “org_silo”, which has 4498-3591=907 blank cells. Proceeding carefully in this way, a systematic pattern of missingness then reveals itself, which matches the pattern uncovered in a more automated fashion using Mplus. (If handling MD manually with Excel, you don’t need to read the next subsection on using Mplus; instead, skip down to the subsection that begins with the words “After determining the MD patterns in the dataset, resolve the MD appropriately”.)

**Characterizing the MD in a dataset using Mplus.** The Mplus tool’s forte is general linear modeling; however, it also provides additional functionality such as characterizing where in the dataset there is MD. An analysis of the dataset using Mplus returned a summary that looked something like the following table. (Only the variable names have been changed: Mplus only recognizes the first eight characters of a variable name, and so, the variable names when specified in Mplus were shortened; however, to avoid confusing the reader the variable names appearing in the resulting table were then restored back to match those used in the table above.)

```
SUMMARY OF DATA

     Number of missing data patterns             4


SUMMARY OF MISSING DATA PATTERNS


     MISSING DATA PATTERNS (x = not missing)

           		1  	2 	3 	4
 cve_id   		x  	x  	x  	x
 activity_0		x  	x  	x  	x
 activity_2		x  	x  	x  	x
 Start     		x  	x  	x  	x
 End       		x  	x  	x  	x
 org_silo		x
 mis_link		x
 silence   		x
 congruence		x
 communicate		x
 code_dev  		x  	x
 file      		x  	x
 mail_dev 		x     		x
 thread    		x     		x
 commit    		x  	x
 churn     		x  	x

     MISSING DATA PATTERN FREQUENCIES

    Pattern   Frequency     Pattern   Frequency     Pattern   Frequency
    1        3590           	3        1973
    2         907           	4         227
```


Note that summing the number of cases across the four Missing Data (MD) patterns, we obtain 6697 cases, which matches the number of rows in the dataset. 

**After determining the MD patterns in the dataset, resolve the MD appropriately.** We begin the exposition with a brief account of what took place after determining the patterns of MD in the dataset and then follow this with the actions required to resolve the MD.

From discussions with someone knowledgeable about the opensource project as well as the developer of the website scraping tool, two reasons emerged as to why data should or could be missing from the dataset:

1. The mailing log in use until about 2000-2001 was missing due to failure to preserve the original mailing log during the transition to a new mailing list manager for the project in 2001. 

2. For some time periods, there are no commits. We can easily identify such cases because variable “commit” is blank (and also activity_0 is 1 precisely when commit is blank), which prevents the scraping tool from building the file graph used for computing several measures, resulting in those variables also being blank. 

A careful analysis of the dataset (using Mplus) concluded that the above two reasons explained all MD in the dataset. That is, there was no MD that we couldn’t ascribe to one of the above two reasons. 

The Mplus table helps clarify the nature of the missing data. (In this Replicability Package, we chose not to include the Mplus input script we developed for Mplus use, in order to not have to explain a second set of aliases for the variable names (there are constraints that need to be adhered to regarding what variable names are legal in an Mplus script), and because we don’t consider it critical to replication, only clarifying the general nature of the missing data returned by the scraping tool.)

With respect to MD Reason 1 (specified above), about 17% of the rows (MD Patterns 2 and 4 in the Mplus table above) suffered missing values due to the loss of the mailing log (907 + 227 cases). Developers were presumably communicating during the time period but there is no data on such communication.


 * Specifically, this loss of the mailing log prevents computing variables “mail_dev” and “thread” directly; and variables “org_silo” through “communicate” indirectly (if mail activity is lost, we can’t compute one of the graphs used in computing these variables). In the rows affected by the loss of the mailing log, the scraping tool should not impute a value for all these missing values; and it does not—they are indeed blank. 

 * The authors felt that the conditions leading to the loss of the mailing log should have no noticeable causal effect on any of the other variables in the dataset (other than time-related variables such as “Start” and “End”). In other words, the authors deemed that data missing due to MD Reason 1 can be considered Missing Completely at Random (MCAR) [7]. Therefore, Listwise Deletion [7], that is, deleting all rows that had missing data that was MCAR, other than increasing the estimate of standard error, induces no bias. While deleting 17% of the rows is a pretty significant deletion, the authors felt that the retained 83% of the rows was still sufficient to identifying major direct causal relationships. Also, from an explainability and understandability perspective, deleting 17% of the rows seemed preferable to employing pseudo-random number generation to impute values. Finally, choosing deletion rather than imputation also helps ensure the results can be more easily replicated. (The authors did pursue Full Information Maximum Likelihood-based imputation [7] initially, but after such considerations, chose not to use the results but to instead pursue Causal Discovery on the dataset resulting from Listwise Deletion.)

With respect to MD Reason 2, when there is no commit activity within a time period, any measures of features (counts) related to commits should all be 0. While the scraping tool left blanks for their values, the value 0 should be imputed for such missing values.


 * Again, the authors considered each variable one by one to verify that this was sensible. Specifically, having absolutely no commits in a time period prevents building the graphs needed for computing variables “org_silo” through “communicate,” and when there are no commits, the scraping tool also leaves “commit” and “churn” blank. All of these commit-related variables should be zero.
 * The activity_0 variable is equal to 1 specifically in these 2200 (1973 + 227) cases.
 * Note that even though there might have been no commit activity in a time period, there might still be mailing list activity and thus measures associated with any activity in the mailing list (mail_dev and thread) could still be computed, and thus non-blank, that is, after we’ve deleted all rows associated with a lost mailing log (MD Reason 1).

Summarizing, and referring to the table generated by Mplus, we can say this about the dataset:

 * Number of cases with no MD at all (corresponds to MD Pattern 1 in the Mplus table): 3590
 * Number of cases with MD due to MD Reason 1 only (corresponds to MD Pattern 2 in the Mplus table): 907
 * Number of cases with MD due to MD Reason 2 only (corresponds to MD Pattern 3 in the Mplus table): 1973
 * Number of cases with MD due to MD Reasons 1-2 (corresponds to MD Pattern 4 in the Mplus table): 227

Thus, with the above changes to the dataset, of deleting about 17% (907 + 227) of the rows affected by the loss of an old mailing log, and imputing zeros for all remaining missing values, we thus have a dataset of 5563 cases) with no MD and can thus continue onward to the next steps in preparing the dataset for applying causal discovery.

**Detail for performing Listwise Deletion:** In Excel, place a Data > Filter on column “mail_dev” and in the filter select only “(Blanks)” from the dropdown menu. The number of such rows (after the header row) should be 1134 rows. Note that this seems right because that’s the total number of cases covered by Mplus MD Patterns 2 and 4. Then, selecting all those rows (but not the header row), delete them. The result should 5563 rows.

Detail for imputing zeros: within Excel, we performed a global selection of blank cells within the entire dataset (following Listwise Deletion, and thus the only blank cells left now are due to there being no commits for that time quarter), replacing the (blank) content of all such cells by 0. In further detail: we simply invoked the Replace command configured with these options: “Find what:” empty string, “Replace with:” 0, click on Options >> to check “Match entire cell contents”, etc. 17757 replacements were thus made. Note that this is 9*1973, the latter number is the number of cases covered by Mplus MD Pattern 3, which is the MD pattern corresponding to blank cells arising only due to there being zero commits for the time period. The 9 blank cells for each of these rows correspond to the variables indicated in the Mplus table above showing the missingness for MD Pattern 3.

Here’s the file we obtained by resolving the MD as described above:

```diff
+ openssl_social_smells_timeline..renameVariables..resolveMD.csv
```
