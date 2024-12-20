Adam: Adaptive Moment estimation
Uses many alpha (learning rate)

Increase alpha if weights and biases are changing in the same direction.
Decrease alpha if weights and biases are oscillating in different directions

To use in code:
```python
mode.compile(optimizer=tf.keras.optimizers.Adam(learning_rate=1e-3), loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True))
```
