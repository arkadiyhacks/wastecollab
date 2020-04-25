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

    $ mkdir github
    $ cd github
    $ git clone https://github.com/JetsonHacksNano/resizeSwapMemory

To change the size of swap file You can edit the number after -g for preferred amount of memory swap. For further details see his tutorial: https://www.jetsonhacks.com/2019/11/28/jetson-nano-even-more-swap/:

    $ cd resizeSwapMemory
    $ ./setSwapMemorySize.sh -g 6

After that the reboot is required. 

## 2.2 Max Performance mode
Now it's time to power Jetson Nano with 5V-4A power supply. Don't forget to put jumper pin/wire on the connectors. 

Jetson Nano can run either on 5W or 10W. We want 10W, so run:

    $ sudo nvpmodel -m 0
    $ sudo jetson_clocks


To examine the swap memory:

    zramctl 

You can notice that the amount per each CPU increased to 1.5GB - now we have a swap memory of 6GB in total. You can also see that in the in the *System Monitor* App under *Resources*. This app also allows to monitor the usage of your memory throughout inferencing. 


## 2.3 Python3 and Pip 
Python3 comes preinstalled to Jetson Nano as a part of JetPack. You can check your version by running 

    python3 --version


Pip is python package installer. To install pip run

    sudo apt install python3-pip

## 2.4 Virtual Environment
I was naive and decided not to install virtual environment from the beginning of the project. Then I learnt that some packages are incompatible with others so I had to reflash the SD card multiple times. Please learn on my mistakes and work in virtual environment, it would save you from stress and tears. 

To install virtual environment package and virtual environment wrapper which contains set of functions to handle virtual environment (as well as remove it if needed)

    sudo pip3 install virtualenv virtualenvwrapper

Create a hidden folder which would hold your virtual environments

    mkdir ~/.virtualenvs

You can check that it exists by running 

    ls -a 

Modify /.bashrc  file by 

    $ echo -e "\n# virtualenv and virtualenvwrapper" >> ~/.bashrc
    $ echo "export WORKON_HOME=$HOME/.virtualenvs" >> ~/.bashrc
    $ echo "export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3" >> ~/.bashrc
    $ echo "source /usr/local/bin/virtualenvwrapper.sh" >> ~/.bashrc

Press *ctr+X* on the keyboard, *y* to save it and then *enter* to proceed

To finally install virtual environment

    source ~/.bashrc

 To create a virtual environment with the name *v1*

     mkvirtualenv v1

 You would notice added (v1) on the command prompt
To exit the virtual environment:

    deactivate
 
 To return to virtual environment:

    workon v1

## 3.0 Installing OpenCV 3.4.4
This installation is in particular long one. This is a primary reason for virtual environment as uninstalling OpenCV directly is a pain. 

uninstall preinstalled opencv 

    sudo apt-get purge libopencv*

install numpy 

    pip3 install numpy

Installing OpenCV dependancies. To work with image files:

    $ sudo apt-get install libjpeg-dev libpng-dev libtiff-dev
    $ sudo apt-get install libjasper-dev

Install following packages so you can work with your camera stream and process video files:
    
    $ sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
    $ sudo apt-get install libxvidcore-dev libx264-dev


https://jkjung-avt.github.io/opencv3-on-tx2/

## 4.0 Running YOLOv3-tiny with OpenCV
First you train the model and then you deploy it. I trained the model on google colab. I uploaded darknet repository and images with annotations to my google drive. Then I mounted google drive into the google colab. Colab gives free GPU, so it is perfect for testing, however the given runtime is just 12 hours. Good news - you can continue training on your last iteration. So I managed to train the dataset with about 2800 images up to 53 000 iterations and it took me about 30 hours in total. The more you train the smaller the loss function --> the more accurate is the algorithm. The function converges at some point meaning the loss function does not get smaller, meaning you can stop training. The code was optimized based on the tutorial by Ivan Goncharov. 

I do not include the complete guide on how to train the dataset as it depends on the paths to your folder of darknet and navigation within it. However, I would provide main 



## 5.0 Deepstream YOLO

    $ cd ~/deepstream_sdk_v4.0.2_jetson/sources/objectDetector_Yolo
    $ deepstream-app -c deepstream_app_config_yoloV3_tiny.txt 
    
    
editing deepstream_app_config_yoloV3_tiny.txt to enable CSI camera (Raspberry Pi)
4=RTSP 5=CSI
type=5
camera-width=1280
camera-height=720
camera-fps-n=30
camera-fps-d=1
num-sources=1

    export CUDA_VER=10.0
    make -C nvdsinfer_custom_impl_Yolo

    # uri=file://1_4.mp4


Apparently steps https://forums.developer.nvidia.com/t/yolo-for-deepstream-app/70693#5317562
Steps as well https://github.com/anguoyang/deepstream-plugins


https://forums.developer.nvidia.com/t/deepstream-gst-nvstreammux-change-width-and-height-doesnt-affect-fps/83286

