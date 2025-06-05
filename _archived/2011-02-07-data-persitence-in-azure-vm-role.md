---
layout: post
title: Data Persitence in Azure VM Role
date: '2011-02-07T02:58:00.001+05:30'
author: aks
tags:
- VM Role
- Azure
modified_time: '2011-02-07T03:18:10.343+05:30'
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-856216639769030454
blogger_orig_url: http://techyfreak.blogspot.com/2011/02/data-persitence-in-azure-vm-role.html
---

Azure VM Role involves creating a base image, uploading it to Azure using 
csupload, and then creating a servic model in Visual Studio to point to the 
uploaded base image. 

Once Azure VM Role is deployed, it creates <span class="fullpost">a new drive 
with letter D: and name it Resources. A shortcut to this drive is added to 
your C: drive (Windows Drive). Once you restart the machine the data on drive 
D: would be persited, however all the data or any software installed after VM 
ROle is deployed would be gone. Also, in case of system/hardware failures, 
even the data in drive D: will not persist. 

But, what if you need to have data persistance in Azure VM Role. For example, 
you are installing SQL Server or may be Sharepoint on your VM Role. In this 
case you want your SQL Server's data files to persist. The option here would 
be to mount azure drive and this azure drive will hold your data files. Again, 
mounting an Azure drive is not a plain simple scenairo and it desribed here - 
[Mounting Azure drive in VM 
Role](http://techyfreak.blogspot.com/2011/02/mounting-azure-drive-in-azure-virtual.html). 