# Robot Navigation in Constrained Pedestrian Environments using RL

Claudia Perez-D’Arpino, Can Liu, Patrick Goebel, Roberto Martın-Martın, Silvio Savarese

Stanford University

2020
___

## Abstract

In mobile robot navigation in human environments:
* Many literatures focused on open spaces. 
* This paper focuses on the indoor where additional challenges of constrained spaces usually exist (e.g. corridors, doorways, crosswalks, etc.)

The authors present RL approachto learn policies capable of dynamic adaptation to the presence of moving pedestrians while navigating in constrained environments.
* Policy network receives guidance from a motion planner that provides waypoints to folow a **globablly planned trajectory**.
* Whereas reinforcement component handles the **local interactions**.

The authors explored a compsitional principle for multi-layout training and find that:
* Policies trained in a small set of geometrically simple layouts successfully generalize to unseen and more complex layouts (that exhibit composition of the simple structural elements available during training).

## Introduction
Narrow indoor spaces such as corridors, doorways, and crosswalks populated by humans are extremely hard problems for navigtion solution.
* New models are needed that can navigate around pedestrians in tight spaces with constraints imposed by obstacles, objects, walls, doors, etc. based only on sensor information.

The authors propose:
* Motion Planner + RL Policy
  * Motion Planner - Provides coarse static strategies to avoid local minima
  * RL Policy - Learns to follow the path given by the planner & Deviates from it to adapt the current conditions of the environment (e.g. moving pedstrians) as observwed with the sensors. 

Typical indoor spaces:
* Common geometric elements _ Diverse composition of these elements

**Key Idea**

Does Multi-layout training on a small set of canonical environments generalize to navigation on more comple layouts?
* The authors explored a multi-layout training:
  * A policy is trained in a number of simple canonical layouts (e.g. corridors, door exit, etc.), not the composed layouts of multiple combined navigation chanllenges.
    * The authors found that policies trained in the small set of geometrically simple layouts are also successful on more complex layouts exhibiting composition of these simple shapes.

![image](https://user-images.githubusercontent.com/83327791/210597295-e6f52471-819d-4880-bc6e-97588f0f1a25.png)
![image](https://user-images.githubusercontent.com/83327791/210605944-001f74be-5596-46ec-a0a2-a1ab79c9cc7c.png)
* WALLS - The authors generated simple environments (called WALLS) in simulation.
* MESH - Then the authors transferred the policies learned on WALLS to 2 unseen 3D reconstructions of real-world environments (called MESH).

## Related Work
Early solutions of navigation in human environments:
* motion planner + controller
  * [14] S. M. LaValle. **Planning algorithms.** Cambridge university press, 2006.
  * [15] V. Kunchev, L. Jain, V. Ivancevic, and A. Finn. **Path planning and obstacle avoidance for autonomous mobile robots: A review.** 2006.
  * Suffered from "freezing robot" problem
    *  To overcome these limitations, ML [18] and Imitation Learning [1, 19, 20, 21] were proposed
    *  Imitation Learning also has an issue that the agent doesn't generalize byond what has been seen in the dataset --> Requires a large amount of human demonstrations to train successfully.
* Deep RL
  * The dependency on large training data can be solved by a self-exploratory loop such as RL
  * Some previous works applied Deep RL to develop multi-agent collision avoidance systems focused on optimizing pth efficiency while finding a feasible trajectory around pedestrians
  * [8, 23, 24] - SARL in CrowdNav, CADRL, SA-CADRL respectively
* Motion Planner + Learning
  * Motion planner to provide guidance to a learned controller
  * Learned controller to adapt the plan to the current surrounding conditions as observed with the robot's sensors
  * Pokle et al. - Deep local trajectory replanning and control for robot navigation.
  * Gao et al. - Integrating planning and deep learning for goal-directed autonomous navigation

## Learning to Navigate in Pedestrain Environments
Proposal:
* Sampling-base motion planner + reactive low level sensorimotor policy learned with RL
  * Sampling-base motion planner - Queries to navigate the shortest path to the goal location 

Reactive Navigation
* Modeled the safe navigation problem among humans as POMDP (Partially Observed Markov Decision Process)
  * ![image](https://user-images.githubusercontent.com/83327791/210602606-fdb35822-cf5a-4d99-b8cb-0ef4bb9bf33d.png)
  * S: State Space; A: Action Space; O: Observation Space; Tau: State Transition Model; R: Reward at state s; Gamma: Discount Factor
    * ![image](https://user-images.githubusercontent.com/83327791/210602945-91a7b49a-271e-4c89-b830-a8394334de5e.png)
  * ![image](https://user-images.githubusercontent.com/83327791/210604355-cc55d54e-797f-47f7-bb5a-24be96fb2ced.png)
  * ![image](https://user-images.githubusercontent.com/83327791/210603943-6b2e415c-9a63-4c18-9cef-afc403b73690.png)
  * ![image](https://user-images.githubusercontent.com/83327791/210603338-0f760f9b-0aaa-4dd1-8a89-b260aa71a589.png)
* Reward Function
  * The reward function R encourages the policy to learn to reach the final goal in the shortest time possible following the given path but deviating from it to avoid collisions. 
  * ![image](https://user-images.githubusercontent.com/83327791/210604519-33a0ad67-4c4a-4e57-b00b-6beba94d07df.png) 
* Policy Gradient
  * ![image](https://user-images.githubusercontent.com/83327791/210603598-b6697588-54d2-496c-bc16-971fa5446357.png)
  * Optimizes the parameter (theta) to maximize the expected future return J 
* ![image](https://user-images.githubusercontent.com/83327791/210604066-f5d7d4b2-f472-42a9-a9af-bdd0d352b731.png)
* The authors used SAC (Soft Actor Critic) algorithm to learn both a Value Function (Critic) and a Policy (Actor) approximated by Depp Neural Networks.

## Experimental Evaluation and Results
![image](https://user-images.githubusercontent.com/83327791/210609256-c7ef5847-7719-4722-88c5-eb116ee829bf.png)
![image](https://user-images.githubusercontent.com/83327791/210609619-19b72e76-265f-4b1a-9c97-5d9f5e956657.png)
* Notations:
  * A - Corridor; B - Door Exit; F - Crosswalks 

The evaluation is conducted using 8 independently-trained policie networks.
* 2 policies are trained on the WALLS environments using a single layout (Single case of geometric constraint e.g. Wall B).
* 4 policies are trained on the WALLS environments using a multi-layout (Multiple cases of geometric constraints e.g. Walls A,B,D).
* 2 policies are trained on the MESH environements.

The authors used such metrics for evaluation:
* Success Rate (S) - % of episodes where the robot reaches its goal without colliding or timing out
* Collsion Rate (C) - % of episodes during whic hthe robot collides with either a pedestrian or static object (e.g. a wall)
* Collision with Pedestrians (PC) - % of episodes involving collisions with pedestrians
* Collision with Obstacles (OC) - % of episodes involving collisions with static obstacles
* Timeout Rate (TO) - % of episodes where the robot runs out of time before reaching its goal
* Personal Space Overlap (PSO) - Distance between the robot and pedestrians for which the robot is closest than the specified personal space threshold (aggregated across all pedestrians and averaged over length of the episode)

Observations
* Transferring MESH (complex 3D geometry) training to any other environments resulted in a significant performance drop
  * This could be due to overfitting
* Transferring WALLS (simple scenes using blocks-like shapes) training performed more consistently across domains.

## Conclusion
The multi-layout training showed:
* The ability to train on simplified model and deploy on a more complex unseen environemnt (with composition of the basic layouts)
* Policies trained on walls-worlds are able to generalize to unseen 3D reconstructions of real environments (e.g. a supermarket)

The results from the experiments show and offer a practical advantage of removing the need to train on target environments with high reconstruction fidelity.

