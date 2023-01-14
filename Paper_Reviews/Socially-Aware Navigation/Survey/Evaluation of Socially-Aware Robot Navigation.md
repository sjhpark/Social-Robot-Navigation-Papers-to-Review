# Evaluation of Socially-Aware Robot Navigation
Yuxiang Gao and Chien-Ming Huang
Published: Jan 2022
---
**This survey paper focuses on evaluation protocols rather than the algorithmic methods and systems that enable socially-aware navigation.**

## Abstract
There still are no agreed-upon evlaution protocols or benchmarks to allow for the systematic development and evaluation of socially-aware navigation.

## 1. Introduction
Mobile robots should not only navigate safely and efficiently but also abide to social expectations as they move through human environments.

  * Althaus et al., 2004. - Respect personal space
  * Katyal et al., 2021 - Avoid cutting through social groups
  * Kato et al., 2015 - Move at a velocity that does not distress nearby pedestrains
  * Huang et al., 2014 - Approach people from visible directions
  * Truong and Ngo, 2018 - Approach people from visible directions while maintaining relevant social dynamics

Socially Aware Navigation
  * Kruse et al., 2013 - Human-aware navigation
  * Kretzschmar et al., 2016 - Socially compliant navigation
  * Shiomi etal., 2014 - Socially acceptable navigation
  * Mavrogiannis et al., 2017 - Socially competent navigation

While research on socially-aware navigation is active over the past years, there haven't been standard evaluation protocols - including methods, scenarios, datasets,and metrics - to benchmark research progress.

Commonly agreed-upon evlaution protocols are key to fruitful progress in research in socially-aware navigation.

Socially-Aware Navigation is related to:
  * Human Trajectory Prediction
  * Agent and Crowd Simulation
  * Robot Navigation

## 3. Evaluation Methods, Scenarios, and Datasets

### 3.1. Evaluation Methods
Mavrogiannis et al. (2019) classified the evaluation methods into 3 categories:
  * Simulation study
  * Experimental demonstration
  * Experimental study

### 3.1.1. Case Studies
it is common to break down socially-aware navigation into sets of primitive, routine navigational interactions such as **passing and crossing**.
  * As such, prior research has utilized case studies to illustrate robot capabilities in handling these common navigational interactions (passing and crossing).
  * Kretzschmar et al. (2016) reported a study demonstrating how their inverse reinforcement learning approach allowed a robotic wheelchair to pass two people walking together in a hallway without cutting through the group.
  * Rios-martinez et al. (2013) used a set of predefined simulated configurations of human behaviors (e.g., moving around and interacting with each other) to illustrate their proposed method for reducing discomfort caused by robot movements.

### 3.1.2. Simulation and Demonstrations
Simulation experiments have been regularly utilized in recent years due to advances in **reinforcement learning** and data-driven approaches to socially-aware navigation (e.g., Chen C. et al., 2019; Li et al., 2019; Liu Y. et al., 2020)
  * Chen et al. (2020)
    * First evaluated their method for crowd navigation in a simulated circle crossing scenario with five agents, after which they provided a demonstration of their method using a Pioneer robot interacting with human subjects.

Simulation platforms have been developed for robot navigation due to the popularity of simulation-based evaluation:
  * PedsimROS (Okal and Linder, 2013)
    * 2D simulator based on Social Force Model (SFM) (Helbing and Molnár, 1995).
    * Integrated with the ROS navigation stack and enables easy simulation of large crowds in real time.
  * Simplistic 2D simulation (Gerkey et al., 2013)
  * MengeROS (Aroor et al., 2017)
    * 2D simulator for realistic crowd and robot simulation. 
    * It employs several backend algorithms for crowd simulation, such as: 
      * Optimal Reciprocal Collision Avoidance (ORCA) (Van Den Berg et al., 2011)
      * Social Force Model (SFM) (Helbing and Molnár, 1995)
      * PedVO (Curtis and Manocha, 2014)
  * Matterport3D (virtualized real environment) (Change t al., 2017)
  * High-fidelity simulation leveraging existing physics and rendering engines, Webots (Xia et al., 2018)
  * AI2-THOR (Kolve et al., 2019)
  * CrowdNav (Chen C. et al., 2019)
    * 2D crowd and robot simulator that serves as a wrapper of OpenAI Gym (Brockman et al., 2016), which enables training and benchmarking of many reinforcement learning based algorithms.
  * SEAN-EP (Tsoi et al., 2020)
    * An experimental platform for collecting human feedback on socially-aware navigation in online interactive simulations
    * In this web-based simulation environment, users can control a human avatar and interact with virtual robots.
    * It also supports simultaneous data collection from multiple participants and offloads the heavy computation of realistic simulation to cloud servers
    * Its web-based platform makes large-scale data collection from a diverse group of people possible.
  * SocNavBench (Biswas et al., 2021)
    * Framework that aims to evaluate different socially-aware navigation methods with consistency and interpretability
    * As opposed to most simulation-based approaches where agent behaviors are generated from crowd simulation [e.g., using Optimal Reciprocal Collision Avoidance (ORCA) (Van Den Berg et al., 2011) or Social Force Model (SFM) (Helbing and Molnár, 1995)], human behaviors in SocNavBench are grounded in real-world datasets (i.e., UCY and ETH datasets)
    * SocNavBench renders photorealistic scenes based on the trajectories recorded in these datasets and employs a set of evaluation metrics to measure path (e.g., path irregularity) and motion (e.g., average speed and energy)
  * CrowdBot Simulator (Grzeskowiak et al., 2021)
    * Benchmarking tool for socially-aware navigation that leverages: 
      * The physics engine and rendering capabilities of Unity and the optimization-based Unified Microscopic Agent Navigation Simulator (UMANS) (van Toll et al., 2020) to drive the behaviors of pedestians

### 3.1.3. Laboratory Studies
As opposed to case studies (section 3.1.1) which often involve presecribing human test subjects' behaviors, laboratory studies utilize experimental tasks to stimulate people's natural behaviors and responses within specific contexts.

Laboratory studies can be either:
  * Controlled experiments
    * Allow for statistical comparisons of navigation algorithms running on physical robots in semirealistic environments
  * Exploratory studies
    * Bera et al. (2019) - Conducted an exploratory in-person lab study with 11 participants to investiage their perceptions of a robot's navigational behaviors in response to their assumed emotions.

### 3.1.4. Field Studies
While laboratory experiments allow for controlled comparisons, they bear reduced ecological validity.

Thus, field studies are used to explore people’s interactions with robots in naturalistic environments.
  * e.g. RHINO (Burgard et al., 1998) and MINERVA (Thrun et al., 1999) were deployed as tour guide robots in museums to study their collision avoidance behaviors and how people reacted to them.
  * Satake et al. (2009) - Conducted a fild deployment in which a mobile robot approached customers in a shopping mall to recommend shops
  * Shiomi et al. (2014) - Investigated socially acceptable collision avoidance and tested their method son a mobile robot deployed in a shipping mall
  * Trautman et al. (2015) - Collected 488 runs of their experiment in a crowded cafeteria across 3 months to validate their algorithms

A benefit of deploying robots in the field is that they may reveal unexpected human behaviors.
  * e.g. Young children tend to bully the mobile robot
  * Brščić et al. (2015) - Conducted research on how to recognize and avoid potential bullying behaviors in the field

### 3.2. Primitive Scenarios
![image](https://user-images.githubusercontent.com/83327791/210169961-50660e8e-a529-4794-b8b0-dca4bf7e6705.png)
• Passing: This scenario captures interactions in which two
agents or groups are heading in opposite directions, usually
in constrained spaces such as hallways or corridors, and
need to change their respective courses to pass each other.

• Crossing: This scenario captures interactions in which two
agents or groups cross paths in an open space; it also
considers if one of the agents or groups is stationary.
Common examples of this scenario are circle crossing,
where all agents are initiated on points of a circle (e.g.,Chen C. et al., 2019; Nishimura and Yonetani, 2020), and
square crossing, where all agents are initiated on the corners
of a square (e.g., Guzzi et al., 2013b).

• Overtaking: This scenario captures interactions in which
two agents or groups are heading in the same direction and
one of them overtakes or passes the other.

• Approaching: This scenario captures interactions in which a
robot intends to approach or join a stationary or moving group
or individual. This scenario is observed when a robot attempts
to join a static conversational group (e.g., Truong and Ngo,
2018; Yang et al., 2020), initiate an interaction (e.g., Kato et al.,
2015) or follow a moving social group (e.g., Yao et al., 2019).
2016)

• Following, leading, and accompanying: This scenario
captures interactions in which a robot intends to join a
moving group by following (e.g., Yao et al., 2019), leading
(e.g., Chuang et al., 2018), or accompanying the group sideby-
side (e.g., Ferrer et al., 2017; Repiso et al., 2020).

### Datasets
![image](https://user-images.githubusercontent.com/83327791/210171143-02a2af29-e00e-4e5c-9f25-497e8643e188.png)
  * These datasets typically capture human movement in terms of trajectories or visual bounding boxes in various indoor and outdoor environments.
  * The datasets are used to train models for predicting pedestrian trajectories and for generating robot movement in the presence of pedestrians.
    * In particular, they are commonly utilized in modern data-driven approaches to socially-aware navigation, such as:
      * deep learning methods (e.g., Alahi et al., 2016; Zhou et al., 2021; Kothari et al., 2020)
      * reinforcement learning (e.g., Chen et al., 2017a; Li et al., 2019)
      * generative adversarial networks (GAN) (e.g., Gupta et al., 2018; Sadeghian et al., 2018a).

Datasets are also used to evaluate and benchmark the performance of socially-aware navigation.
  * e.g. Datasets ETH (Pellegrini et al., 2009) and UCY (Lerner et al., 2007) have been widely utilized in comparing navigation baselines (e.g., Sadeghian et al., 2018a; Bisagno et al., 2019; Gupta et al., 2018).

**One way to use the data of human trajectories in evaluation is to replace one of the human agents with the test robot agent and compare the robot’s trajectory with the corresponding prerecorded human trajectory.**

## 4. Evaluation Metrics
Three key aspects of socially-aware navigation (Kruse et al., 2013):
  * **Discomfort** - Represents the level of annoyance, stress, or danger as induced by the presence of the robot
  * **Naturalness** - Captures motion-level similarity between robots and people
  * **Sociability** - Encapsulates how well the robot follows the social norms expected by surrounding pedestrians

### 4.1. Navigation Performance
  * Navigation Efficiency:
    * Path Efficiency
      * The ratio of the distance of two waypoints to the length of the agent’s actual path between those points (Mavrogiannis et al., 2019)
    * Relative Throughput (Guzzi et al., 2013)
      * The ratio of the number of targets the agent can reach if it ignores all collision and social constraints to the number of targets an agent can reach in an actual simulation
    * Average Velocity
    * Mean Time to Goal
  * Success Rate (Arrival Rate)
    * Success rate is common to be related to the number of collisions and timeouts
    * Weighted success rate metrics have been prposed to consider aspects of navigation efficiency, such as path length and compltion time
 
### 4.2. Behavioral Naturalness
Metrics related to naturalness focus on low-level behavioral patterns, i.e., how human-like and smooth robot movements are

![image](https://user-images.githubusercontent.com/83327791/210171841-ddb299cd-a6a7-462e-a9d7-77d154eea740.png)
  * Displacement Errors
    * ADE
    * FDE
    * Dynamic Time Warping
      * An algorithm commonly used for matching spoken-word sequences at varying speeds—to transform one trajectory into another via time re-scaling.
      * A dynamic time warping distance can then be calculated to compare trajectories produced by agents moving at different velocities.
  * Smoothness
    * Non-smooth paths (irregular paths or jittery movements) are not only inefficient but also causing discomfort to nearby pedestrians. 
    * Path Irregularity ![image](https://user-images.githubusercontent.com/83327791/210172088-07fec0b0-3dcd-4bae-ab4e-c18bf2bdb107.png)
    * Topological Complexity 
      * Greater path entanglement means that the agents are more likely to encounter each other during navigation.

### 4.3. Human Discomfort
![image](https://user-images.githubusercontent.com/83327791/210172174-c0f3c8c1-cffd-4bab-b533-0ab9cef4b0e8.png)

Discomfort (either physical or psychological) is typically quantified by Spatial Models and Subjective Ratings

### 4.3.1. Spatial Models
Spatial Models for Individuals:

The impact of a mobile robot's navigational behavior on human comfort is difficult to quantify.
 * Research suggests that the psychological comfort of humans is affected by interpersonal distance (Aiello, 1977; Baldassare, 1978; Greenberg et al., 1980).
 * Proxemic theory by Hall, 1966, suggests that an individual's perceived personal space consists of several layers of concentric circles structured by their social functions. ![image](https://user-images.githubusercontent.com/83327791/210198136-6e26ce97-0c72-4b36-82bb-4bbf4d73dc8e.png)
 * Rios-Martinez et al., 2015 - Shapes other than concentric circles (ovoids, ellipses, etc.) have been used to represent personal spaces and encode more complicated social rules 
 * SFM (Social Force Model) (Helbing and Molnar, 1995)
   * Represents the constraints of personal space as attractive or repulsive forces originating from each agent. ![image](https://user-images.githubusercontent.com/83327791/210198399-939ddcfa-8165-40b8-843b-57305c3fe0a0.png)

Spatial Models for Groups:
Static, conversational groups can be modeled using f-formation (Kendon, 2010).

Extended Social Force Model (ESFM) was proposed to simulate dynamic social groups by Moussaid et al., 2010. ![image](https://user-images.githubusercontent.com/83327791/210198768-43078d94-ebfe-4afa-bd0b-6f30ee612df6.png)

### 4.4. Sociability
Measuring sociability is still largely difficult and is considered one of the key challenges in the field of socially-aware navigation (Mavrogiannis et al., 2021).

## 5. Discussion of Limitations
### 5.1. Limitations of Existing Evaluation Protocols/Methods
Recent works on socially-aware navigation rely heavily on datasets and simulation experiments for evaluation (Mavrogiannis et al., 2021).
  * This trend has been accelerated by advances in RL and data-driven approaches in general (e.g., Luber et al., 2012; Zhou et al., 2012; Alahi et al., 2014; Alahi et al., 2016; Kretzschmar et al., 2016; Park et al., 2016).
  * Limitations: This type of evaluation makes strong assumptions about human and robot behaviors.
    * In simulation experiments, researchers typically rely on pedestrian behavior models such as ORCA (Van Den Berg et al., 2011) and SFM (Helbing and Molnar, 1995).
      * Reciprocal behavior models such as ORCA assume that each agnt is fully aware of its surroundings and the position and velocity of the other agents.
  * 2D simulators such as Stage (Gerkey
et al., 2003) and CrowdNav (Chen C. et al., 2019) are lightweight and easy to extend, but they oversimplify and abstract, rendering their results difficult to apply to the real world.

Simulation experiments are commonly followed by demonstrations with physical robots in a real-world setting; while appropriate for proofs-of-concept, these demonstrations are mainly illustrative and lack statistical rigorousness.

### 5.1.2. Evaluation Metrics
**Metrics for Discomfrot:**

Can be characterized generally by Physical and Psychological Safety.

Prior Works have relied upon spatial models:
  * Hall (1966) - Theory on proxemics and personal space
  * Kendon, 2010 - f-formation for groups
  * Hlbing and Molnar, 1995 - The Social Force Model (SFM)
  * Moussaid et al., 2009 - The Extended Social Force Model (ESFM)
  * Limitations of spatial model-based metrics:
    * Homogeneity - All agents are assumed to be identical (possessing the same personal space and social forces, etc.)
    * Do not have sufficient granularity to represent environmental contexts.
      * e.g. Although people move and interact diffntly in different contexts, repulsive forces of SFM are all treated the same.
    * Difficult to encode high-level social norms into these spatial models.

**Metrics for Naturalness:**

Qualitfying the similarity between the robot's trajectory and trajectory observed in human data

  * ADE
  * FED
  * Dataset-oriented evaluation protocol has several limitations
    * Human navigational behaviors and trajectories are context-dependent.
      * The recorded human behaviors in a dataset are specific to the scenario in which the data was collected.
    * Robots and humans afford distinct navigational behaviors and expectations.
      * At the physical level, robots are quite dissimilar to humans and therefore afford different navigational behaviors, such as moving speed.
    * The majority of existing datasets are limited to 2D trajectories and neglect the fact that navigational behaviros are multiodal in nature.

**Metrics of Sociability:**

Prior Works:
  * Pacchierotti et al. (2006) - Defined a set of social rules for hallway interactions, suggesting that robot should: 
    * 1) signal its intention by proactively moving
to the right 
    * 2) stay as far away from humans as the width of the
hallway allows
    * 3) wait until a person completely passes by before resuming normal navigation in order to avoid causing discomfort.
  * Salek Shahrezaie et al. (2021) emphasized that social
rules differ based on environmental contexts.

### 5.2. Open Probelms of Socially-Aware Navigation
Assumptions that are a result of the oversimplification and
abstraction built into simulators:
  * Homogeneity
    * All agents are driven by a static behavior engine.
  * Omniscience
    * All agents have full awareness of their surroundings (Fraichard and Levesy, 2020)

Most spatial models for crowd behavior and proxemics are derived from population data.
  * The experiments and simulations using these spatial models often do not support diverse (by cultures, contexts, etc.) representation of different groups of people (Hurtado et al., 2021).
  
There is abundant evidence demonstrating that age (e.g., Nomura et al., 2009; Flandorfer, 2012), personality (e.g.,
Walters et al., 2005; Robert, 2018), gender (e.g., Flandorfer, 2012; Strait et al., 2015), and cultural (e.g., Lim et al., 2020) differences may affect people's perceptions of and interactions with robotos.


