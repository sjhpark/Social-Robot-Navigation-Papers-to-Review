# [SARL] Crowd-Robot Interaction: Crowd-aware Robot Navigation with Attention-based Deep Reinforcement Learning

Changan Chem Yuejiang Liu, Sven Kreiss and Alexandre Alahi

2019
---
_* Key Idea:*_
1. Many works using DRL techniques for social navigation consiers the navigation problem as a one-way Human-Robot interaction wihtout counting Human-Human interactions.
2. Thus, the authors model Human-Robot Interaction using Human-Human Interaction since Human-Human interaction indirectly affects the robot's navigation planning capability.

## Abstract
Recent works (w/ respect to 2019) have demonstrated DRL techniques to learn socially cooperative policies.
* However, their cooperation ability deteriorates as the crowd grows.
  * This is b/c they consider the problem as a one-way Human-Robot interaction problem. 

The authors want to:
* Go beyond first-order Human-Robot interaction
* Model crowd-robot interaction (CRI)

**Thus, the authors propose:**
1. Human-Robot pairwise interactions with a self-attention mechanism
2. Jointly model Human-Robot as well as Human-Human interaction in DRL framework
* The authors model captures Human-Human interactions in dense crowds that indirectly affects the robot's anticipation capability.
* The authors' proposed attentive pooling mechanism learns collective importance of neighboring humans w/ respect to their future states.

## Introduction
Traditional approaches for robot navigation:
* a. Consider moving agents as static obstacles
  * [1] J. Borenstein and Y. Koren, “Real-time obstacle avoidance for fast
mobile robots,” 1989
  * [2] J. Borenstein and Y. Koren, “Real-time obstacle avoidance for fast
mobile robots in cluttered environments,” 1990
  * [3] J. Borenstein and Y. Koren, “The vector field histogram-fast obstacle
avoidance for mobile robots,” 1991
  * [4] D. Fox, W. Burgard, and S. Thrun, “The dynamic window approach
to collision avoidance,” 1997 <- DWA
    * DWA is a popular local path planner that considers pedestrains as static obstacles.
* b. React to moving agents through a one-step lookahead
  * [5] J. v. d. Berg, M. Lin, and D. Manocha, “Reciprocal Velocity Obstacles
for real-time multi-agent navigation,” 2008 <- RVO
    * RVO guarantess collision-free navigation, but considers humans as moving obstacles thus does not consider socially-aware navigation.
    * The velocity obstacle (Fiorini & Shiller, 1998) of a virtual agent induced by a moving
obstacle in a game level is the set of all velocities for the virtual agent that will result
in a collision between the virtual agent and the moving obstacle within some short
time interval into the future, assuming that the dynamic obstacle maintains a
constant velocity (https://www.intel.cn/content/dam/develop/external/us/en/documents/unc-collision-avoidance-184932.pdf)
  * [6] J. van den Berg, S. J. Guy, M. Lin, and D. Manocha, “Reciprocal n-
Body Collision Avoidance,” 2011
  * [7] J. Snape, J. v. d. Berg, S. J. Guy, and D. Manocha, “The Hybrid Reciprocal
Velocity Obstacle,” 2011
* c. Understanding human behavior and complying with their cooperative rules
  * [8] T. Fong, I. Nourbakhsh, and K. Dautenhahn, “A survey of socially
interactive robots,” 2003 <- Survey
  * [9] T. Kruse, A. K. Pandey, R. Alami, and A. Kirsch, “Human-aware robot
navigation: A survey,” 2013 <- Survey
    * [Review](https://github.com/sjhpark/Social-Robot-Navigation-Papers-to-Review/blob/main/Paper_Reviews/Socially-Aware%20Navigation/Survey/Human-Aware%20Robot%20Navigation:%20A%20Survey.md)
  * [10] N. Roy, P. Newman, and S. Srinivasa, “Feature-Based Prediction of
Trajectories for Socially Compliant Navigation,” 2013
  * [11] H. Kretzschmar, M. Spies, C. Sprunk, and W. Burgard, “Socially
compliant mobile robot navigation via inverse reinforcement learning,” 2016
* d. Obstacle avoidance methods that jointly plan plausible paths for all the decision-making agents
  * [18] P. Trautman and A. Krause, “Unfreezing the robot: Navigation in
dense, interacting crowds,” 2010
    * Does not count the stochasticity of neighbors' behaviors
    * High computational cost for densely populated environemtns

As an alternative, _**RL frameworks**_ have been used to train computationally efficient policies that encode interactions and cooperation among agents.
* [19] CADRL 2016, [20] SA-CADRL 2017, [22] GA3C-CADRL 2018
* [21] P. Long et al., “Towards Optimally Decentralized Multi-Robot Collision
Avoidance via Deep Reinforcement Learning,” 2017
* _**However, these works are still limited in 2 aspects:**_
  * 1. Collective impact of crowd is modeled by a simplified aggreagation of the pairwise interactions which may fail to fully represent all the interactions
    * e.g. Maximin operator for CADRL (to pick the best action in the worst case for the crowd)
    * e.g. LSTM for GA3C-CADRL (to process state of each neighbor sequentially in reverse order of the distance to the robot)
  * 2. Most methods focus on one-way human-robot interactions, but ignore human-human interactions which could also indirectly affect the robot.
 
_**The authors' model:**_
1. Extracts features for pairwise interactions between robot and each human
  * Self-attention mechanism Infers relative importance of neighboring humans w/ respect to their future states 
2. Captures interactions among humans via local maps
3. Can take into account an arbitrary number of agents
* These was inspired by:
  * [13] A. Alahi et al., “Social LSTM: Human Trajectory Prediction in
Crowded Spaces,” 2016
  * [14] A. Vemula, K. Muelling, and J. Oh, “Social Attention: Modeling
Attention in Human Crowds,” 2017
  * [15] A. Gupta et al., “Social GAN: Socially Acceptable Trajectories with
Generative Adversarial Networks,” 2018

## Background
### Related Work
* A. Social Force [23], [24], [25]
* B. Interacting Gaussian Process (IGP)
  * models the trajectory of each agent as an individual Guassian Process.
  * proposes an interaction potential term to couple the individual Gaussian Process for interaction.
* C. RVO [5] & ORCA [6]
  * Reactive methods
  * Assume all the agents follow the same policy

* D. Imitation Learning 
  * Learns policies from demonstrations of desired behaviors  
  * [31] L. Tai, J. Zhang, M. Liu, and W. Burgard, “Socially Compliant Navigation
  through Raw Depth Inputs with Generative Adversarial Imitation
  Learning,” 2017
  * [32] P. Long, W. Liu, and J. Pan, “Deep-Learned Collision Avoidance
  Policy for Distributed Multiagent Navigation,” 2017
  * [33] Y. Liu, A. Xu, and Z. Chen, “Map-based deep imitation learning for
  obstacle avoidance,” 2018
* E. Inverse Reinforcement Learning
  * Learns the underlying cooperation features from human data using the maximum entropy method 
  * [10] N. Roy, P. Newman, and S. Srinivasa, “Feature-Based Prediction of
Trajectories for Socially Compliant Navigation,” 2013
  * [11] H. Kretzschmar, M. Spies, C. Sprunk, and W. Burgard, “Socially
compliant mobile robot navigation via inverse reinforcement learning,” 2016
  * [34] M. Pfeiffer et al., “Predicting actions to act predictably: Cooperative
partial motion planning with maximum entropy models,” 2016
*  _Learning outcomes of Imitation Learning and Inverse Reinforcement Learning both depend on the scale and quality of demonstrations._

* F. Reinforcement Learning
  * Learns sensorimotor policies in static and dynamic environments from raw observations
    * [21] P. Long et al., “Towards Optimally Decentralized Multi-Robot Collision Avoidance via Deep Reinforcement Learning,” 2017
    * [36] L. Tai, G. Paolo, and M. Liu, “Virtual-to-real deep reinforcement learning: Continuous control of mobile robots for mapless navigation,” 2017
  * Learns socially cooperative policies with the agent-level state information
    * [19] CADRL 2016, [20] SA-CADRL 2017, [22] GA3C-CADRL 2018
  * _Instead of using these methods, the authors' uses a neural network model to capture the collective impact of the crowd._

* G. Deep Neural Network
  * Handcrafted methods
    * [38] G. Antonini, M. Bierlaire, and M. Weber, “Discrete choice models of pedestrian walking behavior,”
  * Data-driven methods
    * [13] A. Alahi et al., “Social LSTM: Human Trajectory Prediction in Crowded Spaces,” 2016 
  * Generative models 
    * [15] A. Gupta et al., “Social GAN: Socially Acceptable Trajectories with Generative Adversarial Networks,” 2018
    * [40] T. Fernando, S. Denman, S. Sridharan, and C. Fookes, “Soft + Hardwired Attention: An LSTM Framework for Human Trajectory Prediction and Abnormal Event Detection,” 2017
    * [41] A. Sadeghian et al., “SoPhie: An Attentive GAN for Predicting Paths Compliant to Social and Physical Constraints,” 2018
  * Attention models
    * [14] A. Vemula, K. Muelling, and J. Oh, “Social Attention: Modeling Attention in Human Crowds,” 2017
    * [43] A. Sadeghian et al., “Car-net: Clairvoyant attentive recurrent network,”
  
  The authors design a social attentive pooling module to encode crowd cooperative behaviors in a DRL framework.
  
### Problem Formulation
_**Navigation: Robot moves to a goal through a crowd of n humans**_
* Navigation is formulated as a sequential decision making problem in a RL framework.
* Observable states
  * position ![image](https://user-images.githubusercontent.com/83327791/212254156-82d7273b-c5ef-4417-8861-15ad5a883cbe.png)
  * velocity ![image](https://user-images.githubusercontent.com/83327791/212254247-7fbb8e80-dbe3-4f3e-a273-7612cba9df07.png)
  * radius r
* Unobservable states
  * ![image](https://user-images.githubusercontent.com/83327791/212254496-4e0d9b58-e7a7-4f66-a133-97f4260e6b04.png)
  * ![image](https://user-images.githubusercontent.com/83327791/212254552-1a6358c0-2e57-42af-bdc4-b6f605765e7a.png)
  * Each human agent is aware of observable state but not unobservable state
  * The robot is aware of both observable and unobservable states
* State of robot at time t ![image](https://user-images.githubusercontent.com/83327791/212254854-c9b53194-11b4-49b4-bf8e-841715ebf637.png)
* States of humans at time t
* Joint state for robot navigation at time t ![image](https://user-images.githubusercontent.com/83327791/212255066-85eae306-1fd8-4d42-b8f2-5ee902774e9c.png)

_**Optimal Policy**_ ![image](https://user-images.githubusercontent.com/83327791/212255136-346be8cf-b6f1-48e8-aa79-e35bd1be766e.png)
* Optimal policy is to maximize the expected return: ![image](https://user-images.githubusercontent.com/83327791/212255337-46c64abf-df11-414d-bc7e-45fad5de6a6b.png)
* Reward at time t ![image](https://user-images.githubusercontent.com/83327791/212255480-792652d2-4044-4f0c-83cc-8963f1d9709b.png)
* Optimal Value Function ![image](https://user-images.githubusercontent.com/83327791/212255582-b26c5497-1201-4c68-954b-ee9ccc21ff3c.png)
* Transition Probability from time t to t+1 ![image](https://user-images.githubusercontent.com/83327791/212255799-89bb6f2d-3edd-4b7d-9205-b4eb9285c42f.png)

_**The authors follow the formulation of the reward function (defined in CADRL & SA-CADRL)**_
* ![image](https://user-images.githubusercontent.com/83327791/212255970-5fd9c5f6-ab72-41ba-901e-ff25e1f0e2fa.png)
  * minimum separation distance between robot and humans ![image](https://user-images.githubusercontent.com/83327791/212256077-62523a79-9bb0-44e5-ac4e-6995787591bd.png)

### Value Network Training
The value network is trained by the _temporal-difference method_ with _standard experience replay_ and _fixed target network techniques_.
![image](https://user-images.githubusercontent.com/83327791/212256512-46b1582d-38b6-49d7-8cb2-dfbcb6139ca1.png)
* Lines 1-3: The value network V is initialized with imitation learning using a set of demonstrator experiences D. 
  * Experience Replay Memory E is also initialized.
* Lines 4-14: The model is subsequently refined from experience of interactions.
  * Line 7: Optimal policy (optimal action) at time tcalculated from reward + discounted value function 
  * Line 8: Save (state, action, reward, next state) at the optimal action at time t

## Approach
Socially attentive network
![image](https://user-images.githubusercontent.com/83327791/212340216-0316e479-8248-4fda-a0c7-7f66c296424b.png)
* Interaction module - models Human-Robot interactions and encodes Human-Human interactions via coarse-grained local maps
* Pooling module - aggregates the interactions into a fixed-length embedding vector by a self-attention mechanism
* Planning module - estimates the value of the joint state of the robot and crowd for social navigation

### A. Parametrization
The authors followed the same parameterization as in [19] CADRL and [22] GA3C-CADRL.
* Robot is at the origin.
* x-axis points toward the robot's goal
* ![image](https://user-images.githubusercontent.com/83327791/212341186-6d7f4b84-7eeb-47a2-8aa4-527a6a73d01c.png)

### B. Interaction Module
Modeling all pairs of human interactions is computationally heavy.

Thus, the authors used a pairwise interaction module.
* Pairwise interaction module models Human-Robot interaction while using local maps as a coarse representation for Human-Human interactions.

Interaction Module
![image](https://user-images.githubusercontent.com/83327791/212344087-aaf9be20-cd93-4025-ae62-42ee8b401bf5.png)
* Local Map Tensor M_i: a L x L x 3 tensor centered at eac human i to encode the presence and velocities of neighbors.
  * L: size of neghborhood
  * ![image](https://user-images.githubusercontent.com/83327791/212344496-722da3c6-6c95-4057-9015-0636ad552a0f.png)
  * Indicator function ![image](https://user-images.githubusercontent.com/83327791/212344853-4d10ab1a-f1ab-4bfe-a24d-012466ede2f9.png)
    * 1 if the relative position ![image](https://user-images.githubusercontent.com/83327791/212345008-d135fb0e-77f7-4c7b-9c45-41381f39c9aa.png) of human i and human j is located in the cell (a,b)
* Embedding Vector e_i (fixed length vector using a MLP) ![image](https://user-images.githubusercontent.com/83327791/212345873-34837ae5-bdc3-4405-a643-433791dba65e.png)
  * State of human i, map tensor M_i, and state of robot are embedded into the Embedding Vector e_i.
  * ![image](https://user-images.githubusercontent.com/83327791/212347271-ba7ade05-aa23-4479-8039-9e12aee368ab.png)
  * ![image](https://user-images.githubusercontent.com/83327791/212347338-2928bda4-b770-485f-b402-3c5306734357.png)
* Pairwise interaction feature between robot and person i ![image](https://user-images.githubusercontent.com/83327791/212346494-b7e02acc-b2c7-4d98-853d-89275acaa138.png)
  * ![image](https://user-images.githubusercontent.com/83327791/212346597-10bafa55-bced-4970-9199-728f63f95ec5.png)
  * ![image](https://user-images.githubusercontent.com/83327791/212346615-4dfed8e5-91b3-4652-a623-3064e413aabf.png)

### C. Pooling Module
![image](https://user-images.githubusercontent.com/83327791/212346742-463ab578-c468-4bc7-84be-95df2137b558.png)
* The model needs to handle an arbitrary number of huamns that is hugely different by different scenes.
  * GA3C-CDARL [22] proposed: Feeding states of all humans into an LSTM sequentially in descending order of their distances to the robot, considering closest human as the most influencial agent.
    * However, this is not always true. Other factors such as speed and direction also essential to correctly estimate the importance (influence) of a neighbor agent.
* The authors propose a social attentive pooling module to learn the relative importance of each neighbor and the collective impact of the crowd.
  * ![image](https://user-images.githubusercontent.com/83327791/212349656-7de52061-49eb-49ad-8ce7-d0d1e584055a.png)
    * ![image](https://user-images.githubusercontent.com/83327791/212349893-d54c5adf-0855-4b88-9988-9aec7af5e1d3.png)
  * ![image](https://user-images.githubusercontent.com/83327791/212349987-63e456e8-d9bb-4416-939c-e99a7a9f2b12.png)

### C. Planning Module
Estimates the state value v for cooperative planning
![image](https://user-images.githubusercontent.com/83327791/212350902-e5b9e3ab-8920-4f17-b423-e16707384277.png)
* ![image](https://user-images.githubusercontent.com/83327791/212350959-4baf6138-cfc1-4bb9-8974-22c8ee079d22.png)

## Experiment
The authors built a simulation environment in Python for robot navigation in crowds.
* The simulated humans are controlled by ORCA.
  * ORCA uses parameters sampled from Gaussian distribution for having behavioral diversity. 

![image](https://user-images.githubusercontent.com/83327791/212354780-9a7730c1-9d72-4f8b-93b2-6150d266b042.png)
* Invisible setting: Robot is invisible to neighboring humans
* Visible setting: Robot is visible to neighboring humans
