# Offline / Batch RL

Learning optimal policies from a fixed dataset — no interaction with the environment.

## When You Need This

- Can't explore safely (healthcare, autonomous driving)
- Historical data exists but live system is too expensive/slow
- Environment is a real production system you can't experiment on
- Want to evaluate a new policy on old logs before deployment

## Core Challenge: Distribution Shift

The training data was collected by behavior policy π_b. You're learning a different policy π.

**Problem**: π wants to take actions that π_b rarely took. Q-values for those actions are poorly estimated → agent confidently chooses bad actions because Q overestimates unseen actions.

## Key Algorithms

### CQL (Conservative Q-Learning)
**Idea**: Penalize Q-values for unseen actions. Be conservative.

Adds a penalty term: minimize Q-values for actions NOT in the dataset, while maximizing for actions that ARE.

**When**: Current state-of-the-art for offline RL. Discrete or continuous.

### IQL (Implicit Q-Learning)
**Idea**: Avoid querying out-of-distribution actions entirely. Use expectile regression — learn the upper expectile of the value distribution without ever asking "what if I took action X."

**When**: Simpler than CQL, often more stable. Good for continuous control.

### BCQ (Batch-Constrained Q-Learning)
**Idea**: Constrain the policy to only take actions that appear in the dataset (within some threshold). Generative model learns which actions are "allowed."

### Decision Transformer
**Idea**: Treat RL as a sequence modeling problem. Feed (return-to-go, state, action) sequences into a transformer. Condition on desired return to generate actions.

**When**: You want to specify desired performance level. Good for goal-conditioned problems.

## Practical Guidance

**Offline RL IS supervised learning on MDP data.** But standard supervised learning fails because:
- Actions affect future states (not i.i.d.)
- The policy being learned differs from the data collection policy

**Critical practice**: Hold out a validation set of trajectories. Evaluate via importance sampling or FQE (Fitted Q-Evaluation). Never trust offline evaluation blindly — the only real test is online.

**When offline RL fails**:
- Dataset doesn't cover good actions → no algorithm can recover
- Dataset is too narrow (single policy) → severe distribution shift
- Problem has long horizon → errors compound

**Workaround**: Offline pretraining + online fine-tuning. Train on historical data, then deploy with small exploration (ε-greedy with low ε) to fill gaps.
