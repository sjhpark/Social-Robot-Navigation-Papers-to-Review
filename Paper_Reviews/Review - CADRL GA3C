# Motion Planning Among Dynamic, Decision-Making Agents with Deep Reinforcement Learning

- In contrast to the previous methods (CADRL, SA-CADRL), GA3C-CADRL **does not assume
the behavior of other decision-making agents**.
    - Existing approaches make subtle assumptions about other agents such as homogeneity or a specific motion model over short timescales.
- GA3C-CADRL also uses **LSTM** to enable the algorithms to make decisions based on **“arbitrary number (any number)” of other agents** (CADRL and SA-CADRL use fixed number of agents).
    - This mimics the real world where any number of objects can be present.
    - The LSTM model outputs a single final hidden state h_n. This final hidden state encodes the entire state of the other agents in a fixed-length vector.

- GA3C:
    - GPU/CPU A3C
    - A3C: a set of A2C
        - Allows multiple agents explore and experience by themselves
        - Combines policies and value functions obtained from each agent’s A2C
