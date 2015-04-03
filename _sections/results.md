---
title: "Results"
order: 6
---


## Analysis of the Olympics Dataset

We now move to the analysis of the London 2012 Twitter dataset and its relation with the known schedule of the Games. We focus on one representative day, July 29th, during which several sport events took place at different times and concurrently. We use both topic detection methods, show the signals they extract, and check to what extent they are capable of extracting signals that we can understand in terms of the schedule.

The topic detection methods are set up as follows. For the masked NTF method, we decompose the tensor using a fixed number of components, using a tolerance value of $$10^{-4}$$ for the stopping condition, and limiting the number of iterations to $$50$$.
For the agglomerative NMF method, we decompose each interval matrix using a fixed number of components. We use a tolerance value of $$10^{-4}$$ for the stopping condition, and limit the number of iterations to $$20$$.
We use 250 topics for the Masked NTF, and 50 components per time intervals in the Agglomerative NMF.

Figure [2](#bigstream) shows a streamgraph representation of time-varying topics extracted using the two methods we have discussed.
Two global activity peaks are visible in both streamgraphs: the peak at about 2.30pm UTC
was triggered when Elizabeth Armistead won the silver medal in road cycle;
the peat at about 7pm UTC is driven by the bronze medal in 400m freestyle to Rebecca Adlington.
In the stream graphs, for clarity, each topic is annotated using only its topmost weighted term.
This makes it difficult to assess a visual correspondence between the same topics across the two representations,
as the term with top weight may be different for the two term vectors even though the vectors are overall very similar
(in terms of cosine similarity).
On closer inspection, many precise correspondences can be established between the topics extracted
by the Masked NTF method and those extracted by the Agglomerative NMF method:
for example, the topic _armistead_ in the top streamgraph matches the topic _congratulation_ in the bottom one.
An interactive streamgraph visualization of the London 2012 Twitter dataset
is available at
[http://www.datainterfaces.org/projects/emoto/](http://www.datainterfaces.org/projects/emoto/).


## Comparison with the Olympics Schedule

### Event Selection

In order to show the possible correspondence between the extracted topics and sport events,
we manually annotate the schedule collected from the official London 2012 Olympics page for July 29th, 2012.
As the number of events in a day can be substantial and we want to focus on events with higher impact on social media, we retain events that are either finals or team sports match.
We annotate each event with a set of at most three terms extracted from the schedule, as described in Section \ref{sec:data}.
For a team sport, we use the sport name and the countries of the two teams, otherwise, we put the name of the sport and its characteristics, e.g., the discipline for swimming.

### Matching Topics and Events

For each event, we use a matching criteria to select one of the extracted topics from each of the set of topics produced by the methods.
Since we want to select a topic in which all event annotated terms appear with a high weight in its term vectors,
we define our matching score based on the geometric average of the weights of the event annotated terms in the topic's term vectors:

$$\langle  w \rangle = \sqrt[n]{h_{w_1 r} \, h_{w_2 r} \, \dots \, h_{w_n r}}$$

For Masked NTF, for each event, we choose the topic with the highest corresponding geometric average $$\langle  w \rangle$$.
In the agglomerative NMF case, for each event, we choose the topic with the highest corresponding geometric
average $$\langle  w \rangle$$ weighted by $$\log(n)$$ where $$n$$ is the number of components in the selected cluster.
We use $$\log(n)$$ in order to favor the selection of clusters with a higher number of aggregated components,
otherwise the most detailed clusters which aggregates only one component are always selected.
Since the Agglomerative NMF method produces a tree structure in which each node agglomerates a set of components and represents topic activity,
we have to calculate such matching result for each node, and select the node for which such matching result is the highest.

### Results and Observations

At this point, we have, for each event, a topic which was selected in each method, and the corresponding matching result.
In Figure [3](#fig:distinf), we show the schedule events for the top $$20$$ highest matching results.
In the lefthand figure, we show, for each one of the top $$20$$ matching results, the topic extracted by the Masked NTF method,
while in the righthand figure, we show the topic extracted by the Agglomerative NMF method.
The results are sorted by the corresponding matching weight.

For each event, on its top left corner, we show the manually annotated terms used for the matching.
The shaded blue area shows the exact interval during which the event was occurring according to the official Olympics schedule.
In the same area, the solid green line represents the temporal structure of the topic with higher matching result according to our matching criteria.
Such values roughly represent the amount of activity for such topic and are normalized according to the peak of activity.
We show the value for this peak in the top right side, along with the matching results between parenthesis.
In the Agglomerative NMF graph (on the right) we show as a dotted line the activity in time for the given terms regarding the number of tweets that have such terms (tweet count).
We remark that by considering only the dotted line the timing of many events on the right side of the figure does not match the schedule timings, i.e., merely counting tweets is not sufficient at this resolution level.
We also measured the number of tweets where the terms are co-ocurring, and in this case the number of tweets is so small that it does not allow  the detection of any structure in time.

We evaluated these activity profiles using the CrowdFlower Web-based crowdsourcing platform (restricted to Amazon Mechanical Turk workers).
Each work unit asks a worker to visually inspect and compare two timelines: the one to be evaluated, and a reference timeline corresponding to the known time intervals for sport events taken from the Olympic schedule.
Each work unit looks like a row from Figure [3](#fig:distinf).
Our evaluation was based on 100 work units evenly distributed among 5 types:
1) (NMF) work units based on the results of Agglomerative NMF; 2) (CNT-NMF) work units with activity profiles generated by simply counting the number of tweets with the terms used in matching the NMF topics; 3) (NTF) work units from the Masked NTF approach; 4) (CNT-NMF) same as (CNT-NMF) for Masked NTF; 5) synthetic work units ("gold" units) used to assess worker quality.
For each work unit, we asked the workers whether the two timelines matched exactly (Yes), matched partially (Partially) or not at all (No).
95% of the judgments for gold work units were correct. We only retained those users who correctly judged more than 80% of the gold units.
Figure [4](#crowdsourced) shows the distribution of judgements for the different types of work units.
The left hand side of the figure shows the distribution obtained for all work units,
while the right hand side shows the distribution restricted to work units with more than 80\% of agreement across different workers.
According to this evaluation, both NTF and NMF outperform the count-based methods.

We see that for most of the events there is a close temporal alignment between the event schedule and the topic structure, at the scale of the hour or less.
We see that such temporal alignment is much closer than when compared to the peaks of activity generated by counting tweets.

We observe that the mismatches in the temporal alignment are caused by two different factors.
The first one is due to a low matching results, like the event annotated with (football, mexico, gabon).
It means that the term vectors for the given topic does not represent with high confidence the terms used to annotate the event.
The second one is due to a different behaviour in collective attention.
This happens for example in the case of swimming events, where the first part of the event is related to eliminatories and
the second part is related to the finals.
In such cases, the peak in activity arrives when the event finishes and the attention goes to the winner.
