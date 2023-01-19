# Arena-Rosnav: Towards Deployment of Deep-Reinforcement-Learning-Based Obstacle Avoidance into Conventional Autonomous Navigation Systems
Linh Kästner, Teham Buiyan, Lei Jiao, Tuan Anh Le, Xinlin Zhao, Zhengcheng Shen and Jens Lambrecht1

IROS 2021
---

## Abstract
Key Ideas:
* There have been many DRL approaches for navigation. However, they are NOT suitable for long-range navigation due to:
  * proness to local minima
  * lack of long term memory
* My thought: However, the authors' approach still does not differentiate static & dynamic obstacles from standing & moving humans.
* My doubt: According to the authors' performance report, the model-based local planners such as MPC and TEB outperformed the learning-based local planners (CADRL and ARENA) when the obstacle velocity was less than around 0.2m/s, wouldn't it mean that the model-based local planners will outperform the learning-based local planners when the obstacles are static (e.g. surrounding pedestrians are standing)?
* Question: Plot (f) in Figure 5 and Plot (f) in Figure 6 seem to be swapped?

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
![image](https://user-images.githubusercontent.com/83327791/213253277-935d00cb-d9b4-4b10-9068-e4e98c347cfd.png)
* 2 Neural Networks: 1 for value function & 1 for policy function
  * Inputs -> 2 Fully Connected (FC) layers -> Gated Recurrent Unit (GRU) -> FC layer -> Output layer -> Policy function
  * Inputs -> 2 Fully Connected (FC) layers -> Gated Recurrent Unit (GRU) -> FC layer -> Output layer -> Value function
    * After each FC layer, ReLU activation was used 
* Inputs
  * 360 degree Lidar scan data
  * goal position w.r.t robot
  * angle w.r.t. robot

#### 4. Training Setup
![image](https://user-images.githubusercontent.com/83327791/213254936-ac81c181-2d8f-461e-b475-616cb45d201d.png)
* The robot agent is trained on randomized environments to avoid over-fitting and enhance generalization.
  * Walls and static obstacles are spawned randomly after each episode.
  * Curriculum training: More obstacles are spawned if a success threshold is reached. Else, less obstacles are spawned.

#### 5. Inegration of Obstacle Avoidance and Alternative Approaches
For comparison of the authors' DRL-based local planner "Arena" with other approaches, the authors integrate various optimization-based obstacle avoidance algorithms into ROS.
* Timed Elastic Bands (TEB) (model-based local planner)
* Dynamic Windows Approach (DWA) (model-based local planner)
* Model Predictive Control (MPC) (model-based local planner)
* CADRL (learning-based obstacle avoidance based on DRL)
* SARL from CrowdNav (learning-based obstacle avoidance based on DRL)

## Results and Evaluation
Local Planner comparison:
* Compared Arena with TEB, DWA, MPC, and CADRL.
* A* planner is used as the global planner for all the local planners compared.

Tested on 2 different maps:
* Office
* Empty map

### A. Qualitative Evaluation
![image](https://user-images.githubusercontent.com/83327791/213258696-83aa49f4-29f0-4e44-9414-e08669b7b077.png)
* ARENA replans more efficiently in highly dynamic environment by using the spatial and time horizons which triggers a replanning when the robot is stuck for more than 4 seconds.
* ARENA and CADRL manage to keep a direct path towards the goal and still have less collisions compared to the model-based planners (TEB, DWA, MPC).
* In general, model-based approaches (TEB, DWA, MPC) follow the global path more consistently while learning-based apporaches (CADRL, ARENA) make decision earlier thus more often got off-track when avoiding obstacles.

### B. Quantitative Evaluation
![image](https://user-images.githubusercontent.com/83327791/213262587-31c74668-c0a4-4554-a736-0e5bf9f034f9.png)
* **based on the authors' evlaution report about Overall Performance in pg.7, it seems to me that the plot (f) in Figure 5 is about Empty Map, not Office Map.**
  * ![image](https://user-images.githubusercontent.com/83327791/213332324-f71f7bc0-aaf1-482b-bbdc-4b36a6f7ddba.png) 

![image](https://user-images.githubusercontent.com/83327791/213262655-5dafefe9-1a31-4304-80e4-467268a7db38.png)
* **based on the authors' evlaution report about Overall Performance in pg.7, it seems to me that the plot (f) in Figure 6 is about Office Map, not Empty Map**

![image](https://user-images.githubusercontent.com/83327791/213327152-645f5f01-5dcd-4946-a077-2e68d4755a22.png)

Metrics used:
* Avg distance traveled
* Avg time to reach the goal
* Collision rates with all objects
* Success rate (rate of reaching goal with less than 2 collisions)
  * time out = 3 minutes

#### 1. Safety and Robustness
Learning-based local planners (CADRL and ARENA) managed to have a high success rate with a high number of dynamic obstacles than model-based local planners (TEB, DWA, MPC).
- Learning-based local planners even outperformed more when the obstacle velocity was higher.
- Overall, ARENA planner was superior in terms of success rate and safety with increasing number of obstacles and their velocity.

#### 2. Efficiency
In terms of efficiency, MPC outperformed the other model-based and learning-based local planners.
- MPC achieved the shortest time and shortest trajectories.
- However, MPC's success rate decreased and collision rates increased and were not the best when the number of obstacles and their velocity increased.

#### 3. Overall Performance
All the local planners' average perofmances based on each metric (safety, robustness, efficiency) are compared relatively against the ARENA's average performance. The results are in percentage.

Office Map (maybe)
* ![image](https://user-images.githubusercontent.com/83327791/213329089-5a568be7-4026-43cf-8c51-b42a0da2692b.png)
  * **Based on this performance report, I have one doubt. If the model-based local planners such as MPC and TEB outperformed the learning-based local planners (CADRL and ARENA) when the obstacle velocity was less than around 0.2m/s, wouldn't it mean that the model-based local planners will outperform the learning-based local planners when the obstacles are static (e.g. surrounding pedestrians are standing)?**

Empty Map (maybe)
* ![image](https://user-images.githubusercontent.com/83327791/213328992-ec121525-c2a6-4865-b307-67d2f652f30d.png)

TEB and MPC outperformed ARENA when the obstacle velocity is less than around 0.2m/s.

Model-based local planners performed better in the Office (has static obstacles) than Empty Map (no static obstacles) because they are highly dependent to the global planner and a global map where static obstacle are used as marker points. 

ARENA started to outperform all the other local planners from the obstacle velocity greater than around 0.2m/s.

CADRL achieved similar performance to ARENA. However:
* CADRL requires the exact positions of obstacles.
* CADRL only works with circular obstacles.
* In contrast, ARENA was trained on laser scan observations, so ARENA can work with any kind of obstacles without prior knowledge of its exact position. 

