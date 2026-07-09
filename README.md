# eval-reinforcement-learning

Technical skill for applying reinforcement learning. Decision-focused, implementation-ready.

## What this skill does

Answers the practical questions of RL engineering:

- **Is my problem an RL problem?** → Decision tree in SKILL.md
- **Which algorithm should I use?** → Selection matrix + detailed comparison
- **How do I implement [algorithm]?** → `references/value-methods.md`, `references/policy-methods.md`, `references/entropy-methods.md`
- **How do I design the MDP?** → `references/problem-framing.md`
- **How do I engineer the reward?** → `references/reward-engineering.md`
- **How do I explore effectively?** → `references/exploration-strategies.md`
- **How do I deploy safely?** → `references/deployment-patterns.md`
- **What about offline RL? Multi-agent? Model-based?** → Dedicated reference files

## Structure

```
eval-reinforcement-learning/
├── SKILL.md                          ← Entry point: decision trees, algorithm selector
└── references/
    ├── algorithm-selection.md        ← When NOT to use each algorithm
    ├── value-methods.md              ← Q-Learning, SARSA, DQN, Double DQN, Dueling
    ├── policy-methods.md             ← REINFORCE, A2C/A3C, PPO, TRPO, DDPG
    ├── entropy-methods.md            ← SAC, maximum entropy framework
    ├── exploration-strategies.md     ← ε-greedy to curiosity to entropy
    ├── problem-framing.md            ← Spotting RL problems, MDP design checklist
    ├── reward-engineering.md         ← Shaping, sparse rewards, avoiding hacking
    ├── offline-batch-rl.md           ← CQL, IQL, learning without interaction
    ├── model-based-methods.md        ← Dyna, planning, when models help
    ├── multi-agent-rl.md             ← Centralized/decentralized architectures
    └── deployment-patterns.md        ← Sim → shadow → A/B → rollout → monitoring
```

## Usage (Hermes Agent)

```
/eval-reinforcement-learning                           → Decision trees + algorithm selection
/eval-reinforcement-learning for [my problem]          → Diagnosis + recommendation
/eval-reinforcement-learning algorithm-selection       → Detailed comparison
```

Compatible with any agent that supports the skill format (Hermes Agent, Claude Code, GitHub Copilot CLI, Amp).

## Coverage

| Domain | Algorithms |
|---|---|
| Value methods | Q-Learning, SARSA, DQN, Double DQN, Dueling DQN, Rainbow |
| Policy methods | REINFORCE, A2C, A3C, PPO, TRPO, DDPG |
| Entropy methods | SAC (Soft Actor-Critic) |
| Exploration | ε-greedy, Boltzmann, UCB, Thompson sampling, entropy bonus, curiosity, parameter noise |
| Offline/Batch | CQL, IQL, BCQ, Decision Transformer |
| Model-based | Known models, learned world models, Dyna, MCTS, MPC |
| Multi-agent | Centralized critic, decentralized execution, parameter sharing, communication |
| Deployment | Simulation, shadow mode, A/B testing, gradual rollout, monitoring, safety |

## License

MIT
