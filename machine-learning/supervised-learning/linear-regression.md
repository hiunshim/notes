A model that estimates a linear relationship. The goal is the find a line that best fits the data by minimizing the error between predictions and actual values.

Model:
$$f(x) = wx + b$$
[[cost]] Function:
$$J(w,b) = \frac{1}{2m} \sum\limits_{i = 0}^{m-1} (f_{w,b}(x^{(i)}) - y^{(i)})^2 \tag{1}$$

Objective: minimize J(w, b)
