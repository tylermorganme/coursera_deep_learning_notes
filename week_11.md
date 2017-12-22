## Classic Deep Nets
- AlexNet
- VGG - 16
- LeNet - 5

## Resnets
- Very deep nets are hard to train because of vanishing and exploding gradients.
- Resnets seek to solve this issue.
- Resnets are built out of a residual block.
- A residual block calculates the non-linear function using the some of the layers linear portion and the activation of a previous layer.
- The connection of the previous activation to the later node is known as a `skip connection`.
- An entire residual block includes both the first activation and the second combined activation. This way every nth layer ends up with with the additional change.
- In theory with a plain neural network the training error should go down infinitely as the number of layers increase. In practice they actually tend to get worse as some point. Resnets on the other hand will continue to see improvements.
- The reason for this is that sometimes it is best for the neural net to perform an identity function (i.e. the input equal the output), but in practice it is sometimes difficult for neural nets to train towards identity functions. By using resnets it is easier for the network to train an identity function. An identity function in the network will neither hurt nor help your results which is why it prevent declining results.
- If you have an output for a residual layer with more units than the initial output you can add zero padding to the initial output or multiply it my a parameter matrix of the correct size to rescale it in which case you also need to train the parameters.

## 1 x 1 convolutions
- In practice this is like using a single neuron that applies a non-linearity to all the channels of an input.
- 1 x 1 consolutions are also known an network in network convolutions.
- While pooling layers will change the width and height of a matrix, 1x1 filters can change the channel dimension.

## Inception Network
- Inception networks will run multiple different convolution and/or pooling operations on a single input and then stack the resulting matrices as the output.
- In practice you need to make sure that adequate padding and stride are used to make sure that all of the resulting matrices have the same dimensions.
- There is often a high computational cost with running inception networks.
- To prevent high computation costs you can scale your inputs using 1x1 convolutions AKA a "bottle neck" layer.
- At this point at appears that using a bottle neck layer within reason does not appear to hurt performance even though it significantly reduces the number of layers.
- When performing pooling as part of inception you can perform the bottle neck after the pool to shrink the number of layers.
- In the additional research paper that tried inception it has side branches that periodically siphon off the intermediate outputs and try to run a Softmax calculation on them to see how well it predicts.
- Inception networks appear to have a regularizing effect as observed in the side branch results.
- The original network was known as GooleNet.
- It is possible to use a inception and resnet at the same time.

TODO: How to tell if you are getting exploding or shrinking gradients.

## Using ConvNets
- Look for open source implementations of well documented networks.
- You often make much faster progress if you take weights that someone else has already developed and use them to perform transfer learning. This is known as freezing the deep layers and you just adding a new softmax layer and train the network.
- Additionally you can just save the outputs of the frozen layers and train a shallow softmax network the saved layers which will save computation time.
- Transfer learning works well when you have a small training set.
- If you have a larger training set you might just try freezing fewer layers of the original network and train later layers.
- If you have a lot of data you might just train the entire network.

## Common Data Augmentation for Computer Vision Problems
- Mirror the data
- Random cropping
  - Make sure to use reasonable large subsection or you might not get identifiable piece of the image.
- Color shifting, distorting each of the color channels.
- Principal Color analysis, PCA, is an advanced way to distort the colors. It keeps the overall tint the same.
- Tips for Competitions:
  - Ensembling is when you train several networks independently and average their outputs. Average they $\hat{y}$'s not their weights.
- Multi-crop as test time. Run the classifier on multiple cropped versions of the test image and average the results.
- Start with published architectures.
- Use open source implementations is possible.
