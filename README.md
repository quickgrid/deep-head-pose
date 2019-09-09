# HopelessNet: Improving Hopenet #

<div align="center">
  <img src="https://i.imgur.com/K7jhHOg.png" width="380"><br><br>
</div>

**Hopenet** is an accurate and easy to use head pose estimation network. It uses the 300W-LP dataset for training the models and have been tested on real data with good qualitative performance.

The original repository of Hopenet is [GitHub](https://github.com/natanielruiz/deep-head-pose)
For details about the method and quantitative results please check their CVPR Workshop [paper](https://arxiv.org/abs/1710.00925).

Here I am trying to revisit this method and improve its performance, specifically for testing on AFLW2000 dataset.  
I applied minor changes to the code so that I can work with in using **PyTorch version 1.1** and **Python3**.

<div align="center">
<img src="output-amir.gif"/><br><br>
</div>

## Better Training for Hopenet

The best reported results for AFLW2000 dataset, provided in the CVPRW paper (Table 1), are:  
Yaw: 6.470, Pitch: 6.559, Roll: 5.436, and **MAE: 6.155**

As reported in the paper, to achieve this result, they used below settings:
* Training Dataset: 300W-LP
* Alpha: 2
* Batch Size: 128
* Learning Rate: 1e-5

Using the provided code, I tried similar settings.  
Except for **batch size** for which I had to reduce to **64** due to the memory limitation of my GPU.  
What I found was after few epochs, the test error starts raising.  
To achieve a smoother error curve, I reduced the **learning rate** to **1e-6** and tried the training with different alpha values.

The best model I got so far was from **alpha = 1** which performs as below on AFLW2000:  
Yaw: 5.4517, Pitch: 6.3541, Roll: 5.3127, **MAE: 5.7062**  
The snapshot of this model can be downloaded from [models/hopenet_snapshot_a1.pkl](https://github.com/shahroudy/deep-head-pose/raw/master/models/hopenet_snapshot_a1.pkl).

## Improve the Efficiency of the Model with HopeLessNet :D

The original Hopenet method uses a ResNet50 convnet which is considered to be a heavy weight and inefficient model, specifically to be used on embedded or mobile platform.  
To mitigate this issue, we can think of replacing this module with a lighter network e.g. ResNet18, Squeezenet, or MobileNet.  
An argument is now added to the train_hopenet.py and test_hopenet.py modules called "arch" which can change the base network's architecture to:
* ResNet18
* ResNet34
* ResNet50
* ResNet101
* ResNet152
* Squeezenet_1_0
* Squeezenet_1_1
* MobileNetV2

The best performing model with **ResNet18** architecture [snapshot](https://github.com/shahroudy/deep-head-pose/raw/master/models/hopenet_resnet18.pkl) achieves:  
Yaw: 6.0897, Pitch: 6.9588, Roll: 6.0907, **MAE: 6.3797**

With MobileNetV2 architechture [snapshot](https://github.com/shahroudy/deep-head-pose/raw/master/models/mobilenetv2.pkl) I could reach to:  
Yaw: 7.4363, Pitch: 7.0631, Roll: 6.7466, **MAE: 7.0820**

And with Squeezenet_1_0 architechture [snapshot]() we can get:

Lastly, Squeezenet_1_1 architechture [snapshot]() could perform:
