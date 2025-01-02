**Return**
$R_1 + \gamma(R_2) + \gamma^{2}(R_3) + ...$
$\gamma$  typically close to one (e.g., 0.9, 0.99, 0.999)
You can see that the total return will reduce as it takes more steps (you can think of finance where money decreases over time due to inflation).

**Goal**
Find a policy $\pi$ that tells you what action $a = \pi(s)$ to take in ever state $s$ so as to maximize the return.
e.g. where should the robot go?  \[100, 0, 0, robot, 0, 40]

**Terms**

|                          | Mars rover                                  | Helicopter                                  | Chess                                       |
| ------------------------ | ------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| states                   | 6 states \[100,0,0,0,0,40]                  | position of helicopter                      | pieces on board                             |
| actions                  | left, right                                 | how to move control stick                   | possible move                               |
| rewards                  | 100, 0, 40                                  | +1, -1000                                   | +1, 0, -1                                   |
| discount factor $\gamma$ | 0.5                                         | 0.99                                        | 0.999                                       |
| return                   | $R_1 + \gamma(R_2) + \gamma^{2}(R_3) + ...$ | $R_1 + \gamma(R_2) + \gamma^{2}(R_3) + ...$ | $R_1 + \gamma(R_2) + \gamma^{2}(R_3) + ...$ |
| policy $\pi$             | Find $\pi(s)=a$                             | Find $\pi(s)=a$                             | Find $\pi(s)=a$                             |

**Markov Decision Process (MDP)**
The term Markov in the MDP or Markov decision process refers to that the future only depends on the current state and not on anything that might have occurred prior to getting to the current state. In other words, in a Markov decision process, the future depends only on where you are now, not on how you got here.

**Bellman Equation**
Deterministic (transition probability 1):
$$
Q(s, a) = R(s) + \gamma \max_{a'} Q(s', a')
$$
Stochastic:
$$
Q(s, a) = R(s, a) + \gamma \sum_{s'} P(s' \mid s, a) \max_{a'} Q(s', a')
$$

**$\epsilon$-greedy policy
- option 1
	- pick action $a$ that maximizes $Q(s,a)$
- option 2 ($\epsilon$-greedy policy $\epsilon=0.05$)
	- with probability 0.95, pick action $a$ that maximizes $Q(s,a)$. Greedy, "Exploitation"
	- with probability 0.05, pick action $a$ randomly. This is called "Exploration" and is better to do than option 1.
	- starting $\epsilon$ from a high value and gradually decreasing is also a valid strategy.
