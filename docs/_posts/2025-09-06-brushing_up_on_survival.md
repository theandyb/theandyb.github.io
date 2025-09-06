---
layout: post
title: Brushing up on Survival Analysis
tags: [notes, biostatistics, survival]
comments: true
mathjax: true
author: Andy Beck
---

Biostatistics as a field encompasses such a broad spectrum of topics that it's no surprise some areas may be neglected in one's training. In my particular case, my focus has been on statistical genetics and genomics, and I've had little exposure in the way of topics such as *clinical trias* and *survival analysis*, which, depending on who you ask, are key areas of expertise for someone wanting to call themselves a biostatistician. Well, I'd like to think of myself as a "biostatistician", so why not brush-up on my understanding of the subjects? (*that, and I'm interested in learning more about how these inform analyses we might do with genetic data*). 

Here, I intend to organize my notes as I re-learn Survival Analysis  (or rather, "learn" Survival Analysis, especially once I move beyond Kaplan-Meier curves and basic Cox Proporional Hazards models). I will keep a list of the references I read at [the end of the post](#references), but I will be primarily following the book "[Survival Analysis: A Self-Learning Text, 3rd Edition](https://link.springer.com/book/10.1007/978-1-4419-6646-9)" (which seems appropriate, given the self-learning nature of this effort). These notes may or may not be helpful to anyone other than me, and might (or rather, will) contain gaps (and errors!).

## What is Survival Analysis?

* In short, survival analysis is concerend with **outcome variables** that describe *time-to-events*
    * Generally, one kind of event ("failure"), although several competing events show up in, well, _**competing events models**_
    * *The beloved coalescent model of Population Genetics is also a process concerned with times to an event (albeit on a different time scale than the examples listed in chapter 1 of Kleinbaum & Klein)*
* A feature of most survival data is **censoring**, in which case we do not know the exact failure time of a subject
    * **Right censored**: We have a lower-bound as to when they could have failed, usually the end of the study
        * In other words: the subject did not fail until after the end of the study, so all we know is that they survived up to the end of the study
    * **Left censored**: We have an upper-bound as to our estimate of when the failure occurred
        * In other words: we know a subject failed at some time at or before an observation time $$t$$
    * **Interval censored**: We don't know when a subject failed, but we have both an upper- and lower- bound
        * The example given by Kleinbaum & Klein is for a hypothetical HIV study, where the event of interest is HIV infection, but what we actually observe is the time that a subject tests positive; so if a patient tests negative at time $$t(i)$$, but then tests positive at time $$t(i+1)$$, then we know the time of infection is in the interval $$(t(i),t(i+1)]$$

### Survivor Curves and Hazard Functions

The two primary quatitative terms used in describing survival analysis problems are the **survival function** and the **hazard function/rate**.

* The survivor function: $$S(t) = \Pr(T > t)$$, where $$T$$ is a random variable representing the time-to-failure
    * $$S(0) = 1$$ and $$S(\infty) = 0$$
    * Fitted survival curves on real data are usually **step functions**, with steps at observations times where failures (and censoring?) are observed
* The hazard function: $$h(t) = \lim_{\Delta t \to 0} \frac{\Pr(t \leq T < t + \Delta t \| T > t)}{\Delta t}$$
    * The hazard function is a **rate** and not a **probability**
* The **goal of survival analysis** is often to compare survival or hazards rates between subsets of subjects
    * Example: time to relapse for two different cancer treatments

Given a survival function $$S(t)$$, we can compute the corresponding hazard function $$h(t)$$, and vice versa:

$$S(t) = \exp [-\int_{0}^{t} h(u)du])$$

$$h(t) = - [\frac{dS(t)/dt}{S(t)}]$$


### Censoring Assumptions

*My notes here are sparse and need to be filled in more*

There are three censoring assumptions (and their counterparts) that are considered (assumed?) by survival models. In the following we assume that we are interested in comparing survival or hazard functions between two or more well-defined subgroups.

1. Censoring is **independent** (if not: **non-independent**)
    * Within a subgroup, subjects who are censored at time $$t$$ should be representative of all subjects in the subgroup who remained at risk at time $$t$$
2. Censoring is **random** (or **non-random**)
    * Stronger assumption than independent
    * Subjects who are censored at time $$t$$ should be representative of all subjects who remained at risk at time $$t$$
3. Censoring is **non-informative**
    * Encapsulates the above two
    * The censoring event itself shouldn't provide any information about future survival time
        * Counter-example would be if sicker patients were more likely to drop out of a study.

<a id="references"></a>
## References

### Books

* __[Survival Analysis: A Self-Learning Text, 3rd Edition](https://link.springer.com/book/10.1007/978-1-4419-6646-9)__ by David G. Kleinbaum and Mitchel Klein
* __The Statistical Analysis of Failure Time Data__ by John D. Kalbfleisch and Ross L. Prentice