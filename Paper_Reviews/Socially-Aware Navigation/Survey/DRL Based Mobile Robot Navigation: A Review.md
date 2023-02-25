# DRL Based Mobile Robot Navigation: A Review

Kai Zhu, Tao Zhang

2021

---

## Key Ideas:
### RL
- agent: any decision maker
- environment: everything outside the agent

- Interaction process btw the agent and environment can be modeled as an MDP
  - S: state of the environemnt
  - A: action taken by the agent
  - R: reward value obtained
  - P: state transition probability

- return (cumulative reward) ![image](https://user-images.githubusercontent.com/83327791/221380738-7c3b7a79-14c9-40cf-9896-f0cb28b8a9ca.png)
- value function ![image](https://user-images.githubusercontent.com/83327791/221380783-706e58b5-c8f5-440d-a497-4afbad890c24.png)
  - recursive form (Bellman Equation): ![image](https://user-images.githubusercontent.com/83327791/221380816-b606d9f1-d0c6-4a39-9a96-4916a6684478.png)
- action value function ![image](https://user-images.githubusercontent.com/83327791/221380793-778e2df9-18e8-4e2c-9b86-f75411e21862.png)
  - recursive form (Bellman Equation): ![image](https://user-images.githubusercontent.com/83327791/221380828-9fed2e9e-4031-425a-8b87-50dde843a6ad.png)

### DRL
- RL uses dynamic programming to recursively obtain approximate solution to a Bellman equation to obtain current value function.
  - This requires massive memory consumption. Thus, 2 learning methods were proposed:
    - Monte Carlo
    - Temporal-Difference (TD)
  - However, in high-dimensional problems, the traditional RL faces the curse of dimensionality (computation sharply increases with the increase in the humber of inputs) 
    - As a solution, DRL was proposed. DRL approximates any nonlinear function by training deep neural networks and learns the inherent laws and essential characteristics of the input data.

The concept of DRL was firstly proposed in 2013 by Mnih et al.
- They proposed DQN to learn to play Atari 2600 games based on image input at a level beyond human experts'.

- DRL methods describe navigation as a Markov Decision Process (MDP).
- states: sensor observations
- goal: maximizing the expected rutrn (cumulated future rewards) of the action
- advantages:
  - being mapless (SLAM is not require)
  - strong learning ability
  - low dependence on sensor accuracy (some level of sensor noise is okay) 



