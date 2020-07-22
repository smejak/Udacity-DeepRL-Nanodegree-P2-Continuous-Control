# Project 2 of the Udacity Deep Reinforcement Learning Nanodegree
## Learning Algorithm
### DDPG
The learning algorithm used in this project is the Deep Deterministic Policy Gradient (DDPG), first introduced in the paper [Continuous Control with Deep Reinforcement Learning](https://arxiv.org/pdf/1509.02971.pdf). 
DDPG is an actor-critic method, which makes use of two neural networks for approximating and evaluating an optimal policy. The actor learns to deterministically approximate the optimal policy and the critic learns to evaluate the optimal action-value function by using the best action according to the actor. DDPG can be thought of as a type of DQN which is optimized for continuous action spaces.
### Experience Replay
The agent makes use of a replay buffer from which past experiences are sampled. This leads to better data efficiency as a single experience can be used multiple times and also results in better learning, because correlation from consecutive timesteps is diminished.
### Soft Updates
As in the DQN algorithm, DDPG uses a local and a target network (for both the actor and the critic), however, the target networks are updated using the so called soft updates, which have been shown to lead to faster learning convergence. At every timestep, a small percentage of the local networks' weights are passed to the target networks.
### Batch Normalization
As described in the [paper](https://arxiv.org/pdf/1509.02971.pdf), to avoid the issue that different dimensions of the state space may have different units and scales, I adopted the batch normalization technique by adding batch normalization layers to both actor-critic neural networks.
### Ornstein-Uhlenbeck process
The Ornstein-Uhlenbeck process was used to add noise to the action space for better exploration efficiency.
![](/algo.png)
## Implementation
The implementation of the DDPG algorithm can be found in __ddpg_agent.py__. The neural network architectures for the actor and the critic can be found in __model.py__. The networks were constructed using PyTorch. Both networks have two fully connected layers of 128 units and two layers nn.LayerNorm() for batch normalization. The input to the networks is the state size, which for this environment is 33 nodes. The output of the actor network has 4 nodes corresponding to the action size and for the critic network 1 node.
### Hyperparameters
The hyperparameters for the DDPG agent can be found in __ddpg_agent.py__.
Parameters used for the training function are:
* maximum number of timesteps per episode: 1000
## Results
```
Episode 100	Average Score: 28.85
Environment solved in 104 episodes! Average Score: 30.289049322986976
```
![](/graph.png)
The results show that the environment has been successfully solved as the average score over 100 consecutive episodes for all 20 agents reached +30.
## Ideas for Future Work
Altough the agents managed to learn very effectively, I think there is definitely room for improvement. For instance, the hyperparameters used in __ddpg_agent.py__ are the same as in Udacity's pendulum notebook and could be altered for better performance. In addition, the [paper](https://arxiv.org/pdf/1509.02971.pdf) mentions the Ornstein-Uhlenbeck noise was added to the network policy whereas this implementation only adds it to the action space. Furthermore, I began this project with a single agent implementation in mind, however, I was unable to make the DDPG agent to train, therefore I could try implementing other actor-critic methods such as A3C, A2C, or D4PG which might work better.
