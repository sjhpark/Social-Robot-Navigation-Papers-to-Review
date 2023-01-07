# [SOADRL] Robot Navigation in Crowded Environments Using Deep Reinforcement Learning

Lucia Liu, Daniel Dugas, Gianluca Cesari, Roland Siegwart, Renaud Dube

2020

---
## Abstract
* The authors used a combined imitation learning and deep RL approach for motion planning in crowded environments.
* The authors separated processing information of static and dynamic objects to enable the network to learn motion patterns that are tailored to real-world environments.
* The authors' model is also designed to handle limited field of view of the equipped sensor.

## Introduction
With improvements in ML and DRL, recent works started to use neural networks for robot navigation in dynamic environments.
* End-to-end solutions have been developed in some works, allowing navigation through dynamic enviroments based on raw sensor input data.
  * [9] - T. Fan, X. Cheng, J. Pan, D. Manocha, and R. Yang, “Crowdmove: Autonomous mapless navigation in crowded scenarios,” 2018.
  * [10] - M. Pfeiffer, S. Shukla, M. Turchetta, C. Cadena, A. Krause, R. Siegwart, and J. Nieto, “Reinforced imitation: Sample efficient deep reinforcement learning for map-less navigation by leveraging prior demonstrations,” 2018.
  * [11] - H.-T. L. Chiang, A. Faust, M. Fiser, and A. Francis, “Learning navigation behaviors end-to-end with autorl,” 2018.
  * These approaches enable dynamic obstacle avoidance, BUT are limited in modeling socially-acceptable interactions with humans.
  * Safety is not ensured from these end-to-end policies.
* Some other works modeled humans explicitly to capture individual interactions BUT did not differentiate pedestrians from static obstacles.
  * [12] SA-CADRL, [13] GA3C-CADRL, [14] SARL from CrowdNav
    * Assume a 360 degree FOV (field of view).
    * SARL developed a neural network that explicitly models human-robot and robot-robot interactions. However, it does not specifically model static obstacles but rather incorporated static obstacles as standing pedestrians.

The authors suggest that the robot should move differently when approaching static obstacles compared to dynamic obstacles (pedestrians).

The authors developed **SOADRL (Social Obstacle Avoidance using DRL)** based off GA3C-CADRL and SARL.
* Enables the network to explicitly use the info of different obstacle types and thus differentiate static obstacles from pedestrians.
* Allows for an arbitrary amount of pedestrians and the absecence of pedestrians.
* Uses A3C (Asynchronous Advantage Actor-Critic) framework.
* Applies a limited FOV (to be similar to real-life sensor setups).

## Background
### Related Work
Combining collision-free navigation and modeling human behavior
* [18] - Z. Chen, C. Song, Y. Yang, B. Zhao, Y. Hu, S. Liu, and J. Zhang, “Robot navigation based on human trajectory prediction and multiple travel modes,” 2018.
* Firstly, predicts future trajectories of humans and then calculates the best possible paths to the goal.
* Limitations:
  * Computationally expensive as this requires the prediction of each pedestrian's trajectory.

Using uncertainty in learning pedestrian motion pattern and path planning.
* [19] - B. Luders, G. Aoude, J. Joseph, N. Roy, and J. How, “Probabilistically safe avoidance of dynamic obstacles with uncertain motion patterns,” 07 2011.
* Limitations:
  * Uncertainty in the predictions can result in frozen robot situations, where safe path is not guaranteed.

Assuming human movements are based on certain reactive policies.
* [7] ORCA, [8] RVO
* Limitations:
  * Sub-optimal in real-world environments (do not represent the real-world pedestrians well)

### DRL using A3C (Asynchronous Advantage Actor-Critic)
The authors use a policy-based model-free RL method.
* Directly parameterizes the policy $\pi$(a | s, $\Theta$) and updates the parameters $\Theta$ via Gradient Ascent on the expected return.
* The framework used to perform policy optimization is A3C.
  * A3C:
    * maintains Polcy function $\pi$(a | s_t, $\Theta$) (Actor) with parameters $\Theta\
    * maintains Value function V(s_t, $\Theta$ _v) (Critic) with parameters $\Theta$ _v
    * updates $\Theta\$ and $\Theta$ _v after every n steps.
    * A3C can run separate learning environments in parallel - Leverages multiple cores which acceleates the learning process

In RL, the environment is formulated as a Markov Decision Process (MDP).
* ![image](https://user-images.githubusercontent.com/83327791/211128737-f42f1de8-027a-40a6-b99d-6c2626f69f00.png)

Goal: Finding a policy $\pi$ that maximizes the expected return for every state.
* ![image](https://user-images.githubusercontent.com/83327791/211128807-e012fb0b-9794-42e0-8704-656eaf77ec27.png)
  * ![image](https://user-images.githubusercontent.com/83327791/211128847-11f257dc-73c7-479d-b25c-28b7c6fd1221.png)
  * state s - contains info about humans in the environment, static obstacles, goal position, agent's current speed & size
  * action space A - contains discrete control commands (velocity, rotation)

## Approach
Neural Network Model
* ![image](https://user-images.githubusercontent.com/83327791/211129394-f98ae631-c303-43b3-857e-d519ed7bfe24.png)
  * 3 Channels that separately process:
    * Goal Location and Robot State
    * Pedestrians
      * Calculates an attention score (used by Chen et al. [14] - SARL in CrowdNav) for each pedestrian to model human-human and human-robot interactions.
        * The interaction module captures these interactions through maps that contain all agents in the neighborhood for each human.
        * The pooling module computes an attention score for each pedestrian and combines these to a value that represents the crowd.
    * Static Obstacles represented by Angular Map and Occupancy Grids
      * Angular Map:
        * ![image](https://user-images.githubusercontent.com/83327791/211129700-687381cb-de7b-4d54-9625-d1ff31b86b13.png)
        * ![image](https://user-images.githubusercontent.com/83327791/211129724-761ae5d2-676e-4707-bba3-9948f18f4e18.png)
        * Can be easily obtained in real-time from the robot's sensors.
        * Reflects the importance of close obstacles.
      * Occupancy Grids:
        *  Encodes all obstacles within a certain ragne, allowing the agent to take more optimal actions from a long-term perspective.
  * Final layer for the value function outputs a single value of an estimated future discounted reward of the state
  * Final layer for the policy function outputs the probability of being the optimal action for each possibe action in the discrete action space which is normalized using a softmax

The authors trained SOADRL in an environment that simulates the movesments of crowds.
* The authors inserted human agents into the environment that are controlled by ORCA.
  * IMO: ORCA is sub-optimal and thus does not well represent real-world human motions.

The authors used supervised imitation learning to reduce the training time of their model.

## Experiments












