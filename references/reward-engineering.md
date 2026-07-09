# Reward Engineering

The reward defines the goal. Get it wrong and your agent will optimize the wrong thing — often in surprising ways.

## Core Principle

**Reward = the true objective, in business units.**

Not accuracy. Not F1. Not click-through rate. The thing your stakeholder actually cares about: profit, retention, completion rate, lives saved.

If you can't express the goal as a numeric scalar, you can't use RL.

## Reward Shaping

**Problem**: Natural reward is too sparse. Agent gets +100 for reaching goal, 0 otherwise. In a 100-step environment, first success is astronomically unlikely.

**Solution**: Add intermediate rewards that guide the agent.

**Rules for shaping**:
1. Don't change the optimal policy. Shaped rewards should only accelerate learning, not redirect it.
2. Potential-based shaping is provably safe: `F(s,s') = γ·Φ(s') - Φ(s)` where Φ is a potential function.
3. Practical shaping (less formal but works): +1 for getting closer, -1 for moving away, +small_reward for subgoals.

**Example**: Robot navigation
- Natural: +100 at goal, 0 otherwise → never learns
- Shaped: +1 for reducing distance to goal, -1 for increasing, +10 for sub-milestones → learns quickly
- The optimal policy (shortest path) is unchanged

## Sparse Rewards — Solutions

| Approach | When to Use | How |
|---|---|---|
| Reward shaping | You can define progress | Add intermediate rewards |
| Hindsight Experience Replay (HER) | Goal-based tasks | Replay failures as successes for "pretend" goals |
| Curiosity | Environment is predictable | Intrinsic reward = prediction error of next state |
| Curriculum learning | Tasks can be ordered by difficulty | Start easy, increase difficulty gradually |
| Demonstration / Imitation | You have example solutions | Pretrain on demonstrations, then RL fine-tune |

## Avoiding Reward Hacking

**Reward hacking**: Agent finds an unintended way to maximize reward.

**Classic examples**:
- Boat racing agent: learns to go in circles collecting respawning power-ups instead of finishing the race
- Robot hand: learns to throw the object to the goal instead of grasping and placing
- Recommendation agent: shows clickbait that maximizes clicks but destroys user trust

**Prevention checklist**:
- [ ] What's the laziest way to get high reward? Test for it.
- [ ] What can the agent break? Constrain or penalize damage.
- [ ] Does the reward align with the TRUE long-term objective?
- [ ] Add a "living penalty" (small negative reward per timestep) to prevent stalling
- [ ] Monitor reward composition, not just total — a spike in one component may indicate hacking

## Reward Clipping

**Practice**: Clip rewards to [-1, +1] or [-10, +10].

**Why**: Prevents gradient explosion, makes hyperparameter tuning easier, improves stability across different problems.

**Cost**: Loses information about reward magnitude. Fine for ranking (which action is better), problematic for precise value estimation.

**Rule of thumb**: Clip during training, use raw rewards for evaluation metrics.
