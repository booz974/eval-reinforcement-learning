# Deployment Patterns — From Sim to Production

## The Deployment Pipeline

```
Simulation ──▶ Shadow Mode ──▶ A/B Test ──▶ Gradual Rollout ──▶ Full Production
   │               │              │              │                  │
   └─ Iterate fast  └─ Log only    └─ 1-10% live  └─ 10-50-90%      └─ 100%
                     no impact     compare        increase           monitor
```

### Stage 1: Simulation
- Environment: simulation only
- Purpose: develop agent, tune hyperparameters, validate MDP
- Exit criteria: Policy converges reliably, reward is stable, no obvious reward hacking

### Stage 2: Shadow Mode
- Environment: real, but actions NOT executed
- Agent observes real states, logs what it WOULD do. Compare to production baseline.
- Purpose: validate that agent's decisions are reasonable in real data
- Exit criteria: Decisions pass sanity checks. No obviously dangerous actions.

### Stage 3: A/B Test
- Environment: real, agent actions executed for small % of traffic
- Randomized controlled trial: agent vs baseline
- Purpose: measure actual impact on business metrics
- Exit criteria: Agent statistically significantly outperforms baseline on TRUE business metric, not proxy.

### Stage 4: Gradual Rollout
- Increase traffic % over days/weeks
- Monitor at each increment
- Rollback if metrics degrade

### Stage 5: Full Production
- 100% traffic
- Continuous monitoring
- Periodic retraining

## Monitoring (Production RL)

**What to monitor** (different from ML):

| Metric | Why | Alert If |
|---|---|---|
| Mean reward | Is agent still optimizing? | Significant drop |
| Reward distribution | Averages hide extremes | Sudden change in variance or tails |
| Action distribution | Has policy changed? | Actions not seen in training appear frequently |
| State distribution | Is environment drifting? | States outside training distribution |
| Policy entropy (if stochastic) | Has agent collapsed? | Entropy near zero |
| Reward components (if composite) | Which part is driving change? | One component spikes independently |

## Safety in Production

### Reward Hacking Detection
- Sudden reward spikes without corresponding business metric improvement
- Agent finds degenerate behavior (e.g., pauses game to avoid losing)
- Actions become extremely deterministic or cyclic

### Constraints
- **Hard constraints**: NEVER take action X. Implement as action masking (set Q to -∞ or probability to 0).
- **Soft constraints**: Penalize action Y proportionally. Add to reward.

### Kill Switch
- Automated: if reward drops below threshold for N consecutive evaluations, revert to baseline
- Manual: human operator can disable agent instantly
- Gradual: reduce agent's traffic share to 0% over minutes

## Scaling RL

### Parallel Environments
Simplest scaling: run N copies of environment simultaneously, collect N× more experience per wall-clock second.

```
Single env:   [env] → 1 step/sec
Parallel:     [env₁] [env₂] ... [env_N] → N steps/sec
```

**When**: On-policy algorithms (PPO, A2C). Linear speedup up to CPU core count.

### Distributed Architectures

**IMPALA-style** (Importance Weighted Actor-Learner):
- Many actors generate experience in parallel
- Single learner updates policy from all actors' data
- Actors periodically sync with latest policy
- Handles actor-lag via V-trace correction

**R2D2-style** (Recurrent Replay Distributed DQN):
- Distributed DQN with LSTM for partial observability
- Actors write to shared replay buffer
- Learner samples from buffer, trains, updates actors

### GPU Optimization
- Batch inference: process multiple observations in parallel on GPU
- Model parallelism: split large networks across GPUs
- Mixed precision: float16 for forward pass, float32 for gradients

## Counterfactual Evaluation

**Problem**: How do you evaluate a new policy using old logs, without deploying it?

**Importance Sampling**: Weight old trajectories by π_new(a|s) / π_old(a|s). Unbiased but high variance for long episodes.

**Doubly Robust**: Combines importance sampling + direct value estimation. Lower variance.

**FQE (Fitted Q-Evaluation)**: Learn Q-function of new policy from old data, then estimate value.

**Practical rule**: Use offline evaluation to FILTER candidates (reject clearly bad policies), not to certify (don't trust it for final go/no-go). Online A/B test is the only reliable evaluation.
