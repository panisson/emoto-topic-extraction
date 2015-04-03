---
title: "Methods"
order: 5
---

## Masked Non-negative Tensor Factorization

###Problem Statement


As explained in section \ref{sec:data}, the tensor $$\mathcal T \in \mathbb{R}^{I \times J \times K}$$ with $$I$$ tweets, $$J$$ terms
and $$K$$ intervals is a natural way to represent the tweets and their contents with respect to the time.
 The tensor has the advantage to  directly encompass
 the relationship between tweets posted at different hours and consequently between topics of the different hours. The tensor factorization as described below allows to uncover topics together with their temporal pattern.


 Before describing the process of factorization itself and its output, one needs to introduce the concept of canonical decomposition (CP). CP in $$3$$ dimensions aims at
 writing a tensor $$\mathcal T \in \mathbb{R}^{I \times J \times K}$$
 in a factorized way that is the sum of the outer product of three vectors:

$$\mathcal{T} = \sum_{r=1}^{R_\mathcal{T}} \,  \mathbf{a}_r \circ \mathbf{b_r} \circ \mathbf{c_r}$$

where the smallest  value of $$R_\mathcal{T}$$ for which this relation exists, is the rank of the tensor $$\mathcal{T}$$. In other words, the tensor $$\mathcal{T}$$ is expressed with a sum of rank-$$1$$ tensors.
The set of vectors
$$a_{\{1,2,\dots,R\}}$$ (resp. $$b_{\{1,2,\dots,R_\mathcal{T}\}}$$,$$c_{\{1,2,\dots,R_\mathcal{T}}\}$$) can be re-written as a matrix $$\mathbf{A} \in \mathbb{R}^{I \times R_\mathcal{T}}$$
(resp $$\mathbf{B} \in \mathbb{R}^{J \times R_\mathcal{T}} $$,$$\mathbf{C} \in \mathbb{R}^{K \times R_\mathcal{T}}$$) where each of the $$R_\mathcal{T}$$ vectors
is a column of the matrix. The decomposition of Eq.~\ref{approx} can also be represented in terms of the three matrices $$\mathbf{A},\mathbf{B},\mathbf{C}$$ as  $$\left[ \mathbf{A},\mathbf{B},\mathbf{C} \right]$$.
A visual representation of such a factorization, also called Kruskal decomposition,
is displayed on Fig [1](#figure-1).


### Factorization Methodology

Regarding the extraction of topics, the aim is not to decompose the tensor in its exact form but to approximate the tensor by a sum of rank-$$1$$ tensors with a number of terms smaller than the rank of the original tensor.
This number $$R$$ corresponds to the number of topics that we want to extract (see Fig. [1](#drawing_factorization).
Such an approximation of the tensor leads to minimize the difference between $$\mathcal{T}$$ and $$\left[ \mathbf{A},\mathbf{B},\mathbf{C} \right]$$:

$$\min\limits_{\mathbf{A},\mathbf{B},\mathbf{C}} {\Vert \mathcal{T} -\left[ \mathbf{A},\mathbf{B},\mathbf{C} \right] \Vert}^2_F$$

where $$\Vert  \Vert $$ is the Frobenius norm.
We transform the $$3$$-dimensional problem (Eq.~\ref{3d-problem}) in $$2$$-dimensional sub-problems by unfolding the tensor $$\mathcal{T}$$ in three different ways. This process called matricization
gives rise to three modes $$\mathbf{X}_{(1)},\mathbf{X}_{(2)},\mathbf{X}_{(3)}$$. The mode-$$n$$ matricization consists of linearizing all the indices of the tensor except $$n$$. The three
resulting matrices have respectively a size of $$I \times JK$$,$$J \times IK$$ and $$K\times IJ$$. Each element of the matrix $$\mathbf{X}_{(i=1,2,3)}$$ corresponds to one element of the tensor $$\mathcal{T}$$ such that each of the mode contains all the values of the tensor.
Due to matricization, the factorization problem given by Eq.\ref{approx} can be reframed in factorization of the three modes. In other terms, maximizing the likelihood between
$$\mathcal{T}$$ and $$\left[ \mathbf{A},\mathbf{B},\mathbf{C} \right]$$ is equivalent to minimizing
the difference between each of the mode and their respective approximation in terms of $$\mathbf{A},\mathbf{B},\mathbf{C}$$.
The factorization problem (PARAFAC) in Eq.\ref{3d-problem} is converted to the three following sub-problems where we added a condition of non-negativity of the three modes:

$$\min\limits_{\mathbf{A}\geq 0} {\Vert \mathbf{X}_{(1)} -\mathbf{A}(\mathbf{C}\odot \mathbf{B})^T \Vert}^2_F$$

$$\min\limits_{\mathbf{B} \geq 0} {\Vert \mathbf{X}_{(2)} -\mathbf{B}(\mathbf{C}\odot \mathbf{A})^T \Vert}^2_F$$

$$\min\limits_{\mathbf{C} \geq 0} {\Vert \mathbf{X}_{(3)} -\mathbf{C}(\mathbf{B}\odot \mathbf{A})^T \Vert}^2_F$$

where $$\odot$$ is the Khatri-Rao product which is a columnwise Kronecker product, i.e. such that
$$\mathbf{C}  \odot \mathbf{B}  =[c_1 \otimes b_1 c_2 \otimes b_2  \dots c_r \otimes b_r]$$. If $$\mathbf{C} \in \mathbb{R}^{K \times R}$$
and $$\mathbf{B} \in \mathbb{R}^{J \times R}$$, then the Khatri-Rao product $$\mathbf{C} \odot \mathbf{B} \in \mathbb{R}^{KJ \times R}$$.
In our case of study, $$\mathbf{A},\mathbf{B},\mathbf{C}$$  will give each access to a different information: $$\mathbf{A}$$ allows to know at which topic belongs a tweet, $$\mathbf{B}$$ gives the definition of the topics
with respect to the features and $$\mathbf{C}$$ gives the temporal activity of each topic.

Several algorithms have been developped to tackle the PARAFAC decomposition. The two most common are one method based on the projected gradient and the Alternating Least square method (ALS). The first one is convenient for its ease of implementation
and is largely used in Singular Value Decomposition (SVD) but converges slowly.
In the ALS method, the modes are deduced successively by solving Eq \ref{2d-problem}. In each iteration, for each of the sub-problem, two modes are kept fixed while the third one is computed. This process is repeated until convergence.
In our case, we use a nonnegativity constraint to make the factorization  better posed and the results meaningful.
 One thus uses nonnegative ALS (ANLS [[29](#Paatero)]) combined with a block-coordinate-descent method in order to reach the convergence faster.
Each of the step of the algorithm needs to take into account the Karush-Kuhn-Tucker (KKT) conditions to have  a stationary point. Our program is based on the algorithm implemented by [[30](#Park-NTF)].



### Masked Adaptation of the NTF

We cannot directly perform the NTF on the tensor [Tweets $$\times$$ Features $$\times$$ Interval] built as explained aboved as this tensor has a "block-disjoint" structure peculiar to the tweets.
Indeed each tweet has non-zero values only at one interval because a tweet is emitted only at a given time. Each interval $$k$$ has only $$I_k$$ active tweets. In each slice $$\textbf{T}_k$$ of the tensor, only $$I_k$$ rows have meaningful values.
 So, we are only interested in reproducing the tensor part which contains the meaningful values. In order to focus on these meaningful values, one needs to consider an adapted version of the tensor $$\mathcal{T}$$.
We first consider  the tensor $$\mathcal{T}$$ built as explained above. We generate a first set of matrices $$\mathbf{A},\mathbf{B},\mathbf{C}$$ which could approximate the tensor. At the next step, one tries to decompose a
tensor $$\bar{\mathcal{T}}$$ where the values are a combination of the values of $$\bar{\mathcal{T}}$$  and of the values of $$\left[ \mathbf{A},\mathbf{B},\mathbf{C} \right]$$.  More exactly, this tensor has the same size than $$\mathcal{T}$$
and the same values than $$\mathcal{T}$$ for the rows $$I_k$$ of each slice $$\mathbf{\bar{T}}_k$$. The complementary values are given by $$\left[ \mathbf{A},\mathbf{B},\mathbf{C} \right]$$.
In other terms, at each step, the tensor that we approximate is updated by:

$$\bar{\mathcal{T}}= {\mathcal{T}} \boxdot {\mathcal{W}} + (1-{\mathcal{W}})\left[ \mathbf{A},\mathbf{B},\mathbf{C} \right]$$

where $$\boxdot$$ is the Hadamard product (element-wise product) and $${\mathcal{W}}$$ is a binary tensor of the same size than $${\mathcal{T}}$$ with $$1$$-values only when the values of $${\mathcal{T}}$$ at this position are meaningful.
The particular structure of the tensor (disjoint blocks in time) could be perceived as a "missing values" problem in the tensor, this problem has been for example tackled in [[31](#royer:hal-00725289)].

Concretely, the implementation is an adaptation of a Matlab program [[30](#Park-NTF)]  which uses the Tensor Toolbox [[32](#Kolda:2009:TDA:1655228.1655230)]. This adaptation includes the introduction of a
mask (via the tensor of weight) as mentionned above and the rewriting of some operations to avoid memory issues. This point is not detailed here as it is not part of the main point of the paper.



### Stream Matrix Construction

We calculate the strength of each topic with respect to the time by using both the information about the link between each topic and each tweet  and about temporal pattern of the topics. These informations are available through
$$\mathbf{A}$$ and $$\mathbf{C}$$  and the consequent strength of a topic $$r$$ on each interval of time $$k$$ is given by:

$$s_{rk}=\sum_{i|k} a_{ir} * c_{kr}$$

where $$\sum_{i|k}$$ is a sum over the tweets indexed by $$i$$ occurring at the interval indexed by $$k$$. The set of elements $$s_{\{r,k\}}$$ with $$r=\left[1,R\right] $$ and $$k=\left[ 1,K \right]$$ forms the
stream matrix $$\mathbf{S}$$.
Each topic is then defined by a terms vector and each of this term vector is given by a column of $$\mathbf{B}$$.




## Agglomerative Non-negative Matrix Factorization

### Non-negative Matrix Factorization


For each tensor slice $$\textbf{T}_k$$, we compute a non-negative factorization by minimizing the following error function,

$$\min\limits_{W,H} {\Vert \textbf{T}_k - \textbf{W}^{(k)} \textbf{H}^{(k)} \Vert}^2_F \, ,$$

where $$\Vert  \Vert $$ is the Frobenius norm, subject to the constraint that the values in $$\textbf{W}^{(k)}$$ and $$\textbf{H}^{(k)}$$ must be non-negative.
The non-negative factorization is achieved using the projected gradient method with sparseness constraints, as described in [[33](#lin2007projected),[34](#hoyer2004non).
The factorization produces
a matrix of left vectors $$\textbf{W}^{(k)} \in \mathbb{R}^{I_k \times F}$$
and a matrix of right vectors $$\textbf{H}^{(k)} \in \mathbb{R}^{F \times J}$$, where $$F$$ is the number of components used in the decomposition.
The matrix $$\textbf{H}^{(k)}$$ stores the term vectors of the extracted components at interval $$t$$.
The matrix $$\textbf{W}^{(k)}$$ is used to calculate the
strength of each extracted component, which are represented in a matrix $$\textbf{Z} \in \mathbb{R}^{F \times K}$$ given by

$$z_{fk} = \sum\limits_{i=1}^{I_k} { \frac{ w^{(k)}_{if} } {    \sum\limits_{f'=1}^F  {w^{(k)}_{if'} }     } }$$

where $$ z_{fk}$$ is the strength of factor $$f$$ at interval $$k$$.


### Component Clustering

In order to track topics over time, we need to merge components into topics depending on how similar they are.
Since each component is defined by a term vector,
we can calculate a similarity matrix of all possible pairs of term vectors using cosine similarity.
This matrix is fed to a standard agglomerative hierarchical clustering algorithm,
known as UPGMA [[35](#Sokal1958)],
that at each step
combines the two most similar clusters into a higher-level cluster.
Cluster similarity is defined in terms of average linkage:
that is, the distance between two clusters $$c_1$$ and $$c_2$$ is defined as the average
of all pair-wise distances between the children of $$c_1$$ and those of $$c_2$$.

The hierarchical clustering produces a tree that can be cut at a given depth to yield a clustering at a chosen level of detail.
That is, by varying the threshold similarity we use for the cut we can go from a coarse-grained topical structure, with few clusters that may merge unrelated topics, to a fine-grained topical structure, with many clusters that may separate term vectors that otherwise could be regarded as the same continuous topic over time.
The cut threshold needs to be chosen based on criteria that depends on the application at hand.

Each choice for the cut yields a number of clusters $$C$$ and a
map function $$\mathcal{C}(r,f) \rightarrow k$$ that associates the component index $$f$$ at time interval $$k$$
to a topic cluster $$r$$.
This function collects all components associated to cluster $$r$$ in a set $$\mathbb{C}_{rk}$$ for each interval $$k$$.


### Stream Matrix Construction

When constructing the stream matrix, the number of topics $$R$$ in the stream matrix is given by the number of clusters generated by the clustering step.
In order to calculate the entries $$s_{rk}$$ of the resulting stream matrix $$\textbf{S}$$, we aggregate the strengths of the clustered components.
We build a stream matrix $$\textbf{S} \in \mathbb{R}^{R \times K}$$, with $$R$$ topics and $$K$$ intervals, given by


$${s_{rk}} = {\sum\limits_{f \in \mathbb{C}_{rk}} { z_{fk} }}$$


Finally, we extract the term vectors that are associated to each cluster.
Each cluster will be associated to a term vector $$\textbf{h}^{(k)}_{r} \in \mathbb{R}^{J}$$ that is the average of all term vectors $$\textbf{h}^{(k)}_{f}$$ associated to that cluster in the component clustering step.
