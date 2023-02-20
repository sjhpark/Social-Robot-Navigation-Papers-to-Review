# Inverse Reward Design

Dylan Hadfield-Menell, Smitha Milli, Pieter Abbeel, Stuart Russell, Anca D. Dragan

2017

---

## Abstract
Key Idea:
- It is hard to design a reward function that actually captures what we want.
  - Using the same reward may result in undersired behaviors if encountering new scenrairos (e.g. new types of terrain).
- The authors introduce Inverse Reward Design (IRD).
  - Infers true reward function from proxy (designed / hand-crafted) reward function.
- IRD is a little different from IRL.
  - IRD directly observes a prixy reward function while IRL observes optimal behaviors from demonstrations.
  - Im IRD, the observed reward incentivizes approximately optimal behavior w.r.t the true reward.

## Introduction
Specifying reward functions is very challenging.

Failure modes of misspecified reward design:
- _Negative Side Effect_ (term used from Amodei et al. 2016)
  - Fails to capture important aspects and thus leads to poor behavior
- _Reward Hacking_
  - exploits undersired behavior to collect more rewards
  - e.g. a vacuum cleaner ejects collected dust to collect even more dust (more rewards).
  
Kye insights
- A robot should account for uncertainty about its reward function insead of trating the reward function as fixed.
  - This will enable risk-averse when the robot encounters is unclear about what to do when countering new scenarios.
- The robot must acquire the right kind of uncertainty.
![image](https://user-images.githubusercontent.com/83327791/220181206-5540f9fe-ddfb-4417-978e-5e51febb3155.png)

Inverse RL uses human demonstrations as observations.

3 contribbutions in this paper:
- 1. Define IRD as inferring the true reward function given a proxy (designed) reward function.
- 2. Propose a solution to IRD and justify how an intuitive algorithm which treats the proxy reward as a set of expert demonstrations can serve as an effective approixmation.
- 3. Check if combining this approach with risk-averse planning (uncertainty about proxy reward function to be risk-averse at unclear situation) is robust to misspecified rewrds.

## Inverse Reward Design
![image](https://user-images.githubusercontent.com/83327791/220191590-a79c365e-d64e-4dee-98e2-4cf1ea0a3206.png)
- ![image](https://user-images.githubusercontent.com/83327791/220192099-e1f2c37f-7fe6-469d-a4a8-a65d7633574b.png): an MDP without rewards

## Related Work
- Optimal reward design
- Inverse reinforcement learning
  - infers the reward function by observing demonstrations of optimal behavior. 

## Approximating the Inference over True Rewards
Proposed assumption: Proxy reward functions are likely to extent that they incentivize high utility behavior in the training MDP.

![image](https://user-images.githubusercontent.com/83327791/220201690-a8102ea7-9d99-4aee-8434-1dfaae2d72b9.png)
- ![image](https://user-images.githubusercontent.com/83327791/220201819-1b2460f9-178b-4e51-bab1-6e271a54adaf.png): feature vectors
- ![image](https://user-images.githubusercontent.com/83327791/220201861-9fad9e3a-18b2-4920-8e29-e496a5e651e4.png): trajectory

### Observation Model
![image](https://user-images.githubusercontent.com/83327791/220202427-cbe4e6ec-495d-47a8-ae81-fdbc96d764a3.png)

### Training MDP & Testing MDP
![image](https://user-images.githubusercontent.com/83327791/220196985-7c8ede43-3860-469c-a4b0-950ebb196445.png)

### Challenge
![image](https://user-images.githubusercontent.com/83327791/220197105-9c3b22aa-d543-4896-937e-157e650c9979.png)






