# Socially aware robot navigation in crowds via deep reinforcement learning with resilient reward functions

Xiaojun Lu, Hanwool Woo , Angela Faragasso , Atsushi Yamashita and Hajime Asama

2022

---

Key Ideas:
- Previous DRL-based crowd robot navigatiom methods used simply defined reward functions.
  - e.g.  ![image](https://user-images.githubusercontent.com/83327791/221377338-ff52cff5-c867-4fbf-966a-330c07ad48a4.png)
  - Any distance between collision (d_t < 0) and uncomfortable distance (d_t <= 0.2m), the robot gets the same penalty -0.1.
- The authors proposed to use a resilient reward function (R2F) where the robot gets more penalty if its is more closer to the human.
  - ![image](https://user-images.githubusercontent.com/83327791/221377416-6ad5ff0f-460d-49ca-a06b-0569223e0cbf.png)
  - Introduced _uncomfortable distance_ which is determined by the pedestrian density.
    -  ![image](https://user-images.githubusercontent.com/83327791/221377463-72a6b0e9-545c-43d2-91fb-90d51710c59b.png)
    -  While within the uncomfortable distance defined by the pedestrian density, robot gets more penalty if it is closer to the pedestrian.
  - DRL with this resilient reward function outperformed previous DRL methods with simple reward functions.
