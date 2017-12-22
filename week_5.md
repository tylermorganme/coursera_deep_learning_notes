# Train/Dv/Test Sets
- The training set is what you train your algorithm on then you judge it against the dev (AKA cross-validation set) then finally judge the performance based on the test set.
- Back in the fast a 50/20 or a 60/20/20 split were common.
- Now with very large data sets it is common to have 90%+ training data.
- Basically you're dev and test set just have to be big enough to be representative of the usefulness of your algorithm.
- Make sure that the dev and training sets come from the same distribution.
- It might be okay in some scenarios to not have a test set.
# Bias vs. Variance
-  High bias means underfitting the data.
- High variance means overfitting the data.
- High variance models have low training error and high dev error.
- High bias models have high training error and similar dev error.
- A model that has both will have high training error and even higher dev error.
- High Bias?
  - Yes: Try a bigger network. Train longer. Try more advanced algorithms. Better NN architecture.
  - No: High Variance?
    - Yes: Get more data. Tye regularization. Try a more appropriate architecture.
    - No: done
- There used to be a lot of talk about the trade-off between bias and variance.
- In modern deep learning so long as you can built a bigger network or get more data you can drive down one without hurting the other.

## Regularization
- A good way to deal with high variance problems.
- You typically don't need to regularize for $b$, but you can.
- $\lambda$ is known as a regularization parameter.
- A common use is to perform $L_2$ regularization which in neural networks is added as follows:
$J=\frac{1}{m}\sum_{i=1}^mL(\hat{y},y^{(i)}) + \frac{\lambda}{2m}\sum_{l=1}^{L}||w^{[l]}||^2$
- Regularization tends to make your model be simpler.
- If your regularization parameter is relatively large then your network will become roughly linear.

### Dropout Regularization
- Randomly eliminate nodes on each example for each different training set.
- Inverted Dropout
  - Pick a probability, $keep_prob$, that a node will be dropped.
  - Create a matrix, $d^{[l]}$ of 1's and 0's where the probability of any value being 1 is keep_prob.
  - Element-wise Multiply $d^{[l]}$ by $a^{[l]}$.
  - Divide $a$ by keep_prob. This is known as Inverted Dropout.
- In practice this unit can't rely on any one input because it could be randomly removed.
- It's feasible to vary $keep_prob$ by layer.
- Often you will use a relatively low probability on large layers and maybe even use probability of 1 on small layers. And almost always have a 1 or very high chance on the input layer.
- Dropout is very frequently used in computer vision.
- The cost function of gradient descent is no longer well defined so it's harder to check for convergence of your cost. You can, however, test without dropout then add it.
- You only apply dropout and multiply by $keep_prob$ during training and not when testing.

## Other Regularization Forms
- Adding augmentation (e.g distortions) can help increase regularization.
- Stopping training early when dev set error had minimized can help prevent overfitting.
- Sometimes it can be hard to do both early stopping and optimize the cost function because you are trying to do both at once.

# Speeding Up Training

## Normalizing Training sets
- Normalizing the features create more spherical contours so it will be quicker to optimize.
- You want the values to be close to zero and of similar scale.
- Zero out the mean, $x:= x-\mu$
- Normalize Variance, $x := x/\sigma^2$

## Exploding/Vanish gradients
- With really deep networks your weights and gradients can end up becoming very large or very small deep into the network.
- A way of adjust for this is to multiply the values of your random initialization by $\sqrt{\frac{1}{n^{[l-1]}}}$ for the sigmoid function or tanh function or $\sqrt{\frac{2}{n^{[l-1]}}}$ (AKA He Initialization) or  $\sqrt{\frac{2}{n^{[l-1]}+ n^{[l]}}}$ for ReLU functions.

## Gradient Checking
- You can find an approximate gradient for values by using the formula $\frac{f(\theta+\epsilon)-f(\theta-\epsilon)}{2\epsilon}$ where $\epsilon$ is a very small number.
- Turn all of the $W$ and $b$ parameters into a bit parameter vector $\theta$.
- Turn all of the $dW$ and $db$ gradients into a bit parameter vector.
- Loop over each value parameter and calculate the approximate gradient and then check it against the calculated derivative value.
- If you use a value of $\epsilon$ such as $10^{-7}$ and the difference between the two is less than $10^{-7}$ than they are probably correct. If it is great than $10^{-3}$ you should probably worry.
- You can find the difference using the equation $\frac{||d\theta_{approx}- d\theta||_ 2}{||d\theta _ {approx}||_ 2+||d\theta||_ 2}$
- Don't use gradient check in training just to debug otherwise it make your training slow.
- Remember your regularization when you are doing gradient checking.
- Gradient checking doesn't work with dropout.
  - You can implement it without, gradient check it, then turn on dropout.
-
