---
title: "Data and Representation"
order: 4
---

###Notation
The following notations are used throughout the paper.
Scalars are denoted by lowercase letters, e.g., $$x$$, and vectors are denoted by boldface lowercase letters, e.g., $$\textbf x$$,
where the $$i$$-th entry is $$x_i$$.
Matrices are denoted by boldface capital letters, e.g., $$\textbf X$$, where the $$i$$-th column of matrix $$\textbf X$$ is $$\textbf x_i$$, and the $$(i, j)$$-th entry is $$x_{ij}$$.
Third order tensors are denoted by calligraphic letters, e.g., $$\mathcal{A}$$.
The $$i$$-th slice of $$\mathcal{A}$$, denoted by $$\textbf{A}_i$$,
is formed by setting the last mode of the third order tensor to $$i$$.
The $$(i, j)$$-th vector of $$\mathcal{A}$$, denoted by $$\textbf{a}_{ij}$$,
is formed by setting the second to last and last modes of $$\mathcal{A}$$ to $$i$$ and $$j$$ respectively,
and the $$(i,j,k)$$-th entry of $$\mathcal{A}$$ is $$a_{ijk}$$.

###Twitter Dataset
The Emoto dataset consists of around 14 million tweets collected during the London 2012 Summer Olympics using the public Twitter Streaming API.
All tweets have at least one of 400 keywords,
including common words used in the Olympic Games -- like athlete, olympic,
sports names and twitter accounts of high followed athletes and media companies.
Tweets were collected during all the interval of 17 days comprising the Olympic Games, from July 27 to August 12 2012.

###Event Schedule
In order to investigate the relation between the extracted time-varying topics and the sport events of the Olympic Games, we use the schedule available on the official London 2012 Olympics page\footnote{http://www.london2012.com/schedule-and-results/}, where the starting time and duration of most events is reported together with metadata about the type of event (discipline, involved teams or countries, etc.)


###Data Preprocessing

For the text analysis performed in this paper, URLs are removed from the original tweet content.
The remaining text is used to build a vocabulary composed of the most common 30,000 terms,
where each term can be a single word, a digram or a trigram.
352 common words of the English language are also removed from the vocabulary.

In order to localize Twitter users, we examine the user profile descriptions
and use an adapted version of [GeoDict](https://github.com/petewarden/geodict) to identify, if possible, the user country.
To study the relation between the extracted topical activity and the schedule of the Olympic events,
we focus on tweets posted by users located in the UK, only. This allows us to avoid potential confusion
arising from tweets posted in countries, such as the USA, where Olympics events were broadcasted with delays
of several hours due to time zone differences. This selection leaves us with a still substantial amount of data
(about one third of the full dataset) and simplifies the subsequent temporal analysis, even though it probably oversamples
the attention payed to events that involved UK athletes.

For the scope of this study, we represent the data as a sparse third-order tensor $$\mathcal T \in \mathbb{R}^{I \times J \times K}$$, with $$I$$ tweets, $$J$$ terms and $$K$$ time intervals.
We aggregate the tweets over 1-hour intervals, for a total of $$K=408$$ intervals.
The tensor $$\mathcal T$$ is sparse: the average number of terms (also referred as features in the following) for each tweet is typically no more than 10, compared to the 30k terms of our term vocabulary.
Moreover, as each tweet is emitted at a given time, each interval $$k$$ has a limited number of active tweets, $$I_k$$.
A tensor slice $$\textbf{T}_k \in \mathbb{R}^{I \times J}$$ is a sparse matrix with non-zero values only for $$I_k$$ rows.
$$\textbf{T}_k$$ represent the sparse tweet-term matrix observed at time $$k$$. The term values $${t}_{ijk}$$ for each tweet $$i$$ are normalized using the standard Term Frequency and Inverse-Document Frequency (TF-IDF) weighting,
$${t}_{ijk} = \mathrm{tf}(i, j) \times \mathrm{idf}(j)$$,
where $$\mathrm{tf}(i, j)$$ is the frequency of term $$j$$ in tweet $$i$$, and
$$
\mathrm{idf}(j) = \log{\frac{\left|D\right|}{1+\left|\{d : j \in d\}\right|}}
$$
%
where $$|D|$$ is the total number of tweets and $$\left|\{d : j \in d\}\right|$$ is the number of tweets where the term $$j$$ appears.

###Visualizing Topics over Time
The methods that we present in this paper are able to extract topical-temporal structures from $$\mathcal T$$.
Such topical-temporal structures can be represented a stream matrix $$\textbf{S} \in \mathbb{R}^{R \times K}$$ with $$R$$ topics and $$K$$ intervals.
Each component $$R$$ is also characterized by a term-vector $$\textbf{h} \in \mathbb{R}^{J}$$ that defines the most representative terms for that component.
In order to visualize such topical-temporal structures represented as a stream matrix,
we use the method described by Byron and Wattenberg~\cite{byron2008stacked},
which yields a layered stream-graph visualization.
