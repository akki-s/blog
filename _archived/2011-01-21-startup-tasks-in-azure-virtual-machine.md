---
layout: post
title: Startup Tasks in Azure Virtual Machine (VM ) Role
date: '2011-01-21T00:20:00.001+05:30'
author: aks
tags:
- VM Role
- Azure
modified_time: '2011-03-11T02:25:03.388+05:30'
thumbnail: http://3.bp.blogspot.com/_S92PTW6g_pk/TTiAb3I533I/AAAAAAAADb8/awmXCUq36bk/s72-c/Add-Installer-To-Windows-Service.png
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-3814366724338119692
blogger_orig_url: http://techyfreak.blogspot.com/2011/01/startup-tasks-in-azure-virtual-machine.html
---

Since the release of Windows Azure SDK 1.3 we can now have startup tasks 
defined in the *ServiceDefinition *file for web/worker roles. However, this is 
not allowed for a **Virtual Machine role**. So what to do in case you need to 
have some tasks/scripts to be run on your VM Role’s startup?  For example, 
your VM Role runs some software applications that require some configuration 
information that can be available only at run time. 

So, how will you have startup tasks in a Virtual Machine Role?<span 
class="fullpost"> If you think that it’s just an easy task that involves 
adding you tasks in the Windows Startup folder 
(*C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup*), then you are 
mistaken. This is not going to work as the tasks in the startup folders are 
executed only when a user logs in to the machine, but this is not what you 
want. Your tasks should run whenever your role is started. 

The solution for this is to use [VM Role 
Adapter](http://msdn.microsoft.com/en-us/library/gg466226.aspx). This 
basically involves creating a Windows Service that is configured to Start 
Automatically when windows start. This is really simple and involves following 
steps: 

1. Create a **WindowsService **project in Visual Studio, let’s call it 
VMRoleStartup and write the operation you want to be executed at VM Role’s 
startup in the **OnStart()** method. 

2. Add the installer class to the service by opening service’s class in 
designer mode and right-clicking. Select “**Add Installer**” from the menu 
as shown below. This class is used to install the service. 

<div class="separator" style="clear: both; text-align: center;"><table 
align="center" cellpadding="0" cellspacing="0" class="tr-caption-container" 
style="margin-left: auto; margin-right: auto; text-align: center;"><td 
style="text-align: center;">[<img alt="Add Project installer to Windows 
Service" border="0" 
src="http://3.bp.blogspot.com/_S92PTW6g_pk/TTiAb3I533I/AAAAAAAADb8/awmXCUq36bk/s1600/Add-Installer-To-Windows-Service.png" 
/>](http://3.bp.blogspot.com/_S92PTW6g_pk/TTiAb3I533I/AAAAAAAADb8/awmXCUq36bk/s1600/Add-Installer-To-Windows-Service.png)<td 
class="tr-caption" style="text-align: center;">Add Project installer to 
Windows Service 
3. This will create a new class ProjectInstaller.cs with serviceInstaller and 
serviceProcessInstaller. 

<table align="center" cellpadding="0" cellspacing="0" 
class="tr-caption-container" style="margin-left: auto; margin-right: auto; 
text-align: center;"><td style="text-align: center;">[<img alt="Project 
Installer Class" border="0" 
src="http://2.bp.blogspot.com/_S92PTW6g_pk/TTiAeRtQcqI/AAAAAAAADcM/LSa8ztbnIWk/s1600/Project-Installer.png" 
/>](http://2.bp.blogspot.com/_S92PTW6g_pk/TTiAeRtQcqI/AAAAAAAADcM/LSa8ztbnIWk/s1600/Project-Installer.png)<td 
class="tr-caption" style="text-align: center;">Project Installer Class 

4. Go the properties of serviceProcessInstaller and select Account as 
LocalSystem. This is required so that the service will run as an administrator 
account. 

<table align="center" cellpadding="0" cellspacing="0" 
class="tr-caption-container" style="margin-left: auto; margin-right: auto; 
text-align: center;"><td style="text-align: center;">[<img alt="Service 
Process Installer Properties" border="0" 
src="http://2.bp.blogspot.com/_S92PTW6g_pk/TTiAc73DFeI/AAAAAAAADcE/nCpgYkpf2yI/s1600/configure-serviceprocess-installer.png" 
/>](http://2.bp.blogspot.com/_S92PTW6g_pk/TTiAc73DFeI/AAAAAAAADcE/nCpgYkpf2yI/s1600/configure-serviceprocess-installer.png)<td 
class="tr-caption" style="text-align: center;">Service Process Installer 
Properties 
5. Go to the properties of the serviceInstaller class and make following 
changes: 

    • Make sure that the Service name is same as that of your Service name 
    • Add Description and DisplayName as you want to be displayed in 
Services Management Console. 
    • You can set DelayedAutoStart to True, to make sure that your VM role 
comes into a stable state before your tasks are started. 
    • Set StartType to Automatic. 

<table align="center" cellpadding="0" cellspacing="0" 
class="tr-caption-container" style="margin-left: auto; margin-right: auto; 
text-align: center;"><td style="text-align: center;">[<img alt="Service 
Installer Properties" border="0" 
src="http://4.bp.blogspot.com/_S92PTW6g_pk/TTiAcbrXbXI/AAAAAAAADcA/yhI7nayg80A/s1600/configure-serviceinstaller.png" 
/>](http://4.bp.blogspot.com/_S92PTW6g_pk/TTiAcbrXbXI/AAAAAAAADcA/yhI7nayg80A/s1600/configure-serviceinstaller.png)<td 
class="tr-caption" style="text-align: center;">Service Installer 
Properties<span class="fullpost"> 

6. In the ProjectInstaller.cs class add the following code to start the 
service after it is installed. 


<table cellpadding="0" cellspacing="0" class="tr-caption-container" 
style="float: left; margin-right: 1em; text-align: left;"><td 
style="text-align: center;">[<img alt="Code to start windows service after 
installation" border="0" 
src="http://2.bp.blogspot.com/_S92PTW6g_pk/TTiAe_InqLI/AAAAAAAADcQ/YtFl5TkdEpM/s1600/startup-code.png" 
/>](http://2.bp.blogspot.com/_S92PTW6g_pk/TTiAe_InqLI/AAAAAAAADcQ/YtFl5TkdEpM/s1600/startup-code.png)<td 
class="tr-caption" style="text-align: center;">Code to start windows service 
after installation<span class="fullpost"> 
Please note that **MountXService **here refers to your service class name. 
<span class="fullpost"> 

<span class="fullpost">7. Build the application under Any CPU or x64 platform 
(because VM Role runs under x64 platform). 

8. Copy the .exe file (along with dlls if required) to VM Role and install it 
using **installutil **by running it from command prompt as an administrator 
user. 
“**installutil**” is available in 
“C:\Windows\Microsoft.Net\Framework64\v4.0.30319” path. 


<table align="center" cellpadding="0" cellspacing="0" 
class="tr-caption-container" style="margin-left: auto; margin-right: auto; 
text-align: center;"><td style="text-align: center;">[<img alt="Install 
windows service using installutil" border="0" 
src="http://2.bp.blogspot.com/_S92PTW6g_pk/TTiAdSW7WXI/AAAAAAAADcI/NGLq-0tFI2o/s1600/installutil.png" 
/>](http://2.bp.blogspot.com/_S92PTW6g_pk/TTiAdSW7WXI/AAAAAAAADcI/NGLq-0tFI2o/s1600/installutil.png)<td 
class="tr-caption" style="text-align: center;">Install windows service using 
installutil<span class="fullpost"> 
9. Upload your image to Azure and create a service model for VM Role to 
connect to this image. 

This is it, now you are done and when your VM Role starts your windows service 
will be started and will execute the code you put up in it's OnStart method. 