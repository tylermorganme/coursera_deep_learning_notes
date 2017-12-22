# Neural Networks
- Square bracket super scripts refer to layers. Round brackets refer to training examples.
- The inputs to the neural network go into the input layer.
- Each input is one of the features of a single example, $x^{(i)}$
- The middle layers are the hidden layers.
- The final layer is the output layer.
- Sometimes the input layer will be referred to as $a^{[0]}$.
- When referring to the a neural network as being an "n-layer" network you don't include the input layer.
- Layers have parameters $w$ and $b$ associated with them. $w$ has the dimensions $numUnits \times numInputs$ and $b$ has the dimensions $numUnits \times 1$. Where $numUnits$ is the number of units in the current layer and $numInputs$ is the number of units in the previous layer.
- Each layer, $l$ is made up of multiple units, $i$.Each unit takes all of the inputs and calculates an activation. The activation of a particular unit is label as $a_i^{[l]}$. The activation of a unit layer on a training example is $a^{[l](m)}$.
- The activation, $a^{[l](i)}$, is defined as $\sigma(z^{[l](i)})$ where $z^{[l](i)} = W^{[l]}x^{(i)}+b^{[l]}$.
- The activations of each layer become the inputs for the next layer.
- The final estimate is referred to as $\hat{y}$

## Vectorizing Multiple Inputs
- To calculate the activations for an entire training set you can use the following functions.
  - $Z^{[n]}=W^{[n]}X+b^{[n]}$  for $l=1$
  - $A^{[n]}=\sigma(Z^{[n]})$  for $l=1$
  - $Z^{n}=W^{[n]}+A^{[n-1]}+b^{[n]}$ for $l\ !=1$
  - $A^{[n]}=\sigma(Z^{[n]})$ for $l\ !=1$

- Alternatively is you refer to $X$ as $A^{[0]}$ you can simply use the later two equations.

- $W$ is a matrix where each row is a neuron and each column is a parameter corresponding to an input.
- $X$ is a matrix  where each column is an example as each row is a feature.
- $Z$ and $A$ are matrices where each column is a training example and each row is a neuron.

## Activation Functions
- Using a hyperbolic tangent function, AKA tanh function where $tanh(z)=\frac{e^z-e^{-z}}{e^z+e^{-z}}$ is almost always better than using the sigmoid function. Technically this is just a shifted version of the sigmoid function that is vertically centered on zero. It takes on values between -1 and 1.
- It is typically better to use the sigmoid function for the final layer because it will give an answer between 0 and 1.
- Another popular option is the ReLU function $a$ where $a=max(0,z)$
- As a general rule of thumb, if your output need to be in values from 0 to 1 than the sigmoid function is a good option, for all other purposes the ReLU function tends to be a better options.
- A leaky ReLU tends to work better than a ReLu function, but the ReLU activation function is used much more frequently. A regular ReLU is zero when Z < 0 but a leaky ReLU has a gradual positive slope when Z < 0. A leaky ReLU function would have a formula such as $g(z) = max(0.01\times z,z)$.

## Why do you need non-linear activation activation function?
- Using a linear activation function will results in a model that is no better than linear regression.
- You could use a linear activation function in your output layer because you want a real number that represent a value such as for a house.
## Derivation of Activation Functions
- The derivate of the $sigmoid$ function is $\sigma(z)(1-\sigma(z))$
- The derivative of the $tanh$ function is $1-(tanh(z))^2$
- The derivative of the ReLU function is:
  - 0 is z < 0
  - 1 if z > 0
  - undefined is z =
  - It's not 100% mathematically correct, but you can just set the gradient to 1 is z >=0 to avoid the undefined
- The derivative of the leaky ReLU function is the same as that of the regular ReLU function except when z < 0 in which cause it is equal to the multiplicative constant associated with z (i.e. 0.01 in the example above).
## Gradient Descent for Neural Networks
- For a two layer neural network
  - $dz^{[2]} = a^{[2]}-y$ *
  - $dW^{[2]} = dz^{[2]}a^{[1]}$
  - $db^{[2]} = dz^{[2]}$
  - $dz^{[1]} = W^{[2]\intercal}dz^{[2]}\times g^{[1]\prime}z^{[1]}$
  - $dW^{[1]} = dz^{[1]}x^{\intercal}$
  - $db^{[1]} = dz^{[1]}$

* NOTE: This is for binary classification. Other types of classification will require different gradients.

## Gradient Descent Vectorized
- $dZ^{[2]} = A^{[2]}-Y$
- $dW^{[2]} = \frac{1}{m}dZ^{[2]}A^{[1]\intercal}$
- $db^{[2]}=\frac{1}{m}np.sum(dZ^{[2]}, axis = 1, keepdims = True)$
- $dZ^{[1]}=W^{[2]\intercal}dZ^{[2]}\times g^{[1]\prime}Z^{[1]}$
- $dW^{[1]} = \frac{1}{m}dZ^{[1]}X^{\intercal}$
- $db^{[1]}=\frac{1}{m}np.sum(dZ^{[1]}, axis = 1, keepdims = True)$

## Randomly Initializing Parameters
- Setting variables initially to zero then all of your hidden units will compute the exact same functions.
- Instead you should set them to random numbers (i.e. `np.random.randn((2,2))*0.01`).
- You can initialize $b$ to zeros (i.e. `np.zero((2,1))`).
- It's best to use small weights because calculating the gradients of large values of $w$ is very computationally slow.

TODO: Learn more about Generative Adversarial Networks
