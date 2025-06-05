---
layout: post
title: Changing Drive Letter of an Azure Drive (aka X-drive)
date: '2011-02-07T03:20:00.002+05:30'
author: aks
tags:
- VM Role
- Azure
- Azure Drive
modified_time: '2011-04-19T17:21:20.705+05:30'
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-7091385374513973594
blogger_orig_url: http://techyfreak.blogspot.com/2011/02/changing-drive-letter-of-azure-drive.html
---

<div dir="ltr" style="text-align: left;" trbidi="on">Sometimes it might be 
necessary that you want your Azure drive to be always mounted on a fixed drive 
letter. Consider a scenario of an Azure VM Role where you need to mount an 
azure drive for data persistance and your VM demands the same letter for you 
azure drive, e.q. you installed SQL Server on your VM Role and for mdf files 
you specified azure drive as path so as to make the data persist. 

But now, we know that Azure drives are mounted on random-drive letters. To 
always have a fixed letter what you can do is<span class="fullpost"> that 
after your drive is mounted, you can change the drive letter to a fixed value 
using diskpart from within the windows service you use to mount the drive in 
VM Role, or from other part of code if you are not working with VM Role. Check 
this post to know [how to mount Azure Drive in VM 
Role.](http://techyfreak.blogspot.com/2011/02/mounting-azure-drive-in-azure-virtual.html) 

To get a basic idea on how to change drive letter using diskpart visit this 
Microsoft support link : 
[http://support.microsoft.com/kb/928543](http://support.microsoft.com/kb/928543) 

To change the drive letter of the mounted Azure Drive using diskpart, we will 
create a temporary file in local resource storage. This temp file will be used 
to store the current and target drive letters, and using this we can construct 
diskpart commands. Following code can be used to achieve the same: 
<div class="MsoNormal" style="line-height: normal; margin-bottom: 0cm; 
mso-layout-grid-align: none;"><span class="fullpost"><span style="color: blue; 
font-family: Consolas; font-size: 9.5pt;">   <span class="fullpost"> 
<div class="MsoNormal" style="line-height: normal; margin-bottom: 0cm; 
mso-layout-grid-align: none;"> 
<div class="MsoNormal" style="line-height: normal; margin-bottom: 0cm; 
mso-layout-grid-align: none; text-indent: 36pt;"><span style="color: green; 
font-family: Consolas; font-size: 9.5pt;">//create temporary diskpart 
file<span style="color: blue; font-family: Consolas; font-size: 9.5pt;"><div 
class="MsoNormal" style="line-height: normal; margin-bottom: 0cm; 
mso-layout-grid-align: none; text-indent: 36pt;"><span style="color: blue; 
font-family: Consolas; font-size: 9.5pt;">string<span style="font-family: 
Consolas; font-size: 9.5pt;"> diskpartFile = drive.CachePath + <span 
style="color: #a31515;">"\\diskpart.txt";<div class="MsoNormal" 
style="line-height: normal; margin-bottom: 0cm; mso-layout-grid-align: none;"> 
<div class="MsoNormal" style="line-height: normal; margin-bottom: 0cm; 
mso-layout-grid-align: none; text-indent: 36pt;"><span style="color: blue; 
font-family: Consolas; font-size: 9.5pt;">if<span style="font-family: 
Consolas; font-size: 9.5pt;"> (<span style="color: 
#2b91af;">File.Exists(diskpartFile))<div class="MsoNormal" style="line-height: 
normal; margin-bottom: 0cm; mso-layout-grid-align: none; text-indent: 
36pt;"><span style="font-family: Consolas; font-size: 9.5pt;">{<div 
class="MsoNormal" style="line-height: normal; margin: 0cm 0cm 0cm 36pt; 
mso-layout-grid-align: none; text-indent: 36pt;"><span style="color: #2b91af; 
font-family: Consolas; font-size: 9.5pt;">File<span style="font-family: 
Consolas; font-size: 9.5pt;">.Delete(diskpartFile);<div class="MsoNormal" 
style="line-height: normal; margin-bottom: 0cm; mso-layout-grid-align: none; 
text-indent: 36pt;"><span style="font-family: Consolas; font-size: 
9.5pt;">}<div class="MsoNormal" style="line-height: normal; margin-bottom: 
0cm; mso-layout-grid-align: none;"> 
<div class="MsoNormal" style="line-height: normal; margin-bottom: 0cm; 
mso-layout-grid-align: none; text-indent: 36pt;"> <div class="MsoNormal" 
style="line-height: normal; margin-bottom: 0cm; mso-layout-grid-align: none; 
text-indent: 36pt;"><span style="color: blue; font-family: Consolas; 
font-size: 9.5pt;">string<span style="font-family: Consolas; font-size: 
9.5pt;"> driveLetter = drive.DriveLetter; 
<div class="MsoNormal" style="line-height: normal; margin-bottom: 0cm; 
mso-layout-grid-align: none;"><div class="MsoNormal" style="line-height: 
normal; margin-bottom: 0cm; mso-layout-grid-align: none; text-indent: 
36pt;"><span style="color: green; font-family: Consolas; font-size: 
9.5pt;">//start the process<span style="font-family: Consolas; font-size: 
9.5pt;"><div class="MsoNormal" style="line-height: normal; margin-bottom: 0cm; 
mso-layout-grid-align: none; text-indent: 36pt;"><span style="color: blue; 
font-family: Consolas; font-size: 9.5pt;">using<span style="font-family: 
Consolas; font-size: 9.5pt;"> (<span style="color: #2b91af;">Process 
changeletter = <span style="color: blue;">new <span style="color: 
#2b91af;">Process())<div class="MsoNormal" style="line-height: normal; 
margin-bottom: 0cm; mso-layout-grid-align: none; text-indent: 36pt;"><span 
style="font-family: Consolas; font-size: 9.5pt;">{<div class="MsoNormal" 
style="line-height: normal; margin: 0cm 0cm 0cm 36pt; mso-layout-grid-align: 
none; text-indent: 36pt;"><span style="font-family: Consolas; font-size: 
9.5pt;">changeletter.StartInfo.Arguments = <span style="color: #a31515;">"/s" 
+ <span style="color: #a31515;">" " + diskpartFile;<div class="MsoNormal" 
style="line-height: normal; margin: 0cm 0cm 0cm 72pt; mso-layout-grid-align: 
none;"><span style="font-family: Consolas; font-size: 
9.5pt;">changeletter.StartInfo.FileName = System.<span style="color: 
#2b91af;">Environment.GetEnvironmentVariable(<span style="color: 
#a31515;">"WINDIR") + <span style="color: 
#a31515;">"\\System32\\diskpart.exe";<div class="MsoNormal" 
style="line-height: normal; margin: 0cm 0cm 0cm 36pt; mso-layout-grid-align: 
none; text-indent: 36pt;"><span style="font-family: Consolas; font-size: 
9.5pt;">changeletter.Start();<div class="MsoNormal" style="line-height: 
normal; margin: 0cm 0cm 0cm 36pt; mso-layout-grid-align: none; text-indent: 
36pt;"><span style="font-family: Consolas; font-size: 
9.5pt;">changeletter.WaitForExit();<div class="MsoNormal" style="line-height: 
normal; margin-bottom: 0cm; mso-layout-grid-align: none; text-indent: 
36pt;"><span style="font-family: Consolas; font-size: 9.5pt;">}<div 
class="MsoNormal" style="line-height: normal; margin-bottom: 0cm; 
mso-layout-grid-align: none;"> 
<div class="MsoNormal" style="line-height: normal; margin-bottom: 0cm; 
mso-layout-grid-align: none; text-indent: 36pt;"><span style="color: #2b91af; 
font-family: Consolas; font-size: 9.5pt;">File<span style="font-family: 
Consolas; font-size: 9.5pt;">.Delete(diskpartFile);<div class="MsoNormal" 
style="line-height: normal; margin-bottom: 0cm; mso-layout-grid-align: 
none;"><span class="Apple-style-span" style="font-family: Consolas; font-size: 
x-small;"> 