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
* Framework for training and testing DRL algorithms into Conventional navigation approaches
  * A navigation system incorporating DRL-based local planners into conventional navigation stacks for long-range navigation
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
  * Limitation: Only simple sub-sampling of the gloabl path is employed. No replanning capability is employed which leads to hidrane when there are multiple humans blocking the way.
    * _Thus, the authors introduce an intermdeiate planner to spawn way-points more intelligent._
    * _The authors add replanning functionality based on spatial and time horizons._

## Methodology






