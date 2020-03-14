# Wastecollab
### **The goal of this project is to create a waste detection and identification system based on Nvidia Jetson Nano**

## 1.0 Setup of Jetson Nano 
The guideline on the setup given by Nvidia Team is great, so just follow it https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#intro

## 1.1 Components to make Jetson Nano work
I ordered components individually in a different time frame. As a result I had to reflash the micro SD card (start from the beginning). As an advice, order everything straight away and start working when you received all components. 

## 2.0 Environment Setup 
When you connected to wi-fi let's make Ubuntu work for us. First, I tried to work via SSH - that is connecting to the Nvidia Jetson Nano directly through my laptop. However, I found it limiting and moved to the work in the environment of Jetson Nano. 
To download the repository which contains everything to get started type in the terminal of Jetson:

`git clone https://github.com/dusty-nv/jetson-inference`
*Hello AI World* is great for the beginning to get a grasp of capabilities of Jetson Nano. I will cover steps directly related to this project. 
