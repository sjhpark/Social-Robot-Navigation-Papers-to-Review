# Learning Reward Functions by Integrating Human Demonstrations and Preferences

Malayandi Palan*, Nicholas C. Landolfi*, Gleb Shevchuk, Dorsa Sadigh

2019

---

## Abstract
Key ideas
- Current approaches to learn reward functions:
  - IRL (Inverse Reinforcement Learning)
    - difficult to get high-quality demonstrations
  - Preference-based Learning
    - inefficient since it learns a continuous high-dimensional function from binary feedback / preference (e.g. good or bad)
- Proposal:
  - Want to learn accurate reward functions efficiently.
  - DemPref: uses both demonstrations and preference to learn a reward function.
    - use the demonstrations to learn a coarse prior over the space of reward functions (to reduce the effective size of the space from which queries are generated)
    - use the demonstrations to ground the (active) query generation process (to improve the quality of generated queries)
    - alleviate the efficiency issue faced by standard preference-based learning methods

## Introduction
It is hard to handcarft a reward function that perfectly encodes the robot's desired behavior in every aspect
- can lead to undesired and even dangerous behavior

### Current approaches to learn a reward function without handcrafting
- Inverse Reinforcement Learning (IRL)
  - learns a reward function directly from expert demonstrations
  - requires high-quality demonstrations
    - not feasible (e.g. demonstrations of high DOF robotic arm control is not feasible)
- Preference-based Learning
  - learns a reward function by repeatedly asking a human to pick between two options
  - inefficient since it learns a continuous reward function from binary feedback

### Proposed approach (DemPref): Learning a reward function from both demonstrations & preference
Key insight: demonstrations and preferences are complementary.

![image](https://user-images.githubusercontent.com/83327791/220206293-5d6c6ea2-970d-4834-9059-e74ff32f6d95.png)
- Use IRL to learn a prior over reward functions from demonstrations.
  - This significantly reduces the size of the space of possible reward functions.
- Use preference-based learning with the learned prior to find the true reward function.

## Background on Reward Learning
Inverse Reinforcement Learning (IRL)
- learns reward functions from experts (mostly humans) demonstrations.
- demonstrations may not accurately describe how the human wants the robot to behave.
- human needs to teleoperate the robot.
  -  difficult to demonstrate with high DOF robot.

Imitation Learning (IL)
- directly learns a policy from experts demonstrations, which bypasses learning the reward function.

Preference-based Learning
- learns a reward function from querying the human for her preference between two options (e.g. two trajectories of robot)
- might represent the human's true reward function more accurately than demonstrations since it is easier for humans to pick between two options than demonstrating a full length behavior
- however, it is less informative than demonstrations.

Active Preference-based Learning
- active preference-based learning alleviates the less information issue by generating the most informative query at each step.
- however, this requires solving an optimization problem over control space at each step which is inefficient where control space is complex high-dimensional.

## Approach Overview and Modeling Details of DemPref
Stages to learn the reward function:
- 1. Human provides demonstrations --> Initialize a probabilistic distribution of reward functions --> Use this learned probabilistic distribution as a prior over reward functions.
  - reward distribution (probability distribution of reward function): ![image](https://user-images.githubusercontent.com/83327791/220210442-5a535ca0-56bc-4eb9-987a-399cfb1c220c.png)
    - human demonstrations: ![image](https://user-images.githubusercontent.com/83327791/220210479-cededf6b-dfd9-4919-a2cb-c80a9cb84e61.png)
    - weight vector: w
  - introduce noisiness in human behavior by assuming the human provides demonstrations according to a Boltzmann distribution over trajectory space: ![image](https://user-images.githubusercontent.com/83327791/220210671-d1594a02-f51f-4a26-8a90-9cf2e1c68fdc.png)
    - parameter that captures the level of human's suboptimality when providing demonstrations![image](https://user-images.githubusercontent.com/83327791/220210702-2b7e4e73-5d7a-433c-a14b-e2d435b59fdb.png)
  - desired distribution over w via a Bayesian update: ![image](https://user-images.githubusercontent.com/83327791/220210881-2a983126-1f27-44e9-a434-1a043f179bf7.png)
- 2. Actively query the human for its preference between two generated options to learn the true reward function.
  - probability that human will choose trajectory 1 over trajectory 2: ![image](https://user-images.githubusercontent.com/83327791/220211404-c570c0f1-d11f-4e45-b1ec-28835840a6a0.png)
  - generating maximally-informative queries: ![image](https://user-images.githubusercontent.com/83327791/220211598-8362271e-21de-4179-9ce4-356be3bfc033.png)

To maximize the amount of information extracted from each query for preference, the human will be queried for a ranking (instead of preference) of the trajectories.

Multiple human demonstrations will be stored in a buffer, and random samples of the demonstrations will be used in the next query.
- At the end of each iteration, the human's most preferred trajectory replaces the selected demonstration in the buffer.

Reward Function
![image](https://user-images.githubusercontent.com/83327791/220209649-afbac443-ad3f-4241-b95f-505cc9418035.png)
- trajectory ![image](https://user-images.githubusercontent.com/83327791/220209722-8d199ac4-53a3-4cb1-b117-3394667cd259.png)
  - x: state
  - u: action
