---
title: 'Weekly review of Reinforcement Learning papers #1'
date: 2021-03-15
permalink: /posts/2021/03/weekly-review-of-reinforcement-learning-papers-1/
tags:
  - Review
  - Reinforcement learning
  - Machine learning
  - Artificial Intelligence
gallery:
  - url: posts/2021/03/weekly-review-of-reinforcement-learning-papers-0/
    image_path: weekly_review0/mario.gif
    alt: "placeholder image 1"
    title: "Title for image 1"
---

{% include gallery image_path="weekly_review0/mario.gif" caption="my caption" %}

Every Monday, I present 4 publications from my research area. Let's discuss them!











[[← Previous review](/posts/2021/03/weekly-review-of-reinforcement-learning-papers-0/)][Next review →]

## Paper 1: Learning to Fly — a Gym Environment with PyBullet Physics for Reinforcement Learning of Multi-agent Quadcopter Control

[[Paper](https://arxiv.org/abs/2103.02142)] — Panerati J. and al.

It is quite common to see robotic environments containing robotic arms or navigation robots. But have you ever tried your learning algorithms on drones? That’s what the authors of this paper propose: an open-source OpenAI gym environment based on PyBullet for different tasks involving one or more quadricopters.

Figure from the article : Rendering of a gym-pybullet-drones simulation with 10 Crazyflie 2.x on a circular trajectory and a rubber duck for scale.
Reinforcement learning requires a large amount of data. This is why robotic environments are largely simplified. This simplification allows to collect enough data to learn a policy properly. But this comes at the cost of the realism of the simulation. For this reason, most robotic environments are not suitable for real-world deployment of learned policies. The authors tried to make the simulation as close as possible to reality by simulating the dynamic behavior of quadricopters, the cameras they carry, collisions and aerodynamic effects.

![](/images/weekly_review1/paper1_1.png)

_Figure from the paper : RGB, depth, and segmentation. A Crazyflie 2.x can be given image processing capabilities by the AI-deck._

These environments allow training on a large number of tasks: dynamic speed control, stabilization, etc. But what I find most interesting is the interface for multi-agent reinforcement learning (we are talking about drone swarms here!).

As far as performance is concerned, it varies a lot depending on whether you want to simulate the vision of drones or not. If we want to simulate the vision, it is possible to simulate an environment of 5 drones with a real time factor of 2.5. If we want to simulate without the vision, it is possible to simulate simultaneously 4 environments of 80 drones while keeping a real time factor of 0.8 (tests made with a PC with an Intel i7–8850H processor and a Nvidia Quadro P2000 graphics card).
I’m looking forward to the next publications that will expose the results obtained on real drones!

## Paper 2: Adaptive Reward-Free Exploration

[[Paper](https://arxiv.org/abs/2006.06294)] — Kaufmann E. and al.

Remember when you were a kid, you were told that

_“Reinforcement learning is learning what to do — how to map situations to actions — so as to maximize a numerical reward signal”_ — Sutton, R. S., & Barto, A. G. (2018). Reinforcement learning: An introduction. MIT press.

What if I tell you that this is not always true. What if I tell you that some work suggests that part of learning can be done without the need for a reward. The challenge of this part of the research is to discover the environment as much as possible, with a minimal number of interactions. This probably reminds you of the work of the Upper Confidence Bound (UCB) where the agent “knowledge” of the environment is quantified. Thus, agent, encourages actions for which we know the least about the outcome (see UCB from Agrawal (1995)).

Let’s get back to the point. The authors have tackled the exploration without reward. Their algorithm, called Reward-free Upper Confidence Reinforcement Learning (RF-UCRL). What is interesting is that they were able to bound the number of episodes needed to obtain a sufficiently large interaction dataset to allow the learning of any reward function. Here is the resulting equation.

![](/images/weekly_review1/paper2_1.png)

Where
- _N\_e(δ, ε)_ is the number of episodes required to output an ε-approximation of the optimal policy with probability _1 − δ_,
- _S_ is the number of states,
- _H_ the horizon (max length of an episode)
- _A_ the number of actions

I spare you the calculations, but I find it interesting that the authors were able to formally demonstrate this result mathematically. This is a rare approach in machine learning where we are often pleased to propose and observe “it works” or “it doesn’t work”.
You may have noticed (as for the UCB based algorithms) that all these results are valid for discrete finite observation and action spaces. If it is possible to discretize the action space, it is less simple for the observation space.

I admit that it is the first time that I see such an approach of exploration which remains valid for any reward function. There is a lot more to say, thanks for this great publication.

## Paper 3: The AI arena: A framework for distributed multi-agent Reinforcement Learnring

[[Paper](https://arxiv.org/abs/2103.05737)] — Staley E. and al.

The authors of this paper have produced a work that should mark a turning point in multi-agent reinforcement learning. They have extended the OpenAI gym interface that we all know to make it much more flexible, and also extend in my opinion the notion of multi-agent reinforcement learning. Here is a non-exhaustive list of what their extension allows:
The nature of the agents can vary, each agent can have its own goal, its own policy, its own learning algorithm, its own action space, and observation space. It is not a question of giving a limited observation to a certain agent. The very nature of observations and actions may differ. To take a concrete example: it is possible to have a drone (hi paper 1) that will process images and a robot arm that will process joint positions. The learned policies can be shared completely or partially within an environment, and even within several parallelized environments. We can therefore consider parallelized learning algorithms (e.g. A3C) for which the workers could be shared between these different environments.
It should be noted that this framework has been designed to be highly parallelizable, by directly manipulating the MPI standard. This guarantees that each worker policy and each environment has its own process.

![](/images/weekly_review1/paper3_1.png)

Figure from the paper : Communication groupings among AI Arena processes.

A real added value to this work is that in a few lines of code, it is possible to define quite complex environments. Check out this example from the publication.

![](/images/weekly_review1/paper3_2.png)

_Figure from the paper : Python code and process diagram for a TanksWorld training scheme against four enemy strategies._

I find the name “arena” quite well chosen, in the sense that the environment does not contain a goal in itself, but should be seen as a shared space where several heterogeneous agents can interact. I am very impressed by their work, so I invite you to go and see their publication and their code. I’m in the process of launching my first trainings myself.

## Paper 4: Image Augmentation Is All You Need: Regularizing Deep Reinforcement Learning from Pixels

[[Paper](https://arxiv.org/abs/2004.13649)] — Kostrikov I. and Yarats D. and al.

The interaction with the environment is expensive. The number of trajectories is therefore limited. To artificially increase the number of interactions, one of the methods is to learn a model of the environment, and to interact with this learned environment. This is model-based learning. This method is attractive, but in practice, some environments cannot be learned easily. So we use model-free learning. How can we increase the number of interractions if we do model-free learning? In this paper, the authors propose a method that they borrow from our computer vision cousins, called data augmentation. Data augmentation consists in enlarging an image dataset by adding distorted or noisy versions of the images that constitute it.

![](/images/weekly_review1/paper4_1.jpg)

_Data augmentation is enlarging the dataset with the flipped, rotated, cropped… versions of an image. Source image_

Surprisingly, without needing to adapt the model-free algorithms, data augmentation allows a great improvement of the results. Here are the results obtained on 3 environments:

![](/images/weekly_review1/paper4_2.png)

_DrQ[K=1, M=1] refers to SAC augmented with random shifts. You can see that, by applying a simple data augmentation, the learning performance increases significantly._

The second contribution of this publication is the regularization they propose on the augmented data. I let you consult the publication for more details. The main point is that this regularization allows to improve the performances a bit (_DrQ[K=2, M=1,2]_ on the graph).

## Bonus Paper: Human-level control through deep reinforcement learning

[[Paper](https://www.nature.com/articles/nature14236?wm=book_wap_0005)] in Nature (2015)—Mnih V., Kavukcuoglu K, Silver D. and al.

This week I want to write about a “must know”. This is the publication that introduced the Deep Q-Network (DQN) algorithm. The power of this algorithm has been demonstrated on the 49 Atari 2600 games. For the majority of the games, the results far surpass the best state-of-the-art algorithms.

![](/images/weekly_review1/paper5_1.png)

_Figure from the paper: Comparison of the DQN agent with the best SOTA RL methods. Professional human games tester = 100% ; random play = 0%_

Let us briefly revisit DQN. Here are the key points, which distinguish it from tabular methods. (1) the Q-value function is approximated with a neural network (convolutionnal). This point is essential, since it allows to generalize among states that have not been encountered, contrary to tabular methods; (2) the exploration is based on an epsilon-greedy policy. The agent takes a random action with an epsilon probability; (3) the transitions are stored in a memory (D), and the neural network is trained to predict the value function associated with a pair (state, action). The well-known algorithm is thus:

![](/images/weekly_review1/paper5_2.png)

It is impressive to realize that this publication is only six years old. So much progress has been made since then. Research is going fast, new great results are appearing regularly and this publication is a testament to that.

---

It was with great pleasure that I presented you my four readings of the week. I hope you will enjoy this second edition. I am looking forward to your feedback.



