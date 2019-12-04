# D.A.D.S

 Inspired by the work of [Samuel Arzt](https://www.youtube.com/channel/UC_eerU4SleeptEbD2AA_nDw) and [m baske](https://www.youtube.com/channel/UCqMSNJyrG5zWrjl-_hYdF0g), the "Dynamic Autonomous Driving Stage", or **D.A.D.S**, is an experiment in the training and deployment of Reinforcement Learning algorithms.

## Demo
put a gif here showing the web UI and Car

## Table of contents
* [How to Use it](#how-to-use-it)
  * [Buttons](#clone-repository)
    * [Start](#navigate-to-localhost)
    * [Reset](#navigate-to-localhost)
    * [Switch Camera Views](#install-dependencies)
    * [Randomize Obstacles](#run-doggydoor.py)
  * [Metrics](#clone-repository)
    * [Successes / Crashes](#navigate-to-localhost)
* [Why it was Built](#uses)
  * [Standalone Website](#standalone-website)
* [How it was Built](#how-it-was-built)
  * [Unity ML Agents](#learning-about-keras)
  * [Learning Parameters](#learning-about-keras)
  * [Model Architecture](#learning-about-keras)
  * [Final Model Results](#learning-about-keras)
* [Acknowledgments](#acknowledgments)
* [License](#license)

## How to use it
Simple visit [WEBSITE], a loading screen should pop up and you'll be directed to the start screen!

put a gif here of me typing in the url and the page loading
and put a gif on each of the buttons sub point


  * #### Buttons
  This is an overview of all the buttons in the UI of D.A.D.S
    * #### Start
    This is an overview of all the buttons in the UI of D.A.D.S
    * #### Reset
    This is an overview of all the buttons in the UI of D.A.D.S
    * #### Camera Views
    This is an overview of all the buttons in the UI of D.A.D.S
    * #### Randomize Obstacles
    This is an overview of all the buttons in the UI of D.A.D.S
  * #### Metrics
  This is an overview of all the buttons in the UI of D.A.D.S
    * #### Successes / Crashes
    This is an overview of all the buttons in the UI of D.A.D.S

---

## Why it was Built
I recently covered Reinforcement Learning in my DL class at university and was excited to dig deeper into it and create something tangible. I had seen amazing results from Unity RL projects like [m baske's AI Drones](https://www.youtube.com/watch?v=MKDBcKNJVS4) or his [V2 Battle Robots](https://www.youtube.com/watch?v=krzmg9eOeDM), so I figured Unity's [ML Agents package](https://github.com/Unity-Technologies/ml-agents) was a great way to get started with the process of Reinforcement Learning.

If you're at all interested in Reinforcement Learning I highly suggest checking out Unity's [ML Agents package](https://github.com/Unity-Technologies/ml-agents), and more specifically, their [getting started project](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Learning-Environment-Create-New.md), which teaches you how to train a ball that rolls to a target.

My suggested reading order would be:
1. [Installation](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Installation.md)
2. [RL Background](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Background-Machine-Learning.md)
3. [Getting Started Project](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Learning-Environment-Create-New.md)

**P.S.** Another great resource is the [Immersive Limit](https://www.youtube.com/channel/UC1c0mDkk8R5sqhXO0mVJO3Q) youtube channel. Adam Kelly, (the host) is an incredible teacher and has some great beginner projects.


## How it was Built
  * #### Unity ML Agents

  ![](https://i.imgur.com/EuaHaxk.jpg)

  This entire project is built on [Unity's ML-Agents package](https://github.com/Unity-Technologies/ml-agents). This package bridges the gap between Unity and the popular, python based, machine learning library "Tensorflow." With Tensorflow + Unity we can implement complex Reinforcement Learning models to act as the "brains" of our agents, and train them within a their Unity Environments.

  If you're not familiar with the idea of Reinforcement Learning, the basic concept is that our agent, in this case a car, is set loose in a dynamic environment, (ex. a videgame). It takes in observations from this environment (ex. distances to objects or camera views), and can take actions in the environment (ex. moving left or right). The engineer then sets rewards or punishments for the outcomes you want them to create or avoid. (ex. give a punishment if the agents crashes into something & give a reward if the agents makes it to their target unscathed.)

  ![](https://i.imgur.com/DsNZ5jB.png)

  This simple diagram shows the flow of information and actions between the agents and their environment quite well. By following this diagram's flow for millions and millions of iterations, each time slowly updating our network's weights to make the high reward outcomes more likely, our agent will hopefully learn the task we wanted it to.

  That is a basic overview of [Proximal Policy Optimization](https://medium.com/@jonathan_hui/rl-proximal-policy-optimization-ppo-explained-77f014ec3f12) or "PPO" Reinforcement Learning.

  If you want to get a wider overview on RL, I recommend this youtube [video](https://www.youtube.com/watch?v=JgvyzIkgxF0).

  * #### Learning Parameters
  The main 2 observation sources the car in this environment uses are it's front facing camera and 10 "Lidar" sensors.
  ![](http://g.recordit.co/95CrrF5p3e.gif)

  * #### Model Architecture
  this is kind of what it looks like, the general idea, passing images into smaller features information and numbers

  ![](https://neurohive.io/wp-content/uploads/2018/11/vgg16-1-e1542731207177-570x321.png)


  but we actually use residual cnn networks then pass those extracted features (floats) to a full connected network along with the other agent inputs, (speed, vector3 of target, raycast distances)
  ![](https://miro.medium.com/max/2374/1*beczGvPaBBnyauKItYGTYQ.png)

  * #### Final Model Results



## Acknowledgments
* [Samuel Arzt](https://www.youtube.com/channel/UC_eerU4SleeptEbD2AA_nDw)
* [Adam Kelly](https://www.youtube.com/channel/UC1c0mDkk8R5sqhXO0mVJO3Q)
* [m baske](https://www.youtube.com/channel/UCqMSNJyrG5zWrjl-_hYdF0g)

## License
This project is licensed under the GNU License - see the [LICENSE.md](LICENSE.md) file for details
