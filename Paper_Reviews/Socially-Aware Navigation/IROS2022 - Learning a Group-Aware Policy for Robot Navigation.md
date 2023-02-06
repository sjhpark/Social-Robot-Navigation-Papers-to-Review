# Learning a Group-Aware Policy for Robot Navigation

Kapil Katyal, Yuxiang Gao, Jared Markowitz, Sara Pohland, Corban Rivera, I-Jeng Wang, Chien-Ming Huang

IROS 2022
---

Key Idea:
- Social robot navigation should also consider groups, not just individuals.
- Used a polygon based on the convex hull to represent the space of human group.

Areas to Improve:
- The reward function used by the authors is still hand-crafted which might not holistically account for all the necessary social norms.

## Abstract
Prior research focused on:
- Modeling pedestrians as independent and intentional individuals

Proposal:
- It is important for mobile robots to respect human groups, not just individuals, when navigating in crowds.
  - Learn group-aware navigation policies based on dynamic human group (people walking together) formation via DRL.
  - We want our robot to avoid cutting through a social group.

## Introduction
Prior Research that treated people as individuals:
- [5]: Treated humans as dynamic obstacles to avoid.
- [7],[8],[9]: Modeled social norms for socially appropriate robot navigation.
- According to [10],[11], the majority of people walk in groups.
- [12] says that 70% of pedestrians in a commercial environment walked in groups.

Approach:
- Learn navigation policies that allow the robot to safely reach the goal while minimizing impact to both individuals and groups.
  - RL based on PPO (proximal policy optimization) that combines robot navigation performance and group-aware social norms --> for learning a robust policy
  - a novel reward function (hand-crafted) that uses the convex hull of a group as the group space --> minimize impact to pedestrain groups and improve navigation performance

## Background and Related Work
The authors considered human social groups as 2 categories:
- Static social group (e.g. a standing group having conversation at a cocktail party)
- Dynamic social group (e.g. groups of people walking together)

Related Previous Work that regarded pedestrians as inidividual agents:
- [18],[19]: Investigated reactive methods for motion planning.
- [20]: Considered modeling pedestrian intention.
- [7],[21],[22],[23],[24],[25]: Learned from human data to realize social appropriateness.
- [8],[26],[27],[28]: Used handcrafted rules as planning constraints.
- [29]: SFM (Social Force Model) - modeled pedestrain social motion using attractive and repulsive forces.

Related Previous Work that considered static social groups
- [34]: Investigated how to enable robots to recognize static/standing social groups.
- [35],[36]: Investiaged how to approach static/standing social groups.

Related Previous Work that considered dynamic social groups
- [37],[38]: Explored methods to capture intra-group coherence.
- [39]: Explored method to catpure inter-group differences.
- [41]: Added dynamic constraints for O-space based on the walking direction and group cohesion.
  - O-space is a common shape of static group formation, but not for dynamic group formation. 

Related Previous Work that used ML techniques
- [43],[44]: Used RNNs and GANs to accurately predict human motion for individuals.
- [15]: Used RNNs and GANs to accurately predict human motion for groups.
- However, RNN and GAN based methods only predict human motion and do not generate navigation policies for robots.

Related Previous Work that used RL
- [13],[14]: Inverse RL to imitate humans and realize socially approrpiate movements.
- [45],[46]: Used DRL for robot navigation.
- [47] GA3C CADRL: Used attention-based DRL in crowded environments.
- [17] CrowdNav: Used attention-based DRL to capture human-human and human-robot interactions in crowded environments.

## Problem Formulation
Used RL to learn a policy to meet the objectives.
- States:
  - ![image](https://user-images.githubusercontent.com/83327791/216878738-a32219c3-6020-4a52-bb47-149f452e7e2f.png)
- Action Space: 
  - discretized Action Space into 5 speeds ranging from 0.2m/s to 1.0m/s & 16 rotations ranging from 0 to 2pi & a stop command --> total 81 possible actions in the Action Space
- Leveraged the simulation environment from CrowdNav [17] to represent pedestrian motion in groups for training and evaluation.
- Used an extended Social Force Model (SFM) tp simulate pedestrian motion in dyamic social groups
  - ![image](https://user-images.githubusercontent.com/83327791/216879233-13833b01-6dff-4cde-b20c-aeade1f3722d.png)
  - directional acceleration of the agent (dv/dt) = desired goal attractive force + obstacle repulsive force + sum of social repulsive force from other agents + group force
    - ![image](https://user-images.githubusercontent.com/83327791/216880128-88e5a206-2ab6-4e6b-91ed-040a2a3a752a.png)

## Approach
### Reward Function
- ![image](https://user-images.githubusercontent.com/83327791/216881348-76dbead9-7ea6-4c74-84ee-71cdd8314ce5.png)
- ![image](https://user-images.githubusercontent.com/83327791/216881850-2ab284b6-93a4-4475-9c12-9a4957353d0a.png): distance fro mthe robot to the edge of the convex hull of surrounding group j
  - Convex hull: ![image](https://user-images.githubusercontent.com/83327791/216881804-52b77502-c9c0-4f65-a830-c8e0205c8fe6.png)

### Neural Network Architecture
![image](https://user-images.githubusercontent.com/83327791/216882371-d33d6f9e-8dea-49e4-87e5-f9192da6c124.png)

 




