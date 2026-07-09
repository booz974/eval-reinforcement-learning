# Problem Framing — Identifying RL Problems & Designing MDPs

## How to Spot an RL Problem

Look for these signals. The more you find, the stronger the RL fit:

### High-Level Signals
1. **Sequential decisions**: You make choices over time, not one-shot.
2. **Strategic**: You optimize for cumulative long-term outcome, not immediate result.
3. **Environment doesn't reset after each decision**: Each action leaves a mark.

### Low-Level Signals (Spot the Components)
1. **Entity (Agent)**: Something making choices. Could be software, a robot, a recommendation system.
2. **Environment**: A bounded context the agent operates in. Behind the interface could be simulation or reality.
3. **State**: What changes when actions happen? What can you observe?
4. **Action**: What can the agent actually do? Keep this limited.
5. **Reward**: Can you quantify success numerically in meaningful units?

### Counter-Indicators (Probably NOT RL)
- Pure prediction/classification → supervised ML
- No active decision-maker → ML or optimization
- Single-step decisions with no state change → bandits or A/B testing
- Can't define a numeric reward → not ready for RL yet

## MDP Design

Translating a real problem into an MDP is the hardest and most important step.

### State Design
- What observations matter for decision-making?
- Keep features minimal — every extra feature expands exploration space
- Include contextual info: time, external conditions, history (or use RNN)
- The agent sees ONLY what you give it. You have oracle knowledge — the agent doesn't.
- Discrete states are easier. If continuous, start with discretization to validate.

### Action Design
- What can the agent control?
- Limit action space — fewer actions = faster learning
- Continuous actions: define bounds (steering [-1, +1], force [0, 100])
- Consider hierarchical actions for complex tasks (high-level: "go to room", low-level: motor control)

### Reward Design (see reward-engineering.md for depth)
- Single scalar per timestep
- In business units: profit, retention, completion, lives saved
- Sparse rewards kill learning. Dense > sparse.
- Watch for unintended incentives: what can the agent do to hack this reward?

### Initial State
- Fixed starting point: easier to learn, less general
- Random starting conditions: more robust, harder to converge
- Start fixed, randomize later

## Simulation

**Golden rule: Build a simulation FIRST. Never start with a live environment.**

Reasons:
- Iteration speed: seconds vs hours/days
- Safety: bad policies can't damage real systems
- Data: can generate infinite training data
- Debugging: perfect state visibility

**Sim-to-real gap**: Your simulation WILL differ from reality. Mitigations:
- Domain randomization: vary simulation parameters (friction, noise, delays)
- Ensemble of simulations: train on multiple different sims
- Gradual transition: start in sim, progressively add real data
- Add noise to observations and actions

## The MDP Design Checklist

```
[ ] States contain everything needed for decisions (Markov property)
[ ] State features are minimal (remove redundant/correlated)
[ ] Actions are limited and well-defined
[ ] Each action has a predictable effect on state
[ ] Reward is a single scalar in meaningful units
[ ] Reward is frequent enough (every timestep ideally)
[ ] Initial state is defined (fixed or distribution)
[ ] Terminal states are clearly identified (if episodic)
[ ] A simulation exists (or is planned) for development
[ ] The reward cannot be trivially hacked
```
