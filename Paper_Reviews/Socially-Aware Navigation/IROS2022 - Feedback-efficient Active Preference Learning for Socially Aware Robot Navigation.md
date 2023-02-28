# Feedback-efficient Active Preference Learning for Socially Aware Robot Navigation
Ruiqi Wang, Weizheng Wang, and Byung-Cheol Min

IROS 2022

---

## Abstract
Key Idea:
- Although existing learning-based methods achieved better performance than model-based methods, there are still shortcomings:
  - RL depends on the handcrafted reward function 
    - Does not holistically quantify social compliance
      - Handcrafting a meticulous reward function that emulates non-quantifiable social compliance is complex and challenging
    - Can lead to reward exploitation
      - Handcarfted reward can encourage robots exploit high rewards via some undesired and unnatural actions that can impair human comfort.
  - Inverse RL requires expensive human demonstrations
- The authors propose:
  - feedback-efficient active preference learning (FAPL)
    - distills (simplifies the representation of the preferences and make it more usable for the robot's decision-making process) human comfort and expectation into a reward model to guide the robot agent to exlore latent asepcts of social compliance.
    - relies on human feedback, rather than human demonstrations.
  - Active preference learning will express preferences for a pair of robot navigation trajectories.
    - It has been shown that people feel much easier to make relative judgements than direct rating. (e.g. comparing two actions and tell which is more happy is easier than looking at one action as tells if it is happy or not). 
  - Experts demonstrations will be stored into an initialized experience replay buffer as expert experiences.
  - Curious exploration will be stored in the experience replay buffer as exploration experiences.
  - Experiences stored in the experience replay buffer will be dvided into multiple segments. In each feedback step, human will be queried to show preference over two segments of the experiences.

## Introduction
Two categories of existing research in socially-aware navigation:
- Model-based methods
  - [3],[4],[5]
  - model the social conventions and dynamics of crowds as additional parameters for traditional multi-agent collision-free robot navigation algorithms
  - hard to contain precise rules followed by all pedestrians
  - oscillatory trajectories can happen
- Learning-based methods
  - learning-based methods have achieved better performance than the model-based methods.
  - 2 main types of learning-based methods:
    - **RL**: [6] SARL, [7] CrowdNav, [8]
    - **Inverse RL**: [9],[10],[11]

- Shortcomings of RL methods: 
  - Handcrafting a meticulous reward function that emulates non-quantifiable social compliance is complex and challenging.
  - Handcarfted reward can encourage robots exploit high rewards via some undesired and unnatural actions that can impair human comfort.
- Shortcomings of Inverse RL (IRL) methods:
  - IRL learns a policy or reward from human demonstrations.
    - Advantage of IRL: 
      - IRL can avoid handcrafting/engineering reward function which further avoid reward exploitation and allow introducing human insights and comfort into robot policy.
    - Disadvantage of IRL:
      - However, obtaining sufficient and accurate human demonstrations is expensive and requires careful feature engineering.

Thus, the authors introduce FAPL (Feedback-efficient Active Preference Learning) for socially-aware robot navigation.

FAPL
- Active preference learning distills (simplifies the representation of the preferences and make it more usable for the robot's decision-making process) a reward model.
- The learned reward model will imitate a human teacher in the subsequent RL process to guide the robot to explore the latent space of social compliance.
- ![image](https://user-images.githubusercontent.com/83327791/218599543-dbe771c6-d0af-4e5a-a3b0-7deb7cb51e1f.png)

## Background
### Related Work:
- [6] SARL, [7] CrowdNav, and [8] used handcrafted reward function which penalizes based on distance between robot and human.
  - ![image](https://user-images.githubusercontent.com/83327791/218600421-cabc769b-0906-4d4e-b934-1572b38cc79f.png)
  - However, distance is not the only consideration in robot behavior that impacts human comfort. Thus, this handcrafted reward function is unlikely to satisfy the broad and non-qunatifiable human expectations of robots in socially compliant navigation.
  - Handcrafted reward function can also result in reward exploitation. Reward exploitation can let the robot to learn undesired and unnatural behaviors to achieve high rewards.
- IRL requires expensive human demonstrations to cover all latent aspects of social compliance.

### Active Preference Learning or Feedback Learning:
- Relies on human feedback, rather than human demonstrations to guide the learning process
- FAPL requires the following data to learn the latent aspects of social compliance:
  - Experience data: The robot collects experience data by interacting with the environment and observing its own actions and the consequences. This data is used to learn the reward model and to estimate the value of different actions.
  - Feedback data: The human teacher provides feedback to the robot about its behavior in social situations. This feedback can take the form of binary rewards or numerical scores, which indicate whether the human teacher approves or disapproves of the robot's behavior. This feedback data is used to update the reward model and to learn the latent aspects of social compliance.
  - Action data: The robot selects actions to take based on its current policy and the learned reward model. The action data represents the decisions made by the robot, and it is used to evaluate the performance of the robot and to update the reward model.

### Problem Formulation
The navigation task with humans was formulated as POMDP (partially observable Markov decision process) just as [6] SARL, [7] CrowdNav, and [8] did.
  - ![image](https://user-images.githubusercontent.com/83327791/218607082-8bdaaa12-ed54-4301-9b51-c63dfe6f02a8.png)

### Soft Actor-Critic Training Framework
Soft Actor-Critic (SAC) has 2 parts:
- Soft policy evaluation
  - ![image](https://user-images.githubusercontent.com/83327791/218607818-92c2127a-08e7-48dc-84b7-e95b3b49fe4b.png)
- Soft policy improvement
  - ![image](https://user-images.githubusercontent.com/83327791/218607869-3f1f0052-6248-4b6f-a29d-143a09e03779.png)

### Approach
![image](https://user-images.githubusercontent.com/83327791/218599543-dbe771c6-d0af-4e5a-a3b0-7deb7cb51e1f.png)
  - ![image](https://user-images.githubusercontent.com/83327791/218609242-0be4cec9-87a7-4dca-8104-c7d1ca8e58f5.png)


