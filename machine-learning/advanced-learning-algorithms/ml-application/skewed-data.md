**Rare disease classification**
Lets say we got 1% error, 99% correct diagnoses.
If only 0.5% of patients actually have the disease, it's difficult to know if that error rate is actually good.

In this case, instead of using classification error, we can use precision/recall.

| predicted/actual | 1                   | 0                  |
| ---------------- | ------------------- | ------------------ |
| 1                | 15 (true positive)  | 5 (false positive) |
| 0                | 10 (false negative) | 70 (true negative) |
**Precision**: of all patients where we predicted as having the disease, what fraction actually have the rare disease?
True positives/number of predicted positive = 
true positives/(true positives + false positives) = 
15 / (15 + 5) = 0.75
- High precision means that the algorithm is like an asian parent and says the patient has the disease only if they're very certain. It might miss out on a lot of patients that actually have the disease.

**Recall**: of all patients that actually have the rare disease, what fraction did we correctly detect as having it?
true positives/actual positives = 
true positives/(true positives + false negatives) = 
15 / (15 + 10) = 0.6
- High recall means that the algorithm is like grandparents and often predicts that the patient has the rare disease when they don't. It catches most of the patients that actually have the rare disease.

**Precision and Recall Trade-offs**
precision = true positive / predicted positive
recall = true positive / actual positive

If we increase the threshold of the model to give a prediction (e.g., only predict 1/0 if the probability is 70%), then we're decreasing the number of false positives. So this will increase precision. But decreasing the number of false positives also means that we're increasing the number of false negatives since the model will miss identifying the rare disease in some patients.

We would want high precision when the treatment is risky or costly. But if the treatment is idempotent and cheap, we can spam it so we'll just lower the precision.

**F1 score**
How to compare precision/recall numbers?
1. NOT GOOD - Average = (P + R) / 2
2. VERY GOOD - F1 Score = 2 * (PR) / (P + R)
F1 Score Formula - AKA "Harmonic Mean":
$$
F_1 = \frac{1}{\frac{1}{2} \left( \frac{1}{\text{Precision}} + \frac{1}{\text{Recall}} \right)}
$$
$$
F_1 = 2\cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}
$$
$$
\begin{aligned}
   prec&=&\frac{tp}{tp+fp}\\
   rec&=&\frac{tp}{tp+fn},
\end{aligned}
$$

|             | Precision | Recall | Average | F1 Score |
| ----------- | --------- | ------ | ------- | -------- |
| Algorithm 1 | 0.5       | 0.4    | 0.45    | 0.444    |
| Algorithm 2 | 0.7       | 0.1    | 0.4     | 0.175    |
| Algorithm 3 | 0.02      | 1.0    | 0.501   | 0.0392   |
