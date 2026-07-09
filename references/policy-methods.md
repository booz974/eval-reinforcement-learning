# Policy Methods — REINFORCE, A2C, PPO, TRPO, DDPG

Methods that directly learn a policy π(a|s; θ) mapping states to action probabilities (or deterministic actions).

## Why Policy Methods?

Value methods (DQN) require discrete actions and an argmax over Q-values. Policy methods:
- Handle continuous actions natively (output distribution parameters)
- Learn stochastic policies (natural exploration)
- Can learn deterministic policies (DDPG)
- Often more stable with function approximation

## REINFORCE (Vanilla Policy Gradient)

**Mechanism**: After each episode, for each step: increase log-probability of action proportional to the return G_t from that step.

**Update**: `θ ← θ + α · G_t · ∇_θ log π(a_t|s_t; θ)`

**Key insight**: G_t is the "weight" — good actions (high return) get reinforced, bad actions get suppressed.

**Pros**: Simplest policy gradient. ~20 lines of code. Good for prototyping.
**Cons**: Extremely high variance. No sample reuse (on-policy). Slow convergence.

**REINFORCE + Baseline**: Subtract a baseline b(s) from G_t to reduce variance: `θ ← θ + α · (G_t - b(s)) · ∇_θ log π`. b(s) is typically V(s) — the average return from this state. Now you're reinforcing actions that beat average.

## Actor-Critic (A2C / A3C)

**Architecture**: Two networks sharing early layers:
- **Actor** π(a|s; θ): outputs action probabilities
- **Critic** V(s; φ): estimates state value (the baseline)

**Advantage**: A(s,a) = Q(s,a) - V(s). "How much better was this action than average?"

**Update**:
- Actor: `θ ← θ + α · A(s,a) · ∇_θ log π(a|s)`
- Critic: `φ ← φ - β · ∇_φ (A(s,a))²`  (minimize advantage = make better predictions)

**A2C (Advantage Actor-Critic)**: Synchronous. Wait for all parallel envs to finish step, then update.
**A3C (Asynchronous)**: Each worker updates global params independently. No synchronization. Faster but noisier.

## PPO (Proximal Policy Optimization)

**The problem it solves**: Basic policy gradient can take a catastrophically large step that destroys the policy. TRPO fixes this with constrained optimization (KL divergence constraint). PPO achieves the same effect with a simple clip.

**Core idea**: Limit how much the policy can change in one update.

**Clipped objective**:
```
ratio r(θ) = π_new(a|s) / π_old(a|s)
L_CLIP = min(r(θ)·A, clip(r(θ), 1-ε, 1+ε)·A)
```

If advantage A > 0: don't increase probability ratio beyond 1+ε.
If advantage A < 0: don't decrease ratio below 1-ε.

**Why it works**: Clipping creates a "trust region" — the policy can't move too far in one update. This prevents the catastrophic collapses that plague REINFORCE.

**Hyperparameters**: ε = 0.2 (clip range), typically 4-10 epochs per batch of data, minibatch updates.

**Why PPO dominates industry**:
- 1 line of code for the clip (vs TRPO's conjugate gradient)
- Works with RNNs, CNNs, any architecture
- Stable across learning rates and architectures
- Parallelizable (multiple envs)

## TRPO (Trust Region Policy Optimization)

PPO's predecessor. Uses KL divergence constraint `KL(π_old || π_new) ≤ δ` to limit policy change. Solves constrained optimization with conjugate gradient + line search.

**When to use over PPO**: Only if you need theoretical monotonic improvement guarantees. Otherwise PPO is simpler and performs equivalently.

## DDPG (Deep Deterministic Policy Gradient)

**Hybrid approach**: DQN ideas (replay buffer, target networks) + deterministic policy gradient (outputs specific action, not distribution).

**Components**:
- Actor μ(s; θ): outputs deterministic action
- Critic Q(s,a; φ): evaluates actions
- Both have target networks (like DQN)
- Exploration via Ornstein-Uhlenbeck noise added to actions

**When to use**: Continuous control where you want off-policy sample efficiency but don't need stochastic policy. SAC has largely superseded it.

## Implementation Order (Start Here)

1. REINFORCE + baseline → validate your MDP works (~30 min)
2. Switch to PPO → stable continuous control (~1 hour tuning)
3. Try SAC → if sample efficiency is the bottleneck
