---
layout: post
title: Azure VM Role CSUPLOAD Error 0x80070070 Cannot prepare VHD
date: '2011-01-05T21:28:00.000+05:30'
author: aks
tags:
- VM Role
- Azure
modified_time: '2011-01-05T21:28:15.771+05:30'
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-5622103930602410692
blogger_orig_url: http://techyfreak.blogspot.com/2011/01/azure-vm-role-csupload-error-0x80070070.html
---

<div>Today I was trying to upload my base image to Azure via csupload so that 
i can attach this vhd to one of my virtual machine (VM) role. However, I got a 
strange error with no details. I kept on looking for the reason and finally I 
found the solution. 
The error was as below: 
<blockquote>Unexpected Error 0x80070070</blockquote><blockquote>Cannot prepare 
VHD C:\Users\user1\Desktop\baseimage.vhd</blockquote> 
## Reason: 
<span class="fullpost">The reason for this error as i discovered was that the 
disk from where it was uploaded was full. When you use csupload it uses 
temporary directory for preparing image and by default it uses the location 
from where you are uploading your base image. So if this disk is full you get 
this error. 

## Solution: 
Delete some files from your drive to have more available space or use a 
location on another drive for temporary purpose using the switch <span 
class="Apple-style-span" style="-webkit-border-horizontal-spacing: 2px; 
-webkit-border-vertical-spacing: 2px; border-collapse: collapse; font-family: 
'Segoe UI', Verdana, Arial; font-size: 13px; line-height: 
18px;">**TempLocation ** 