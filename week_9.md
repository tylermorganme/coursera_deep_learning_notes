## Error Analysis
- Going through any manually checking erroneous labeling and checking to see if there are common sources of error.
- Deep learning algorithms are quire robust to random errors in the training data set.
  - Check errors that are due to incorrect labels.
  - Check errors that are due to all other causes.
  - If significantly more errors are caused by other things than incorrect labels it's probably not worth fixing the incorrect labels.
- If you decided to fix up the dev set make sure to do the same thing for the test set.
- You really should build the first system quickly then iterate.


## Training with Different Training and Test/Dev Distributions
- It is almost always advisable to test against the type of data that you care about classifying.
- It's common to get a bunch of training data that isn't exactly like the data that you want to test against. In this case you should put some of the actual data you want to test against in the training set but don't muddy the test data with the less than ideal training data.
- If your training and test data come from a different distribution it can be hard to identify true variance issues.
  - To identify this you can create a training-dev set that works just like a dev set except it comes from the same distribution as the training data. This way if you see variance again you know that it truly is variance and not a difference between the distribution of the various sets. If there is a significant difference between the training-dev error and the dev error it is likely a distribution problem.
- The difference between the human-level error and the training set error is the avoidable bias.
- The difference between the training set and training-dev set error is the variance.
- The difference between the training-dev and dev error is the data mismatch.
- The difference between the dev and test set error is the degree of overfitting.

## Transfer Learning
- Pre-training is when you take the network and weights from a previous model and have it classifying something different.
- Fine tuning is when you train the weights of the last layer of a pre-trained network on a new classification problem.
- Sometimes you will take out the output node and replace it with a whole new neural network.
- Transfer learning makes sense when:
  - You have a lot of data from the original model and a small amount of data for the new model you want to train towards.
  - When both classifications have the same input.
  - Low level features from the original classification could be use for learning the second classification.

## Multi-task Learning
- When you have one network simultaneously detect multiple things.
- In the output layer of the network there is a node for each of the potential classifications.
- The difference between multi-task learning and Softmax is that each sample can have multiple classifications.
- Your loss function needs to take into account each classification you are trying to perform.
- When multi-task learning makes sense:
  - Training on a set of tasks that could benefit from having shared lower-level features
  - Usually the amount of data you have for each task is quite similar.
  - Can train a big enough network to do well on all tasks. Multi-task learning typically only works well with large networks compared to using multiple smaller networks.
- Transfer learning is used more commonly than multi-task learning.

# End-to-End Deep Learning
- When a data processing system requires multiple stages that can be replaced with a single neural network.
- Pros:
  - Lets the data speak.
  - Less hand designing of features needed.
- Cons:
  - May need large amounts of data.
  - Excludes potentially useful hand-designed components. Hand-designing can be especially important when you only have small training sets.
  - When you use end-to-end learning.
    - Do you have sufficient data to learn a function of the complexity needed to map x to y?

TODO: Learn more about motion planning.
