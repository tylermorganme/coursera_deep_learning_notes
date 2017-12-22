# Tuning Process
- For most applications the learning rate is the most important hyperparameter to tune.
- Momentum, $\beta$, the mini-batch size, and the number of hidden units are next in importance.
- Then The numbers of layers and the learning rate decay.
- In practice using the defaults values for $beta1$, $beta2$, and $\epsilon$ is common.
- Try random combinations of parameters instead of trying parameters at intervals.
- Start with a course search where you parameters are randomly chosen from a wide area than do a more narrow search based on promising results from the previous courser search.

## Picking the appropriate scale for hyperparameters
- For some parameters to such a the number of layers and the number of features it is common to randomize from a uniform distribution.
- For parameters like $\alpha$ where you might be looking for values between 0.0001 and 1 it is often better to choose from a log scale. In the examples above this would be accomplished using the code below:
```
r = -4*np.random.rand()
alpha = 10**r
```
This will ensure that each order of magnitude gets sampled roughly uniformly.
- For $\beta$ you also want to choose from a logarithmic distribution.
- If you wanted to test over the scale $\beta = 09...0.999$ you are saying you want to samples from $1-\beta = 0.1 ... 0.0001$. For this reason you use code as below:
```
r = -4*np.random.rand()
beta = 1-10**r
```
## Hyperparameters Tuning in practice
-  You should try and revisit the tuning of your hyperparameters at least once every couple of months to account for new data and potential changes to the system such as server settings.
- When you are tuning a machine learning algorithm often you will either tend a single model or experiment on a bunch of models at once.
  - If you tune a single model you will likely revisit it frequently and manually tune things based on observations.
  - If you have a lot of resources and can run models in parallel you might just set up a bunch of models with random parameters, let them run and come back later to pick the best one.

## Batch Normalization
- The process of normalizing a hidden layer as the input for another hidden layer.
- In practice this is typically done on $z$ values but could also be performed on $a$ values.
- These are the equations for batch normalization:
  - $\mu = \frac{1}{m}\sum_iz^{(i)}$
  - $\sigma_2 = \frac{1}{m}\sum_i(z^{(i)}-\mu)^2$
  - $z_{norm}^{(i)}=\frac{z^{(i)}-\mu}{\sqrt{\sigma^2+\epsilon}}$
  - $\tilde{z}^{(i)} = \gamma z_{norm}^{(i)}+\beta$
- This is performed on each batch.
- The term $\epsilon$ is used to prevent division by zero.
- The terms $\gamma$ and $\beta$ are new parameters that get computer during gradient descent.
- When you perform batch normalization the $b$ term becomes unimportant. This is because batch norm zeros out the mean of the values so there is no point in including an additive constant.
- Covariant shift refers to when the distribution of your X values shifts.
- Covariant shift is important to later layers in a neural network because as the program learns the distribution of the activations of the previous layer can shift. By performing batch normalization you are standardizing the mean and variance, thus to some extent preventing covariant shift. The effect of this is to prevent your later laters from having to constantly shift.
- There is a slight regularization effect from using batch normalization. Using bigger batches will reduce the regularizing effect.
- In practice it is common to determine the average and standard deviation between batches using an exponentially weighted average during test time.

## Softmax Regression
- Softmax is a method of running neural networks with non-binary outputs.
- $c$ is the number of classes.
- Each node of the output layer will predict the probability that it falls within a different class.
- You computer the neural network as normal but use a softmax activation function for the final layer.
- The softmax function is:
  - $t = e^{(z^{[l]})}$
  - $a^{[l]} = \frac{t}{\sum_{j=1}^{n^{[L]}}t_i}$
- The output of the softmax function is a $c\times1$ vector of probabilities.
- The loss function for softmax is $L(\hat{y}, y) = -\sum_{j=1}^cy_jlog(\hat{y}_j)$
- In effect the loss function tries to make whatever the probability corresponding to the correct classification is as high as possible.
- This is a form of maximum likeliness estimation in statistics.
- The cost function is $J=\frac{1}{m}\sum_{i=1}^mL(\hat{y}, y)$
- Y and $\hat{Y}$ are $c\times m$ dimensional matrices in the vectorized versions. They are what is known as "one hot" encoding of the values where each Y is a series of zeros with a single 1 value for corresponding to the correct encoding.
- The one partial derivative for softmax regression is $dz^{L}=\hat{y}-y$
# Deep Learning Frameworks
- Caffe/Caffe2
- CNTK
- DL4J
- Keras
- Lasagne
- mxnet
- PaddlePaddle
- TensorFlow
- Theano
- Torch

- Important Criteria
  - Ease of programming (development and deployment)
  - Running Speed
  - Governance
