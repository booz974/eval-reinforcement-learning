# Multi-Agent RL (MARL)

When multiple agents learn simultaneously in a shared environment.

## When MARL Applies

- Multiple interacting decision-makers (not just one)
- Agents can be cooperative, competitive, or mixed
- Examples: traffic light coordination, trading agents, game AI, fleet management, ad bidding

## Architectures

### Centralized Critic, Decentralized Actors
**How**: During training, critic sees all agents' observations + actions. During execution, each agent acts using only local observations.

**Why**: Best of both worlds. Training leverages global information. Execution is decentralized and scalable.

**When**: Cooperative settings. Agents share a common goal.

### Fully Decentralized
Each agent trains independently, treats others as part of environment.

**Problem**: Environment becomes non-stationary from each agent's perspective (others keep changing their policies).

**When**: Works surprisingly well for simple problems. Fails for complex coordination.

### Centralized Training, Centralized Execution
Single policy controls all agents.

**When**: You control all agents and communication is free. Simplest to implement.

## Key Challenges

| Challenge | Why It Matters | Mitigation |
|---|---|---|
| Non-stationarity | Each agent's environment changes as others learn | Centralized critic, opponent modeling |
| Credit assignment | Which agent contributed to shared reward? | Difference rewards, counterfactual baselines |
| Scalability | Joint action space = exponential in #agents | Parameter sharing, mean-field approximations |
| Coordination | Agents need to coordinate without communication | Communication channels, shared intent |

## Parameter Sharing

**Key trick for scaling MARL**: Share policy parameters across all agents. Each agent gets different observations → different actions, but learns from shared experience.

**When**: Homogeneous agents (same capabilities, similar roles). Dramatically reduces parameters.

**Implementation**: Single policy network, batched across agents. Agent ID or role can be part of observation for differentiation.

## Communication

When agents CAN communicate:
- **Discrete messages**: Agents output a message vector, other agents receive it as input
- **Continuous signals**: Differentiable communication channel, trained end-to-end
- **Attention**: Agents attend to other agents' states/messages

When to add communication: Coordination task where local observations are insufficient.

## Concurrent RL (Special Case)

Not truly multi-agent: N copies of the same agent explore the same environment with different random seeds.

**Key insight from Winder/Dimakopoulou**: Give each agent a different seed that maximizes exploration diversity. Agents share parameters but explore different regions.

**When**: You have millions of customers (each an "agent" in the same environment) and want to learn a global policy that works for all.
