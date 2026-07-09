# Model-Based Methods

Use a model of the environment to plan ahead, rather than learning purely from trial and error.

## When to Use

| Situation | Use Model-Based? |
|---|---|
| You know the environment rules (game, physics, constraints) | Yes — encode them directly |
| Environment is expensive/dangerous to interact with | Yes — learn a model from limited data |
| Sample efficiency is critical | Yes — models reduce interactions needed |
| Environment is complex with unknown dynamics | No — model-free is safer |
| Model errors could be catastrophic | No — model-free doesn't compound model errors |

## Two Approaches

### 1. Known Model (Planning)
You have the transition function P(s'|s,a) and reward function R(s,a).

**Methods**:
- Dynamic Programming: value iteration, policy iteration (Chapter 2 style)
- Monte Carlo Tree Search (MCTS): sample-based planning used by AlphaGo
- Model Predictive Control (MPC): plan N steps ahead, execute first action, replan

**When**: Board games, physics simulation, known constraints. Fast and sample-efficient.

### 2. Learned Model
Learn P(s'|s,a) and R(s,a) from experience, then plan with the learned model.

**Architecture**: World model (predicts next state + reward) + Planner (uses model to choose actions).

**Challenge**: Model errors compound. If your model predicts slightly wrong, planning 10 steps ahead is garbage.

**Mitigations**:
- Short planning horizons
- Ensemble of models (uncertainty-aware planning)
- Use model for policy optimization, not direct planning (Dyna-style)
- Combine model-based planning with model-free learning

## Dyna Architecture

Classic integration: learn model from experience, use model to generate simulated experience, train model-free agent on real + simulated data.

```
Loop:
  1. Act in real environment, get (s,a,r,s')
  2. Update model: learn P(s'|s,a) and R(s,a)
  3. Update policy on real experience
  4. Generate N simulated transitions from model
  5. Update policy on simulated experience
```

**Benefit**: N× more updates per real interaction. But model errors propagate.

## When to Choose Model-Based

Model-based is NOT the default. Use model-free (PPO/SAC) unless:
- You genuinely know the environment dynamics
- Sample efficiency is the dominant constraint
- You have a way to bound model errors
