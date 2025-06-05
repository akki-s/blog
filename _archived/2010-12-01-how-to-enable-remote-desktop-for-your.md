---
layout: post
title: How to enable Remote Desktop for your Windows Azure Roles
date: '2010-12-01T15:06:00.001+05:30'
author: aks
tags:
- Azure
modified_time: '2011-01-06T02:42:23.554+05:30'
thumbnail: http://1.bp.blogspot.com/_S92PTW6g_pk/TPYWjubbNPI/AAAAAAAADH8/ONLiWz9Tn9k/s72-c/01.png
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-8019179590832353787
blogger_orig_url: http://techyfreak.blogspot.com/2010/12/how-to-enable-remote-desktop-for-your.html
---

The latest Azure SDK 1.3 makes it possible to login to the VM of your 
web/worker role instances via Remote desktop. So you are not limited to use VM 
Role in case you need to establish a RDP connection to your VM. 

This helps you to monitor the error events that occur in your events, also you 
can install small softwares, copy few files etc, though it is recommended to 
use Startup tasks with elevated privileges i.e. Admin Mode. (Note that once 
the machine is restarted your softwares will no longer be there, so better use 
startup tasks) 

The steps to enable and configure remote desktop are really simple. 

First you will need to <span class="fullpost">install the latest [Azure SDK 
1.3 tools for Visual 
Studio](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=7a1089b6-4050-4307-86c4-9dadaa5ed018). 
You can just install VSCloudService.exe as it contains SDK as well. 

Once done with the installation, create a new Cloud Service project and add a 
new worker or web role and then foloow the below steps: 

1. Go to http://windows.azure.com and create a new Hosted Service. This hosted 
service will be the one where you will deploy your roles. 
2. Create a pfx certiciate and export its private key. If you already have the 
certificate then upload that certificate to your service else upload the 
certificate you just created. 
3. Then in Visual Studio, Right Click your service and click Publish. 

<div class="separator" style="clear: both; text-align: center;">[<img 
border="0" height="320" ox="true" 
src="http://1.bp.blogspot.com/_S92PTW6g_pk/TPYWjubbNPI/AAAAAAAADH8/ONLiWz9Tn9k/s320/01.png" 
width="314" 
/>](http://1.bp.blogspot.com/_S92PTW6g_pk/TPYWjubbNPI/AAAAAAAADH8/ONLiWz9Tn9k/s1600/01.png) 
<div class="separator" style="clear: both; text-align: center;"><a 
href="http://3.bp.blogspot.com/_S92PTW6g_pk/TPYWmilEApI/AAAAAAAADIA/E11ohkz3YKM/s1600/02.png" 
imageanchor="1" style="margin-left: 1em; margin-right: 1em;"></a> 
<span class="fullpost"> 
4. In the dialog that appears click “Confiure Remote Desktop 
connections…” 

[<img border="0" height="301" ox="true" 
src="http://3.bp.blogspot.com/_S92PTW6g_pk/TPYWmilEApI/AAAAAAAADIA/E11ohkz3YKM/s320/02.png" 
width="320" 
/>](http://3.bp.blogspot.com/_S92PTW6g_pk/TPYWmilEApI/AAAAAAAADIA/E11ohkz3YKM/s1600/02.png) 

5. Enable the checkbox for "Enable connections for all roles". 
6. Select the same certificate that you uploaded to your service in step 2. 
7. Enter username, password and expiration date for the credentials that you 
need to use for remote desktop log in. 

[<img border="0" height="317" ox="true" 
src="http://2.bp.blogspot.com/_S92PTW6g_pk/TPYWnC7WQxI/AAAAAAAADIE/tBAtxNKazN8/s320/03.png" 
width="320" 
/>](http://2.bp.blogspot.com/_S92PTW6g_pk/TPYWnC7WQxI/AAAAAAAADIE/tBAtxNKazN8/s1600/03.png) 

8. Click Ok and Publish. 
9. Now go back to the portal, select the instance and click on Connect button 
at the top. 

This is it. You are done You can optionally save the rdp file you get in the 
last step to your desktop to connect to your instance without going to the 
portal again. 