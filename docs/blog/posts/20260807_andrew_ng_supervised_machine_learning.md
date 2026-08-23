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
Feature scaling is a technique that can improve gradient descent performance. Given a feature $x_1$ with values ranging from $1-5$ and a feature $x_2$ with values ranging from $1-100$. We can say that $x_1$ has a smaller range of values than $x_2$. In this example, a model will choose smaller weights for features that have large values, and larger weights for features with relatively small values. It follows that $w_1$ has a larger range than $w_2$. When plotting the cost function as a function of $w_1$ and $2_2$ on a contour plot, we would see that the algorithm oscillates on the $w_1$ scale for every small step it takes on the $w_2$ scale. The contour plot is asymmetric. This oscillation results in a slower convergence time. To speed up the convergence time, we can scale the features appropriately so that they are on similar scales, reducing the amount of oscillation. 

#### Feature scaling desired outcome
Recall that the objective of feature scaling is to improve gradient descent performance by normalizing features to similar ranges and scales.
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
When we have a particular insight about a dataset, we can engineer features by transforming or combining  existing ones to improve the prediction accuracy. For example, frontage (width of a plot of land) and length of a housing plot may be engineered into an area feature (frontage x length) that can be added to the training example, which can then be used to predict price. 

### Polynomial regression (another form of feature engineering)
If the relationship between a feature and the target
 is nonlinear, polynomial terms can be engineered from the existing feature to provide a better model fit to the data. For example, squaring, cubing, or taking the root of an existing feature and using it as a new feature can help the model provide more accurate predictions. Here, feature scaling is even more important due to the increased scale of the derived features.

 When gradient descent is applied to a model with polynomials, it tends to emphasize the terms that fit the data best (increases their weights) and deemphasizes the other terms. Less weight value implies less important/correct feature.

 An alternate way of thinking about fitting polynomials to targets is that the best features are actually linear relative to the target. 

With feature engineering, complex functions can be modeled.

## Scikit-learn models
Scikit-learn has a gradient descent regression model `sklearn.linear_model.SGDRegressor`

- Performs best with normalized inputs
- `sklearn.preprocessing.StandardScaler` performs z-score normalization, AKA 'standard score'

Code sample using scikit learn below
```Python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import SGDRegressor
from sklearn.preprocessing import StandardScaler
from lab_utils_multi import  load_house_data
from lab_utils_common import dlc
np.set_printoptions(precision=2)
plt.style.use('./deeplearning.mplstyle')

# load the training set
X_train, y_train = load_house_data()
X_features = ['size(sqft)','bedrooms','floors','age']

# scale/normalize the training data
scaler = StandardScaler()
X_norm = scaler.fit_transform(X_train)
print(f"Peak to Peak range by column in Raw        X:{np.ptp(X_train,axis=0)}")   
print(f"Peak to Peak range by column in Normalized X:{np.ptp(X_norm,axis=0)}")

# Create and fit the regression model
sgdr = SGDRegressor(max_iter=1000)
sgdr.fit(X_norm, y_train)
print(sgdr)
print(f"number of iterations completed: {sgdr.n_iter_}, number of weight updates: {sgdr.t_}")

# View the parameters
b_norm = sgdr.intercept_
w_norm = sgdr.coef_
print(f"model parameters:                   w: {w_norm}, b:{b_norm}")
print( "model parameters from previous lab: w: [110.56 -21.27 -32.71 -37.97], b: 363.16")

# Make predictions
# make a prediction using sgdr.predict()
y_pred_sgd = sgdr.predict(X_norm)
# make a prediction using w,b. 
y_pred = np.dot(X_norm, w_norm) + b_norm  
print(f"prediction using np.dot() and sgdr.predict match: {(y_pred == y_pred_sgd).all()}")

print(f"Prediction on training set:\n{y_pred[:4]}" )
print(f"Target values \n{y_train[:4]}")

# plot predictions and targets vs original features    
fig,ax=plt.subplots(1,4,figsize=(12,3),sharey=True)
for i in range(len(ax)):
    ax[i].scatter(X_train[:,i],y_train, label = 'target')
    ax[i].set_xlabel(X_features[i])
    ax[i].scatter(X_train[:,i],y_pred,color=dlc["dlorange"], label = 'predict')
ax[0].set_ylabel("Price"); ax[0].legend();
fig.suptitle("target versus prediction using z-score normalized model")
plt.show()
```

## Linear Regression Practice Lab Lessons Learned
The problem statement is this: Suppose you are the CEO of a restaurant chain and want to expand to new, more profitable cities. Given that you already have restaurants in many cities and profit/population data from those, can you use your data to predict which cities could be good candidates to give your business higher profits?

1. Start by loading the population/profit data for all your existing restaurants.
2. Explore some of the data, and familiarize yourself with some examples. Verify that the data looks as you expect it to. If it doesn't, perhaps the data is in a different range than you were expecting, the units are different, or maybe there is some bad data. Also verify the data's dimensions. It is important  the data is fit for model-building.
3. Build plots to visualize the data!

Linear regression refresher: Building a good linear regression model involves selecting the weights that minimize the cost function. So the cost function is dependent on the weights. Gradient descent improves the weight selection by updating the weights iteratively. It does so by subtracting the product of the learning rate and derivative of the cost function (with respect to the weight) from the previous weight value. When gradient descent can no longer improve the weights, i.e. when there is not a significant difference in cost function between iterations, the model is considered "trained" and can now make predictions on non-training data. 

The lab has you code up the cost function and gradient descent. With vectorized code in mind, I found it was easier to write out my thoughts as comments first, then translate them to vectorized code. My cost function ended up being 3 lines, although it could probably be condensed to 1 (at the cost of readability). 

``` Python
def compute_cost(x, y, w, b): 
    """
    Computes the cost function for linear regression.
    
    Args:
        x (ndarray): Shape (m,) Input to the model (Population of cities) 
        y (ndarray): Shape (m,) Label (Actual profits for the cities)
        w, b (scalar): Parameters of the model
    
    Returns
        total_cost (float): The cost of using w,b as the parameters for linear regression
               to fit the data points in x and y
    """
    # number of training examples
    m = x.shape[0] 
    
    # You need to return this variable correctly
    total_cost = 0
    
    ### START CODE HERE ###
    # x and y are single column vectors
    # model makes a prediction f_wb(x^(i)) based on an input x^(i)
    f_wb = w * x + b # f_wb is a column vector of length m, is the model's prediction for each x
    # cost is the difference between the prediction and true output, squared
    cost_vec = (f_wb - y) ** 2 # vector (length m) of the cost for each training example
    # the cost over all predictions and outputs for the feature set are summed and divided by (2m)
    total_cost = sum(cost_vec) / (2*m)
    
    ### END CODE HERE ### 

    return total_cost
```

```Python
def compute_gradient(x, y, w, b): 
    """
    Computes the gradient for linear regression 
    Args:
      x (ndarray): Shape (m,) Input to the model (Population of cities) 
      y (ndarray): Shape (m,) Label (Actual profits for the cities)
      w, b (scalar): Parameters of the model  
    Returns
      dj_dw (scalar): The gradient of the cost w.r.t. the parameters w
      dj_db (scalar): The gradient of the cost w.r.t. the parameter b     
     """
    
    # Number of training examples
    m = x.shape[0]
    
    # You need to return the following variables correctly
    dj_dw = 0
    dj_db = 0
    
    ### START CODE HERE ###
    
    # single variable linear regression depends on gradient descent for each weight: w, b
    # the gradient descent for each weight is calculated using the partial derivative of the cost function with
    # respect to the weight
    # the partial derivative with respect to w is: (f_wb - y) * x
    # the partial derivative with respect to b is: (f_wb - y)
    # This is absolutely true when there is one training example x, and one target y.
    # But when there are multiple training examples, the gradient should be based on all of the training examples. 
    # So we take the average gradient across all training examples: sum(w_gradients)/m and sum(b_gradients)/m
    
    f_wb = w * x + b # f_wb is a column vector of length m, is the model's prediction for each x
    
    dj_dw = (1 / m) * sum((f_wb - y) * x)
    dj_db = (1 / m) * sum((f_wb - y))

    
    ### END CODE HERE ### 
        
    return dj_dw, dj_db
```

After cwriting compute_gradient, the function can now be used iteratively in a gradient descent algorithm. The algorithm should iteratively improve the weights (the cost function should decrease each time). The algorithm is a loop of: calculate gradient, update weight, measure cost. The end condition can be set as a number of iterations or a mimimum cost function delta between successive iterations.

## Classification
Home stretch of the course! After a few days of refreshing regression basics, I am excited to get to classification. The course so far has been refreshing. I've been able to explore the topics in more depth than I did in the ML4T class.

Some classification applications: determining if email is spam, determining if a transaction is fraudulent, determining if a tumor is malignant. A classification model makes a categorization based on whether the probability of an input being in a category exceeds the decision boundary.

Classification predicts discrete categories, while regression predicts real numbers. Here's a question: given a training data set of single-column real-valued inputs mapped to binary outputs, what happens if I use a linear regression model to deterimine categories? Assume that I have found a linear regression model that fits the data (it probably won't fit all that well, because the training data is not linear. You might see where this is going). To use it for classification, I would need to assign a decision boundary. The decision boundary would be affixed to the model where $f(x)$ is equal to 0.5 (the midpoint between binary outputs 0 and 1). The x-value of this decision boundary is the point that differentiates inputs that get a positive classification. What happens to this decision boundary when outliers are added to the training set? Well, the linear regression would change as a result of the outliers, so the x-value of the decision boundary would also change. This isn't ideal behavior - we don't want a different input to change the way the model makes a classification. An example that is positive, no matter to what degree it is, shouldn't change what prediction the model makes. Since linear regression is affected by these outliers, it's not ideal for classification. So we'll want to use a different function.

### Logistic regression
Despite having "regression" in the name, logistic regression is used for classification! According to Ng, it is one of the most widely used classification algorithms in the world. Logistic regression uses a function called a sigmoid. A sigmoid function $f(z) = 1 / (1 + e^{-z})$ is used for classification because it transforms all inputs to an output existing between 0 and 1.The output can also be understood as the probability that an input should be classified as 1/positive. When z is 0, the sigmoid function is valued at 0.5. 0.5 is often chosen as a decision boundary - the threshold that determines which category an input belongs to. In other words, 50% probability is the threshold separating a negative and positive classification.

#### Decision Boundaries
When the input of the sigmoid function $z$ is substituted with a function such as $-3 + x_0+x_1$ or $x_0^2 + x_1 -1$, the function determines the shape of the decision boundary. 

#### Cost function for logistic regression
In linear regression, the MSE cost function was used because its convexity allowed the algorithm to converge to a minimum. In logistic regression, the same cost function doesn't work well because the logistic function is nonlinear. When MSE is applied to logistic regression, the cost function exhibits several local minima, which is not ideal for the gradient descent algorithm. Instead of MSE, we'll use a different cost function.

Important vocabulary distinction: **loss** is a measure of the difference between a prediction and its target, while **cost** is the sum of losses from an entire training set.

The loss of a sigmoid function is calculated using two separate curves. One is for the case when the true value is positive and the other for when the true value is negative. The behavior of the cost function is as such: when the prediction is close to the true value, the loss is near zero. But when the prediction is close to the opposite of the true value, the loss rapidly approaches infinity. Combined, the two curves make a convex shape, which is an ideal form for gradient descent. The loss function is described as such: $loss(f_{\mathbf{w},b}(\mathbf{x}^{(i)}), y^{(i)}) = (-y^{(i)} \log\left(f_{\mathbf{w},b}\left( \mathbf{x}^{(i)} \right) \right) - \left( 1 - y^{(i)}\right) \log \left( 1 - f_{\mathbf{w},b}\left( \mathbf{x}^{(i)} \right) \right)$. Notice that the loss function is comprised of two main terms. When the true prediction is positive, or 1, one term is nullified. The other term is nullified when the true prediction is negative, or 0. 

This particular loss function is derived using a statistical principle called maximum likelihood estimation, which is an idea from statistics about how to efficiently find ideal parameters from different models.

To derive the cost function, simply add up the losses from each training example and divide by the number of training examples.

#### Batch gradient descent for logistic regression
Just like in regression, the weights are updated by subtracting the a product of the learning rate and the gradient of the cost function from the weight's current value.
$\begin{align*}
&\text{repeat until convergence:} \; \lbrace \\
&  \; \; \;w_j = w_j -  \alpha \frac{\partial J(\mathbf{w},b)}{\partial w_j} \tag{1}  \; & \text{for j := 0..n-1} \\ 
&  \; \; \;  \; \;b = b -  \alpha \frac{\partial J(\mathbf{w},b)}{\partial b} \\
&\rbrace
\end{align*}$

 How is the gradient of the cost function calculated in this case? It turns out, the cost function gradient takes a similar form to the one in linear regression. That is, 
 $\begin{align*}
\frac{\partial J(\mathbf{w},b)}{\partial w_j}  &= \frac{1}{m} \sum\limits_{i = 0}^{m-1} (f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)})x_{j}^{(i)} \tag{2} \\
\frac{\partial J(\mathbf{w},b)}{\partial b}  &= \frac{1}{m} \sum\limits_{i = 0}^{m-1} (f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)}) \tag{3} 
\end{align*}$
The only difference now is that $(f_{\mathbf{w},b}(\mathbf{x}^{(i)})$ is the sigmoid function, rather than a linear function. The derivation is omitted from this lecture, and can be an exercise for the reader.

``` Python
def compute_gradient_logistic(X, y, w, b): 
    """
    Computes the gradient for logistic regression 
 
    Args:
      X (ndarray (m,n): Data, m examples with n features
      y (ndarray (m,)): target values
      w (ndarray (n,)): model parameters  
      b (scalar)      : model parameter
    Returns
      dj_dw (ndarray (n,)): The gradient of the cost w.r.t. the parameters w. 
      dj_db (scalar)      : The gradient of the cost w.r.t. the parameter b. 
    """
    m,n = X.shape
    dj_dw = np.zeros((n,))                           #(n,)
    dj_db = 0.

    for i in range(m):
        f_wb_i = sigmoid(np.dot(X[i],w) + b)          #(n,)(n,)=scalar, prediction for a training example
        err_i  = f_wb_i  - y[i]                       #scalar, error for a full training example
        for j in range(n):
            dj_dw[j] = dj_dw[j] + err_i * X[i,j]      #scalar, for each weight accumulate product of full training example error and X[i, j] (example for that weight). the gradient of each weight depends on these two quantities
        dj_db = dj_db + err_i                         # gradient of bias depends only on the error
    dj_dw = dj_dw/m                                   #(n,)
    dj_db = dj_db/m                                   #scalar
        
    return dj_db, dj_dw 
```

In gradient descent, the weights are updated by a quantity that depends on the learning rate $\alpha$ and the gradient. The gradient is recalculated for each iteration, and depends on the model's error. For non-bias parameters, the weights depend on the product of the error and training examples. For the bias parameter, the gradient is dependent on the error only. The gradient can be understood as a measure of how far away the weights are from their optimal (minimal error) value. When the gradient is large, the model is far from converging. When the gradient is small or near-zero, the weights are near convergence, or have converged.

#### Logistic Regression with Scikit-Learn
``` Python
import numpy as np

# dataset
X = np.array([[0.5, 1.5], [1,1], [1.5, 0.5], [3, 0.5], [2, 2], [1, 2.5]])
y = np.array([0, 0, 0, 1, 1, 1])

from sklearn.linear_model import LogisticRegression

# fit the model
lr_model = LogisticRegression()
lr_model.fit(X, y)

# make predictions
y_pred = lr_model.predict(X)

print("Prediction on training set:", y_pred)

# calculate accuracy
print("Accuracy on training set:", lr_model.score(X, y))
```

### Overfitting
When a model underfits the data, it can also be said to have high bias.

A model that "generalizes" well is one that fits the training set pretty well (but not perfectly!) and accurately predicts unseen examples.

A model that "overfits" fits the training set extremely well (perfectly or near-perfectly) but does not accurately predict unseen samples. These models are said to have high variance. They are highly sensitive to small changes in the training data.

#### 3 techniques to address overfitting
The goal of these techniques is to reduce the variance, which will help the model generalize better.

Increasing the training set size will prevent the model from overfitting a few pieces of data. This may not always be possible in situations where data is scarce.

Using a subset of existing features will also reduce overfitting. The subset to focus on would be those that are most predictive of the output. This technique is called feature selection. The drawback of this method is that some discarded features could be uesful in predicting the output. There do exist algorithms that automatically select the most appropriate features for prediction tasks.

Regularization is a technique that diminishes (but does not totally eliminate) the impact that certain features have on the prediction. Instead of eliminating a particular feature by setting all of its parameters to zero, regularization tunes the examples such that the weights for those features end up being very small. This allows for higher-order models to be used without encountering high variance. In practice, the w weights are typically regularized, as opposed to regularizing b. Regularizing b often has little effect on reducing variance. Regularization is also used in neural networks!

#### Overfitting Lab
- Polynomials of excessively high degree tend to overfit
- conversely, polynomials of excessively low degree tend to underfit
- extreme examples can increase overfitting (assuming they are outliers)
- nominal examples can reduce overfitting
- fitting a line to smaller datasets can be done without pure gradient descent (implementation method not mentioned, however)

#### Modifying the cost function to accomodate regularization
Simply add the weights that you want to minimize to the cost function, and multiply them by relatively large coefficients. The exact quantity doesn't matter as much, but by putting large coefficients in front of these weights, the cost function penalizes these weights heavily when they are large. Assuming that the weights being penalized correspond to higher-order terms, regularization effectively makes the model behave as a lower-order model, thus reducing overfitting.

To generalize this technique, such as in instances where there are many features and we don't know which ones to regularize, we can simply regularize all of them. This is implemented by adding a regularization term to the cost function that looks like $\frac{\lambda}{2m} \sum\limits_{j = 1}^{n} (w_j^2)$, where $m$ is the number of training examples, $n$ is the number of weights, and $\lambda$ is a hyperparameter that controls the strength of the regularization penalty. When $\lambda$ is large, the cost function penalizes the weights so much that the only weight left over is the constant bias term, $b$. When $\lambda$ is very small, the regularization term is effectively nullified, and high variance can exist in the model. The $\frac{\lambda}{2m}$ scaling factor makes it likelier for lambda to work with larger datasets (larger $m$) as it did for smaller ones.

#### gradient descent with regularization
The only difference between the gradients for non-regularized and regularized models is that the gradient for $\mathbf{w}$ includes a new term: the partial derivative of the regularization term with respect to $w_j$, which turns out to be $\frac{\lambda}{m}w_j$. As stated previously, $b$ is not typically regularized, so in most circumstances there is no need to add a regularization term to its gradient in the regularized case, making it identical to the non-regularized gradient.

#### Final lab: building a logistic regression model from scratch
1. Visualize the shape of the data by viewing dimensions of data, peeking at values, plotting features vs. output
``` Python
import numpy as np
import matplotlib.pyplot as plt
from utils import *
import copy
import math

%matplotlib inline
# load dataset
X_train, y_train = load_data("data/ex2data1.txt")

print("First five elements in X_train are:\n", X_train[:5])
print("Type of X_train:",type(X_train))

print("First five elements in y_train are:\n", y_train[:5])
print("Type of y_train:",type(y_train))

print ('The shape of X_train is: ' + str(X_train.shape))
print ('The shape of y_train is: ' + str(y_train.shape))
print ('We have m = %d training examples' % (len(y_train)))

# Plot examples
plot_data(X_train, y_train[:], pos_label="Admitted", neg_label="Not admitted")

# Set the y-axis label
plt.ylabel('Exam 2 score') 
# Set the x-axis label
plt.xlabel('Exam 1 score') 
plt.legend(loc="upper right")
plt.show()
```

2. Implement the sigmoid function. The input to the sigmoid function can be a scalar or a vector. To handle vectors, I used the np.power method to calculate the exponential in the denominator.
``` Python
def sigmoid(z):
    """
    Compute the sigmoid of z

    Args:
        z (scalar or ndarray): A scalar or numpy array of any size.

    Returns:
        g (scalar or ndarray): sigmoid(z), with the same shape as z if z is a numpy array.
         
    """
          
    ### START CODE HERE ### 
    g = 1 / (1 + np.power(np.e,-z))
    ### END SOLUTION ###  
    
    return g
```

3. Implement the cost function. I broke this down into three lines of code, each being a component of the cost: linear model output, $g(z)$, loss, and cost. The linear model output is obtaind by calculating the dot product of the training examples and the weights and adding the bias term. $g(z)$, otherwise known as the sigmoid output, is simply the result of the linear model output when it is fed into the sigmoid function that I wrote in the previous step. To calculate the loss, I used the binary cross-entropy loss (log loss). Notably, the natural log function is typically used, rather than the base 10 log function. Functionally speaking, gradient descent will work with either implementation, but natural log is preferred because it simplifies the gradient calculation. Using base 10 log introduces a scaling factor that clutters the derivation.
``` Python
def compute_cost(X, y, w, b, *argv):
    """
    Computes the cost over all examples
    Args:
      X : (ndarray Shape (m,n)) data, m examples by n features
      y : (ndarray Shape (m,))  target value 
      w : (ndarray Shape (n,))  values of parameters of the model      
      b : (scalar)              value of bias parameter of the model
      *argv : unused, for compatibility with regularized version below
    Returns:
      total_cost : (scalar) cost 
    """

    m, n = X.shape
    
    ### START CODE HERE ###
    
    # total cost is the average loss calculation for each training example
    # the loss function input is the output of the sigmoid function
    # the output of the sigmoid function depends on the output of the linear model
    linear_model = np.dot(X, w) + b
    
    # let g be the sigmoid output
    g = sigmoid(linear_model)
    
    # the loss for each example in g depends on the difference between g and the actual value in y
    loss = (-y * np.log(g)) - ((1 - y) * np.log(1 - g))
        
    # total cost is the sum of all losses divided by the number of training examples
    total_cost = sum(loss) / m
    
    ### END CODE HERE ### 

    return total_cost
```

4. Implement the gradient for logistic regression
The skeleton code for this section uses a double nested for loopm, but I opted to vectorize when possible. Like the cost function calculation, the gradient depends on the difference between the sigmoid output of the linear model (prediction delta) and the true label. dj_db was calculable using vectorization. Each dj_dw value (one for each weight) was obtained by calculating the average value of [the product of each prediction delta and its input feature value], for all feature examples. This was the only part of the code that requried a for loop. 
``` Python
def compute_gradient(X, y, w, b, *argv): 
    """
    Computes the gradient for logistic regression 
 
    Args:
      X : (ndarray Shape (m,n)) data, m examples by n features
      y : (ndarray Shape (m,))  target value 
      w : (ndarray Shape (n,))  values of parameters of the model      
      b : (scalar)              value of bias parameter of the model
      *argv : unused, for compatibility with regularized version below
    Returns
      dj_dw : (ndarray Shape (n,)) The gradient of the cost w.r.t. the parameters w. 
      dj_db : (scalar)             The gradient of the cost w.r.t. the parameter b. 
    """
    m, n = X.shape
    dj_dw = np.zeros(w.shape)
    dj_db = 0.

#     ### START CODE HERE ### 
#     for i in range(m):
#         z_wb = None
#         for j in range(n): 
#             z_wb += None
#         z_wb += None
#         f_wb = None
        
#         dj_db_i = None
#         dj_db += None
        
#         for j in range(n):
#             dj_dw[j] = None
            
#     dj_dw = None
#     dj_db = None
    ### END CODE HERE ###
    
    # not sure why for loop is being used here. trying vectorization instead
    linear_model = np.dot(X, w) + b
    g = sigmoid(linear_model)
    dj_db = (1 / m) * sum(g - y)
    for j in range(n):
        dj_dw[j] = (1 / m) * sum((g - y) * (X[:,j]))
        
    return dj_db, dj_dw
```

5. Using the weights and predictions from gradient descent, plot the results
The weights that gradient descent converges on are the parameters for the decision boundary. Gradient descent code, which was provided, is included here.

``` Python
def gradient_descent(X, y, w_in, b_in, cost_function, gradient_function, alpha, num_iters, lambda_): 
    """
    Performs batch gradient descent to learn theta. Updates theta by taking 
    num_iters gradient steps with learning rate alpha
    
    Args:
      X :    (ndarray Shape (m, n) data, m examples by n features
      y :    (ndarray Shape (m,))  target value 
      w_in : (ndarray Shape (n,))  Initial values of parameters of the model
      b_in : (scalar)              Initial value of parameter of the model
      cost_function :              function to compute cost
      gradient_function :          function to compute gradient
      alpha : (float)              Learning rate
      num_iters : (int)            number of iterations to run gradient descent
      lambda_ : (scalar, float)    regularization constant
      
    Returns:
      w : (ndarray Shape (n,)) Updated values of parameters of the model after
          running gradient descent
      b : (scalar)                Updated value of parameter of the model after
          running gradient descent
    """
    
    # number of training examples
    m = len(X)
    
    # An array to store cost J and w's at each iteration primarily for graphing later
    J_history = []
    w_history = []
    
    for i in range(num_iters):

        # Calculate the gradient and update the parameters
        dj_db, dj_dw = gradient_function(X, y, w_in, b_in, lambda_)   

        # Update Parameters using w, b, alpha and gradient
        w_in = w_in - alpha * dj_dw               
        b_in = b_in - alpha * dj_db              
       
        # Save cost J at each iteration
        if i<100000:      # prevent resource exhaustion 
            cost =  cost_function(X, y, w_in, b_in, lambda_)
            J_history.append(cost)

        # Print cost every at intervals 10 times or as many iterations if < 10
        if i% math.ceil(num_iters/10) == 0 or i == (num_iters-1):
            w_history.append(w_in)
            print(f"Iteration {i:4}: Cost {float(J_history[-1]):8.2f}   ")
        
    return w_in, b_in, J_history, w_history #return w and J,w history for graphing
```

``` Python
np.random.seed(1)
initial_w = 0.01 * (np.random.rand(2) - 0.5)
initial_b = -8

# Some gradient descent settings
iterations = 10000
alpha = 0.001

w,b, J_history,_ = gradient_descent(X_train ,y_train, initial_w, initial_b, 
                                   compute_cost, compute_gradient, alpha, iterations, 0)

plot_decision_boundary(w, b, X_train, y_train)
# Set the y-axis label
plt.ylabel('Exam 2 score') 
# Set the x-axis label
plt.xlabel('Exam 1 score') 
plt.legend(loc="upper right")
plt.show()
```

6. Make a prediction baesd on the learned weights

``` Python
def predict(X, w, b): 
    """
    Predict whether the label is 0 or 1 using learned logistic
    regression parameters w
    
    Args:
      X : (ndarray Shape (m,n)) data, m examples by n features
      w : (ndarray Shape (n,))  values of parameters of the model      
      b : (scalar)              value of bias parameter of the model

    Returns:
      p : (ndarray (m,)) The predictions for X using a threshold at 0.5
    """
    # number of training examples
    m, n = X.shape   
    p = np.zeros(m)
   
    ### START CODE HERE ### 
    # Loop over each example
    # feed each example through f(g(z)). in other words, put it through the linear model, 
    # then put the output of the linear model to the sigmoid function for the prediction
#     for i in range(m):   
#         z_wb = 0
#         # Loop over each feature
#         for j in range(n): 
#             # Add the corresponding term to z_wb
#             z_wb += X[i][j] * w[j]
        
#         # Add bias term 
#         z_wb += b
        
#         # Calculate the prediction for this example
#         f_wb = sigmoid(z_wb)

#         # Apply the threshold
#         p[i] = (f_wb >= 0.5)
        
    z_wb = np.dot(X, w) + b
    f_wb = sigmoid(z_wb)
    p = (f_wb >= 0.5)
        
    ### END CODE HERE ### 
    return p

# Test your predict code
np.random.seed(1)
tmp_w = np.random.randn(2)
tmp_b = 0.3    
tmp_X = np.random.randn(4, 2) - 0.5

tmp_p = predict(tmp_X, tmp_w, tmp_b)
print(f'Output of predict: shape {tmp_p.shape}, value {tmp_p}')

# UNIT TESTS        
predict_test(predict)
```

7. Tying it all together with regularized logistic regression
Given a dataset containing test result data for a microchip, and a determination (pass, failed) of the microchip after QA analysis, we want to predict whether a microchip with test results will pass or fail. 

Plotting the data reveals that the potential decision boundary would be ill-served with a linear model.

So to increase our chances of finding a better model, we construct new features from each data point.

Given two features, $x_1$ and $x_2$, we map the features into all polynomial terms of these features of a 6th order polynomial. 


$$\mathrm{map\_feature}(x) = 
\left[\begin{array}{c}
x_1\\
x_2\\
x_1^2\\
x_1 x_2\\
x_2^2\\
x_1^3\\
\vdots\\
x_1 x_2^5\\
x_2^6\end{array}\right]$$

The function that does this was provided in the lab. The feature mapping function increased the number of features from 2 to 27. 

The intent with this feature mapping is to improve the model's ability to fit the data. Without regularization, the model is susceptible to overfitting/high variance.

To implement regularization, an extra term is added to the cost function for the weights in $\vec{w}$. $b$ doesn't need a regularized term, since it purportedly has little effect on the outcome. 

``` Python
def compute_cost_reg(X, y, w, b, lambda_ = 1):
    """
    Computes the cost over all examples
    Args:
      X : (ndarray Shape (m,n)) data, m examples by n features
      y : (ndarray Shape (m,))  target value 
      w : (ndarray Shape (n,))  values of parameters of the model      
      b : (scalar)              value of bias parameter of the model
      lambda_ : (scalar, float) Controls amount of regularization
    Returns:
      total_cost : (scalar)     cost 
    """

    m, n = X.shape
    
    # Calls the compute_cost function that you implemented above
    cost_without_reg = compute_cost(X, y, w, b) 
    
    # You need to calculate this value
    reg_cost = 0.
    
    ### START CODE HERE ###
    
    reg_cost = (lambda_ / (2 * m)) * sum(w ** 2)
    
    ### END CODE HERE ### 
    
    # Add the regularization cost to get the total cost
    total_cost = cost_without_reg + reg_cost

    return total_cost
```

For the gradient, the update is similar:

``` Python
def compute_gradient_reg(X, y, w, b, lambda_ = 1): 
    """
    Computes the gradient for logistic regression with regularization
 
    Args:
      X : (ndarray Shape (m,n)) data, m examples by n features
      y : (ndarray Shape (m,))  target value 
      w : (ndarray Shape (n,))  values of parameters of the model      
      b : (scalar)              value of bias parameter of the model
      lambda_ : (scalar,float)  regularization constant
    Returns
      dj_db : (scalar)             The gradient of the cost w.r.t. the parameter b. 
      dj_dw : (ndarray Shape (n,)) The gradient of the cost w.r.t. the parameters w. 

    """
    m, n = X.shape
    
    dj_db, dj_dw = compute_gradient(X, y, w, b)

    ### START CODE HERE ###     
    
    dj_dw += (lambda_ / m) * w
        
    ### END CODE HERE ###         
        
    return dj_db, dj_dw
```

It is after this step that gradient descent can be used to learn the weights. Once the weights have been learned, they can be used to plot the decision boundary and predict the inputs.

And that concludes the topics covered in this course! I had a lot of fun learning regression and classification basics and I am excited to continue on to Advanced Learning Algorithms. 

## Areas for Further Exploration
I realized there were a few areas for me to dive deeper into while learning from the lectures. Some fundamentals could be refreshed/learned. Some derivation details were left out, as well as fundamental statistical ideas, such as maximum likelihood estimation.