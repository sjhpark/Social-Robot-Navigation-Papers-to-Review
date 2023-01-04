# Human-Aware Robot Navigation: A Survey

### Thibault Kruse, Amit Kumar Pandey, Rachid Alami, Alexandra Kirsch
### 2013
---

## Abstract
For navigation, the presence of humans requires novel approaches that take into account the constraints of human comfort as well as social rules.

## 1. Introduction
Human-aware navigation is the intersection between research on human robot interaction (HRI) and robot motion planning.
* HRI is a young sub-domain while robot motion planning is an established engineering domain.
* Navigation for robot: Change of the position of its body to move to a different place
  * This is called "base motion".
  
Explicit Interaction
* Explicit interaction happens when an action is made for the purpose of achieving a reaction by the partner.

Implicit Interction
* Implicit interaction happens when actions are modified due to the (potential) presence of a partner, but no reaction
is intended.

### 1.1 Backgound and Scope
Previous works of autonomous robot navigation in human environments:
* Had navigation moudles that prevented collisions with humans or other obstacles using robust navigation strategies such as:
  * minimal required obstacle distance
  * stop-and-wait behavior in situations of conflict
  
## 2. Challenges of Human-aware Navigation (Types of challenges comfort, natural movement and social aspects)
Research on human-aware robot navigation follows different goals:
* Minimizing annoyance and stress to humans
* Robots behaving more natural
* Robots behaving according to cultural norms

**Comfort**
* Comfort is the absence of annoyance and stress for humans in interaction with robots.

**Naturalness**
* Naturalness is the similarity between robots and humans in low-level behavior patterns.

**Sociability**
* Sociability is the adherence to explicit high-level cultural conventions.
* e.g. moving on the right in the corridor

### 2.1 Human Comfort (Comfort)
* Large part of publications approaches Human Comfort by avoiding negative emotional responses (such as fear or ange)
* Human Comfort requirement is mostly inspected regarding a distance between a robot to human.
  * This distance is not only about collision avoidance but also about preventing emotional discomfort of humans.
* "Proxemics" was the concept that was prposed by Edward T. Hall [32].
  * Proxemics is a virtual space around a person that humans mutuall respect.
  * Different social distances depend on the relationship and intention. ![image](https://user-images.githubusercontent.com/83327791/210077078-10a0b750-ebe7-4546-81e4-6a67843247eb.png)

### 2.1.2 Techniques to Improve Comfort
The most common approach to avoid discomfort due to small distance is to define areas around humans as cost functions or potential fields [34, 44, 83, 89, 91].

The comfort aspect of navigation deals with eliminating obvious causes of discomfort. Beyond that, the interaction between robots and humans can still be improved.

### 2.2 Natural Motion (Naturalness)
Natural - Robots moving more similar to how humans move

The assumption is that if a robot behaves like a human (or like an animal), the interaction between humans and the robot becomes easier and more intuitive for the humans.

Aspects of Natural Motion
* Smoothness - refers to both geometry of the path taken as well as the velocity profile
* Motion relative to other agents (e.g. proxemics)

Two Risks in increasing similarity to humans:
* Uncanny Valley [61] - robots can cause discomfort by being too human-like in some aspect
* Inducing false assumptions in observers bout the robot capabilities.
  * However, these risks have not had an impact so far.

Previous Works on Naturalness
* Althaus et al [1]
  * Maintaining a certain distance to the closest person and regulating the robot speed as a function of the distance
  * Robot body to face the middle of the group when standing in formation with more than 1 person

* Garcia et al. [57]
  * Utilized SFM (Social Force Model) as a means to guide a group of humans in a natural way
  * Social Force Models (proposed by Helbing et al.): 
    * Represent moving agents (robots or humans) as masses under virtual gravitational forces.
    * An agent can be drawn towards a goal or a leading person.
    * An agent can be repelled by obstacles and other agents.

Some previous works also worked on naturalness in human crowds.
  * Berg et al. [99] - Investigated reciprocal avoidance as a working social norm
  * Henry ae al. [37] - Investigated global path planning using only local info about the crowd.

It is important for the robot to avoid creating ambiguous signals about its intention.
  * Kruse el al. [47] - Crossing situation was analyzed and the legibility of the robot motion improved by making the robot adopt a trajectory that is similar to that of humans in such situations.
    * The key is to adapt the velocity of the robot
rather than the path, allowing the path to indicate the intention of the robot.

Another challenge comes from moving in densely crowded areas.
  * Muller et al. [62] and Tamura et al. [94] - The robot movs along with the people moving in the same direction leading towards the goal of the robot.

### 2.3. Motion Under Social Constraints (Sociability)
There are social rules and norms such as standing in a queue, letting people leave an elvator before entering, giving priority to elderly people at doorways, walking o the right-hand isde of corridors, etc.

We separate following social rules/norms from comfortable and natural motions.
 * There could be cases where social rules and violated but comfort and naturalness are satisfied.

Several authors investigate improved approach directions to initiate explicit interaction [1, 10, 17, 43,
46, 63, 93], suggesting that the **discomfort is caused by the violation of a social rule, rather than a violation
of a comfort distance.**

Other previous works on Sociability:
 * Helbing [36] - Includes a bias for the robot to evade to a preferred side in cases of conflict.
 * Kirby et al. [44] - Modeled the social norm of passing a person by keeping the right-side in the hallway.
 * Chung et al. [13] - Prposed the idea "spatial effects".
   * A spatial effect is present when humans' motions divert from the shortest or most efficient path to their goal (for following social conventions).
   * e.g. Crossing the road only at crossing; Standing in queues; Passing a TV that is on.

### 2.4. Other Aspects of Human Friendly Navigation
e.g. Moving in formation; Time Efficiency; Robustness

### 2.5. Summary of Human-awarenss in Robot Navigation
The properties that a robot needs to exhibit to navigate in a human-aware fashion can mostly be categorized as comfort, naturalness and sociability.

Major human-aware capabilities that the robot could exhibit during navigation: ![image](https://user-images.githubusercontent.com/83327791/210125223-115bc757-a231-48bf-b0fd-0186a2d378ab.png)

## 3. Towards a Human-Aware Navigation Framework
The previous section defined human-aware navigation based on the existing literature. This section investigates the approaches to satisfy these requirements.

Division of software into Reactive and Deliberate modules (this is common to all navigation frameworks)
 * Reactive - Plans only for a small time ahead during execution
 * Deliberate - Plans the whole motion before executing
 * ![image](https://user-images.githubusercontent.com/83327791/210125365-75625751-7534-401b-9129-1e417c30aad4.png)

### 3.2 Prediction
Two approaches to prediction:
 * Prediction based on reasoning
   * Predictions are justified by assumptions of how agents behave in general.
 * Prediction based on learning
   * Predictions are justified by observations of how agents behave, in particular in special circumstances and environments.

### 3.2.1. Prediction based on Geometric Reasoning
![image](https://user-images.githubusercontent.com/83327791/210126003-ef05c797-dc3f-40a1-be41-565340a7e967.png)
* 8.a. Assuming at any givn time that a person will proceed to walk straight in its current direction at its current speed
* 8.b. Tadokoro et al. [92] - Using stochastic processes to predict probable state transitions of persons tracked in a grid map of the environment
* 8.c. Hoeller et al. [38] - Using potential fields where obstacles are repulsors and a virtual line in front of the person acts as attractor
* 8.d. Representing the uncertainty as growing round or elliptic areas of where the person is suspected to move to
* 8.e. Althoff et al. [2] - Integrated local planning and prediction using random sampling over the future path of several agents (including the robot), stochastically maximizing the expectation that no agent will collide with obstacles or one another
* 8.g. Social Force Model (as applied by Garcia et al. [57])

### 3.2.2. Prediction based on Machine Learning
ML for human prediction has advantage of improving over time, adapting to special circumstances, and low online computational effort (most of the ffort happens offline)

Forka et al. [21] - Used a neural network for one-step ahead preidction (but offer no evidence that this yields better results than linear interpolation)

Chung et al. [12, 13] - Presented a richer neural network model that uses observd short-term trajectories to make short-term and long-term predictions

Bennewitz el al. [6, 7] and Satake et al. [81] - Learn a set of motion patterns for a given map of an environment
from observations of human motion, using the Expectation Maximization Algorithm. Prediction is more of a classification (8.g. in Figure 8)

Thompson et al. [95] - Uses a mixture of annotated spaces and heuristically determined waypoints as estimates for short-term navigation goals (8.h. in Figure 8).

### 3.3. Pose Selection
Pose Selection (Robot Placement) is the task of identifying good places for the robot to stop and start manipulating.
 * e.g. Robot should stand and face the person talking to

Human-aware pose selection takes into account the discomfort or comfort a robot can cause when acting at a given spot.
 * Measuring comfort distances is analyzed for that purpose by several research teams [17, 42, 46, 63, 66, 100, 101].

### 3.4. Path Planning
Path planning is the task of choosing a list of waypoints to follow on a map in order to optimize the performance of the robot according to some global target function, like the distance traveled.

The majority of work on human-aware path planning use graph-based search methods
 * A graph represents states in a 2D map of the environment.
 * Each node is a point in which the robot can be without collision.
 * An edge between two nodes means the robot may travel between the nodes.

### 3.4.1. Global Cost Functions
In HRI, the shortest or most energy efficient path is not necessarily the most desirable.

Instead in HRI the intention is to find a path that is also suficiently safe, comfortable, natural, legible,
etc. to persons in the area.
* **Such desirable path attributes can be represented in global path planning as cost functions.**
* The purpose of a cost function is to assign a cost value to robot actions.
  * Those costs depend on the context, notably the robot environment and its state.
* ![image](https://user-images.githubusercontent.com/83327791/210127158-74d64cb0-f3fe-47cb-87ac-666b274607ab.png) ![image](https://user-images.githubusercontent.com/83327791/210127162-879b15fe-643c-4e34-a9c5-28b702dc0907.png)
* ![image](https://user-images.githubusercontent.com/83327791/210127168-7c6beb67-fa8c-4370-b7bb-c3590ff74eb6.png)

Examples of cost functions used in publications:
![image](https://user-images.githubusercontent.com/83327791/210127281-d8947f07-7a81-457f-9548-dc01fbaf822e.png)
![image](https://user-images.githubusercontent.com/83327791/210127268-9f871818-2dda-4182-98e8-4fba6ac2a4a0.png)
![image](https://user-images.githubusercontent.com/83327791/210127276-bf2d47cf-a60b-440b-a41a-d21ec2ad9c8c.png)
* Fig 9.a. Object Padding - Cost regions around static obstacles so that a robot prefers to move at a greater distance from objects than what is strictly necessary for collision avoidance, unless the space is so limited it has to get close.
* Fig 9.b. Object Occlusion Costs- Try to make the robot avoid motions along paths where the sensors of the robot cannot well perceive the area ahead.
* Fig 9.c. Social Costs of Hidden Zones (Sisbot et al. [89]) - The robot avoiding to appear surprisingly from behind objects close to a human
* Fig 9.d. Basic Comfort Distance Costs [43, 44, 83, 89] - Robot to avoid moving closely to humans where that is avoidable
* Fig 9.e. Visilibity Costs [43, 83, 89] - Assume that humans prefer a robot to move where they can see the robot when the robot has to move close.
* Fig 9.f. Interaction Region Costs - Assume a human prefers a robot not to cross the space between the human and an object the human interacts with (e.g. a TV)
* Fig. 9.g. Social Convention Costs - Adding costs on the social conventions such as right/left hand rules
* Fig. 9.h. Costs in Front of Moving Humans - No costs should be applied in path planning while humans are moving in the same direction as the robot. Instead, the robot may benefit from planning a path that follows a person if the robot wants to move in the same direction, in particular for moving in crowds.
  * This relates most closely to natural motion rather than comfort or safety.

### 3.6. Local Planning
Local Planning (Reactive Planning; Collision Avoidance) - The task of finidng commands for the robot actuators for the immediate future

A publication by Fraichard [24] defines criteria for safe robot motion in the sense of collision avoidance as:
 * A navigation needs to reason about
   * Its own dynamics
   * The future dynamics of moving obstacles
   * Infinite time horizon
     * However, for the real world, the time horizon does not need to be infinite since the future quickly deteriorates due to uncertainty and sensor limitations.
     * Appropriate minimum time horizon depends on the field-of-view of the robot and the maxiimally expected dynamics of all objects in the environmnt.
 * Following these constraints alone guarantee safety only, not social acceptability nor efficiency (not adequate for humans).

Usually there is nothing human-aware about local planning.
 * However, some authors use the knowledge about the nature of obstacles, and derive special behaviors when an obstacle is known to be human - This allows human-aware aspects in local planning

Fraichard [24] summarized several kinds of local planners.
The most used local planners are:
 * Sampling Algorithms
   * In sampling algorithms, the local planner calculates (simulates) the future positions of the robot under different velocity commands (sample velocities), prunes those that are in collision (sort out invalid simulated positions/trajectories), and selects the one of the remaining trajectories that best fits a goal function.
   * Sampling algorithms are easy to extend with social cost functions and additional sensor inputs, but representation of dynamic obstacles is often not part of the solution (e.g. DWA considers all obstacles to be static)
     * It seems to me that the DWA (Dynamic Window Approach) local planner in ROS uses this sampling algorithm where multiple trajectories are simulated based off sample velocity commands along with cost calculated per simulated trajectory. Then, it prunes to sort out invalid trajectories (in collision; negative cost) and pick the best trajectory (minimum positive cost) out of valid trajectories.
 * Velocity Obstacle (VO)
   * VO assumes perfect knowledge about the shape of obstacles and other agents' future motions. Then, finds an analytic solution that prevents collisions.
   * VO works for multi-robot navigation (as shown for examples in [11, 25])
   * However, it is more difficult to implement analytical solutions with nonlinear trajectories, noisy obstacle data, uncertainty, and cost functions.

## 4. Evaluation Methods for Human-aware Navigation
There are two main evaluation strategies:
 * Simulation based evaluation
   * Uses a simulation based on a model of what causes human-discomfort.
 * User Study based evaluation
   * Presents participants with a robot and ask them to rate robot qualities via a questionnaire.
