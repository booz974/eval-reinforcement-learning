# Entropy Methods — SAC

## The Problem

Standard RL converges to a single deterministic optimal policy. But:
- That policy may be brittle (fails on unseen states)
- It stops exploring once it thinks it's optimal
- It may have missed better strategies due to premature convergence

## Maximum Entropy RL

**Core idea**: Maximize BOTH reward AND policy entropy.

**Objective**: `J(π) = E[∑ γᵗ (r(sₜ,aₜ) + α·H(π(·|sₜ)))]`

Where H(π(·|s)) = -∑ π(a|s) log π(a|s) is the entropy. Higher entropy = more random, more diverse actions.

**Why it works**: The agent is literally rewarded for being uncertain about which action to take. This forces it to keep exploring AND find a robust policy that handles multiple situations well.

## SAC (Soft Actor-Critic)

**Three networks**:
- **Actor** π(a|s): stochastic policy outputting mean + std of Gaussian
- **Two Critics** Q₁, Q₂: use min(Q₁, Q₂) to reduce overestimation (like Double DQN)
- **Target critics**: Polyak-averaged copies (not hard-copied like DQN)

**Key features**:
- Off-policy (uses replay buffer → sample efficient)
- Entropy bonus in both policy AND value updates
- Automatic temperature tuning: α can be learned to maintain target entropy
- Works for discrete AND continuous actions

**Temperature α**: Controls exploration vs exploitation trade-off.
- α → 0: SAC becomes standard RL (no entropy bonus)
- α → ∞: Agent acts randomly
- Auto-tuning: `α ← α - β·∇_α (log π(a|s) + target_entropy)` — adjusts to maintain desired randomness

## SAC vs PPO — When to Switch

```
Start with PPO (simpler, stable, parallelizable)

Is sample efficiency the bottleneck?
  └─ Yes ──▶ Switch to SAC

Is the environment non-stationary?
  └─ Yes ──▶ SAC (entropy keeps adapting)

Do you need deterministic behavior at test time?
  └─ Yes ──▶ SAC can run deterministically (use mean, not sample)

Is wall-clock speed the priority?
  └─ Yes ──▶ Stay with PPO (parallel environments win)
```

## Implementation Notes

- SAC is more sensitive to reward scaling than PPO. Normalize rewards.
- The entropy coefficient α is the most important hyperparameter. Auto-tune it.
- Two Q-networks + target networks + actor = 5 networks total. More moving parts than PPO.
- Typical architecture: 2-3 hidden layers of 256 units, ReLU, for both actor and critics.
