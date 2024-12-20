---
aliases:
  - Supervised Learning
---
[[linear-regression]] provides the model (y = wx + b) and defines the problem of predicting outcomes. This is used when we want a ranging value as the output such as housing price prediction. The Loss Function measures the error for a single prediction while the [[cost]] Function aggregates this loss over the entire dataset. The [[gradient]] of the cost function guides the model on how to adjust its parameters to reduce error. [[gradient-descent]] uses the gradient to iteratively minimize the cost function and find the optimal parameters for the linear regression model.

Loss Function measures individual errors. Cost Function calculates the overall error (sum of losses). Linear Regression defines the prediction model. Gradient provides the direction to reduce the error. Gradient Descent is a process to optimize the parameters to minimize the cost.

Warning:
Multiple linear regression is NOT multivariate regression