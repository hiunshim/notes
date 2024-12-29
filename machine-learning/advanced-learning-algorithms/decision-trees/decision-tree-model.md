### Decision Tree
Tree based on features (e.g., pointy vs. floppy ears)
1. how to split?
	1. maximize/minimize "purity" (all cats left node, all dogs right node)
2. when to stop splitting?
	1. when a node is 100% one class
	2. when splitting a node will result in the tree exceeding a maximum depth
	3. when improvements in purity score is below a threshold
	4. when number of examples in a node is below a threshold
### Entropy
Entropy is a measure of impurity. H(p1) where p1 = fraction of examples that are cats.
![[entropy-example.png]]
For a binary classification, where p_0 = 1 - p_1, the entropy H(p_1) is given by:
$$
H(p_1) = -p_1 \log_2(p_1) - (1 - p_1) \log_2(1 - p_1)
$$
The entropy H(S) of a dataset S is calculated as:
$$
H(S) = - \sum_{i=1}^n p_i \log_2(p_i)
$$
#### Choosing a split: Information Gain
Choose the split that results in the most "reduction" (aka information gain) in entropy.
![[information-gain-example.png]]
**Information gain**
$$ H(p_1^{root}) - (w^{left} H(p_1^{left}) + w^{right} H(p_1^{right})) $$
![[information-gain-equation.png]]

#### One hot encoding
Using 1 and 0 for categorial features. 1 if the feature exists, 0 if it doesn't.

#### Continuous features
Just look at information gain and pick the best one. Split it on the continuous value that has the best information gain.

### Tree ensembles
Since decision trees are sensitive based on what feature it was split on, it's good to make multiple decision trees. This is called "ensemble of trees".
#### Random forest
**Bagged decision tree**: given a training set of size *m*, use sampling with replace to create a new training set of size *m* and train a decision tree on the new data set. Repeat is *B* times.
**Randomizing the feature choice**: choose *k* features out of *n* features where *k < n* and *k* typically $\sqrt(n)$.
Combining these two is the random forest algorithm.
#### XGBoost (eXtreme Gradient Boosting)
Most common implementation of decision tree ensembles.
We still sample with replacement (kind of, it assigns different weights to each sample) but instead of creating a new training set of size *m* by picking a sample with $1/m$ probability, make it more likely to pick misclassified examples from previously trained trees (kind of like deliberate practice - practicing hard parts of piano).
- open source implementation of boosted trees
- fast and efficient implementation
- good choice of default splitting criteria and criteria for when to stop splitting
- built-in regularization to prevent overfitting
- high competitive algorithm for machine learning competitions like Kaggle

**Classification**
```python
from xgboost import XGBClassifier
model = XGBClassifer()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
```
**Regression**
```python
from xgboost import XGBRegressor
model = XGBRegressor()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
```

### Decision Trees vs Neural Networks
- Decision Trees and Tree Ensembles
	- works well on tabular (structured) data
	- not recommended for unstructured data (image, audio, text)
	- fast
	- small trees may be human interpretable
- Neural Networks
	- works well on all types of data, including tabular (structured) and unstructured data
	- may be slower than a decision tree
	- works with transfer learning (can carry out pre-training and fine tune)
	- when building a system of multiple models working together, it might be easier to connect multiple neural networks
