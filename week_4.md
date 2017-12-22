# Deep L-layer Neural Networks
- It's hard to predict in advance how deep a neural network has to be beforehand.
- $L$ will equal the number of layers in a neural network.
- $n^{[l]}$ will be the number of units in layer $l$. The same goes for $a^{[l]}$, $z^{[l]}$, $w^{[l]}$

# Forward Propagation In a Deep Network.
- This works the same as with a two layer network except you have to loop over each layer, feeding in the output of the previous layer. (i.e. there is no vectorized way to do this).

## Hyperparameters
- The learning rate, $\alpha$
- \# of iterations
- \# of hidden layers, $L$
- \# of hidden units, $n^{[i]}
- Choice of activation functions


**Notation**:
- Superscript $[l]$ denotes a quantity associated with the $l^{th}$ layer.
    - Example: $a^{[L]}$ is the $L^{th}$ layer activation. $W^{[L]}$ and $b^{[L]}$ are the $L^{th}$ layer parameters.
- Superscript $(i)$ denotes a quantity associated with the $i^{th}$ example.
    - Example: $x^{(i)}$ is the $i^{th}$ training example.
- Lowerscript $i$ denotes the $i^{th}$ entry of a vector.
    - Example: $a^{[l]}_i$ denotes the $i^{th}$ entry of the $l^{th}$ layer's activations).
