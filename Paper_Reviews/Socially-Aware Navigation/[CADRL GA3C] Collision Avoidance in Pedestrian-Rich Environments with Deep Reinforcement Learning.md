# Collision Avoidance in Pedestrian-Rich
Environments with Deep Reinforcement
Learning

**Abstract—**

- Cons of existing RL-based works:
    - Assume homogeneity of agent properties
    - Use specific motion models over short timescales
    - Lack a principled method to hand a large varying number of agents
- Thus, this paper:
    - Develops an algorithm that:
        - Learns collision avoidance among a variety of **heterogeneous**, **noncommunicating**, **dynamic** agents without assuming they follow any particular behavior rules.
        - Extends previous work by using **LSTM** that enables:
            - Observations of an arbitrary number of other agents, instead of a small, fixed number of neighbors
    
    **Intro—**
    
    - Challenges: Other agents’ intents and policies (i.e., goals and desired paths) are typically unknown to the planning system.
    - These issues motivate the use of decentralized collision avoidance algorithms.
    - Decentralized collision avoidance:
        - non-cooperative method
            - First predict the other agents’ motion
            and then plan a collision-free path for the vehicle with respect to the other agents’ predicted motion.
            - prone to freezing robot problem
        - cooperative method
            - Uses reaction-based methods & trajectory-based methods
            - Models interaction in the planner, such that the vehicle’s action can influence the other agent’s motion, thereby having all agents share the responsibility for avoiding collision
    - Key Challenges:
        - the number of other agents in the environment can vary btw timesteps or experiments
            - —> Relaxing the assumptions on the other agents’ behavior models improved performance
        - finding a policy that makes realistic assumptions about other agents’ belief states, policies, and intents (without assuming that the other agents follow any particular behavior model)
            - —> Using LSTM to encode spatial representations (rather than temporal) the address that the number of neighboring agents could be large and vary in time.
    
    **Background—** 
    
    **A. Problem Formulation**
    
    - Non-communicating multiagent collision avoidance problem
        - can be formulated as a sequential decision making problem
        - objective: minimizing expected time to goal
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5ec08cf8-6f56-4537-8fbf-6173b0377761/Untitled.png)
        
    
    **B. Related Work**
    
    - Model-based Approaches:
        - Reaction-based methods:
            - Use one-step interaction rules based on geometry or physics
            to ensure collision avoidance (Optimizes a one-step cost while satisfying collision avoidance
            constraints.).
            - Does not know about other agents’ hidden intents, so the agent just reacts quickly to other agents’ changes in motion.
            - Often are based on Markovian policy (history does not matter).
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fd43a478-a2e2-4aba-a799-f82b772ffa96/Untitled.png)
            
        - Trajectory-based methods (computationally expensive):
            - Compute plans on a longer timescale to produce smoother paths
            - A subclass of non-cooperative approaches **propagates the other agents’ dynamics forward in time** and
            then **plans a collision-free path with respect to the other
            agents’ predicted paths**.
            - In crowded environments,
            the set of predicted paths could occupy a large portion of
            the space, which leads to the freezing robot problem.
                - A subclass of cooperative approaches has been proposed as a solution.
                    1. other agents’ hidden states (goals) are inferred from their observed trajectories
                    2. a centralized path planning algorithm is employed to find jointly feasible paths. 
                    
                    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2dc4a675-a9af-43d8-a752-619737bec70c/Untitled.png)
                    
    - Learning-based Approaches:
        - RL framework
    
    **C. Reinforcement Learning**
    
    - RL is for solving sequential decision making problems with unknown state transition dynamics.
        - Typically, a sequential decision making problem can be formed as a Markov Decision Process (MDP).
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aa549673-e2a7-418a-8053-80bcb7ec142b/Untitled.png)
        
        - S: State space
        - A: Action space
            - nature choice of action space: u = [linear speed, angular speed]
        - P: State-transition model (Probabilistic state-transition model)
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f00084eb-d48b-4216-a4f2-b49df9de3662/Untitled.png)
            
            - The authors’ previous work avoid the complexity in modeling state-transition model by assuming that other agents continue their current velocities for a duration delta T.
            - The author’s latest work do not make any arbitrary assumptions about state transition dynamics.
        - R: Reward function
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8c66cfe8-2b67-4380-9fc6-39cbfc9bcbe2/Untitled.png)
        
        - gamma: Discount factor
        - Optimal Value Function
            - Expected cumulative rewards
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0f0d1a77-1f85-48d6-b8a1-939ecc63e2f4/Untitled.png)
        
        - Deep RL:
            - It is common to use RL with DNN to estimate high-dimensional continuous value function and/or policy.
        - Decision-making Policy
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5d8d7c86-5ac5-489d-9061-9694794a80de/Untitled.png)
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/957ab39d-b1c1-499a-97c6-c9a1095031f8/Untitled.png)
            
        - The authors use GA3C (hybrid GPU/CPU A3C (Actor-Critic))

**Approach—**

- **GA3C-CADRL (GPU/CPU Asynchronous Advantage Actor-Critic for Collision Avoidance with Deep RL) :**
    - RL: RL training process seeks to find the optimal policy which maps from an agent’s
    observation of the environment to a probability distribution across actions and executes the action with highest probability.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0aaae115-654d-46f8-9064-699f19accc4d/Untitled.png)
    
    - The authors separated the state of the world in two:
        1. Info about the agent itself
        2. Everything else in the world
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fb4d74e3-ce8f-4a7d-b88a-45a8301c1f63/Untitled.png)
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27895382-05e8-4d42-b623-ae9c699bb4bc/Untitled.png)
        
    - This multiagent RL problem formulation is solved with GA3C in a process called GA3C-CADRL.
- **Handling Multiple Agents:**
    - Feedforward NNs or CNN require a fixed-size input.
    - Thus, the authors used **RNN** with **LSTM**. RNN accepts an **arbitrary-length sequence inputs** to produce a fixed-size output.
        - a sequence of inputs: a variable number of observable states of ith other agent
        - LSTM receives inputs
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7f16c597-8ffa-4384-ad51-e1f3db5dddc7/Untitled.png)
            
        - LSTM produces outputs
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/853b0580-38a7-4b2f-999e-5a92374d44d5/Untitled.png)
            
        - Then, h_n is passed to the next network layer for decision making.
        - **LSTM & Network Architecture**
            - **LSTM takes other agent’s observable states as inputs sequentially by the order of decreasing distance. The further agent’s observable state will be fed the the further cell in the LSTM. Thus, the closest agent’s observable state is fed to the closest cell in the LSTM and has the most effect on h_n.**
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/565b6954-fe20-490f-ae80-210b0194d131/Untitled.png)
            
- An important benefit of the new framework (GA3C CADRL) is that the policy can be trained on scenarios involving **any number of agents**, whereas the maximum number of agents had to be defined ahead of time with previous work (CADRL / SA-CADRL).
