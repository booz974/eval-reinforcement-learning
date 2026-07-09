# Value Methods — Q-Learning, SARSA, DQN

Methods that learn a value function Q(s,a) or V(s), then derive the policy from it (e.g., argmax over Q-values).

## Q-Learning (Tabular)

**Mechanism**: `Q(s,a) ← Q(s,a) + α[r + γ·max_a' Q(s',a') - Q(s,a)]`

**Key properties**:
- Off-policy: learns optimal policy regardless of behavior
- Guaranteed convergence for tabular case (finite states, visit all pairs infinitely often)
- TD error δ = r + γ·max Q(s',a') - Q(s,a) drives all learning

**Implementation**: Table (dict or array) mapping (state, action) → value. ε-greedy for exploration.

**When to use**: Small discrete problems. Always start here before scaling to deep RL — validates your MDP.

## SARSA

**Mechanism**: `Q(s,a) ← Q(s,a) + α[r + γ·Q(s',a') - Q(s,a)]`

**Key difference from Q-Learning**: Uses the action ACTUALLY taken (a'), not the max. This means SARSA accounts for exploration penalties.

**When to use**: Risky environments where bad exploratory actions have real consequences (e.g., a robot falling off a cliff during exploration — SARSA learns to avoid the cliff edge; Q-Learning learns the optimal path that skirts dangerously close).

## Deep Q-Network (DQN)

**Core idea**: Replace the Q-table with a neural network: Q(s,a; θ). Input = state, output = Q-value per action.

**Three essential components** (DQN fails without any of them):

1. **Experience Replay**: Store (s, a, r, s', done) in fixed-size buffer (~10⁵-10⁶). Sample random minibatches. Breaks temporal correlation and reuses data.

2. **Target Network**: Separate Q-network Q(s,a; θ⁻) for computing TD targets. Updated every C steps by copying: θ⁻ ← θ. Prevents the "moving target" problem.

3. **Reward Clipping**: Clip rewards to [-1, +1]. Stabilizes gradients.

**Loss function**: `L(θ) = E[(r + γ·max_a' Q(s',a'; θ⁻) - Q(s,a; θ))²]`

**DQN Variants**:

| Variant | What it fixes | How |
|---|---|---|
| Double DQN | Overestimation bias | Use online net to SELECT action, target net to EVALUATE |
| Dueling DQN | Wasted capacity on similar actions | Split Q into V(s) + A(s,a) - mean(A) |
| Prioritized Replay | Uniform sampling wastes time | Sample transitions with high TD error more often |
| Rainbow | Combines all improvements | DQN + Double + Dueling + Prioritized + Noisy Nets + Distributional + n-step |

## Hyperparameters That Matter

| Param | Typical Range | Effect |
|---|---|---|
| Learning rate α | 0.0001 - 0.001 | Too high = unstable; too low = slow |
| Discount γ | 0.9 - 0.99 | Higher = more far-sighted |
| ε (exploration) | 1.0 → 0.01 annealed | Start fully random, end mostly greedy |
| Replay buffer size | 10⁵ - 10⁶ | Bigger = more stable, more memory |
| Batch size | 32 - 256 | 32 is standard, 256 for bigger models |
| Target update freq C | 100 - 10000 steps | Trade-off: stability vs speed |

## Common Failure Modes

| Symptom | Likely cause | Fix |
|---|---|---|
| Q-values explode | No target network or reward clipping | Add both |
| Q-values collapse to 0 | Learning rate too high | Reduce α |
| Agent never improves | Not enough exploration | Increase ε, slower annealing |
| Catastrophic forgetting | Replay buffer too small | Increase buffer, lower learning rate |
| Overestimation (Q >> true) | DQN's max bias | Use Double DQN |
