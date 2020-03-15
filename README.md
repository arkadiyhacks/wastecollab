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

## 2.1 Swap file
By default Jetson Nano has 2GB of swap memory that allows for extra RAM memory if needed. When performing heavy inference it becomes insufficient, so it is essential to upgrade the swap. I prefer the method created by Jetson Hacks. 

To examine the swap memory:

    zramctl 

You will notice that there are four entries (one for each CPU of the Jetson Nano) /dev/zram0 – /dev/zram3. Each device has an allocated amount of swap memory associated with it, by default 494.6M, for a total of around 2GB. This is half the size of the main memory.

Cloning the directory:

    $ cd github
    $ git clone https://github.com/JetsonHacksNano/resizeSwapMemory

To change the size of swap file You can edit the number after -g for preferred amount of memory swap. For further details see his tutorial: https://www.jetsonhacks.com/2019/11/28/jetson-nano-even-more-swap/:

    $ cd resizeSwapMemory
    $ ./setSwapMemorySize.sh -g 6

After that the reboot is required 

    sudo reboot 

To examine the swap memory:

    zramctl 

You can notice that the amount per each CPU increased to 1.5GB - now we have a swap memory of 6GB in total. You can also see that in the in the *System Monitor* App under *Resources*. This app also allows to monitor the usage of your memory throughout inferencing. 

## 2.2 Python3 and Pip 
Python3 comes preinstalled to Jetson Nano as a part of JetPack. You can check your version by running 

    python3 --version

To quit python3 environment

    quit()

Pip is python package installer. To install pip run

    sudo apt install python3-pip

## 2.3 Virtual Environment
I was naive and decided not to install virtual environment from the beginning of the project. Then I learnt that some packages are incompatible with others so I had to reflash the SD card multiple times. Please learn on my mistakes and work in virtual environment, it would save you from stress and tears. 

To install virtual environment package

    sudo pip3 install virtualenv

Create a hidden folder which would hold your virtual environments

    mkdir ~/.virtualenvs

You can check that it exists by running 

    ls -a 

 Installing virtual environment wrapper which contains set of functions to handle virtual environment (as well as roemove it if needed)

    sudo pip3 install virtualenvwrapper

To set a location for virtual environments:

    export WORKON_HOME=~/.virtualenvs

Install and run nano

    $ sudo apt install nano
    $ nano ~/.bashrc

Scroll to the bottom with arrow key and add two lines

    $ export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
    $ ./usr/local/bin/virtualenvwrapper.sh

Press *ctr+X* on the keyboard, *y* to save it and then *enter* to proceed

To finally install virtual environment

    . .bashrc

 To create a virtual environment with the name *v1*

     mkvirtualenv v1

 You would notice added (v1) on the command prompt
To exit the virtual environment:

    deactivate
 
 To return to virtual environment:

    workon v1

## 3.0 Installing OpenCV

https://www.pyimagesearch.com/2018/05/28/ubuntu-18-04-how-to-install-opencv/



