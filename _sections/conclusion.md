---
title: "Summary and Future Work"
order: 7
---

The topic detection techniques we discussed here afford tracking the attention that a community of users devotes to multiple concurrent topics over time, teasing apart social signals that cannot be disentangled by simply measuring frequencies of term or hashtags occurrences. This allows to capture the emergence of topics and to track their popularity with a high temporal resolution and a controllable semantic granularity.
The comparison with an independently available schedule of real-world events shows that the response of Twitter to external driving retains a great deal of temporal and topical information about the event schedule, pointing to more sophisticated uses of Twitter as a social sensor.

The work described here can be extended along several directions.
It would be interesting to develop and characterize on-line versions of the techniques we used here,
so that topic emergence and trend detection could be carried out on live microblog streams.
Because of its temporal segmentation, the Agglomerative NMF case lends itself rather well
to on-line incremental computation, whereas a dynamic version of the Masked NTF
technique would be more challenging to achieve.

Another interesting direction for future research would be to augment the tweet-term-time tensor with a fourth dimension representing the location of the users, so that the latent signals we extract could expose correlation between topics, time intervals and locations, exposing geographical patterns of collective attention and their relation to delays, e.g., in the seeding by mass media across different countries.
