---
title: Supervised Machine Learning Basics
date:
  created: 2026-08-07
tags:
  - Notes
  - Machine Learning
---

After taking Machine Learning for Trading (ML4T) this spring, where I built a trading bot using machine learning principles, I was curious about applying the algorithms to areas outside of finance. In the course, I built a few machine learning algorithms from scratch in Python. It would pretty tedious to build the algorithms from scratch or tweak them to every use case ~~ain't nobody got time for that~~, however, and I wanted to focus more on their applications in new domains. Andrew Ng's Supervised Machine Learning course seemed like a great way to dip my toe in the water and use some modern-day frameworks, so I started the course.

Within the first few minutes of the course, I was presented with a few examples of supervised learning in practice: spam filtering, speech recognition, machine translation, online advertising, self-driving cars, and defect detection in manufacturing. Online advertising was the most illustrative example: features would be the ad and user info, and the output would be whether the user clicks on the ad. In the case of

Unsupervised learning received some coverage as well. An example of an unsupervised learning algorithm is in Google's news aggregation tile, where similar stories from different news outlets are clustered together. This is an example of a clustering algorithm. A clustering algorithm ~~finds structure in data without being explicitly told what that structure should look like~~ groups similar data together. So in the case of the google tile, it finds similar news articles based on several keywords that they share - these keywords are discovered by the algorithm itself!

More generally, an unsupervised learning algorithm receives data with labeled inputs and unlabeled outputs. 
Clustering is one example. Anomaly detection is another. An example of anomaly detection is detecting fraud in the financial systems, such as unusual transactions. Another example of unsupervised learning is dimensionality compression, where the resulting dataset is much smaller.

I was curious why mean squared error was used in ML4T. This course addresses a few reasons why MSE is a useful cost function:
- taking the mean of the errors prevents the cost function from unboundedly increasing as the training set increases in size
- the shape of a squared function is convex. in other words, the cost function will always have a minimum which gradient descent can exploit in all dimensions

## Gradient descent

$\omega = \omega - \alpha \frac{\partial J(\omega)}{\partial \omega}$
  - with respect to model weights, Ng cautions against using non simultaneous updates in gradient descent because this is not how gradient descent is typically implemented, which means the non simultaenous update may have different characteristics. instead, all weights should be updated before the next iteration

gradient descent is described as an interative update to a parameter. the updated parameter gets the result of the product of (a learning rate factor and [partial] derivative) subtracted from the previous parameter value.

In the ML community, a "batch" gradient descent algorithm is one that is used to tune the model on the "batch" - that is, the whole - of training data.

## Learning rate
If the learning rate $\alpha$ is too small, gradient descent makes tiny updates and takes a long time to reach the minimum. If $\alpha$ is too large, the updates can overshoot the minimum, bounce around, or even fail to converge.

The main idea is that $\alpha$ controls the step size of each gradient-descent update. The best learning rate is large enough to make progress efficiently, but small enough that the cost keeps moving downward instead of oscillating or diverging. In practice, a dynamically-adjusted alpha could yield better results. For example, alpha can be relatively large at the outset, but iteratively diminished the closer the GD function gets to the local minima.

Ng notes that even with a fixed learning rate, it is still possible for GD to converge on the local minimum. Imagining a simple quadratic curve: as the iterative updates to the parameter approach the minimum, the magnitude of the  gradient decreases such that the magnitude of the parameter's updates reduce over time.

## Convex function
The quadratic mean squared error (MSE) function is a convex function. The GD algorithm will always converge to its global minimum. This is in contrast to functions that have multiple local minima. The GD algorithm is not guaranteed to converge to the global minimum because it ultimately depends on where the initial guess of the parameters is.