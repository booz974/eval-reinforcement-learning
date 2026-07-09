# Algorithm Selection — Detailed Comparison

## Decision Flow

```
Action space?
  ├─ Discrete ──▶ State space size?
  │                ├─ Small (<1000) ──▶ Tabular Q-Learning or SARSA
  │                └─ Large ──▶ DQN (images) or PPO (sensors)
  │
  └─ Continuous ──▶ Need sample efficiency?
                     ├─ Yes ──▶ SAC
                     └─ No ───▶ PPO
```

## When NOT to Use Each Algorithm

| Algorithm | Don't use when... |
|---|---|
| Q-Learning (tabular) | State space too large for table (>10⁴ entries). Use DQN. |
| DQN | Continuous actions. Use PPO or SAC. |
| PPO | You have offline data only. Use batch RL. |
| SAC | Simple discrete problem. Overkill. Use Q-Learning. |
| REINFORCE | Production. Variance too high. Use PPO. |
| A2C/A3C | Single environment. Overhead not worth it. Use PPO. |
| DDPG | Need stability. Use SAC or PPO instead. |
| TRPO | Want simple implementation. Use PPO. |

## Trade-off Dimensions

### Sample Efficiency (interactions needed to learn)
```
Best ←──────────────────────────────→ Worst
SAC    DQN    PPO    A2C    REINFORCE    Q-Learning (tabular)
```

### Wall-Clock Speed (with parallel environments)
```
Best ←──────────────────────────────→ Worst
A3C    PPO    A2C    SAC    DQN    REINFORCE
```

### Implementation Complexity
```
Simplest ←──────────────────────────────→ Hardest
Q-Learning   REINFORCE   DQN   PPO   A2C   SAC   TRPO   DDPG
```

### Stability (likelihood of convergence without tuning)
```
Best ←──────────────────────────────→ Worst
Q-Learning   PPO   SARSA   SAC   DQN   A2C   DDPG   REINFORCE
```

## Choosing Between PPO and SAC

This is the most common decision for continuous control. Here's the tiebreaker:

| Factor | Pick PPO | Pick SAC |
|---|---|---|
| You have parallel environments | ✓ (faster wall-clock) | |
| Sample efficiency critical | | ✓ (off-policy) |
| Need simple code | ✓ (clipping is 1 line) | |
| Need robust exploration | | ✓ (entropy bonus) |
| Non-stationary environment | | ✓ (keeps exploring) |
| On-policy constraint acceptable | ✓ | |
| Want state-of-the-art results | | ✓ (often beats PPO on benchmarks) |

Rule of thumb: **Start with PPO.** Switch to SAC if sample efficiency becomes the bottleneck.
