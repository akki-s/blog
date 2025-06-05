---
layout: post
title: Debug SSIS Script Component
date: '2007-08-07T15:34:00.000+05:30'
author: aks
tags:
- SQL Server
modified_time: '2007-08-07T15:37:23.072+05:30'
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-41082307973026046
blogger_orig_url: http://techyfreak.blogspot.com/2007/08/debug-ssis-script-component.html
---

While working with your SSIS package, have not you ever tried debugging a 
script component transformation by putting a breakpoint in the VB code? Well, 
i did and found that, unfortunately, it does not work. 

On the other hand we are able to debug a script task using breakpoints in the 
same way as we do in Visual Studio IDE. But now how we go ahead with debugging 
a script component? 
<span class="fullpost"> 
The only options to do the same are to either use the Row Count component or a 
Data Viewer. 

Row Count task won’t be that much useful as it simply states how many rows 
passes through it. 

On the other hand we can utilize the Data Viewer as a much better way to debug 
our script component. To add a Data Viewer, select the connector arrow leaving 
the script component that you want to debug, right click it and select Edit 
(you can also simply double click on that arrow). This will open up the Data 
Flow Path Editor. Just click on Add to add the data viewer. On the Configure 
Data Viewer screen, select Grid as the type. Click the Grid tab. Now you can 
select all those columns that you wish to see are in the Displayed Columns 
list. Now just close this window. 

Now if you run your package, a Data Viewer window will be displayed and it 
will be filled with the data just after the script component is executed. This 
will be the data output by the Script Component.  Click the Play button to 
continue package execution, or simply close the window. This way you can 
monitor all the data rows going through the script component. 

I will admit that this work around of using a data viewer for debugging can 
never make up for the visual studio kind of debugging, but this is all we have 
got. We just can hope that future versions will have better debugging for a 
script component also as it is for the script task currently. 