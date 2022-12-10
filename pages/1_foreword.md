---
layout: default
title: Foreword
parent: Home
nav_order: 1
---


## 1. Foreword

The intent of this document is to provide instructions for the replication of the study described in “A Socio-technical Perspective on Software Vulnerabilities: A Causal Analysis” hereafter called the “Original Study.”

Given the non-determinism of search algorithms provided in our primary tool for Causal Discovery, Tetrad [see references in original study], we would not expect a replication to provide detailed results identical to those obtained for the original study; however, in the two years since the original study, the software stack underlying the original study results has itself changed, and thus even greater divergence might be expected. In particular, the Tetrad tool has gone through several versions from a pre-release of 6.8.0 (August 2020) used in the study to 7.1.2-2 (November 2022), the latest publicly-available release as we write this replication package. That two-year old pre-release is no longer publicly available and thus we ended up having to replicate our own study with Tetrad version 7.1.2-2 during which the main search algorithm used, Fast and Greedy Equivalent Search (FGES), has evolved somewhat as has the user interface to Tetrad and some of the features of the tool we employed (affecting screenshots created two years ago).

We thus had an opportunity to follow our own replication package but with a newer version of the main tool used. What we learned was this: the details will change (e.g., which specific social smells in the current time period affect which specific work-rate variables in the next time period) but the overall conclusions documented at the end of our study still hold, in particular, quoting from the study’s conclusions:

> “results: …social smells are indeed important factors that mediate significant project outcomes in terms of the incidence of and the effort associated with security vulnerabilities.”
