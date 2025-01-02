### Collaborative filtering

**Algorithm**
Calculate loss per feature (user for movie rating), sum the loss to get total cost, run gradient descent.

**Binary labels**
ex: favs, likes, clicks - 1,0
- From regression to binary classification
	- previously for regression:
		- predict $y^{(i,j)}$ as $w^{(j)} \cdot x^{(i)} + b^{(j)}$
	- for binary labels:
		- predict probability of $y^{(i,j)} = 1$ given by $g(w^{(j)} \cdot x^{(i)} + b^{(j)})$ where $g(z)=\dfrac{1}{1 + e^{-z}}$

**Mean normalization**
Useful for cases when a user didn't rate anything. Instead of predicting that this user will like nothing, do the following:
1. calculate the average rating of all users for each movie
2. subtract that mean value from all of the ratings
3. use $w^{(j)} \cdot x^{(i)} + b^{(j)} + \mu^{(i)}$ for the cost function

**Tensorflow**
Auto Diff for derivatives
```pyton
optimizer = keras.optimizers.Adam(learning_rate=1e-1)
iterations = 200
for iter in range(iterations):
	with tf.GradientTape() as tape:
		cost_values = cofiCostFuncV(X,W,b,Ynorm,R, num_users,num_movies,lambda)
	grads = tape.gradient(cost_values, [X,w,b])
	optimizier.apply_gradient(zip(grads, [X,w,b]))
```
**Cost function**
$$
J({\mathbf{x}^{(0)},...,\mathbf{x}^{(n_m-1)},\mathbf{w}^{(0)},b^{(0)},...,\mathbf{w}^{(n_u-1)},b^{(n_u-1)}})= \left[ \frac{1}{2}\sum_{(i,j):r(i,j)=1}(\mathbf{w}^{(j)} \cdot \mathbf{x}^{(i)} + b^{(j)} - y^{(i,j)})^2 \right]
+ \underbrace{\left[
\frac{\lambda}{2}
\sum_{j=0}^{n_u-1}\sum_{k=0}^{n-1}(\mathbf{w}^{(j)}_k)^2
+ \frac{\lambda}{2}\sum_{i=0}^{n_m-1}\sum_{k=0}^{n-1}(\mathbf{x}_k^{(i)})^2
\right]}_{regularization}
\tag{1}
$$
The first summation in (1) is "for all $i$, $j$ where $r(i,j)$ equals $1$" and could be written:

$$
= \left[ \frac{1}{2}\sum_{j=0}^{n_u-1} \sum_{i=0}^{n_m-1}r(i,j)*(\mathbf{w}^{(j)} \cdot \mathbf{x}^{(i)} + b^{(j)} - y^{(i,j)})^2 \right]
+\text{regularization}
$$

**Cold start problem**
A recommendation system is unable to give accurate rating predictions for a new product that no users have rated, and for a new user that has rated few products.

**Summary**
- Each user is represented by a preference vector $w$, and each movie is represented by a feature vector $x$.
- The algorithm predicts ratings by learning these vectors to match the actual ratings.
- Similar users are grouped naturally by learning similar vectors.
- No predefined features (like genres) are needed; it only uses the ratings data to find hidden patterns in user preferences and movie attributes.

### Content-based filtering
**collaborative vs. content-based**
collaborative: recommend items based on ratings of users who gave similar ratings
content-based: recommend items based on features of user and time

**Summary**
- Each user has a preference vector $w$ that shows how much they like predefined features (like action or romance).
- Each movie has a feature vector $x$ that describes its explicit attributes (e.g., action level, romance level).
- A bias $b$ adjusts for users who rate higher or lower than average.
- Ratings are predicted using $w \cdot x+b$, and the algorithm learns how much users like specific features.
- It relies on predefined item features to make recommendations.