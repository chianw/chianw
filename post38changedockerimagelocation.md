# Changing the default location of Docker container image file system

Recently I was working on a project using Visual Studio Code to do development on containers running on a remote Linux VM host with Docker installed. It turns out that Docker by default will store the container file layers into /var/lib/docker which is part of the Linux root file system, and this will fill up disk space very quickly when you have multiple containers running. A better design is to add a data disk to the Linux VM and then use it to store the container file layers.

The 2 important articles I followed to get this done are listed below.

## Adding data disk to Azure Linux VM and mounting it to the file system

Follow the documentation [here](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/attach-disk-portal?tabs=ubuntu)


## Configure Docker to use the data drive to store container file layers instead of /var/lib/docker

Follow this article [here](https://evodify.com/change-docker-storage-location/)

