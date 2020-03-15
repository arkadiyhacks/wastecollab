# Wastecollab
### **The goal of this project is to create a waste detection and identification system based on Nvidia Jetson Nano**

## 1.0 Setup of Jetson Nano 
The guideline on the setup given by Nvidia Team is great, so just follow it https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#intro

## 1.1 Components to make Jetson Nano work
I ordered components individually in a different time frame. As a result I had to reflash the micro SD card (start from the beginning). As an advice, order everything straight away and start working when you received all components. 

## 2.0 Environment Setup 
When you connected to wi-fi let's make Ubuntu work for us. First, I tried to work via SSH - that is connecting to the Nvidia Jetson Nano directly through my laptop. However, I found it limiting and moved to the work in the environment of Jetson Nano. 

Upgrading and updating preinstalled packages and libraries:
```
$ sudo apt-get update
$ sudo apt-get upgrade
```
It's great to have all folders organised. Lets create github directory to clone all repositories there
    $ mkdir github

## Swap file
By default Jetson Nano has 2GB of swap memory that allows for extra RAM memory if needed. When performing heavy inference it becomes insufficient, so it is essential to upgrade the swap. I prefer the method created by Jetson Hacks. 

To examine the swap memory:

    $ zramctl 

You will notice that there are four entries (one for each CPU of the Jetson Nano) /dev/zram0 – /dev/zram3. Each device has an allocated amount of swap memory associated with it, by default 494.6M, for a total of around 2GB. This is half the size of the main memory.

Cloning the directory:
    $ cd github
    $ git clone https://github.com/JetsonHacksNano/resizeSwapMemory

To change the size of swap file You can edit the number after -g for preferred amount of memory swap. For further details see his tutorial: https://www.jetsonhacks.com/2019/11/28/jetson-nano-even-more-swap/:

    $ cd resizeSwapMemory
    $ ./setSwapMemorySize.sh -g 6

After that the reboot is required 

    $ sudo reboot 

To examine the swap memory:

    $ zramctl 

You can notice that the amount per each CPU increased to 1.5GB - now we have a swap memory of 6GB in total. You can also see that in the in the *System Monitor* App under *Resources*. This app also allows to monitor the usage of your memory throughout inferencing. 










