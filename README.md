# Wastecollab
### **The goal of this project is to create a waste detection and identification system based on Nvidia Jetson Nano**

## 1.0 Setup of Jetson Nano 
The guideline on the setup given by Nvidia Team is great, so just follow it https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#intro

## 1.1 Components to make Jetson Nano work
I ordered components individually in a different time frame. As a result I had to reflash the micro SD card (start from the beginning). As an advice, order everything straight away and start working when you received all components. 

## 2.0 Environment Setup 
When you connected to wi-fi let's make Ubuntu work for us. First, I tried to work via SSH - that is connecting to the Nvidia Jetson Nano directly through my laptop. However, I found it limiting and moved to the work in the environment of Jetson Nano. 

Upgrading and updating preinstalled packages and libraries:
    
$`sudo apt-get update`

$`sudo apt-get upgrade`


## Swap file
By default Jetson Nano has 2GB of swap memory that allows for extra RAM memory if needed. When performing heavy inference it becomes insufficient, so it is essential to upgrade the swap. I prefer the method created by Jetson Hacks. 

To examine the swap memory:

`zramctl` 

Cloning the directory:

`git clone https://github.com/JetsonHacksNano/resizeSwapMemory`

To change the size of swap file:
```
cd resizeSwapMemory
./setSwapMemorySize.sh -g 6
```

You can edit the number after -g for preferred amount of memory swap. For further details see his tutorial: https://www.jetsonhacks.com/2019/11/28/jetson-nano-even-more-swap/












