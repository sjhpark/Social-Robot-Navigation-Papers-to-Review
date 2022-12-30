# Human-Aware Robot Navigation: A Survey

### Thibault Kruse, Amit Kumar Pandey, Rachid Alami, Alexandra Kirsch
### 2013
---

## Abstract
For navigation, the presence of humans requires novel approaches that take into account the constraints of human comfort as well as social rules.

## 1. Introduction
Human-aware navigation is the intersection between research on human robot interaction (HRI) and robot motion planning.
* HRI is a young sub-domain while robot motion planning is an established engineering domain.
* Navigation for robot: Change of the position of its body to move to a different place
  * This is called "base motion".
  
Explicit Interaction
* Explicit interaction happens when an action is made for the purpose of achieving a reaction by the partner.

Implicit Interction
* Implicit interaction happens when actions are modified due to the (potential) presence of a partner, but no reaction
is intended.

### 1.1 Backgound and Scope
Previous works of autonomous robot navigation in human environments:
* Had navigation moudles that prevented collisions with humans or other obstacles using robust navigation strategies such as:
  * minimal required obstacle distance
  * stop-and-wait behavior in situations of conflict
  
## 2. Challenges of Human-aware Navigation (Types of challenges comfort, natural movement and social aspects)
Research on human-aware robot navigation follows different goals:
* Minimizing annoyance and stress to humans
* Robots behaving more natural
* Robots behaving according to cultural norms

**Comfort**
* Comfort is the absence of annoyance and stress for humans in interaction with robots.

**Naturalness**
* Naturalness is the similarity between robots and humans in low-level behavior patterns.

**Sociability**
* Sociability is the adherence to explicit high-level cultural conventions.
* e.g. moving on the right in the corridor

### 2.1 Human Comfort (Comfort)
* Large part of publications approaches Human Comfort by avoiding negative emotional responses (such as fear or ange)
* Human Comfort requirement is mostly inspected regarding a distance between a robot to human.
  * This distance is not only about collision avoidance but also about preventing emotional discomfort of humans.
* "Proxemics" was the concept that was prposed by Edward T. Hall [32].
  * Proxemics is a virtual space around a person that humans mutuall respect.
  * Different social distances depend on the relationship and intention. ![image](https://user-images.githubusercontent.com/83327791/210077078-10a0b750-ebe7-4546-81e4-6a67843247eb.png)

### 2.1.2 Techniques to Improve Comfort
The most common approach to avoid discomfort due to small distance is to define areas around humans as cost functions or potential fields [34, 44, 83, 89, 91].

The comfort aspect of navigation deals with eliminating obvious causes of discomfort. Beyond that, the interaction between robots and humans can still be improved.

### 2.2 Natural Motion (Naturalness)





