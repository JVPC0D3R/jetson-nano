# ☄️ JETSON-NANO

<p align="justify">
In this repo you will find usefull data to configure your AI Nvidia Jetson-Nano platform.
</p>

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
