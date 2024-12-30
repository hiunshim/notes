Automatically finds data points that are related or similar to each other.

### K-means
1. take random guess where center of each cluster is (cluster centroids)
2. go through data points and see which cluster centroid the data point is closest to. Assign each point to its closest cluster centroid
3. recompute the centroid and move the centroid to the new point based on the assigned data points (average of points assigned to the original cluster centroid)
4. repeat 2, 3

**Cost function for K-means**
"Distortion" function
$$
J(c^{(1)},...c^{(m)},\mu_1,...,\mu_k) = \frac{1}{m} \sum_{i=1}^m ||x^{(i)} - \mu_{c^{(i)}}||^2
$$

**Random initialization**
Choose $K < m$ (obvious since we need less number of clusters than the number of training data). Instead of choosing centroids completely randomly, randomly pick $K$ training examples and set the centroids equal to these $K$ examples.
Run K-means multiple times with random initialization and pick the one with the lowest cost.

**Choosing the number of clusters (K)**
Don't use the "elbow" method. Evaluate K-means based on how well it performs on that later purpose. e.g., compare S,M,L T-shirt sizing vs. XS,S,M,L,XL T-sizing to see the results and evaluate trade-offs like production cost, sales, etc.

**Finding Closest Centroids**
```python
def find_closest_centroids(X, centroids):
    """
    Computes the centroid memberships for every example
    
    Args:
        X (ndarray): (m, n) Input values      
        centroids (ndarray): (K, n) centroids
    
    Returns:
        idx (array_like): (m,) closest centroids
    
    """

    # Set K
    K = centroids.shape[0]

    # You need to return the following variables correctly
    idx = np.zeros(X.shape[0], dtype=int)

    for i in range(X.shape[0]):
        distance = []
        for j in range(K):
            norm_ij = np.linalg.norm(X[i] - centroids[j])
            distance.append(norm_ij)
        idx[i] = np.argmin(distance)
    return idx
```

**Computing New Centroids**
```python
def compute_centroids(X, idx, K):
    """
    Returns the new centroids by computing the means of the 
    data points assigned to each centroid.
    
    Args:
        X (ndarray):   (m, n) Data points
        idx (ndarray): (m,) Array containing index of closest centroid for each 
                       example in X. Concretely, idx[i] contains the index of 
                       the centroid closest to example i
        K (int):       number of centroids
    
    Returns:
        centroids (ndarray): (K, n) New centroids computed
    """
    
    # Useful variables
    m, n = X.shape
    
    # You need to return the following variables correctly
    centroids = np.zeros((K, n))
    
    for k in range(K):
        points = X[idx == k]
        centroids[k] = np.mean(points, axis=0)    
    return centroids
```

**K-Means Algorithm**
```python
def run_kMeans(X, initial_centroids, max_iters=10, plot_progress=False):
    """
    Runs the K-Means algorithm on data matrix X, where each row of X
    is a single example
    """
    
    # Initialize values
    m, n = X.shape
    K = initial_centroids.shape[0]
    centroids = initial_centroids
    previous_centroids = centroids    
    idx = np.zeros(m)
    plt.figure(figsize=(8, 6))

    # Run K-Means
    for i in range(max_iters):
        
        #Output progress
        print("K-Means iteration %d/%d" % (i, max_iters-1))
        
        # For each example in X, assign it to the closest centroid
        idx = find_closest_centroids(X, centroids)
        
        # Optionally plot progress
        if plot_progress:
            plot_progress_kMeans(X, centroids, previous_centroids, idx, K, i)
            previous_centroids = centroids
            
        # Given the memberships, compute new centroids
        centroids = compute_centroids(X, idx, K)
    plt.show() 
    return centroids, idx
```



