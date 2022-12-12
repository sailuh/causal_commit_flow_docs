---
layout: default
title: Conclusion
parent: Home
nav_order: 11
---

## 11. Conclusion

Even though we weren’t able to exactly replicate the original study, we did derive very similar conclusions: social smells impact the current and next time period(s). Almost any measure of work-rate considered can be controlled (we considered 6 different types of work rate) via one or more social smells in the current or previous time period. Social smells do impact work rates. Even when controlling for their effects in the next time period, social smells in this time period to affect work rates in the next time period.
We modified the methodology from what was written in the original study. Rather than:

 * tracking each edge with a null variable on one or both ends that emerges during bootstrapped search
 * rank-ordering the resulting edges by frequency of appearance in bootstrapped search results
 * deriving a trimming threshold that is equal to the 1st percentile frequency for probability of no edge from these null variable-based edges
 * applying that trimming threshold to those edges that formed among the original variables
we utilized an option already present in Tetrad (Majority Ensemble method) to trim based on a more easily stated criterion: for each pair of nodes, when an edge appears between them in a majority of bootstrapped search results, assign an edge to that pair of nodes and orient it the same way as the most frequent orientation experienced among the bootstrapped search results.

This modification to the methodology is an alternative explanation for some of the differences in the details Original Study vs. Replication, in addition to the improvements to the search algorithms themselves. Regardless, there is still high consistency across the two studies and two years of learning and changes to the algorithms, and in any case, the original study’s conclusions seem well supported.
