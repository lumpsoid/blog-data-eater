---
title: "Installing Games in .ISO Format on Linux with CDemu"
date: 2023-06-18
---
# Introduction

In this blog post, we will explore the process of installing games in .ISO format on Linux using CDemu, a package that emulates a CD/DVD reader. We will cover the steps to set up CDemu, load the .ISO image, mount the disk, and finally install a Windows game using Lutris. So, let's dive in!

# Step 1: Installing CDemu

To begin, we need to install the CDemu package, which emulates a CD/DVD reader. This package allows us to load and mount .ISO images. You can install CDemu on your Linux system using your distribution's package manager. -> https://cdemu.sourceforge.io/project/#binary

# Step 2: Starting the Cdemu Daemon

After installing CDemu, we need to start the cdemu-daemon. The daemon is responsible for managing the CD/DVD devices and handling image loading and unloading. Open a terminal and run the following command:
```
cdemu-daemon
```

# Step 3: Loading the .ISO Image

Once the daemon is running, we can load our .ISO image. To determine the device number associated with the CD/DVD reader, use the command:
```
cdemu status
```
Make a note of the device number for future reference.

Next, load the .ISO image using the following command:
```
cdemu load <device number> <path/to/image>
```
Replace `<device number>` with the actual device number you obtained earlier, and `<path/to/image>` with the path to your .ISO image file.

# Step 4: Mounting the Disk

After loading the .ISO image, we need to mount the disk to access its contents. To find the path to the disk, use the command:
```
cdemu device-mapping
```
This will provide you with the necessary path.

To mount the disk, use the following command:
```
mount <path/to/disk> <path/to/mount>
```
Replace `<path/to/disk>` with the path you obtained earlier, and `<path/to/mount>` with the desired mount point.

# Step 5: Installing the Windows Game

Now that the disk is mounted, you can proceed to install your Windows game. One popular tool for this purpose is Lutris, which provides a user-friendly interface for managing games on Linux.

Using Lutris, locate the .exe file of the game you want to install. This file should be accessible within the mounted disk. Follow the installation wizard provided by Lutris to install the game successfully.

# Conclusion:

By following these steps, you can install games in .ISO format on Linux using CDemu. Emulating a CD/DVD reader allows you to load and mount .ISO images, and tools like Lutris simplify the installation of Windows games. Now you can enjoy your favorite games on Linux, even those that come in .ISO format!

Note: The instructions provided in this blog are based on general practices and may vary depending on your Linux distribution and specific setup. Please refer to the documentation and official websites of the respective tools for more detailed instructions.
