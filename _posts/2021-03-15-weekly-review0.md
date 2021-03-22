---
title: 'Weekly review of Reinforcement Learning papers #0'
date: 2021-03-15
permalink: /posts/2021/03/weekly-review-of-reinforcement-learning-papers-0/
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

<!-- ![](/images/weekly_review0/mario.gif) -->
{% include gallery caption="test" image_path="weekly_review0/mario.gif" %}

Every Monday, I present 4 publications from my research area. Let's discuss them!

## Paper 1: First return, then explore

[[Paper](https://www.nature.com/articles/s41586-020-03157-9)] in Nature -Ecoffet A., Huizinga J. and al.

Let's start with a very important article, recently published in Nature. The authors tackle a fundamental problem in reinforcement learning: exploration.
They suggest that one of the difficulties to achieve an efficient exploration is the difficulty to return to an interesting state. Example: an agent manages to reach Mario's advanced state, and thus gets a high reward. Nevertheless, in the next episode of training, this agent is not able to return to this state to continue the exploration of the game. This reminds me a lot of the false hope we have when we see the Cartpole staying straight for a whole episode and then start a failure series again.

They introduce a class of algorithms they call Go-explore, in which an agent is specially trained to return to a given state. The agent builds an "archive" of the visited states. Then, entering in the paradigm of goal-oriented environments (see HER), the agent is trained to return to return to an arbitrary state chosen in its archive (a). Once trained, the agent can return to a desired state (b), and especially, it can return to a state that led to a high reward. It will then be able to continue exploring (c) from that state.

![](/images/weekly_review0/paper1_1.png)

Overview of Go-Explore.Does it work? It works very well: Go-explore outperforms humans in the latest Atari games that were still out of reach of DRL algorithms. It far outperforms the most recent DRL algorithms for other Atari games. Their method also enables to learn efficiently in environments where rewards are very sparse (robotic tasks among others).

![](/images/weekly_review0/paper1_2.png)

Historical progress on Montezuma's RevengeOther interesting things (like the notion of cell) are present in this paper I strongly advise you to go and study it. Congratulations to them.

## Paper 2: Mastering atari with discrete world models

[[Paper](https://arxiv.org/abs/2010.02193)][[Blog post](https://ai.googleblog.com/2021/02/mastering-atari-with-discrete-world.html)] - Hafner D. and al.

Model-based learning methods are sexy, but in practice, little used. It is often very complex to build a reliable model of the environment, especially when very close observations do not mean close state (non-Markovian model).

The authors of this paper have introduced Dreamerv2, a new model-based algorithm that outperforms the best IQN and Rainbow agents. How did they do that? Instead of learning the model directly from the pixels, they learn it from a compact latent vector. Give me a few lines to explain.

![](/images/weekly_review0/paper2_1.png)

Learning process of the world model used by DreamerV2First step: they build a compact representation of the environment from autoencoders and recursive networks. The goal in this first step is to obtain a compact latent vector (z) that best describes the state of the environment and a model. Once the convolutional network yields a satisfactory latent vector, they can train a model-based reinforcement learning algorithm that will take as input (not the pixels, but) this latent vector, and that will interact (not with the real environment, but) with the learned model. The advantage being to reduce considerably the cost of the data, since it comes from a more compact latent space.

The results are quite impressive. In the following gif, the model takes 5 frames as input and predicts the next 45 frames. Can you tell the difference between the model and the reality?

![](/images/weekly_review0/paper1_2.png)

Video predictions of the world model for holdout sequences. I didn't mention the other interesting idea they had: the use of categorical variables for the latent space. To know more about it, I invite you to study the article.

## Paper 3: Resource Management in Wireless Networks via Multi-Agent Deep Reinforcement Learning

[[Paper](https://ieeexplore.ieee.org/abstract/document/9329087?casa_token=DdBIAwlfaRkAAAAA:UZ-ibcsBNKba8WFr5O8Vupss1BCNSJ5It9knYx9IP7sTDw0gq6aUwTgATdtwprINx2aU0dAmn7mx4g)] - Naderializadeh N. and al.

Well, what's the point of reinforcement learning other than playing games? Here is an application I really like: distributed resource management in a wireless network. The network must serve the users in the best way possible through a network of transmitters. Each transmitter is considered as an agent in the multi-agent reinforcement learning paradigm  (I will call them agent-transmitters). These agent-transmitters receive observations from the users they connect to. It also shares observations with other neighboring agent-transmitters. And by correctly choosing the user to serve at each moment, it is possible to maximize the overall performance of the network.

![](/images/weekly_review0/paper3_1.png)

Deep MARL diagram.Wait, how do these networks manage resources nowadays? Well, in general, they use a centralized algorithm, often based on geometric programming or game theory. The advantage of the approach proposed here is to decentralize the  resource management. The agent-transmitters make decisions about resource management without having to know the decisions of all the other agent-transmitters. The goal is to make the system as a whole more robust to an evolving geometry.

This approach has only been tried in simulation. Although the proposed approach can exceed the performance of other decentralized approaches, it struggles to exceed the performance of centralized methods. I consider this to be an encouraging sign for the development of adaptive networks with distributed management! Thank you RL.

## Paper 4: S4RL: Surprisingly Simple Self-Supervision for Offline Reinforcement Learning

[[Paper](https://arxiv.org/abs/2103.06326)] - Sinha S. and al.

It is often said: the cost of interaction with the environment is high. The most common example is probably the autonomous car: we can't afford to crash a car at each learning episode. For this reason, offline learning methods should be considered. An offline learning method allows to train a policy just by "observing" another agent interacting with the environment. To take the example of the autonomous car: using an offline learning method, a policy can improve just by watching me drive the car. By "watching" I mean receiving observations and rewards, but not deciding on actions. Another advantage of offline learning is that it allows to take advantage of previously collected data, for example from a dataset.

It is important to note that data from a dataset is never as good as data from a real interaction with the environment (nothing beats practice!). The risk is therefore that the agent performs too well on the data from this dataset, but bad when it interacts with the real environment. There is a special name for that : out-of-distribution generalization.

The authors have tried to reproduce the different methods of data augmentation that are widely used in the field of vision. Surprising conclusion: adding a simple Gaussian noise on the observation often gives better results than a mix of several data augmentation methods (zero-mean Uniform noise; random amplitude scaling; dimension dropout; state-switch).

Then, they compare the performance of the RL agent that learned on this augmented dataset, and compare it to recent off-policy learning variants of conservative Q-learning (CQL): CURL and VAE. The second surprising result is that S4RL achieves better performance, while being much simpler than its competitors.

I hope that the results obtained in this paper can be reproduced on other environments. It's a paper that makes me think a lot of this sentence of Einstein that I like: "Everything should be made as simple as possible, but no simpler".

## Bonus Paper: Measurement of gravitational coupling between millimetre-sized masses

[[Paper](https://www.nature.com/articles/s41586-021-03250-7)] in Nature - Westphal T. and al.

Do you know the four fundamental forces in physics? The weak interaction, the strong interaction, the electromagnetic force and the gravitational force. For physicists, 4 is too many, much too many. Their dream, their grail, is a short equation that would unify all these forces into a "theory of everything". The standard model has already succeeded in unifying 3 of them: the weak, strong and electromagnetic interactions. But for the moment, nobody has succeeded in integrating the gravitational force into this model.

A new small step in this direction has been published in Nautre. This is the article I would like to write about.
If the gravitational force is easily observable between macroscopic bodies, it is much more complex to observe it between two microscopic bodies. This is why it is largely neglected in the physical equations that apply to microscopic bodies.

For the first time, the authors have observed the gravitational coupling of two bodies of only a few tens of milligrams (90 mg). At this scale, the gravitational force is only a few femto-Newtons. The challenge of such an experiment is to isolate this gravitational force from the ambient noise and electromagnetic forces, which at this scale become more important than the latter.

![](/images/weekly_review0/paper4_1.png)

How to detect a weak gravitational field. Westphal et al.Here is the experimental setup: two 90-milligram masses are attached to each end of a bar suspended from a wire. A third 90-milligram mass periodically moves toward and away from one end of the bar. The gravitational interaction causes the bar to rotate around the axis of the wire. This rotation is measured by a laser reflected on a mirror placed in the center of the bar.

Result: the authors have effectively detected the gravitational interaction between these two masses, however small they may be. A proof that the formulas established by Newton in 1687 remain valid, even for such small objects.
As if to crown this exciting result, the authors also deduced from their experience a value of G which differs only slightly from that agreed at the international level.

Can't wait for the next episode.

---

It was with great pleasure that I presented you my four readings of the week. I hope you will enjoy this first edition. I am looking forward to your feedback.
