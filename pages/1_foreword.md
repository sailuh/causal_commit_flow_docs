---
layout: default
title: Foreword
parent: Home
nav_order: 1
---


## 1.1 Foreword

The intent of this document is to provide instructions for the replication of the study described in “A Socio-technical Perspective on Software Vulnerabilities: A Causal Analysis” hereafter called the “Original Study.”

Given the non-determinism of search algorithms provided in our primary tool for Causal Discovery, Tetrad [see references in original study], we would not expect a replication to provide detailed results identical to those obtained for the original study; however, in the two years since the original study, the software stack underlying the original study results has itself changed, and thus even greater divergence might be expected. In particular, the Tetrad tool has gone through several versions from a pre-release of 6.8.0 (August 2020) used in the study to 7.1.2-2 (November 2022), the latest publicly-available release as we write this replication package. That two-year old pre-release is no longer publicly available and thus we ended up having to replicate our own study with Tetrad version 7.1.2-2 during which the main search algorithm used, Fast and Greedy Equivalent Search (FGES), has evolved somewhat as has the user interface to Tetrad and some of the features of the tool we employed (affecting screenshots created two years ago).

We thus had an opportunity to follow our own replication package but with a newer version of the main tool used. What we learned was this: the details will change (e.g., which specific social smells in the current time period affect which specific work-rate variables in the next time period) but the overall conclusions documented at the end of our study still hold, in particular, quoting from the study’s conclusions:

> “results: …social smells are indeed important factors that mediate significant project outcomes in terms of the incidence of and the effort associated with security vulnerabilities.”

## 1.2 Why do Causal Discovery and Causal Inference?

Causal inference has entered the research methodology discourse in fields as diverse as Econometrics and Epidemiology; and indeed, almost all leading and significant research in these two fields is now conducted in this more rigorous setting.

A more technical discussion of how Causal Discovery and Causal Inference work can be found in the references; however, they are entering more widespread use in:

 * Epidemiological studies, where how one establishes medical facts, guidance, and policy can be, literally, a matter of life or death
 * Economics science, where major decisions are made regarding American and International economics policy.

For just two recent examples, we cite a paper by Miguel Hernán that has been described as one of the most influential papers in 100 years of the American Journal of Epidemiology [2, 3]; and the awarding of the 2021 Nobel Prize for Economics to Angrist and Imbens [4]

However, for our purposes, this may prove a sufficient explanation:

> Regarding the use of Causal Discovery algorithms (FGES): when evaluating candidate edges, causal discovery conditions on (this is the verb “conditions” not the “noun”) other variables when searching for which new edges best improve the score for an intermediate-stage graph during search, evaluating whether a given pair of variables remain correlated even when taking conditioning on a subset of the other variables into account. If the partial (conditional) correlation even fails once, no edge will be credited to the variable pair. Thus, each edge in the resulting graph (a Directed Acyclic Graph or DAG [3]), has had to survive a “gauntlet” set of conditional independence tests (or the score equivalent), with a significant partial correlation found each time (or the edge would be removed). Note that each such conditioning constitutes an alternative explanation for how causality plays out. Though it takes more computation time, Causal Discovery is superior to ordinary (marginal) correlation between variable pairs in a research study as it eliminates many other alternative causal hypotheses for how the values attained by one variable affects/drives what happens with a second variable. 

Continuing on our answer to “why causal discovery/inference:”

> Likewise, causal discovery leads to superior covariate handling in linear/logistic regression [5, 6]. Key to estimating the direct casual effect of a treatment on a response by linear regression is determining which covariates to use. Presumably, the data analyst has experimented with different combinations of variables acting as covariates, but conditioning on the wrong set can create nonsense associations/edges between variable pairs.

And for these reasons, we employed Causal Discovery in our analysis of the OpenSSL dataset.

## 1.3 References

1. Muthén, L. K., & Muthén, B. O. (1998-2017). Mplus User's Guide. Eighth Edition. Los Angeles, CA: Muthén & Muthén.))
2. American Journal of Epidemiology, 2021. “100 Years of the American Journal of Epidemiology.” 100 years, American Journal of Epidemiology, Oxford Academic (oup.com)
3. Miguel A. Hernán, Sonia Hernández-Díaz, Martha M. Werler, Allen A. Mitchell, Causal Knowledge as a Prerequisite for Confounding Evaluation: An Application to Birth Defects Epidemiology, American Journal of Epidemiology, Volume 155, Issue 2, 15 January 2002, Pages 176–184, https://doi.org/10.1093/aje/155.2.176 (Selected as one of the most influential papers in the history of the journal.)
4. Nobel Prize Organization. “Press Release: The Prize in Economic Sciences.” Awarded to Joshua D. Angrist and Guido W. Imbens “for their methodological contributions to the analysis of causal relationships.” The Prize in Economic Sciences 2021 - Press release - NobelPrize.org
5. Wikimedia Foundation. “Simpson’s Paradox.” https://en.wikipedia.org/wiki/Simpson%27s_paradox 
6. Stanford Encyclopedia of Philosophy. “Simpson’s Paradox.” https://plato.stanford.edu/entries/paradox-simpson/ 
7. P. Allison, Missing Data. No. 136 in Missing Data, SAGE Publications, 2001.





