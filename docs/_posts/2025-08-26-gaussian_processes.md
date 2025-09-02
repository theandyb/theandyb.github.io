---
layout: post
title: Gaussian Process Models
subtitle: Nonparametric regression with a parameterized distribution?
tags: [statistics, notes]
comments: true
mathjax: true
author: Andy Beck
---

*This post is a work in progress.*

I've run into Gaussian Process models a few times over the course of my
time in grad school, but haven't sat down and learned what they are and how to apply them. This post does not attempt to be
a tutorial on the subject, but rather a place for me to organize my notes and thoughts as I learn more about them. There are likely bits here that are incorrect and will be corrected as I broaden my understanding of the subject (feel free to leave an issue on the blog's [github repo](https://github.com/theandyb/theandyb.github.io) if you feel so inclined!)

# What are Gaussian Process Models?

In short: they are supervised models that can be used for both regression and classification tasks. Gaussian processes themselves are [stochastic processes](https://en.wikipedia.org/wiki/Stochastic_process), i.e. collections of random variables, such that any finite subset of these random variables has a multivariate normal distribution. Gaussian process models are **non-parametric** in the sense that they do not assume a fixed, finite number of parameters when defining the model, but rather the number of "parameters" grows with the size of the training data set.

When I first encountered Gaussian Processes, I was told something along the lines of "Gaussian processes define a prior distribution over all possible functions." It didn't immediately click when I first heard this, but I did understand the general procedure of: given a prior and a set of training data, we can derive a posterior, which also happens to be Gaussian, for predicting outputs for new observations. The lingering question for me was: ***How on Earth do you define a distribution over functions?***

The answer to that question with regards to Gaussian processes is that to represent a distribution over functions, you only need to be able to define a distribution over the function's outputs at a finite set of points. My understanding is that if we have a set of observations $$f(x_1), f(x_2), \ldots, f(x_N)$$ for some unknown function $$f$$ at a finite set of inputs $$x_1, \ldots, x_N$$, we can define a distribution over function values at the points:

$$
(x_1, \ldots, x_N) \sim N(\mu, \Sigma)
$$

where $$\mu$$ has dimension N, and $$\Sigma$$ defines how "smooth" the distribution of function values is by specfying how close values of the function $$f(x_1), \ldots, f(x_N)$$ should be based on how close values of $$x_1, \ldots, x_N$$ are to each other. I've skipped a bit here, as this is really the **posterior** density for a given set of data points and observations of the function at these points; my understanding is that the prior is similar, with $$\Sigma$$ being a covariance matrix for $$x_1, \ldots, x_N$$, and the prior mean I've seen in most of my reading is generally 0 (with a question for future Andy being: when would you use something other than the 0 vector as your prior mean?). The covariance matrix $$\Sigma$$ is a symmetric matrix constructed using a **kernel function** on pairs of data points (e.g. the i,j-th entry of $$\Sigma$$ is $$k(x_i,x_j)$$ for some kernel function $$k$$). What you can then do for a set of data points $$y_1, \ldots, y_M$$ without observations $$f(y_i)$$ is that you can still define the **joint density** of $$(f(x_1), \ldots, f(x_N), f(y_1), \ldots, f(y_M)$$ as a multivariate Gaussian density of dimension $$N + M$$ with $$\Sigma$$ defined by applying the kernel function to all pairs of points in the joint set $$(x_1, \ldots, x_N, y_1, \ldots, y_M)$$. From the joint density you can then derive the **conditional density** $$f(y_1, \ldots, y_M \| x_1, \ldots, x_M; \Sigma)$$ which is also multivariate Gaussian (with dimension M), and this defines a distribution of function values for f at the new points $$y_1, \ldots, y_M$$.

# So, how do you use these?

The above is well and all, and once we look up the math for deriving the conditional from the joint density should be fairly straight-forward to implement for regression tasks. But what does this look like in practice? How do we choose what kernel function to use? How important is this choice? Gaussian process models are also used for classification: how is the implementation here different than in the regression setting?

## Regression


# Sources I found useful

* [Gaussian Processes for Dummies](https://katbailey.github.io/post/gaussian-processes-for-dummies/)
* [Bayesian Optimization, Part 1: Key Ideas, Gaussian Processes](https://blog.quipu-strands.com/bayesopt_1_key_ideas_GPs)
* [Robust Gaussian Process Modeling](https://betanalpha.github.io/assets/case_studies/gaussian_processes.html)