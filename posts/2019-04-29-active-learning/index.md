---
permalink: /posts/active-learning/
title: "Introduction to Active Learning"
author: "Kliment Mamykin"
date: "2019-05-01"
tags: 
  - active learning
  - datasets
draft: true
---

Introduction
    Why talking about active learning is important
    The main reference for this article is a wonderful book by Burr Settles "Active Learning" (Settles 2012). It is an 

Definitions
    from Settles "Automating Inquiry"

Uncertainty Sampling 

Search Through the Hypothesis Space

Minimizing Expected Error and Variance

Exploiting Structure in Data

Unifying Theory




Related areas of research
    Hyper-parameter optimization
    Human Computation/Aggregating crowdsourced labels
    Week supervision
    Semi-supervised learning - Semi-supervised learning is a learning paradigm concerned with the study of how computers and natural systems such as humans learn in the presence of both labeled and unlabeled data. 
    Truth... 
    Auto-ML
    Data Programming 


https://www.datacouncil.ai/talks/active-learning-why-smart-labeling-is-the-future-of-data-annotation?hsLang=en


Adding to the challenge is that an appropriate label alphabet, the vocabulary of labels, 
is generally unknown at the start of such a process, given an unknown dataset and/or users with ill-defined information needs. 
In some situations, different label alphabets might be appropriate, depending on the task at hand or a user’s individual preferences. 
Analysts often derive the labels appropriate for a specific dataset and task from the data itself, exploiting the characteristics 
encoded in the multivariate data records and dimensions. In other situations, analysts rely on special domain knowledge to come up with 
initial labels. In any of these cases, neither AL tools nor the results of classifiers are particularly helpful for the determination of a 
label alphabet. Furthermore, the label alphabet is often subject to change during the labelling process itself.

"The key idea behind active learning is that a machine learning algorithm can perform better with less training if it is allowed to choose the data from which it learns. An active learner may pose “queries,” usually in the form of unlabeled data instances to be labeled by an “oracle” (e.g., a human annotator) that already understands the nature of the problem. This sort of approach is well-motivated in many modern machine learning and data mining applications, where unlabeled data may be abundant or easy to come by, but training labels are difficult, time-consuming, or expensive to obtain." (Settles 2012)
"Active learning is a subfield of artificial intelligence and machine learning: the study of computer systems that improve with experience" (Settles 2012:19)
"the hallmark of an active learning system is that it eagerly develops and tests new hypotheses as part of a continuing, interactive learning process." (Settles 2012:20)
"Active learning is most appropriate when the (unlabeled) data instances themselves are numerous, can be easily collected or synthesized, and you anticipate having to label many of them to train an accurate system." (Settles 2012:22)
"there are several different specific ways in which the learner may be able to ask queries.The main scenarios that have been considered in the literature are (1) query synthesis, (2) stream-based selective sampling, and (3) pool-based sampling." (Settles 2012:22)
"selective sampling (Atlas et al., 1990; Cohn et al., 1994). The key assumption is that obtaining an unlabeled instance is free (or inexpensive), so it can first be sampled from the actual distribution, and then the learner can decide whether or not to request its label." (Settles 2012:23)
"in most of the literature selective sampling refers to the stream-based scenario described here." (Settles 2012:25)
"The main difference between stream-based and pool-based active learning is that the former obtains one instance at a time, sequentially from some streaming data source (or by scanning through the data) and makes each query decision individually. Pool-based active learning, on the other hand, evaluates and ranks the entire collection of unlabeled data before selecting the best query." (Settles 2012:25)
"Uncertainty sampling is possibly the most popular active learning strategy in practice. Perhaps this is because of its intuitive appeal combined with the ease of implementation." (Settles 2012:34)
"as long as the learner can provide a confidence or probability score along with its predictions, any of the measures in Section 2.3 can be employed with the learner as a "black box."" (Settles 2012:34)
"There are also a variety of heuristics for measuring disagreement in classification tasks, but we will focus on two dominant trends. The first is essentially a committee-based generalization of uncertainty measures. For example, vote entropy" (Settles 2012:45)
"This formulation is a "hard" vote entropy measure; we can also define a "soft" vote entropy which accounts for each committee member's confidence" (Settles 2012:45)
"There is a key conceptual difference in how vote entropy and KL divergence quantify disagreement." (Settles 2012:46)
"In this chapter, we have looked at a variety of active learning approaches that operate on a simple premise: select queries that eliminate as many "bad" hypotheses as possible. This is in contrast to the vanilla uncertainty sampling strategies from Chapter 2, which use a single hypothesis to select queries that simply appear to be confusing." (Settles 2012:48)
"In other words, disagreement-based approaches are an attempt to maximize the mutual information between an instance's unknown label and the learning algorithm's unknown hypothesis. In contrast, entropy-based uncertainty sampling only attempts to maximize the entropy H (Y ) = I (Y ; Y ) , or "self information" of an instance's unknown label." (Settles 2012:49)
"let us shift attention away from what the learner thinks about instances now to what kinds of decisions it might make in the future . In particular, we want a learner to choose questions that, once it knows the answer, is most likely to reduce future error." (Settles 2012:53)
"Methods that aim to reduce classification error by minimizing 0/1-loss or log-loss (Section 4.1) cannot be solved in closed form generally, and require instead that models be re-trained to account for all possible labelings of all possible queries. In contrast, methods that aim to reduce output variance in the squared-loss sense (Section 4.2) have a long history in the statistics literature, can be computed in closed form, and apply equally well to classification and regression problems. They can also be generalized in a rather straightforward way to perform active learning in batches (sometimes with efficiency guarantees based on properties of submodularity)." (Settles 2012:62)
"Still, there are two major drawbacks to these approaches. The first is that they are not as general as uncertainty sampling or disagreement-based methods, and can only be easily applied to hypothesis classes of a certain form" (Settles 2012:62)
"In particular,the variance reduction heuristics are only natural for linear, non-linear, and logistic regression models (which is not surprising, since they were designed by statisticians with these models in mind). How to use them with decision trees, nearest-neighbor methods, and a variety of other standard machine learning and data mining techniques is less clear." (Settles 2012:62)
"The second drawback is their computational complexity due to the inversion and multiplication of the Fisher information matrix." (Settles 2012:62)
"In this chapter, we look at ways of overcoming these problems by explicitly considering the structure of the data while selecting queries." (Settles 2012:63)
"The basic intuition is that informative instances should not only be those with high information content, but also those which are representative of the underlying distribution in some sense (e.g., inhabit dense regions of the input space). The generic information density heuristic captures the main ideas of this approach" (Settles 2012:64)
"big advantage of density-weighting over error and variance reduction is this: if the density terms are pre-computed and cached for efficient lookup, the time required to select the next query is essentially no different than the base informativeness measure, e.g., uncertainty sampling" (Settles 2012:65)
"Exploiting the input distribution also brings to mind interesting unsupervised learning techniques, like clustering,and how they might be incorporated into active learning" (Settles 2012:65)
"Another idea is to pre-cluster the data and begin by querying cluster centroids to comprise the initial L , in effect "warm starting" active learning instead of beginning with a random sample" (Settles 2012:65)
"A key difference between this and most of the active learning algorithms in this book is that the learner queries clusters , and obtains labels for randomly drawn instances within a cluster." (Settles 2012:67)
"active learning and semi-supervised learning both traffic in making the most of unlabeled data, but take largely complementary approaches. It therefore seems natural to combine them so that the learner can exploit both what it does and does not know about the problem." (Settles 2012:70)
"let us consider a single, well-motivated utility measure that aims to maximize the amount of information we gain from that query. As it turns out, all of the general query frameworks we have looked at contain a popular utility function that can be viewed as an approximation to this measure under certain conditions." (Settles 2012:71)
"This measure represents the total gain in information (i.e., the change in entropy or uncertainty) over all the instances." (Settles 2012:71)
"Hence, the entropy -based uncertainty sampling strategy from Equation 2.3 can be seen as an approximation of our idealized utility measure, under two (grossly over-simplifying) assumptions" (Settles 2012:72)
"It would be nice to have a bound on the number of queries required train a good classifier, as well as a guarantee that this number is smaller than the number of labeled instances required in the passive supervised setting" (Settles 2012:73)
"There is no consistently clear-cut winner in terms of the query selection heuristics, however, which suggests that the best strategy may be dependent on the learning algorithm or application." (Settles 2012:79)
"If you do not have the time or resources to invest in sophisticated software engineering for some of these methods, and you are a cowboy with nothing to lose, then simpler methods based on uncertainty and disagreement are probably worth at least a pilot study" (Settles 2012:79)
"A high-level comparison of the active learning strategies in this book." (Settles 2012:80)
"One proposed approach for reducing effort in active learning is automatic pre-annotation , i.e., using the current model's predictions to assist in labeling queries (Baldridge and Osborne, 2004; Culotta and McCallum, 2005)." (Settles 2012:81)
"To help combat this in structured prediction tasks, Culotta et al.(2006) experimented with a technique called correction propagation ,where local edits are used to interactively update (and hopefully improve) the overall structure prediction" (Settles 2012:81)
"An extension of this idea is active learning by labeling features, sometimes called active dual supervision . In this setting, the oracle may label features which are deemed to be good predictors of one or more classes." (Settles 2012:85)
"In fact, Mann and McCallum (2008) found that specifying many imprecise constraints tends to result in better models than fewer more precise constraints, suggesting that human-specified feature labels (however noisy) are useful if there are enough of them. This begs the question of how to actively solicit these kinds of feature labels." (Settles 2012:85)
"Frequently in machine learning, as the state of the art advances we want to switch to a new and improved learner. This can be a problem if we wish to "reuse" this training data with models of a different type, or if we do not even know the appropriate model class (or feature set) for the task to begin with. Nothing is really known theoretically about the ability to reuse labeled data gathered by one learner for another, and the empirical body of work is limited." (Settles 2012:92)
"This highlights a very important issue for active learning in practice. If the best model class and feature set happen to be known in advance — or if these are not likely to change much in the future — then active learning can possibly be safely used. Otherwise, random sampling (at least for pilot studies, until the task can be better understood) may be more advisable than taking one's chances on active learning with an inappropriate model." (Settles 2012:93)
"The best way to think about this is the point at which the cost of acquiring new training data is greater than the cost of the errors made by the current system." (Settles 2012:93)
