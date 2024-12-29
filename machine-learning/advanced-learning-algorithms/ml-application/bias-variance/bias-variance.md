High bias
- Underfit
- J_train is high, J_cv is high
High variance
- Overfit
- J_train is low, J_cv is high (much higher than J_train)
It's possible to have both high bias and high variance
- J_train will be high, and J_cv will be much higher
- Overfit in some parts, underfit in other
- Typically only high bias or only high variance
Just right
- J_train is low, J_cv is low

As degree of polynomial goes up, J_train tends to go down and J_cv tends to go up.

J_cv (cross validation error) will go up in either cases (high bias and high variance). It will only be low when the model fits well.
### How regularization affects bias and variance
[[regularization]]
- Large ƛ = high bias (underfit)
- Small ƛ = high variance (overfit)
- Intermediate ƛ = fits right
Since high bias or high variance will have high J_cv, we can use cross-validation to choose the right regularization parameter.

### Establish a baseline level for model performance
If we have a human performance (baseline) data, training error, and cross validation error, we can guess if the model has high bias or high variance
- (baseline - training error) is large = high bias
- (training error - cv error) is large = high variance

### Learning curves
(x = training set size)
- As x increases, J_train will tend to increase and flatten out. J_cv will tend to decrease and flatten out. It becomes hard to fit more data accurately in training, but will generalize better.
- J_cv will be higher than J_train because the model is fit on the training data.

When the model has high bias, getting more training data won't improve the performance.
![[high-bias.png]]

When the model has high variance, getting more training data will help the model a lot.
![[high-variance.png]]

### Debugging an algorithm
1. Get more training examples
	2. fixes overfitting (high variance)
2. Try smaller sets of features
	2. fixes overfitting (high variance)
3. Try getting additional features
	1. fixes underfitting (high bias)
4. Try adding polynomial features
	1. fixes underfitting (high bias)
5. Try decreasing regularization parameter
	1. fixes underfitting (high bias)
	2. more emphasis on the training set
6. Try increasing regularization parameter
	1. fixes overfitting (high variance)
	2. become smoother as less emphasis is put on the training set

To fix a high bias problem, you can:
- try adding polynomial features
- try getting additional features
- try decreasing the regularization parameter

To fix a high variance problem, you can:
- try increasing the regularization parameter
- try smaller sets of features
- get more training examples
### Neural networks and bias variance
Large neural networks are low bias machines
![[neural-networks.png]]

Usually, large neural networks with well-chosen regularization will do better than a smaller one (almost never hurts to go bigger, it just costs more money).