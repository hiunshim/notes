**Density estimation**
Probability that a data is seen in a normal dataset

**Anomaly detection use cases**
- fraud detection
- manufacturing
- monitor servers

**Gaussian (normal) distribution**
Gaussian distribution:
$$
p(x ; \mu,\sigma ^2) = \frac{1}{\sqrt{2 \pi \sigma ^2}}\exp^{ - \frac{(x - \mu)^2}{2 \sigma ^2} }
$$
Mean:
$$
\mu_i = \frac{1}{m} \sum_{j=1}^m x_i^{(j)}
$$
Variance:
$$
\sigma_i^2 = \frac{1}{m} \sum_{j=1}^m (x_i^{(j)} - \mu_i)^2
$$
- Bell shape
	- peak at $\mu$ (mean)
	- 1 standard deviation = 68%
	- 2 standard deviations = 95%
Use mean and variance to calculate probability of the new data existing in the training set.

**Anomaly detection vs. Supervised learning**
- anomaly: very small number of positive examples (0-20 is common), large number of negative examples.
	- many different "types" of anomalies. hard for any algorithms to learn from positive examples what the anomalies look like. future anomalies may look nothing like the previous ones - e.g., Fraud
	- ex: fraud, manufacturing (find unseen defects), monitor data center machines
- supervised: large number of positive and negative examples
	- enough positive examples to know what it should look like. future positive examples are likely to be similar to the training set - e.g., Spam
	- ex: spam, manufacturing (known seen defects), weather prediction, disease classification

**Choosing what features to use**
Supervised finds features itself by rescaling them but harder for anomaly detection algorithms to do that.

Non-gaussian feature:
- If the graph is skewed, we can log or square root it since log decrease large values by a lot, and smaller values by a little bit. This will make it more gaussian.

Error analysis:
If there's a data that's an anomaly but p(x) is still very high, we can try to come up with more feature that only exist in the anomaly.
e.g. monitoring computers in a data center
- x1 = memory use
- x2 = number of disk access/sec
- x3 = cpu load
- x4 = network traffic
- NEW FEATURE x5 = cpu load/network traffic (if for some reason that computer has a higher cpu load to network traffic ratio compared to others)

**Selecting threshold**
[[skewed-data]]
$$
\begin{aligned}
	prec&=&\frac{tp}{tp+fp}\\
	rec&=&\frac{tp}{tp+fn}\\
	\\
	F_1&=& \frac{2\cdot prec \cdot rec}{prec + rec}
\end{aligned}
$$
for each of the epsilon value, pick the epsilon that resulted in the lowest F1 score.