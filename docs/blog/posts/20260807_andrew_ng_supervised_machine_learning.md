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
Feature scaling is a technique that can improve gradient descent performance. Given a feature $x_1$ with values ranging from $1-5$ and a feature $x_2$ with values ranging from $1-100$. We can say that $x_1$ has a smaller range of values than $x_2$. In this example, a model will choose smaller weights for features that have large values, and larger weights for features with relatively small values. It follows that $w_1$ has a larger range than $w_2$. When plotting the cost function as a function of $w_1$ and $2_2$, we would see that the algorithm oscillates on the $w_1$ scale for every small step it takes on the $w_2$ scale. This oscillation results in a slower convergence time. To speed up the convergence time, we can scale the features appropriately so that they are on similar scales, reducing the amount of oscillation. 

#### Feature scaling desired outcome
Recall that the objective of feature scaling is to improve gradient descent performance by scaling features to similar ranges.
Generally, it is desirable to bound the feature values between -1 and 1. In practice, deviations from this ideal range are acceptable: -3 to 3 and -0.3 to 0.3 are fine. 0 to 3 for $x_1$ is even okay, even when a separate feature $x_2$ is in the range of -2 to 0.5. Situations you might want to rescsale: $x_3$ in the range of -100 to 100, $x_4$ in the range of -0.001 to 0.001. The ranges are too big and too small. In the case of $x_5$ ranging from 98.6 to 105, the range is in-family, but the values themselves are too large. So this feature is a prime candidate for rescaling.
Feature scaling is almost never harmful, so if it is cheap to do, it should be done.

#### Feature Scaling Techniques
Assuming the training data values are all nonnegative, a rudimentary technique is to divide each training feature by the maximum value of its feature set (assuming all training features are positive). This bounds each feature between 0 and 1. A more generalizable version of this technique is to rescale each feature by both its minimum and maximum values using $x_{i_{transformed}} = (x_i - x_{imin})/(x_{imax}-x_{imin})$.

Another technique for centering a feature's values near
 0 is known as mean normalization. To perform a mean normalization for a particular feature $x_i$, find the mean, $\mu_i$ of the feature and the difference between the maximum and minimum value of that feature. To transform each feature example, subtract the mean and divide the result by the difference: $x_{i_{transformed}} = (x_i - \mu_i)/(x_{imax}-x_{imin})$.

Z-score normalization: The result of this technique is a feature with a mean of 0 and a standard deviation of 1. For a feature, calculate the mean $\mu_1$ and standard deviation $\sigma_1$. Subtract $\mu_1$ from a feature example and divide the result by $\sigma_1$: $x_{i_{transformed}} = (x_i - \mu_i)/\sigma_1$.

When normalizing features, it is important to store the mean and standard deviations. After learning the parameters from the model, we want to apply the model to a dataset it has not yet encountered. The mean and standard deviation are used to normalize the unseen data.

Notably, these techniques do not alter the distribution of the data, only its scale.

### Checking gradient descent for convergence
Plot the cost function values against the number of iterations that have occurred. This is called a learning curve. This plot can alert you to a poorly chosen learning rage $\alpha$ or a bug in the code. The cost function has likely converged when the learning curve has flattened out. The number of iterations needed for convergence varies by the application. Another way to flag for convergence is by using an $\epsilon$ value, which is the delta of the cost function between two different iterations that we can define to declare convergence. Ng prefers to use a learning curve to observe the behavior as the algorithm converges. 

### Choosing a learning rate
The learning curve can suggest that the $\alpha$ is too large. If the cost function is oscillating or steadily increasing, $\alpha$ may require a reduction. These behaviors could also suggest that there is a bug in the code, for example if the learning rate term is being added to the weight instead of being subtracted. A functional $\alpha$ results in a steadily decreasing learning curve. A very small $\alpha$ can be used to debug code - if the learning curve is not steadily decreasing, this suggests an error in the code. If there are no errors, $\alpha$ can be increased (but not by too much!) to improve convergence performance. Ng uses threefold scaling factors after a successful attempt with $\alpha$ and adjusts from there.

### Exploratory data analysis
Before training a model on the training set, we plot each feature data against the target data to spot any relationships.

### Feature engineering
