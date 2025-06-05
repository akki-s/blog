---
layout: post
title: Unable to connect to Azure VM Role for a long time after it is Deployed or
  Restarted
date: '2011-01-22T01:03:00.000+05:30'
author: aks
tags:
- VM Role
- Azure
modified_time: '2011-01-22T01:03:11.565+05:30'
thumbnail: http://1.bp.blogspot.com/_S92PTW6g_pk/TTnfQmz_trI/AAAAAAAADcU/dodNPra0Ps8/s72-c/rmf-01.png
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-6111645505301758461
blogger_orig_url: http://techyfreak.blogspot.com/2011/01/unable-to-connect-to-azure-vm-role-for.html
---

I have been really frustrated in waiting for days to log in to my VM Role 
instance via RDP. I have encountered this issue multiple times. Once a VM Role 
is deployed or it is restarted, we can not login via RDP for a long period, 
and it takes a lot of time, sometimes 2-3 (random) days once it is back and 
allows us to connect via RDP. 

## Reason 

This issue occurs due to a problem with the Azure Remote Forwarder Service, 
which runs in your VM Role instance. Due to this service sometimes RDP packets 
get distorted which cresults in connection timing out or closing it. 

## Solution 

The solution I describe here <span class="fullpost">is not really a solution, 
rather it is a workaround. The workaround is to set the Windows Azure Remote 
Forwarder Service from Automatic to Automatic – Delayed, by going to Service 
Management Console (Services.msc) 

<div class="separator" style="clear: both; text-align: center;">[<img 
border="0" height="280" width="400" 
src="http://1.bp.blogspot.com/_S92PTW6g_pk/TTnfQmz_trI/AAAAAAAADcU/dodNPra0Ps8/s400/rmf-01.png" 
/>](http://1.bp.blogspot.com/_S92PTW6g_pk/TTnfQmz_trI/AAAAAAAADcU/dodNPra0Ps8/s1600/rmf-01.png) 

<div class="separator" style="clear: both; text-align: center;">[<img 
border="0" height="400" width="356" 
src="http://2.bp.blogspot.com/_S92PTW6g_pk/TTnfXZjZIEI/AAAAAAAADcc/EODqIm2OoTg/s400/rmf02.png" 
/>](http://2.bp.blogspot.com/_S92PTW6g_pk/TTnfXZjZIEI/AAAAAAAADcc/EODqIm2OoTg/s1600/rmf02.png) 