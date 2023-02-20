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
- 3. Check if combining this approach with resk-averse planning is robust to misspecified rewrds.

## Inverse Reward Design




