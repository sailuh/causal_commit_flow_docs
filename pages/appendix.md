---
layout: default
title: Appendix
parent: Home
nav_order: 13
---

## A.1. A brief reminder of how to determine parents, children, etc.

To examine the parents, say, of two nodes (e.g, mis_link and silence), select both nodes and move them to the Selected panel in the Graph box (select Display Subgraphs when you open the graph box) and then in the graph type box, select Parents and then click on Graph It! See below for screenshot:

![Section A.1. Figure 1](../img/sec_12_appendix_fig_1.png?raw=true "Title")

Here is the Tetrad session file that was used to derive causal subgraphs corresponding to assertions made in the original study:

```diff
+ parents_children_of_the_19_7.1.2-2.tet
```

## A.2. Content Analysis Artifact Reference

Throughout the paper qualitative analysis section, we encode references that support our interpretentation of community activity in OpenSSL mailing lists. The URLs for the provided codes are supplied here. As noted in the paper, these artifacts include e-mails from OpenSSL’s mailing list that begin with the letter M, blog posts (B), and commits (C). For example, to provide evidence of an event announced in the mailing list, we may refer to the e-mail as [M1]. The citations are provided below.


```bibtex
% Artifacts References

@misc{M1,
  howpublished = {\url{https://marc.info/?l=openssl-dev&m=100429666608071&w=2}},
  note = {Accessed: 2020-07-14}
}

@misc{M2,
  howpublished = {\url{https://marc.info/?l=openssl-dev&m=102181324915607&w=2}},
  note = {Accessed: 2020-07-14}
}

@misc{M3,
  howpublished = {\url{https://marc.info/?l=openssl-dev&m=147638421307039&w=2}},
  note = {Accessed: 2020-07-14}
}

@misc{M4,
  howpublished = {\url{https://marc.info/?l=openssl-dev&m=147638473207215&w=2}},
  note = {Accessed: 2020-07-14}
}

@misc{M5,
  howpublished = {\url{https://marc.info/?l=openssl-dev&m=97741948606308&w=2}},
  note = {Accessed: 2020-07-14}
}

@misc{M6,
  howpublished = {\url{https://marc.info/?l=openssl-dev&m=96457413327658&w=2}},
  note = {Accessed: 2020-07-14}
}

@misc{M7,
  howpublished = {\url{https://marc.info/?l=openssl-dev&m=96498092726092&w=2}},
  note = {Accessed: 2020-07-14}
}

@misc{M8,
  howpublished = {\url{https://marc.info/?q=about#Robots}},
  note = {Accessed: 2020-07-14}
}

@misc{M9,
  howpublished = {\url{https://www.openssl.org/community/mailinglists.html}},
  note = {Accessed: 2020-07-14}
}


@misc{M10,
  title = {Web Archive OpenSSL Mailing Lists},
  howpublished = {\url{https://web.archive.org/web/20060220165750/http://www.openssl.org/support/}},
  note = {Accessed: 2020-07-14}
}

@misc{M11,
  howpublished = {\url{https://mta.openssl.org/pipermail/openssl-project/2018-November/001171.html}},
  note = {Accessed: 2020-07-14}
}



@misc{B1,
  howpublished = {\url{https://www.openssl.org/blog/blog/2018/05/16/security-policy/}},
  note = {Accessed: 2020-07-14}
}

@misc{B2,
  howpublished = {\url{https://www.openssl.org/blog/blog/2017/02/13/bylaws/}},
  note = {Accessed: 2020-07-14}
}

@misc{B3,
  howpublished = {\url{https://www.openssl.org/blog/blog/2020/05/12/security-prenotifications/}},
  note = {Accessed: 2020-07-14}
}

@misc{B4,
  howpublished = {\url{https://www.openssl.org/blog/blog/2014/12/23/the-new-release-strategy/}},
  note = {Accessed: 2020-07-14}
}

@misc{B5,
  howpublished = {\url{https://www.openssl.org/policies/roadmap.html}},
  note = {Accessed: 2020-07-14}
}

@misc{B6,
  howpublished = {\url{https://www.openssl.org/blog/blog/2014/12/19/hello/}},
  note = {Accessed: 2020-07-14}
}

@misc{B7,
  howpublished = {\url{https://www.openssl.org/blog/blog/2016/10/12/f2f-rt-github/}},
  note = {Accessed: 2020-07-14}
}

@misc{B8,
  howpublished = {\url{https://www.openssl.org/blog/blog/2014/12/28/website-redesign/}},
  note = {Accessed: 2020-07-14}
}

@misc{B9,
  howpublished = {\url{https://www.openssl.org/blog/blog/2017/06/13/committers/}},
  note = {Accessed: 2020-07-14}
}

@misc{B10,
  howpublished = {\url{https://www.openssl.org/blog/blog/2015/08/01/cla/}},
  note = {Accessed: 2020-07-14}
}

@misc{B11,
  howpublished = {\url{https://www.openssl.org/blog/blog/2018/03/01/last-license/}},
  note = {Accessed: 2020-07-14}
}

@misc{B12,
  howpublished = {\url{https://www.openssl.org/blog/blog/2017/06/17/code-removal/}},
  note = {Accessed: 2020-07-14}
}

@misc{B13,
  howpublished = {\url{https://www.openssl.org/blog/blog/2015/01/05/source-code-reformat/}},
  note = {Accessed: 2020-07-14}
}

@misc{B14,
  howpublished = {\url{https://www.openssl.org/blog/blog/2015/07/28/code-cleanup/}},
  note = {Accessed: 2020-07-14}
}

@misc{B15,
  howpublished = {\url{https://www.openssl.org/blog/blog/2018/11/28/version/}},
  note = {Accessed: 2020-07-14}
}

@misc{B16,
  howpublished = {\url{https://www.openssl.org/blog/blog/2018/12/20/20years/}},
  note = {Accessed: 2020-07-14}
}

@misc{B17,
  howpublished = {\url{https://undeadly.org/cgi?action=article;sid=20140423045847}},
  note = {Accessed: 2020-07-14}
}

@misc{B18,
  howpublished = {\url{https://www.openbsd.org/papers/bsdcan14-libressl/mgp00001.html}},
  note = {Accessed: 2020-07-14}
}

@misc{B19,
  howpublished = {\url{https://opensslrampage.org/page/49}},
  note = {Accessed: 2020-07-14}
}

@misc{B20,
  howpublished = {\url{https://www.openbsd.org/papers/bsdcan14-libressl/mgp00026.html}},
  note = {Accessed: 2020-07-14}
}

@misc{B21,
  howpublished = {\url{https://www.openbsd.org/papers/bsdcan14-libressl/mgp00020.html}},
  note = {Accessed: 2020-07-14}
}

@misc{B22,
  howpublished = {\url{https://www.theguardian.com/technology/2014/apr/11/heartbleed-developer-error-regrets-oversight}},
  note = {Accessed: 2020-07-14}
}

@misc{B23,
  howpublished = {\url{https://www.openssl.org/news/vulnerabilities.html}},
  note = {Accessed: 2020-07-14}
}



@misc{C1,
  howpublished = {\url{https://GitHub.com/openssl/openssl/commit/bd6941cfaa31ee8a3f8661cb98227a5cbcc0f9f3}},
  note = {Accessed: 2020-07-14}
}

@misc{C2,
  howpublished = {\url{https://GitHub.com/openssl/openssl/commit/731f431497f463f3a2a97236fe0187b11c44aead}},
  note = {Accessed: 2020-07-14}
}
```
