---
title: "Introduction"
order: 2
---

Streams of user-generated content from social media and microblogging systems exhibit patterns of collective attention
across diverse topics, with temporal structures determined both by exogenous factors, such as driving from mass media,
and endogenous factors such as viral propagation. Because of the openness of social media, of the complexity of their
interactions with other social and information systems, and of the aggregation that typically leads to the observable stream
of posts, several concurrent signals are usually simultaneously present in the post stream, corresponding to the activity of
different user communities in the context of several different topics. Making sense of this information stream is an inverse
problem that requires moving beyond simple frequency counts, towards the capability of teasing apart latent signals that
involve complex correlations between users, topics and time intervals.

The motivation for the present study is twofold.
On the one hand, we want to devise techniques that can reliably solve the inverse problem of extracting latent signals of
attention to specific topics based on a stream of posts from a micro-blogging system. That is, we aim at extracting the time-varying topical structure of a microblog stream such as Twitter.
On the other hand, we want to deploy these techniques in a context where temporal and semantic metadata about external
events driving Twitter are available, so that the relation between exogenous driving and time-varying topical responses can
be elucidated.
We do not regard this as a validation of the methods we use, because the relation between the external drivers and the
response of a social system is known to be complex, with memory effects, topical selectivity, and different degrees of
endogenous social amplification. Rather, we regard the comparison between the time-resolved topical structure of a
microblog stream and an independently available event schedule as an important step for understanding to what extent
Twitter can be used as a social sensor to extract high-resolution information on concurrent events happening in the real
world.

Here we focus on content collected by the [Emoto project](http://www.emoto2012.org) from Twitter during the London 2012 Olympics, for which a daily schedule of the starting
time and duration of sport events and social events is available and can be used for reference.
In this context, resolving topical activity over time requires to go beyond the analysis and characterization of popularity
spikes. A given topic driven by external events usually displays an extended temporal structure at the hourly scale, with
multiple activity spikes or alternating periods of high and low activity.  We aim at extracting signals that consists of an
association of (i) a weighted set of terms defining the topic, (ii) a set of tweets that are associated to the topic, together with
the corresponding users, and (iii) an activity profile for the topic over time, which may comprise disjoint time intervals of
nonzero activity.
We detect time-varying topics by using two independent methods, both based on non-negative matrix or tensor factorization.
In the first case we build the full tweet-term-time tensor and use non-negative tensor factorization to extract the topics and
their activity over time. We introduce an adapted factorization technique that can naturally deal with the special tensor
structure arising from microblog streams.
In the second case, which in principle affords on-line computation, we build tweet-term frequency matrices over
consecutive time intervals of fixed duration. We apply non-negative matrix factorization to extract topics for each time
interval and we track similar topics over time by means of agglomerative hierarchical clustering.

We then apply both methods to the Twitter dataset collected during the Olympics, which reflects the attention users pay to
tens of different concurrent events over the course of every day. We focus on topical dynamics at the hourly scale, and find
that for those sport events in the schedule that can be semantically matched to the topics we obtain from Twitter, the activity
timeline of the detected topic in Twitter closely matches the event timeline from the schedule.

This paper is structured as follows: Section [1](#sec:background) reviews the literature on collective attention, popularity,
and topic detection in microblog streams. Section [2](sec:data) describes the Olympics 2012 Twitter dataset used for the
study, the event schedule we use as an external reference, and introduces some notations and conventions used
throughout the paper. Section [3](sec:TF) and Section [4](sec:MF) describe the two techniques we use to mine time-varying
topical activity in the Twitter stream.
Section [5](sec:results) discusses the relation between the time-varying
topics we obtain and the known schedule of the Olympics events for one representative day, and provides some general
observations on the behavior of the two methods. Finally, Section [6](sec:conclusions) summarizes our findings
and points to directions for further research.
