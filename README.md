# eval-reinforcement-learning

[![Hermes Skill](https://img.shields.io/badge/Hermes-Skill-6C4DF6)](https://github.com/booz974/eval-reinforcement-learning)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Stars](https://img.shields.io/github/stars/booz974/eval-reinforcement-learning?style=social)](https://github.com/booz974/eval-reinforcement-learning)

> **Your AI agent just became an RL engineer.** Load this skill into Hermes, Claude Code, or any compatible agent and it will tell you which algorithm to use, how to design your MDP, and how to deploy, no RL PhD required.

## Why this exists

Reinforcement learning is powerful but intimidating. Most engineers waste weeks picking the wrong algorithm or debugging a poorly designed reward function. This skill encodes the decision-making frameworks that experienced RL engineers use, so your agent can apply them instantly.

## What it does (30-second demo)

```
You: "I have a recommendation system. Users see products, I want to maximize 
      retention over 6 months, not just immediate clicks. Is this RL?"

Agent: ✅ YES, this is RL.
       → Sequential decisions (each recommendation affects future behavior)
       → Long-term reward (6-month retention, not immediate click)
       → Actions alter state (user's preferences evolve)
       
       Algorithm: Contextual Bandit to start, then PPO for full RL.
       Reward: retention_signal - churn_penalty
       Deploy: Shadow mode first (log recommendations, don't act),
               then A/B test 10% traffic.
       
       Full MDP design in references/problem-framing.md
```

## Coverage

| Domain | What your agent will know |
|---|---|
| **Algorithm selection** | Q-Learning, DQN, PPO, SAC, DDPG, which one for your problem |
| **MDP design** | How to translate real business problems into states/actions/rewards |
| **Reward engineering** | Avoiding reward hacking, sparse rewards, shaping patterns |
| **Exploration** | ε-greedy → curiosity → entropy, what to use when |
| **Deployment** | Simulation → shadow → A/B test → production pipeline |
| **Offline RL** | Learning from historical data when you can't explore live |
| **Multi-agent** | MARL architectures, centralized critics, parameter sharing |

## Structure

```
eval-reinforcement-learning/
├── SKILL.md                          ← Entry point: decision tree + algorithm comparison
└── references/
    ├── algorithm-selection.md        ← When not to use each algorithm (as important as when)
    ├── value-methods.md              ← Q-Learning, SARSA, DQN, Double/Dueling DQN
    ├── policy-methods.md             ← REINFORCE → PPO → TRPO → DDPG
    ├── entropy-methods.md            ← SAC, maximum entropy framework
    ├── exploration-strategies.md     ← ε-greedy to curiosity-driven exploration
    ├── problem-framing.md            ← MDP design checklist, spotting RL problems
    ├── reward-engineering.md         ← Reward shaping, avoiding unintended behaviors
    ├── offline-batch-rl.md           ← CQL, IQL, learning without interacting
    ├── model-based-methods.md        ← When to use models vs model-free
    ├── multi-agent-rl.md             ← Centralized/decentralized MARL patterns
    └── deployment-patterns.md        ← Sim → shadow → A/B → gradual rollout
```

## Installation

```bash
# Hermes Agent
git clone https://github.com/booz974/eval-reinforcement-learning.git ~/.hermes/skills/eval-reinforcement-learning

# Claude Code
git clone https://github.com/booz974/eval-reinforcement-learning.git ~/.claude/skills/eval-reinforcement-learning

# Any agent: copy SKILL.md + references/ into your skills directory
```

## Real problems this solves

- **Ad placement**: "Should I show ad A or B right now?" → Contextual bandit. "What sequence of ads maximizes lifetime value?" → Full RL.
- **Dynamic pricing**: Adjust prices in real-time based on demand, inventory, competitor moves → PPO or SAC.
- **Robot control**: Continuous motor commands, need stable learning → PPO, then SAC for sample efficiency.
- **Game AI**: Discrete actions, visual state → DQN with CNN encoder.
- **Healthcare dosing**: Can't explore live, only historical data → Offline RL (CQL, IQL).

## Why not just use ChatGPT?

ChatGPT gives generic advice and hallucinates RL algorithms. This skill encodes structured, verified knowledge that your agent loads deterministically. It won't tell you to use DQN for continuous actions or forget to mention reward clipping.

## License

MIT. Use it, fork it, ship it.

---

⭐ **Star this repo** if you want your agent to stop guessing and start engineering.
