---
title: "Background"
order: 3
---

The dynamics of collective attention and popularity in social media has been the object of extensive investigation in the literature. Attention can suddenly concentrate on a Web page [[1](#wu07), [2](#ratkiewicz10b)], a YouTube video [[3](#crane08),[4](#figueiredo11),[5](#naaman11)], a story in the news media [[6](#leskovec09)], or a topic in Twitter [[6](#kwak10),[7](#asur11),[8](#yang11)].
Intrinsic features of the popular item under consideration have been related to its popularity profile by means of semantic analysis and natural language processing of user-generated content [[9](#adar07),[10](#huang10),[11](#wu11)].
In particular, a great deal of research [[12](#crane08),[13](#kwak10),[14](#laniado10),[15](#yang11),[16](#Lehmann:2012)] has focused on characterizing the shape of peaks in popularity time series and in relating their properties to the popular item under consideration, to the relevant semantics, or to the process driving popularity.

Within the broad context of social media, Twitter has emerged as a paradigmatic system for the vision of a "social sensor" that can be used to measure diverse societal processes and responses at scale [[17](#jansen09),[18](#Sakaki:2010),[19](#Bollen2011),[20](#mocanu2012twitter)].
To date, comparatively little work has been devoted to extracting signals that expose complex correlations between topics and temporal behaviors in micro-blogging systems. Given the many factors driving Twitter, and their highly concurrent nature, exposing such a topical-temporal structure may provide important insights in using Twitter as a sensor when the social signals of interest cannot be pinpointed by simply using known terms or hashtags to select the relevant content, or when the topical structure itself, and its temporal evolution, needs to be learned from the data.
Saha and Sindhwani [[21](#Saha:2012)] adopt such as viewpoint and propose an algorithm based on non-negative matrix factorization that captures and tracks topics over time, but is evaluated at the daily temporal scale only, against events that mainly consist of single popularity peaks, without concurrency.
Here we aim at capturing multiple concurrent topics and their temporal evolution at the scale of hours, in order to be able to compare the extracted signals with a known schedule for several concurrent events taking place during one day.

As we will discuss in detail, microblog activity can be represented using a tweet-term-time three-way tensor, and tensor factorization techniques can be used to uncover latent structures that represent time-varying topics.
Ref. [[22](#Candecom)] proposed  in $1970$ the Canonical Decomposition (CANDECOM), also called parallel factorization \\
(PARAFAC, [[23](#harshman1970fpp)]), which can be regarded as a generalization to tensors of singular value decomposition (SVD).
Maintaining the interpretability of the factors usually requires to achieve factorization under non-negativity constraints, leading to techniques such as non-negative matrix or tensor factorization (NMF and NTF).
Tensor factorization to detect latent structures has  been extensively used in several domains such as signal processing, psychometrics,  brain science, linguistics and chemometrics [[24](#Shashua:2005:NTF:1102351.1102451),[25](#Cichocki:1253894),[26](#VandeCruys:2009:NTF),[27](#Beyondstreams),[28](#Wang:2011)].
