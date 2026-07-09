---
name: eval-reinforcement-learning
description: "Technical skill for applying reinforcement learning. Use when deciding if RL fits a problem, selecting algorithms (Q-Learning, DQN, PPO, SAC, MARL), engineering rewards, designing MDPs, choosing exploration strategies, or deploying RL to production. Implementation-focused, not academic."
license: MIT
version: "1.0.0"
tags: [reinforcement-learning, rl, deep-rl, decision-making, mlops]
---

# Reinforcement Learning — Technical Skill

Entry point for applying RL. Load this first for decision trees and algorithm selection. Dive into `references/` for implementation details on specific methods.

## Quick Decision: Is This an RL Problem?

```
Can you frame it as sequential decisions? ──No──▶ Not RL. Use supervised ML or heuristics.
    │
   Yes
    │
Does each decision affect future situations? ──No──▶ Use bandits (single-step RL) or A/B testing.
    │
   Yes
    │
Can you define success as a numeric signal? ──No──▶ Define a proxy reward first, then return.
    │
   Yes
    │
Is there an entity making active choices? ──No──▶ Not RL. You need an agent.
    │
   Yes
    ▼
  THIS IS AN RL PROBLEM. Next: design the MDP.
```

## Algorithm Selection (Quick)

| Your situation | Algorithm | Why |
|---|---|---|
| <1000 states, discrete actions, no GPU | Tabular Q-Learning | Guaranteed convergence, zero tuning |
| Images/sensors as input, discrete actions | DQN (+ Double/Dueling) | CNN encodes state, replay buffer stabilizes |
| Continuous actions (robotics, control) | PPO | Stable, simple, industry standard |
| Continuous + need sample efficiency | SAC | Off-policy, entropy bonus keeps exploring |
| Historical data only, can't explore live | Batch RL (CQL, IQL) | Trains from logs, no environment needed |
| Multiple interacting decision-makers | MARL (centralized critic) | Parameter sharing, coordinated exploration |
| Known transition model available | Model-based RL | Sample-efficient planning with known dynamics |
| Just prototyping, need minimal code | REINFORCE + baseline | ~20 lines, surprisingly effective |

## Algorithm Properties Matrix

| Algorithm | Action Space | Sample Eff. | Stability | Tuning Difficulty | Off-Policy |
|---|---|---|---|---|---|
| Q-Learning (tabular) | Discrete | Low | Guaranteed | None | Yes |
| SARSA | Discrete | Low | High | None | No |
| DQN | Discrete | Medium | Medium | Medium | Yes |
| REINFORCE | Both | Low | Low | Low | No |
| A2C/A3C | Both | Medium | Medium | Medium | No |
| PPO | Both | Medium | **High** | Low | No |
| TRPO | Both | Medium | High | High | No |
| DDPG | Continuous | Medium | Low | High | Yes |
| SAC | Both | **High** | High | Medium | Yes |

## How to Use This Skill

- Load the skill → get this decision framework
- Ask about a specific algorithm → I load `references/value-methods.md` or `references/policy-methods.md` etc.
- Describe your problem → I recommend algorithm + MDP design
- Ask for "cheatsheet" → one-page reference
- Ask for "patterns" → reusable implementation patterns for specific situations

## Reference Files (load on demand)

| File | When to load |
|---|---|
| [algorithm-selection.md](references/algorithm-selection.md) | Need detailed comparison, trade-offs, when NOT to use each |
| [value-methods.md](references/value-methods.md) | Q-Learning, SARSA, DQN, Double DQN, Dueling DQN |
| [policy-methods.md](references/policy-methods.md) | REINFORCE, A2C, A3C, PPO, TRPO, DDPG |
| [entropy-methods.md](references/entropy-methods.md) | SAC, maximum entropy framework |
| [problem-framing.md](references/problem-framing.md) | How to spot RL problems, design MDPs, define states/actions/rewards |
| [reward-engineering.md](references/reward-engineering.md) | Reward shaping, sparse rewards, avoiding reward hacking |
| [exploration-strategies.md](references/exploration-strategies.md) | ε-greedy, UCB, Thompson sampling, curiosity, entropy |
| [offline-batch-rl.md](references/offline-batch-rl.md) | Learning from static datasets, CQL, distribution shift |
| [model-based-methods.md](references/model-based-methods.md) | When to use models, planning vs learning |
| [multi-agent-rl.md](references/multi-agent-rl.md) | MARL architectures, centralized vs decentralized |
| [deployment-patterns.md](references/deployment-patterns.md) | Sim-to-real, shadow deployment, A/B testing, monitoring |

## Core Principles

1. **Start simple.** Smallest MDP, simplest algorithm. Complexity only after baseline works.
2. **Simulate first.** Never start with a live environment. Simulation → gradual reality.
3. **Keep cycles short.** If one training run takes >1 hour, simplify the problem.
4. **Reward in business units.** Profit, retention, lives saved — not accuracy or F1.
5. **The agent sees less than you.** Design the MDP from the agent's perspective, not your omniscient view.
