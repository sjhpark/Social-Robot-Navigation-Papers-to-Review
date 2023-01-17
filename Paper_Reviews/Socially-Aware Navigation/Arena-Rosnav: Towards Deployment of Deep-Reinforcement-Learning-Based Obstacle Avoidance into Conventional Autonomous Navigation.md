# Arena-Rosnav: Towards Deployment of Deep-Reinforcement-Learning-Based Obstacle Avoidance into Conventional Autonomous Navigation Systems
Linh Kästner, Teham Buiyan, Lei Jiao, Tuan Anh Le, Xinlin Zhao, Zhengcheng Shen and Jens Lambrecht1

IROS 2021
---

## Abstract
Key Ideas:
* There have been many DRL approaches for navigation. However, they are NOT suitable for long-range navigation due to:
  * proness to local minima
  * lack of long term memory

The authors propose: 
* Framework for training and testing DRL algorithms + Conventional navigation approaches
  * A navigation system incorporating DRL-based local planners into conventional global path planners (navigation stacks) for long-range navigation
    * ROS is fully integrated and enables the deployment of DRL-based planning as part of existing navigation stacks
    * DRL is connected with conventional global planning algorithms (RRT, A*, Dikstra, etc.)

## Introduction
Reliability and safety of navigation in highly dynamic environments is essential in the mobile robot operation.

Classic planning approaches can handle static environments, but do not handle reliable obstacle avoidance in dynamic environments.

Current industrial approaches usually hand-engineer safety restrictions and navigation measures. However,:
* Robots are often programmed to navigate conservatively
* Hand designing the navigation behavior is tedious and not so intuitive in dense environments due to stochasticity (unpredictability) of humans.

DRL emerged as an end-to-end learning approach that directly maps raw sensor output to robot actions.
* Has shown promising results in teaching complex behavior rules, robustness to noise, etc.
* _However, the bottleneck is its limitation for local navigation due to a lack a long term memory and its myopic nature._
  * Reference: [7] D. Dugas, J. Nieto, R. Siegwart, and J. J. Chung, “Navrep: Unsupervised representations for reinforcement learning of robot navigation in dynamic human environments,”, 2020

## Related Works
Navigation employed hand-engineered measures and rules:
* [8] C.-P. Lam, C.-T. Chou, K.-H. Chiang, and L.-C. Fu, “Human-centered
robot navigation—towards a harmoniously human–robot coexisting
environment,” 2010
* [9] J. Guzzi, A. Giusti, L. M. Gambardella, G. Theraulaz, and G. A.
Di Caro, “Human-friendly robot navigation in dynamic environments,” 2013
* Limited when the exact models are unknown
* The hand-engineered rules could become tedious and computationally heavy.

Navigation with safety thresholds:
* [2] S. Robla-Gómez, V. M. Becerra, J. R. Llata, E. Gonzalez-Sarabia,
C. Torre-Ferrero, and J. Perez-Oria, “Working together: A review
on safe human-robot collaboration in industrial environments,” 2017
* Limited when the exact models are unknown
* The hand-engineered rules could become tedious and computationally heavy.

DRL as an end-to-end approach with learning navigation in dynamic environments:
* [5] H.-T. L. Chiang, A. Faust, M. Fiser, and A. Francis, “Learning navigation
behaviors end-to-end with autorl,” 2019
* [11] A. Faust, K. Oslund, O. Ramirez, A. Francis, L. Tapia, M. Fiser,
and J. Davidson, “Prm-rl: Long-range robotic navigation tasks by
combining reinforcement learning and sampling-based planning,” 2018
* [12] A. Francis, A. Faust, H.-T. Chiang, J. Hsu, J. C. Kew, M. Fiser,
and T.-W. E. Lee, “Long-range indoor navigation with prm-rl,” 2020
* [13] H. Shi, L. Shi, M. Xu, and K.-S. Hwang, “End-to-end navigation
strategy with deep reinforcement learning for mobile robots,” 2019
  * DRL-baased local planner utilizing A3C and achieved end-to-end navigation behavior purely from sensor observation

Incorporating a human motion prediction algorithm into navigation to enhance environmental understanding and adjusting navigation for crowded environments
* [16] G. Ferrer and A. Sanfeliu, “Anticipative kinodynamic planning: multiobjective
robot navigation in urban and dynamic environments,” 2019

Socially aware DRL-based planner
* [6] SA-CADRL, 2017
  * Introduced reward functions.
  * Aimed to teach social norms to the robot (e.g. keepting to the right-side in the corridor)
* [17] GA3C-CADRL, 2018
  * Extended CADRL to include the handling of a dynamic amount (arbitrary) of people via LSTM.
  * Modified the approach to solve human randomness.
* [18] SARL
  *  Proposed an obstacle avoidance approach using DRL by modeling human behavior.
  *  Implemented not only Human-Robot interaction but also Human-Human interaction which could affect robot.

The authors incorporate the global planner for long range navigation and specifically deploy the planners on multiple highly dynamic environments.
* Similar to their work, [19] combined a DRL-based local planner with a conventional global planner.
  * [19] R. Güldenring, M. Görner, N. Hendrich, N. J. Jacobsen, and J. Zhang, “Learning local planners for human-aware navigation in indoor environments.”
  * Limitation: Only simple sub-sampling of the gloabl path is employed. No replanning capability is employed which leads to hindrance when there are multiple humans blocking the way.
    * _Thus, the authors introduce an intermdeiate planner to spawn way-points more intelligent._
    * _The authors add replanning functionality based on spatial and time horizons._

## Methodology
DRL + Conventional global path planners
![image](https://user-images.githubusercontent.com/83327791/212993639-e1bde5b5-cadc-47cd-a5fc-d01cb2240b4d.png)
* The way-point generator connects the DRL local planner and gloabl planner.

### A. Intermdeidate Planner - Waypont Calculation
Intermdiate planner: Interconnection between traditional gloabl planner (A*) and DRL-based local planner

The authors introduce a spatial horizon dependent on the robots postion.
![image](https://user-images.githubusercontent.com/83327791/212995659-1a627296-6f0e-4ad4-ad15-e28fe10bacb7.png)

Algorithm 1: Subgoal Calculation

![image](https://user-images.githubusercontent.com/83327791/212995167-55059c8f-c532-41ff-b952-02e3ef7d3a7e.png)
* The shaded circular area around the robot: Spatial horizon of a look-ahead distance dist_{ahead}
* Local goal for the DRL local planner: Subgoal
  * The subgoal (local goal) is determined based on the robot's position p_r and is located within the range of dist_{ahead} and is also close to the global goal.
  * Thus, the subgoal is dynamic and moves with the robot's position. --> Flexible with multiple unknown obstacles
* The global path is replanned and then a new sub-goal is calculated based on new global plan and current robot position p_r if: 
  * the robot is off-course of a certain distance from the global path
  * the time limit is reached without movement (robot stuck)

### B. DRL-based Local Navigation
Robot navigation problem as a POMDP (Partially Observable Markov Decision Problem)
* POMDP described as a 6-tuple: ![image](https://user-images.githubusercontent.com/83327791/212998376-521895aa-0f7a-482f-9d2b-456a32c6656a.png)
  * S: robot state; A: action space; P: transition probability; R: reward; O: observations; $\gamma$: discount factor (0.9)
* Constraints for a collision free navigation twoards the goal:
  * ![image](https://user-images.githubusercontent.com/83327791/212999019-31e600d4-15eb-487c-ab8f-d6f61e4656f7.png)
    * d_{goal}: goal radius
    * d_r: robot's radius
* Value Function: 
  * ![image](https://user-images.githubusercontent.com/83327791/212999903-55853e3d-a33c-4a77-85d7-67ae860fff47.png)
  * value function = accumulating $\Sigma$ the discounted expected rewards $\gamma$ R at each time-step s given an action command $\pi$(s_t) while following policy $\pi$
* Optimal Policy Function (calculated by Bellman Equation iterting over the Value V):
  * ![image](https://user-images.githubusercontent.com/83327791/213000440-28f147e6-c298-4005-b6a7-2e4a5f4fa490.png)
  * optimal policy = max of (current reward + values accmulated from the next state s_{t+1} to the final state w/ transition probability P counted

#### 1. Training Algorithm
Utilized an A3C policy gradient method while training the DRL agent.
* A3C's output is split into Policy (Actor) function $\pi$ & Value (Critic) function V.
* On-policy training - The agent has to be trained on the current status of the network.
* A3C employs asynchronous training of multiple actors simultaneous to avoid sample inefficiency. Each actor with its own environment N.
  * The knowledge gained from all the actors is then concatenated into the final output.
  * Policy output ![image](https://user-images.githubusercontent.com/83327791/213002533-dda38efc-c7c7-4d58-a062-1af6d0d531c2.png): a probability distribution of all actions
  * Advantage output ![image](https://user-images.githubusercontent.com/83327791/213002687-dc3fd1c2-30a7-4e7e-83a5-5eb08e50ec03.png): Assesses the advantage of executing the actions proposed by the policy.

![image](https://user-images.githubusercontent.com/83327791/213003113-916c166e-13d6-4af9-aef9-3b1d91433eb0.png)
* N: the # of simultaneous agents (actors) in A3C

#### 2. Reward System
Total Reward R: Sum of all sub-rewards
* ![image](https://user-images.githubusercontent.com/83327791/213004953-f9770535-66c2-417f-bf9f-0b2df7ff2643.png)
  * Sub rewards:
    * ![image](https://user-images.githubusercontent.com/83327791/213005003-96ec345f-4d89-4edb-8ae9-33a7588b6b33.png)
    * r_s: success reward
    * r_d: distance reward
    * r_p: progress reward
    * r_m: move reward
    * r_c: collision reward

#### 3. Neural Network Architecture and Agent Design


















