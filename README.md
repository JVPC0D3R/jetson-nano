# 🚀 JETSON-NANO

[![Jetson](https://img.shields.io/badge/Jetson_NANO-3a3a3a?style=for-the-badge&logo=nvidia&logoColor=green)](https://github.com/JVPC0D3R/jetson-nano)
[![PyTorch](https://img.shields.io/badge/PyTorch-3a3a3a?style=for-the-badge&logo=PyTorch)](https://github.com/JVPC0D3R/jetson-nano)
[![Bash](https://img.shields.io/badge/bash-3a3a3a?style=for-the-badge&logo=gnu-bash&logoColor=white)](https://github.com/JVPC0D3R/jetson-nano)
[![ROS](https://img.shields.io/badge/ROS-3a3a3a?style=for-the-badge&logo=ROS&logoColor=blue)](https://github.com/JVPC0D3R/jetson-nano)

<p align="justify">
In this repo you will find usefull instructions to configure your AI Nvidia Jetson-Nano platform, featuring Pytorch, Tensorflow, OpenCV, jtop and other tools. 
</p>

<p align="center">
  <img src="https://github.com/JVPC0D3R/resources/blob/main/jetson_gif_jvp.gif" width="300" />
</p>

<p align="justify">
The Ubuntu 20.04 OS image I use is provided by <a href="https://github.com/Qengineering/Jetson-Nano-Ubuntu-20-image">Qengineering</a> and you can download it from the repo or directly from <a href="https://ln5.sync.com/dl/741c98fe0/x8kxkhgs-cgmzk7rf-n4m7pyw8-h64tzbv5/view/default/11304846510004">here</a>. This OS image was originally mounted on a 32GB SD card, so if you are using a 64GB or larger memory, you will need to resize the file system.
</p>

## 🦾 properties

| Device Name                     | GPU                                   | CPU                                  | Memory               | Storage            | Video Encode                                 | Video Decode                                | Power            |
|---------------------------------|---------------------------------------|--------------------------------------|----------------------|--------------------|----------------------------------------------|---------------------------------------------|------------------|
| Jetson Nano Developer Kit         | 128-core NVIDIA Maxwell GPU            | Quad-core ARM Cortex A57 CPU         | 4GB 64-Bit LPDDR4     | MicroSD (Card not included) | 1x 4K30 \| 2x 1080p60 \| 4x 1080p30 \| 9x 720p30 | 1x 4K60 \| 2x 4K30 \| 4x 1080p60 \| 8x 1080p30 \| 18x 720p30 | 5W                |


## 📏 resizing filesystem on an SD Card

<p align="justify">
  
If you've flashed a smaller image onto a larger SD card and want to utilize the remaining free space, you need to resize the filesystem. In order to do so you will have to follow the next steps:

</p>

<p align="justify">
  
1. You need to check if the system recognizes the unused space. You can achieve this using the ``` dh -f ``` command.
  
</p>

<p align="justify">
  
2. Check the name of your root partition using the ``` lsblk ``` command. It will look something like this:
  
</p>

```
  NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
mmcblk0     179:0    0 59.5G  0 disk
├─mmcblk0p1 179:2    0 28.9G  0 part /
└─mmcblk0p2 179:1    0  300M  0 part /boot
  
```
<p align="justify">
  
3. Run the ```sudo fdisk /dev/mmcblk0``` command to enter the fdisk utility, which allows you to manage partitions on the SD card.

</p>

<p align="justify">

4. Press ```p``` to display the partition table. This will show you the current partitions and their sizes.

</p>

<p align="justify">
  
5. Press ```d``` to delete a partition. Then enter the number of the partition you want to delete, which should be the partition with the Jetson Nano image (In this example ```1```). Be very careful with this step as deleting the wrong partition can result in data loss.

</p>

<p align="justify">
  
6. Press ```n``` to create a new partition. For the type of partition, choose primary. For the partition number, enter the same number as the one you deleted. For the first and last sector, you can simply press Enter to use the default values, which will create a partition that uses all available space. Also keep the signature as default, do not delete it.
</p>

<p align="justify">
  
7. Press ```w``` to write the changes to the disk and exit. Note that this step cannot be undone.

</p>

<p align="justify">
  
8. Reboot the Jetson Nano for the changes to take effect. You can do this with the ```sudo reboot``` command.

</p>

<p align="justify">
  
9. After the reboot, run ```sudo resize2fs /dev/mmcblk0p1``` to resize the filesystem on the partition to use all available space.

</p>

<p align="justify">
  
10. Run ```df -h``` again to confirm that the / partition is now using all of the available space on the SD card.
  
</p>


## 🛸 YOLOv8 installation

<p align="justify">
  
1. In order to run <a href="https://github.com/ultralytics/ultralytics">YOLOv8</a>  on the Jetson Nano, you will have to install the Ultralytics Python package by running the ``` pip3 install ultralytics ``` command.

</p>

<p align="justify">
  
2. Create a python script called ```test.py``` that loads the YOLOv8 model and runs it on a USB webcam.

</p>

```python

from ultralytics import YOLO

# Load the model
model = YOLO("yolov8n.pt")

# Detect and show output
model(source = 0, show = True)

```

<p align="justify">
  
3. Run the script with the ```python3 test.py``` command.

</p>

<p align="justify">
  
4. You can also check the GPU/CPU resources running the ```jtop``` command on another terminal.

</p>

## 🤖 ROS installation

<p align="justify">
  
In order to create a robotic platform with <a href="http://wiki.ros.org/noetic/Installation/Ubuntu">ROS</a>  on the Jetson Nano, you will have to follow the next steps.
  
</p>

<p align="justify">
  
1. Setup your sources.list
  
  ```sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'``` 

</p>

<p align="justify">
  
2. Set up your keys
  
  ```curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -``` 

</p>

<p align="justify">
  
3. Make sure your Debian package index is up-to-date with the ```sudo apt update``` command.

</p>

<p align="justify">
  
4. Install ROS
  
    * Full version ```sudo apt install ros-noetic-desktop-full``` 
  
    * Desktop version ```sudo apt install ros-noetic-desktop``` 
  
    * Base version ```sudo apt install ros-noetic-base``` 

</p>

<p align="justify">
  
5. Automatically source the ```setup.sh``` script for every new shell

```
  echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
  source ~/.bashrc
  
``` 

</p>










