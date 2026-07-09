# Exploration Strategies

How the agent discovers what works. The exploration-exploitation trade-off is the core challenge of RL.

## The Exploration-Exploitation Dilemma

- **Exploit**: Choose the best-known action → maximize short-term reward
- **Explore**: Try something new → potentially discover a better strategy

Pure exploitation misses better strategies. Pure exploration never uses what it learned.

## Strategy Progression (Simple → Advanced)

### 1. ε-Greedy
```
With probability ε: random action
With probability 1-ε: best-known action
```
**Tuning**: Start ε=1.0, anneal to ε=0.01-0.05. Anneal too fast → premature convergence. Too slow → wastes time.
**When**: Default starting point. Works for most discrete problems.
**Limitation**: Explores randomly, even in states where it already knows the answer.

### 2. Boltzmann / Softmax Exploration
```
P(a|s) = exp(Q(s,a) / τ) / Σ exp(Q(s,a') / τ)
```
τ (temperature) controls randomness. High τ → uniform. Low τ → greedy.
**When**: Better than ε-greedy for problems where some "wrong" actions are terrible and others are okay.
**Limitation**: Still doesn't direct exploration toward uncertainty.

### 3. UCB (Upper Confidence Bound)
```
a = argmax [Q(s,a) + c · √(log N(s) / N(s,a))]
```
Bonus for rarely-tried actions. Balances estimated value + uncertainty bonus.
**When**: Tabular problems. Theoretically optimal regret bounds.
**Limitation**: Doesn't scale to deep RL (needs visit counts).

### 4. Thompson Sampling
Sample Q-values from posterior distribution, act greedily on sample.
**When**: Bayesian approach available. Strong in bandit problems.

### 5. Entropy Regularization (SAC)
```
reward' = reward + α · H(π(·|s))
```
**When**: Deep RL, continuous control. The agent is paid to stay uncertain.
**Why it's different**: Not a separate exploration mechanism — it's baked into the objective. The policy NEVER stops exploring (unless α → 0).

### 6. Curiosity-Driven Exploration
```
Intrinsic reward = prediction_error(next_state | state, action)
```
Train a forward dynamics model. Reward the agent for visiting states it can't predict.
**When**: Sparse reward environments where even random exploration rarely finds reward.
**Limitation**: "Noisy TV problem" — agent gets fascinated by unpredictable noise (static on a TV). Solution: random network distillation (RND).

### 7. Parameter Noise
Add noise to network WEIGHTS, not actions. More consistent exploration than action noise.
**When**: DDPG, DQN. Better than action noise for some domains.

## Strategy Selection

| Situation | Strategy |
|---|---|
| Discrete, small state space | ε-greedy with annealing |
| Discrete, want optimal exploration | UCB (tabular) |
| Continuous, simple | Gaussian action noise |
| Continuous, production | Entropy bonus (SAC) |
| Sparse rewards, no progress | Curiosity / HER |
| Deep RL, DDPG specifically | Parameter noise |

## Exploration Pitfalls

| Problem | Symptom | Fix |
|---|---|---|
| Too little exploration | Agent converges to suboptimal policy fast | Increase ε, slow annealing |
| Too much exploration | Agent never stops being random | Faster annealing, lower final ε |
| Premature convergence | Policy entropy drops to zero early | Entropy bonus, higher α in SAC |
| Noisy TV (curiosity fails) | Agent stares at random noise | Random Network Distillation |
