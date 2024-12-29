loop
1. choose architecture (mode, data, etc.)
2. train model
3. Diagnostics (bias, variance, and error analysis)

**Error analysis**
We can manually examine a sample of the data that the model misclassified in order to identify common traits and trends. We should optimize to catch the most common type. e.g., spam classifier to catch most common type of spams like password stealing over misspelled words.

**Data augmentation**
Modifying an existing training examples to create a new training example. e.g., distort an image

**Transfer learning**
Technique that lets you use data from a different task.
There are 2 options to fine tune the model:
1. option 1: only replace and train the output layer and keep the other layers from a different model
2. option 2: replace the output layer and retrain all parameters
How does it work? How do we take a model that was trained on detecting animals and make it detect digits?
- Neural networks' layers have detects different things such as edges, corners, curves/basic shapes. The output layer predicts with these types.
Summary:
1. download neural network parameters pre-trained on a large dataset with same input type (images, audio, text, etc.) as your application
2. further train (fine tune) the network on your own data

**ML Project Development Cycle**
1. scope project (define project)
2. collect data (define and collect data)
3. train model (training, error analysis & iterative improvement). go back to step 2 based on analysis.
4. deploy in production (deploy, monitor, and maintain system). go back to step 2 or 3 based on results.

**Deployment**
ML model runs in the inference server. Mobile app makes API call to the inference server. Inference returns the result back to the client.
Software engineering (MLOps) is still needed to:
- ensure reliable and efficient predictions
- scaling
- logging
- system monitoring
- model updates
