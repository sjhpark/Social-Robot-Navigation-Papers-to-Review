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

DRL can b divided into 2 approaches:
- Value-based DRL method: indirectly obtains the agent's policy by iteratively updating the value function.
- Policy-based DRL method: directly uses the function approximation method to establish a policy network, and selects actions through the policy network to obtain the reward value, and optimizes the policy network parameters along the gradient direction for obtaining an optimized policy that maximizes the reward value.

## Value-based DRL methods
Value-based DRL method can only handle discrete and low-dimensional action spaces.

- DQN (Deep Q Network)
  -  ![image](https://user-images.githubusercontent.com/83327791/221385413-f150be26-2b62-499c-ba1e-74d8fb24d8ec.png)
    - Temporal-difference: ![image](https://user-images.githubusercontent.com/83327791/221385665-a5f0f6dc-8258-48ac-bf8e-0a24fbce4230.png)
    - Q-learning: ![image](https://user-images.githubusercontent.com/83327791/221385880-7a67a028-9aa4-4dc3-ba68-ab9499cd6b44.png)
      - https://towardsdatascience.com/intro-to-reinforcement-learning-temporal-difference-learning-sarsa-vs-q-learning-8b4184bb4978

- DDQN (Double DQN)
  - DQN has some shortcomings. One of them is it overestimates the action value function Q.
    -  _"When the DQN calculates the TD error, using the same Q network to select actions and calculate value functions leads to overestimation of the value function."_
  - DDQN uses a dual network structure.
    - The current Q network selects the optimal action.
    - The target Q network evaluates the selected optimal action.
    - By separating sets of parameters for between the action selection and policy evaluation, the overestimation risk is redueced.
  - Loss function of DDQN: 
    - ![image](https://user-images.githubusercontent.com/83327791/221386184-f4f3d3de-ea8d-41f9-9632-f793757e2bd6.png)

## Policy-based DRL methods
Policy-based DRL methods handle continuous and high-dimensional action spaces (e.g. physical control tasks).

- DDPG (Deep Deterministic Policy Gradient)
  - DDPG uses a deterministic policy function ![image](https://user-images.githubusercontent.com/83327791/221386314-f554b981-5669-428f-9df5-e6ee3ef20194.png) unlike the probability distribution function (where random action is selected from the probability distribution) ![image](https://user-images.githubusercontent.com/83327791/221386361-a521fca5-4542-411e-ba85-efb1ce6650c8.png)
![image](https://user-images.githubusercontent.com/83327791/221386338-387a54df-ea54-4dbe-8e84-d01e3748eb14.png)
  - _K_ samples in the experience pool are randomly selected, and the _Q_ network is updated by gradient ascent.
  - Loss function of the Q network:
    -  ![image](https://user-images.githubusercontent.com/83327791/221386463-5af2f998-a7f2-4a59-bc29-abb725afaf94.png)
  - Unbiased estimate of the policy network gradient:
    - ![image](https://user-images.githubusercontent.com/83327791/221386473-cf9fd7ae-1b06-44fe-89ae-a1c455e90fb2.png)
 
- A3C (Asynchronous Advantage Actor-Critic)
  - AC (Actor-Critic) method combines the Value function with the Policy gradient method.
    - Actor: selects actions using the policy gradient method
    - Critic: evaluates those selected actions using the value function method.
    - AC (Actor-Critic) method enables a single-step update in the policy gradient during training (previously, it was a sequence update in the policy gradient, where you have to wait for the sequence to end before evaluating and updating the policy).
  - A3C has some improvements from AC.
    - Parallel agents:
      - Multiple actors explore the environment. 
      - Enables multiple agents with secondary structures to simultaneously update the parameters of the main structure.
  - N-step return:
    - The value function of A3C's Critic is updated through multi-step cumulative return (other algorithms typically use a one-step return of the instant reward calculation function obtained in the sample).
    - N-step return improves the iterative update propragation and convergence speed.
    - A3C can run on a multi-core CPI, so its computational cost is lower.

- PPO (Proximal Policy Optimization)
  - PPO can perform multiple epochs of minibatch updates --> improves the sample utilization efficiency.
    - Traditional policy gradient methods adopt an on-policy method in which the sample minibatch can only be used for one update epoch, and the minibatch must be resampled to implement the next policy update. 
  - ![image](https://user-images.githubusercontent.com/83327791/221386969-d1646cde-f7e1-44dc-aa88-36993c4fe06c.png)
    - ![image](https://user-images.githubusercontent.com/83327791/221386979-164419ae-9f2d-458e-a663-7be686d4df75.png)
  - Clipped surrogate objective function of PPO:
    - ![image](https://user-images.githubusercontent.com/83327791/221387023-f6912ce7-3848-432a-8ff4-0c7769908bfc.png)
      - ![image](https://user-images.githubusercontent.com/83327791/221387033-bf7ed69b-2922-4d30-94a7-6ac29b834fdb.png)
      - ![image](https://user-images.githubusercontent.com/83327791/221387046-293d3c06-4db5-4e54-959f-39d85f980de2.png)
  - By balancing between sample complexity, simplicity, and time effectiveness, PPO outforms A3C and other on-policy gradient methods.

## DRL based Mobile Robot Navigation
![image](https://user-images.githubusercontent.com/83327791/221389157-0f2e974b-850c-42e4-a184-682d0a469f59.png)

### References of DRL based Mobile Robot Navigation
![image](https://user-images.githubusercontent.com/83327791/221389203-10b02ced-ba81-4d07-b9bb-1dbda81cf9e6.png)
![image](https://user-images.githubusercontent.com/83327791/221389220-e88cb170-125a-4784-a9da-570f023bc1fa.png)

## Challenge
Sparse Reward
- e.g. gives reward if reaches the goal, gives penalty if not
- does not adequately cover the holistic environment --> can result in poor training convergence and long training times OR result in undesired behavior of the robot

Solution
- Reward shaping
  - Designing a dense reward (manually)
  - **However, manually, designed reward functions have 2 disadvantages:**
    - Overfitting to certain scenes --> non-universality (not generalized)
    - Inappropriate reward sometimes leads to wrong guidance for learning
- Curriculum learning
  - Desgining appropriate curricula for prgressive learners from simple to complex
    - The complexity of the course is gradually increased.

