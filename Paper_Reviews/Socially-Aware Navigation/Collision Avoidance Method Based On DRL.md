# A Collision Avoidance Method Based On DRL

https://www.mdpi.com/2218-6581/10/2/73

**Abstract**

- Collision Avoidance via DRL for compact spaces (e.g. narrow corridor)
- The authors used DRL to understand the environment based on real-time sensor readings to perform local path planner for obstacle avoidance.

**Intro**

- DRL = RL + DL
    - NN is used to approximate value function.
        - Enables an agent to learn from scratch by itself without experiences and supervisions from humans
        - DRL approach requires a large dataset to train the NN — As the solution, the author developed a simulated environment in Gazebo [10] to collect data.
- Mobile robot navigation, especially obstacle avoidance, is a popular area where DRL becomes solutions.
- Common problem in SOTA methods:
    - SOTA methods often use triggering distance (triggers the obstacle avoidance maneuver within a specific distance), but it is often different from the max range of the sensor — Leads to an improper inference of the environment and thus commands the robot to navigate inefficiently.
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0f33c458-17f2-4156-a52c-e5cb2ac304d4/Untitled.png)
        

**Related Works (SOTA autonomous navigation techniques)**

- Autonomous Navigation
    - Autonomous navigation problems:
        - Localization
        - Path Planning
            - Global path planning
                - A* search algorithm, etc.
                - Cons- Prior knowledge of the environment in the form of a map is required.
                - Cons - If the map changes or is not accurate, the derived path will not be reliable.
            - **Local path planning**
                - The authors used local path planners for obstacle avoidance based on real-time sensory info.
        - Obstacle Avoidance
    - Drawbacks of based methods:
        - The robot can get trapped in a local minima, away from the goal.
        - The robot may not find a path between two closely spaced obstacles.
        - The robot may oscillates when trying to move through narrow passages.
        - Modified version of Virtual Force Field (VFF) method [21] and Virtual Field Histogram (VFH) [22] were used as solutions.
            - Algorithms such as VFH based path planner that  rely solely on triggering distance — Can lead to an improper inference of the environment and thus commands the robot to navigate inefficiently
- DRL methods in Robotics
    - DDPG (Deep Deterministic Policy Gradient)
    - NAF (Normalized Advantage Functions)
    - Asynchronous DRL w/ a Parallel NAF [4]
    - Model-free DRL w/ Soft Q-Learning [30]

**Collision Avoidance Approach**

- Collision avoidance problem is defined as MDP and can be solved using DRL.
- The authors’ approach: DRL & Local path planner
    - input: observations from sensors
    - Doesn’t require a prior map info nor a precise map
- **Problem Formulation:**
    - (vt,wt) = f (Ot)
        - Commands: v: linear velocity, w: angular velocity
        - O: observation from sensor
- **Evaluation on 3 DRL Methods:**
    - DQN
        - cons: overestimation
    - DDQN
    - DDQN w/ PER (Prioritized Experience Replay)
        - Samples the experience based on the priority of each experience in the replay memory.
        - Converged to a certain reward faster than DDQN, but achieved a lower reward in the end. Thus, the authors developed the obstacle avoidance method based on DDQN.
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/756f5c9a-a5c0-497e-a2ec-029737bcbfdc/Untitled.png)
            
- **Collision Avoidance w/ DRL**
    - Defined the collision avoidance problem as MDP.
        - MDP, represented as a tuple <S, A, P, R, gamma>
        - NN with two hidden layers is used to estimate the action-value function Q(s,a; theta)
            - theta: weights
    - State S is formed based on observations from LIDAR sensor (st = Ot).
    - Immediate reward
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c941fcde-014a-411d-862d-b9f833897282/Untitled.png)
        
    - Decaying greedy policy
        - The robot chooses an action based on decaying epsilon-greedy policy
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/13898d98-414d-404e-b180-d6db7590f4a0/Untitled.png)
            
            - beta: decay rate; k: time step
            - epsilon decreases from 1 and will stop decay when it reaches 0.05

**Conclusion**

- More exploration in training —> Stably increased rewards & slow training speed
