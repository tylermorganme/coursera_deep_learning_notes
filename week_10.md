## Convolutional Neural networks
- Convolving a matrix involves combining information from a $n \times n$ input and an $f \times f$ filter.
- If you convolve an $n \times n$ matrix with an $f \times f$ filter you get an $n-f+1 \times n-f+1$.
- The convolution is performed by overlaying the filter over the input at every possible position, taking the sum-product of the filter and the underlying values and assigning the resulting value to the central point where the filter is laid.
- To get around shrinking outputs you can add padding, $p$, to your image by adding a border around your inputs.
- A "valid" convolution has no padding.
- A "same" convolution has padding so that the input is the same size as the output. By definition this means that $p=\frac{f-1}{2}$
- f is usually odd.
- Technically convolution involved mirroring of the filter but by convention neural networks don't mirror the filter. Technically we are performing cross-correlation rather than true convolution.

## Stride
- Stride is the interval of movement of the filter of the inputs.
- In the previous definitions the stride was 1.
- With the addition of stride you get a matrix that is $\lfloor\frac{n+p2-f}{s} + 1\rfloor \times \lfloor\frac{n+p2-f}{s} + 1\rfloor$

## Convolution over volumes.
- You can perform convolutions over 3d data so long as the final dimensions of both filter and volume are the same.
- The resulting convolution follows the same rules as for 2D convolutions for the first two dimensions and have the same third dimensions as the input and filter.
- You can convolute multiple filters, stack the results, and then run additional 3D convolutions on the outputs.

## CNN Dimensions
- Processing a layer of a CNN:
  - Convolute the input with each filter.
  - Process the resulting convolution as a linear function.
  - Process the result of the linear function through a non-linear activation function.
  - Stack the resulting matrices.

- If layer $l$ is a convolutional Layers:
  - $f^{[l]}= filter size$
  - $p^{[l]}= padding$
  - $s^{[l]}= stride$
  - $n_c^{[l]}= numberOfFilters$
  - $n_H^{[l]}=inputHeight$
  - $n_W^{[l]}=inputWidth$
- The dimensions of the convolution will be:
  - Inputs: $n_H^{[l-1]}\times n_W^{[l-1]}\times n_c^{[l-1]}$
  - Output: $n_H^{[l]}\times n_W^{[l]}\times n_c^{[l]}$
  - $n_H^{[l]} = \lfloor \frac{n_H^{[l-1]}+2p^{[l]}-f^{[l]}}{s^{[l]}}\rfloor$
  - $n_W^{[l]} = \lfloor \frac{n_W^{[l-1]}+2p^{[l]}-f^{[l]}}{s^{[l]}}\rfloor$
  - Each filter is $f^{[l]}\times f^{[l]}\times n_c^{[l-1]}$
  - Activations, $a^{[l]}$, have dimensions $n_H^{[l]}\times n_W^{[l]}\times n_c^{[l]}$
    - Vectorized $A^{[l]}$ has dimensions $m \times n_H^{[l]}\times n_W^{[l]}\times n_c^{[l]}$
  - Weight have dimensions $f^{[l]}\times f^{[l]}\times n_c^{[l-1]} \times n_c^{[l]}$
  - The bias, $b^{[l]}$, is an $n_c^{[l]}$ vector.

## Multi-layer CNN
- Processing a multi-layer CNN is like process a single layer except the result of each layer feeds the input of the next layer.
- At the end, it is typical to unroll the final hidden layer's activation matrix into a vector and then feed it to a sigmoid or softmax unit in the output layer.
- The width and height of the output matrices will typically decrease deeper in the network while the number of channels increases.

## Pooling Layers
- Max pooling is similar to convolution but rather than using the convolution operation, the max is found in each filtered area and then that value is attributed to the out.
- Average pooling is the same thing except you take the average over each filtered area.
- Max pooling is used more than average pooling.
- Stride and filter are both hyperparameters used in pooling.
- Pooling outputs follow the same sizing rules and convolution.
- There are no parameters to learn when pooling.
- It is common to follow convolution layers with pooling layers.
- When people talk about the number of layers, $l$, is a neural network they typically exclude pooling layers because they have no learned parameters.

## Fully Connected Layers
- Fully connected layers are the type of layers traditionally used in neural networks where each node of the input connected to each node of the layer, hence they are "fully connected".
- This is accomplished in a CNN by flattening the output of a CNN layer then feeding it as the input to a fully connected layer.
- A pretty common pattern in CNN's is a series of convolution layers followed by pooling layers then a series of fully connected layers and finally a softmax layer.

## Advantages of CNN's
- Parameter sharing. When you used a filter the same parameters are used for each output pixel whereas with a full connected node each node has its own parameters.
- Sparsity of connections. Fully connected networks tend to have parameters that group exponentially.
  - Each output element is only dependent upon the elements that are covered by the filter.
  - This tends to make CNN's less prone to overfitting.
- CNN's are typically good at translational invariance (i.e. you can translate the underlying feature within the image and the CNN will still classify it correctly).
