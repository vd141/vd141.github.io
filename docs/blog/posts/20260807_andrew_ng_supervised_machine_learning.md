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

## Cost function

I was curious why mean squared error was used in ML4T. This course addresses a few reasons why MSE is a useful cost function:
- taking the mean of the errors prevents the cost function from unboundedly increasing as the training set increases in size
- the shape of a squared function is convex. in other words, the cost function will always have a minimum which gradient descent can exploit in all dimensions

Additionally, this course uses a MSE cost function with a coefficient of 1/2. More on why in the gradient descent section.

## Gradient descent

$\omega = \omega - \alpha \frac{\partial J(\omega)}{\partial \omega}$

  - With respect to model weights, simultaneous updates occur when weights are updated in succession. Ng cautions against using non simultaneous updates in gradient descent because this is not how gradient descent is typically implemented, which means the non simultaenous update may have different characteristics. instead, all weights should be updated before the next iteration

gradient descent is described as an interative update to a parameter. the updated parameter gets the result of the product of (a learning rate factor and [partial] derivative) subtracted from the previous parameter value.

In the ML community, a "batch" gradient descent algorithm is one that is used to tune the model on the "batch" - that is, the whole - of training data.

Notably, there exists an alternative to gradient descent, called the normal equation, that works only for linear regression. It can solve for the weights without iterations. It's practical when the number of features is small and may be used in some machine learning libraries for linear regression applications.

### Derivation
The reason for the 1/2 coefficient becomes apparent when deriving the gradient from the MSE cost function. Using the chain rule from calculus, the quadratic is differentiated into a linear function with a coefficient of 2. This is what the 1/2 coefficient from before addresses. 

## Learning rate
If the learning rate $\alpha$ is too small, gradient descent makes tiny updates and takes a long time to reach the minimum. If $\alpha$ is too large, the updates can overshoot the minimum, bounce around, or even fail to converge.

The main idea is that $\alpha$ controls the step size of each gradient-descent update. The best learning rate is large enough to make progress efficiently, but small enough that the cost keeps moving downward instead of oscillating or diverging. In practice, a dynamically-adjusted alpha could yield better results. For example, alpha can be relatively large at the outset, but iteratively diminished the closer the GD function gets to the local minima.

Ng notes that even with a fixed learning rate, it is still possible for GD to converge on the local minimum. Imagining a simple quadratic curve: as the iterative updates to the parameter approach the minimum, the magnitude of the  gradient decreases such that the magnitude of the parameter's updates reduce over time.

The most important takeaway of this section is that by periodically checking the cost function during a gradient descent, you can see whether or not the algorithm is converging or diverging. If it is diverging, this may be a signal to decrease the learning rate. 

## Convex function
The quadratic mean squared error (MSE) function is a convex function. The GD algorithm will always converge to its global minimum. This is in contrast to functions that have multiple local minima. The GD algorithm is not guaranteed to converge to the global minimum because it ultimately depends on where the initial guess of the parameters is.

## Multiple ~~multivariate~~ regression
Several features, one output. a set of features belonging to one training example is also known as a vector. (A multivariate regression involves multiple features predicting multiple outputs, which will not be covered here)

Multiple regression involves finding multiple weights to minimize the cost function of a training set of data. Each input feature is multiplied by its respective weight to produce the model's prediction. Like in linear regression, the bias term can be thought of as the prediction when all features are zero-valued. When features and weights are both represented as vectors, we take the dot product of the two and add the bias term to calculate the model's prediction.

### Vectorization
Vectorization is an efficient technique to perform vector calculations. Instead of using a for loop to calculate the dot product, or manually typing out the element-wise products, which can be tedious, using numpy's dot method requires only one line of code and is computationally more efficient. It is computationally efficient because it uses parallelization (computing multiple calculations simultaneously) to execute the calculations. This parallelism is enabled by the underlying hardware. GPU's and modern CPU's implement Single Instruction, Multiple Data (SIMD) pipelines allowing multiple operations to be executed in parallel.

### Vectorization vs sequential calculation
Imagine using a for loop and a cumulative sum to calculate the cost function of a model that has multiple weights and corresponding training inputs. The algorithm must complete and store the results of one calculation before moving on to the next one in the sequence. In other words, it performs calculations sequentially. In contrast, a vectorized algorithm computes each calculation at the same time, then sums the results of the calculations all at once. 

In small datasets and models, the performance differences between the two may be negligible. However, in larger datasets and models, the differences can be quite dramatic. This is why it is best practice to use vectorization when possible.

### Conventions
Dimension or rank are terms used to describe the number of elements in a vector.
Dimension can also be used to describe the number of indexes in an array (the number of elements the size() function returns)

When declaring a dimensional data struct in python, the first parameter of the constructor typically specifies the shape of the struct. The argument can be a scalar or a tuple.

Matrix dimensions are described with mxn tuples, with the elements representing rows and columns respectively.

The reshape method, when used on a vector/array, can be used to create a matrix from an array by specifying the number of rows and columns. if -1 is used for a row/column, it dynamically computes the respective number of rows/columns based on the number of elements and the value provided for the other dimension, and generates a matrix of  that shape

### Gradient descent for multiple linear regresssion

For multiple regression, the cost function is calculated the same way for each weight, but applied to each weight and its respective input. The gradient descent update for each coefficient becomes

${\omega}_j := {\omega}_j - \alpha \frac{1}{m} \sum_{i=1}^{m} \left(f_{\vec{\omega},b}(\vec{x}^{(i)}) - y^{(i)}\right)x_j^{(i)}$

where $f_{\vec{\omega},b}(\vec{x}^{(i)})$ is the model prediction for training example $i$, and $\vec{x}^{(i)}$ is the feature vector for that example. Note that the inner function $\vec{x}^{(i)}$ and the weights $\vec{\omega}$ are now vectors. Because the weights ${\omega}_j$ are stored in a vector, all the weights are updated simultaneously in each gradient descent iteration.

### Feature Scaling
