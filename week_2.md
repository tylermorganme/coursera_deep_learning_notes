## Logistic Regression
- For the purpose of the course $X$ and $Y$ and given in a columnar format rather than in rows. Making $X$ a $x\times m$ matrix and $y$ a $1 \times n$ matrix.
- Also unlike in the previous class instead of providing the constant parameter as $\theta_0$ is is provided as a separate parameter. So our logistic regression will use two parameters.
- We use the predictive function $\hat{y}=\sigma(w^\intercal x + b)$.
- $\sigma(z)=\frac{1}{1+e^{-z}}$
- $\hat{y}$ can only take values between 0 and 1.
- The Loss function is $L(\hat{y},y)=-(y\times log(\hat{y})+(1-y) \times log(\hat{1- y}))$
- If y = 1, $L=-log(\hat{y})$
- If y = 0, $L = -log(\hat{1- y})$˛
- The cost function is $J(w,b)=\frac{1}{m}\sum_{i=1}^mL(\hat{y}^{(i)},y^{(i)})$
- The loss function represents a single example. The cost function represents to the total loss of the model.
## Gradient Descent
- We want to find $w,b$ that minimize $J(w,b)$
- $J$ is a convex function.
- Initialize $w$ and $b$ to zero for logistic regression. Sometimes people use random initialization but it is unnecessary.
- Gradient descent relies on the partial derivatives of the cost function as follows:
  - $w:=w-\alpha\frac{dJ(w,b)}{dw}$ this might also sometimes be referred to at $dw$
  - $b:=b-\alpha\frac{dJ(w,b)}{db}$ this might also sometimes be referred to at $db$
- Back propagation works by fining the derivative starting at the output layer then working back in each hidden layer until you reach the input layer.

## Logistic Regression Vectorization
- Vectorization is the process of using matrix mathematics to speed up the performance of operations over large data sets.
- Vectorized code runs significantly faster than using explicit for loops.
- This allows for much faster feedback times.
- The vectorized implementation of logistic regression is `Z = np.dot(w.T, x) + b`
- Gradient descent
  ```
  import math

  def sigmoid(x):
    return 1 / (1 + math.exp(-x))

  Z = np.dot(w.T,X) + b
  A = sigmoid(Z)
  dz = A-Y
  dw = 1/m * np.dot(X, dz.T)
  db = 1/m*np.sum(dz)

  w -= alpha*dw
  b -= alpha*db
  ```
- Need to use a for loop to loop over iterations of gradient descent.
- Better to use row vectors or column vectors instead of rank 1 arrays.
