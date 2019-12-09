# D.A.D.S

 Inspired by the work of [Samuel Arzt](https://www.youtube.com/channel/UC_eerU4SleeptEbD2AA_nDw) and [m baske](https://www.youtube.com/channel/UCqMSNJyrG5zWrjl-_hYdF0g), the "Dynamic Autonomous Driving Stage", or **D.A.D.S**, is an experiment in the training and deployment of Reinforcement Learning algorithms.

## Demo
[![Watch the video](https://i.imgur.com/5XHvbvW.jpg)](https://youtu.be/GmSWXvZIGiE)

## Table of contents
* [How to Download](#how-to-download)
* [Why it was Built](#why-it-was-built)
* [How it was Built](#how-it-was-built)
  * [Unity ML Agents](#unity-ml-agents)
  * [Observation Parameters](#observation-parameters)
  * [Model Architecture](#model-architecture)
  * [Final Model Results](#final-model-results)
* [Acknowledgments](#acknowledgments)
* [License](#license)

## How to Download

The easiest way to download this package (along with the Windows Executable) is to clone this github repository...
```
git clone https://github.com/OliverMathias/D.A.D.S.git
```
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

  That is a basic overview of [Proximal Policy Optimization](https://medium.com/@jonathan_hui/rl-proximal-policy-optimization-ppo-explained-77f014ec3f12) or "PPO" Reinforcement Learning. The type used in this project, and showcased in the gif below.

  ![](http://g.recordit.co/95CrrF5p3e.gif)

  If you want to get a wider overview on RL, I recommend this youtube [video](https://www.youtube.com/watch?v=JgvyzIkgxF0).

  * #### Observation Parameters
  The main 2 observation sources the car in this environment uses are it's front facing camera and 10 "Lidar" sensors. The front facing camera is a 120px X 120px color image, and the "Lidar" sensors are simply 10 equally spaced raycasts with a limited range.

  Another auxiliary observation parameter the car receives are it's own position, velocity, and the position of the target.

  The network collects data 10 times per second, where it receives a still image from the color camera, hit distances of all 10 raycasts, and the Vector3 positions/velocities.

  1. Front Facing Camera (this is what the car sees)

   ![](http://g.recordit.co/Em9BT7NVWJ.gif)

  2. "Lidar" Sensors

   ![](http://g.recordit.co/vFtCWUVqCa.gif)
   (It seems like there are more than 10 raycasts but that's just the laggy scene view at 10X speed)

  3. Vector3 Positions/Velocities

   ![](http://g.recordit.co/bia1mclSeV.gif)

  The combination of these sensing methods allow our car to visually, and spatially recognize it's target, as well as keep an eye out for obstacles in all directions surrounding it.


  * #### Model Architecture
  Because this network has to take in pixel data as well at floats, we have to basically line up two networks in a pipeline. Thankfully, ML-Agents makes this super easy so if you want to get the same type of network up and running quickly, just visit the [ML-Agents Observations Docs](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Learning-Environment-Design-Agents.md).

  I'll go over how I would do this if we had to use Keras or Pytorch, and make this model on our own.

  ### Step 1: CNN

  The first step would be to create a "Convolutional Neural Network", or "CNN." This type of network takes an image and learns to extract useful features from it to give a desired output.

  ![](https://neurohive.io/wp-content/uploads/2018/11/vgg16-1-e1542731207177-570x321.png)

  The above image is of a normal CNN. It takes a full scale 224 X 224 image and passes those pixel values through what are called "Convolutional Layers." These layers a basically weighted matrices that each extract different information and pass it on to the next convolutional layer. Because the weighted matrices are smaller than the image (so they can focus on smaller pieces of it) the size of the features or information decreases as it is passed along.

  At the end we can see small weight matrix of 7 X 7 X 512 being ["pooled"](https://machinelearningmastery.com/pooling-layers-for-convolutional-neural-networks/), ["flattened"](https://www.superdatascience.com/convolutional-neural-networks-cnn-step-3-flattening/), and finally passed to a fully connected network (shown in blue) that will make sense of the features extracted by the convolutional layers.

  However, in our case we're going to be using a ["RESNET CNN"](https://towardsdatascience.com/illustrated-10-cnn-architectures-95d78ace614d#e4b1). "RESNET" is short for 'Residual Network', which is the main feature of all types of "RESNET" networks. At the core of this are what's called "residual" or "skip connections".

  ![](https://miro.medium.com/max/704/1*WpX_8eCeTsEcCs8vdXtUCw.png)

  As shown in the above diagram, instead of passing data into one layer, processing it, then passing that processed data into the next layer, we actually pass the un-processed data from each previous layer into every other layer in the network.

  Now this might seem counterintuitive, "Why would we want old data messing up the new processed data?" Well, this method allows us to create EXTREMELY deep neural networks without the network "losing" information as we go deeper, or possibly "Vanishing Gradients."

  ![](https://i.imgur.com/OTyv3VK.jpg)

  In the image above, we see a 3D reconstruction of a CNN that predicts numbers from handwritten digits. As the information moves "up" or through the network, we can see the features become more and more abstract. Imagine how abstract the features would be if instead of 5 layers, we had 50! That's what skip connections help to solve.

  So when we add "skip connections", we're making sure the model is remembering the whole picture instead of getting lost in meaningless features as it gets deeper and deeper.

  ### Step 2: Fully Connected

  The second step in our model is the fully connected layers. These are actually what the RESNET processed image data and other observations are fed into. From this, we have 2 outputs, acceleration and steering angle.

  ![](https://i.imgur.com/fcLWyB4.png)

  While the diagram above isn't entirely accurate, it's a good base for explaining the architecture and size of the fully connected network. In our actual project, the fully connected network has 4 hidden layers (as this one does), but 175 nodes per layer. Meaning that instead of the 855 weights shown in the diagram, our fully connected network has 93,975.

  So after millions and millions of training iterations, our CNN and Fully Connected layers will adjust their weights so that their outputs will give them the highest reward possible given their input observations.

  * #### Final Model Results
  After testing almost 30 different models and hyper-parameters...

  ![](https://i.imgur.com/4zRqovx.jpg)

  One model finally emerged as the reigning champ at around 16M episodes...

  ![](https://i.imgur.com/CDvUOpA.jpg)

  With a steadily increasing average rewards per episode as show above, and a steadily decreasing episode length (finding faster solutions) shown below.

  ![](https://i.imgur.com/VGz16aG.jpg)

  The final accuracy of this model is **93%** if it is left running long enough. Not incredible, but it gets the job done, I know that if I continue to tweak the parameters and learning arena I can get it to somewhere around **99%**. However, I have a ton more projects to get to in the pipeline, and to be honest, it's pretty interesting to see where the RL algorithm falls short.


## Acknowledgments
* [Samuel Arzt](https://www.youtube.com/channel/UC_eerU4SleeptEbD2AA_nDw)
* [Adam Kelly](https://www.youtube.com/channel/UC1c0mDkk8R5sqhXO0mVJO3Q)
* [m baske](https://www.youtube.com/channel/UCqMSNJyrG5zWrjl-_hYdF0g)

## License
This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
