# Reinforcement Learning with VizDoom Environment

This repository contains the code and resources for training and evaluating Proximal Policy Optimization (PPO) models from the Stable-Baselines3 framework, and using the VizDoom environment. Three different scenarios were used for training the models: a basic scenario, a defend the center scenario, and a deadly corridor scenario.

# Preprocessing

* Wrap the DoomGame() environment into a Gym environment
* Grayscale the image
* Reduce the size of the observation image into a 100 x 160 image
* Trim the lower part of the observation image to remove the infos at the bottom of the screen : 85 x 160 image

# Training the models

For each scenario, I have trained a PPO model using the CNN-policy implemented by the Stable-Baselines3. If the two first scenarios remain basic and easy to train, the last model required a very careful fine-tuning of the hyperparameters.

## Scenario n°1 : Basic

In the basic scenario, the agent has 3 possible actions : move to the right, move to the left and shoot. The goal is to kill the ennemy in the environment, which does not move or attack back.

### Trained Model : 

I have trained the model for 100000 steps and the model has successfully learnt the optimal policy. Indeed, the agent immediately moves to the direction of the ennemy and shoot when it is the best moment (which ends the level). Here's a visualization of the agent's performance :

![Demo](demo_basic.gif)

## Scenario n°2 : Defend the Center

In the Defend the Center scenario, the agent has also 3 possible actions : turn to the right, turn to the left and shoot. The aim is to eliminate the monsters that approach us, to survive as long as possible, until our ammunition inevitably runs out.

### Trained Model : 

I have trained the model for 200000 steps but due to instabilities, the best model I've obtained is the one I trained for 160000 steps. The model captured a good understanding of the environment and is able to aim the ennemies, even the ones that are very far. It is not optimal yet because the trained agent still wastes some ammunition but still performs very well as you can observe :

![Demo](demo_defend_center.gif)

## Scenario n°3 : Deadly Corridor

In the Deadly Corridor scenario, the agent has now 7 possible actions : move left, right, forward, backward, turn left and right and shoot. The aim is to reach a shield bonus at the bottom of the level. However, the level is defended by enemies that must be eliminated to survive. 

### Trained Model : 

Here, to facilitate agent training, I've introduced two new mechanisms : Reward Shaping and Curriculum Learning

* Reward Shaping consists in modifying the function of the rewards perceived by the agent to help him better understand which actions are truly positive and which are not. In this case, the initial reward function only takes into account the distance to the bonus we wish to recover. To encourage the agent to defend himself and eliminate enemies effectively, I've added a positive value to the reward on damage caused to enemies, and a negative value on ammunition wasted and health points lost. 

* Curriculum Learning involves training our model in increasingly difficult environments. At first, the model understands nothing about the environment, so we have to help it discover what's good, in this case killing enemies and collecting the bonus. Then the difficulty is increased, so that the model can effectively carry out the positive actions it has discovered. Without this mechanism, our novice agent would be killed too quickly and would not be able to effectively learn how his environment works. To implement this aspect, I have tweaked the level difficulty from 1 to 5 when training : the ennemies where shooting more and more efficiently against us as the difficulty increases.

I have trained the model for 200000 steps (40000 for each difficulty level). This environment remains much more complex than the previous ones due to the possibility of moving and the higher number of possible actions. Therefore, a lot of hyperparameter tuning was necessary. The trained model is still quite random and does not manage to reach the bonus. I think training for longer and trying new hyperparameters could help. For now, he looks like he shoots around without really being able to distinguish the ennemies from the walls : 

![Demo](demo_deadly_corridor.gif)

# Acknowledgments

This project utilizes the [ViZDoom](https://github.com/Farama-Foundation/ViZDoom) environment.

The Proximal Policy Optimization algorithm is based on the work of Schulman et al. You can find the original paper [here](https://arxiv.org/abs/1707.06347).

Feel free to reach out if you have any questions or feedback ! I can also send the weights of the trained models 🫡