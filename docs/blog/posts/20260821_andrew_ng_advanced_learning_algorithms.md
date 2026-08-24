---
title: Advanced Learning Algorithms
date:
  created: 2026-08-21
tags:
  - Notes
  - Machine Learning
---

## Foreword
After completing the Supervised Machine Learning course as part of the Machine Learning Specialization taught by Andrew Ng on Coursera, I am now working on the Advanced Learning Algorithms course, which covers neural networks, training, and decision trees, as well as how to build a practical machine learning system. I coded a decision tree as part of an assignment and read about neural networks in ML4T, so I am excited to learn from Andrew Ng's perspective and get some hands-on experience with neural networks.

## Neural Network History
The original motivation for neural networks (NN) was to mimic, with software, how the human/biological brain learns and thinks. Today, NN, also known as artificial neural networks (ANN), have become very different from how we think the brain works and learns. Some of the biological motivations still remain in the way we think about ANN or computed NN today.

NN was first invented in the 50's. NN were then used in the 80's an early 90's to recognize handwritten postal codes for mail routing and dollar figures in handwritten checks. The fell out of favor, and experienced a resurgence in popularity in the mid 2000's. It was also rebranded into "deep learning". Since then, neural networks have revolutionized many application areas. The first area that deep learning had a significant impact on was speech recognition systems (Li Deng and Geoff Hinton). Next, the technology made inroads to computer vision, with ImageNet being a milestone enabler for future research in the domain. The next "era" of deep learning became text processing or natural language processing (NLP). Nowadays, NN are used in climate change, medical imaging, online advertising, product recommendations, and many application areas of machine learning.

Neural networks were initially constructed to mimic the present understanding of the human brain. Thoughts travel through the neurons in the brain as electrical signals. A neuron gathers electrical signals through its dendrites, then sends its output to another neuron via its axon. It is this simplified model of the neuron that the neural network was based on, but they differ in that multiple neurons are simulated at once, accept the inputs, and collectively output a number. Ng caveats that biological understanding of the brain is limited, and neuroscientists continue to discover new things about the brain. The deep neural networks nowadays have been built up from engineering best practices, and any semblance of these to biological models of the brain is speculative. So mimicking current brain models is said to be unlikely to yield raw intelligence.

## Neural Networks Today
Why have deep neural networks exploded in popularity? As datasets were getting larger and larger, it was getting difficult to scale the performance of traditional learning algorithms like logistic and linear regression. Researchers realized that as neural network sizes scaled up, performance increased. For "big data" applications, larger neural networks enabled performance on use cases such as speech recognition, image recognition, natural language parsing, etc. Computing deep neural networks is also why processing, or "compute", specifically GPU processing is in such high demand.

## From Logistic Regressions to a Neurons
Suppose we are trying to predict a category based on numerical data. In logistic regression, the input would be the numerical data and the output would be the category. In a neural network, the output is called an activation, which is a term that comes from neuroscience. The activation would ouput a probability of the input belonging to a category, just like a logistic regression does.

## Talkin' 'bout Architecture (of a Neural Network)
On the opposite ends of the network are the input layer and the output layer. The input layer is the multifeatured data. The output layer is the prediction. In the classification case, it is the probability of the input layer being of a certain category. In between the input and outer layer are "hidden" layers, which each contain multiple neurons. A "hidden" layer is called such because the data in the neurons is hidden, unlike the data in the training set. In a layer, each neuron depends on some combination of the activations from the previous layer. The first layer depends on the input layer. The output layer depends on some combination of activations from the previous layer and outputs a probability. The neural network learns which activations from the previous layer contribute most to each neuron in the next layer. One conception of these hidden layers is that they are the features that the model has created from the previous layer. A neural network model with many hidden layers has essentially "engineered" or "learned" features of features. This is useful because engineered features can predict better than their constituent features can. It also eases the burden of creating/selecting features from the practitioner.

The activations emitting from a layer can be understood as a vector of activations. 

When building a neural network, a decision is made on how many layers the neural network will have as well as how many neurons each layer will contain. These decisions will affect the performance of the model. Tradeoffs will be discussed later in the course.

A multilayer perceptron is another way of describing a multilayer neural network.

## How do neural networks identify a person in a photo?
Given a black-and-white portrait photo of a person, the photo can be understood as an mxn grid representation of pixel values, each taking on a value between 0 and 255. This is the data that will be the neural network's input layer. To reshape the grid into an acceptable format, we vectorize the matrix. Vectorization in this context is an operation that takes the matrix of data and stitches it together columnwise, resulting in a 1-dimensional vector of values that can be fed into a multilayer perceptron. 

As stated before, each layer contains a new set of features that are constructed from those in the previous layer, and it is difficult to know the meaning of each layer's neurons - recall that the practitioner has decided on the number of neurons a layer will have and that the algorithm is learning the best activation to assign to each neuron. To build an intuitive understanding of a neural network, we can create a mental model by imagining what kinds of features each layer is "extracting" from the photo in this case. 

Imaginging that the neural network in this case consists of three hidden layers, the first layer's purpose can be understood as edge detection, where each neuron is detecting the number tiny lines with orientations ranging from $0^\circ$ to $360^\circ$. The original photo might contain thousands of these. The next layer might "learn" to group together these tiny lines to form facial features, such as a nose, ears, eyes, mouth. The final hidden layer might learn to construct faces of different shapes and sizes from these facial features. Based on the faces constructed, the output layer assigns a probability that the picture is of a particular person. As the data is processed in each layer, each layer outputs activations of a higher level feature. The neural network is learning higher level features from the activations of the previous layer - all the practitioner has done is select the number of layers and the number of neurons in each layer.

## Constructing a layer of neurons, terminology
What's actually happening in each neuron? In a classification neural network, each neuron is a linear combination of the previous layer's activations and the weights $\vec{w}$ and $b$ particular to that neuron. The number of weights in a neuron depends on the number of activations from the previous layer. The polynomial combination of the weights is called the pre-activation. The pre-activation is calculated by adding the bias term to the dot product of the neuron's weights and the previous layer's activations $\vec{a}$. The pre-activation is fed into the sigmoid function to produce the neuron's activation. The activation is the input for the next layer.

Other conventions include the input layer being called layer 0, and all successive layers indexed by their respective position relative to the input layer (layer 1, 2, 3...). The output layer is the final layer. All layers that are not layer 0 or the output layer are called "hidden" layers. The function in a neuron that outputs the activation is called the activation function. In classification, the activation function is the sigmoid function.

An element of an activation $\vec{a}$ is also known as a unit. When identifying the activation unit or its constituents $\vec{w}$ and $b$, a superscript denotes the layer that it's in. The subscript denotes the neuron that it belongs to. The input layer can also be denoted as $\vec{a}^{[0]}$. 

### Forward Propagation
Activations of lower-index layers are being fed-forward to higher-index layers, hence the term forward propagation. It is typical to reduce the number of units per layer when progressing through the forward-prop neural network.

## Tensorflow and Keras
Tensorflow is a machine learning tool package that was released by Google. In 2019, Google integrated Keras into Tensorflow and released Tensorflow 2.0. Keras is a framework developed independently by François Chollet that creates a simple, layer-centric interface to Tensorflow. 

``` Python
import numpy as np
import matplotlib.pyplot as plt
import tensorflow as tf
from tensorflow.keras.layers import Dense, Input
from tensorflow.keras import Sequential
from tensorflow.keras.losses import MeanSquaredError, BinaryCrossentropy
from tensorflow.keras.activations import sigmoid
from lab_utils_common import dlc
from lab_neurons_utils import plt_prob_1d, sigmoidnp, plt_linear, plt_logistic
plt.style.use('./deeplearning.mplstyle')
import logging
logging.getLogger("tensorflow").setLevel(logging.ERROR)
tf.autograph.set_verbosity(0)

# training data
X_train = np.array([[1.0], [2.0]], dtype=np.float32)           #(size in 1000 square feet)
Y_train = np.array([[300.0], [500.0]], dtype=np.float32)       #(price in 1000s of dollars)

# this is how to implement a simple linear regression in Keras
linear_layer = tf.keras.layers.Dense(units=1, activation = 'linear', )

# display weights
linear_layer.get_weights() # []
# there are no weights because the model has not been instantiated

# inputs to the linear layer must be 1d vector column, hence the reshape
a1 = linear_layer(X_train[0].reshape(1,1)) # training on just the first example
print(a1) # result is a tensor (array) with shape 1,1 (one entry)

w, b= linear_layer.get_weights() # a single weight and bias are returned because this is a linear model
print(f"w = {w}, b={b}") # these are random values. recall that you can fit an infinite number of lines to a point

# we can set our own weights
set_w = np.array([[200]])
set_b = np.array([100])

# set_weights takes a list of numpy arrays
linear_layer.set_weights([set_w, set_b])
print(linear_layer.get_weights())

# comparing tensorflow output with simple dot product - they should be the same
a1 = linear_layer(X_train[0].reshape(1,1))
print(a1)
alin = np.dot(set_w,X_train[0].reshape(1,1)) + set_b
print(alin)

# now we compare the predictions when the models are trained on all the data
prediction_tf = linear_layer(X_train)
prediction_np = np.dot( X_train, set_w) + set_b

# predictions are identical
plt_linear(X_train, Y_train, prediction_tf, prediction_np)

# below, the code is creating a tensorflow model that contains a logistic layer. tensorflow is often used to create multi-layer models. the Sequential model is a convenient means of constructing these models
# a logistic neuron can be created like so:
model = Sequential(
    [
        tf.keras.layers.Dense(1, input_dim=1,  activation = 'sigmoid', name='L1')
    ]
)

# shows the layers and number of parameters in the model. There is only one layer in this model and that layer has only one unit. The unit has two parameters, w and b
model.summary()

# we can display information about a layer like so:
logistic_layer = model.get_layer('L1') # references the layer we named 'L1'
w,b = logistic_layer.get_weights() # the weights are random
print(w,b)
print(w.shape,b.shape)

# set the weights to known values and validate
set_w = np.array([[2]])
set_b = np.array([-4.5])
# set_weights takes a list of numpy arrays
logistic_layer.set_weights([set_w, set_b])
print(logistic_layer.get_weights())

# get predictions (tensorflow's prediction matches that of the sigmoid function)
a1 = model.predict(X_train[0].reshape(1,1))
print(a1)
alog = sigmoidnp(np.dot(set_w,X_train[0].reshape(1,1)) + set_b)
print(alog)
```
