# A Deep Learning Approach To Multi-Context Socially-Aware Navigation

**Abstract**

- **Contexts**: Classes such as art gallery, hallway, living room, etc.
- **Proposal**: Context classification to allow a robot to change its navigation strategy based on the observed social scenario
- Socially-Aware Nav considers social behavior to improve navigation around people.
    - Most existing research incorporates social norms into robot path planning for a single context.
        - Methods working for hallway behavior might not work for other context (e.g. approaching people)
    - The authors developed a high-level decision-making subsystem, a model-based context classifier, and a multi-objective optimization-based local planner to achieve socially-aware trajectories for autonomously sensed contexts.
        - Using a context classification system, the robot can
        select social objectives that are later used by Pareto Concavity Elimination Transformation (PaCcET) based local planner to generate safe, comfortable and socially-appropriate trajectories for its environment.

**Intro**

- Human-human interpersonal navigation behavior is governed
by social rules, which depend heavily on the environmental
context.
- Context-aware social behavior related to navigation is important for a successful human-robot interaction (HRI).
- The authors propose using CNN and SVM to detect the on-going interaction context.
- Using autonomous context classification and a PaCcET (Pareto Concavity Elimination Transformation)-enabled local planner, socially-aware navigation behaviors not just for a single context but for multiple contexts can be achieved.

**Related Work**

- Social Force Model (SFM):
    - Uses social forces to consider pedestrians near a robot as external-projecting forces.
    - Detects abnormal behaviors in a crowd by classifying behaviors as normal and abnormal.
        - Context-specific
- DRL in motion planning (that accounts for social norms)
    - e.g. CADRL
    - DRL and IRL methods for path planning problems need a considerable amount of data, computational time, and memory for a single context.

**Approach**

- Social contexts (group formations, waiting in queue, etc.) are difficult to be studied by 2D cameras, so laser scan data was used to detect and track people in a scene.
- The positions of the tracked people were used to calculate the following features which were later used in training a linear SVM to distinguish between waiting in line and group formations:
    - Circularity:
        - Represents people forming a group
        - How close a set of points should be to a true circle.
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/900e53f4-88a5-428c-ad79-31a08bb9132a/Untitled.png)
        
    - Linearity:
        - Represents people forming a line
        - The property by which a set of points can be graphically represented as a straight line
