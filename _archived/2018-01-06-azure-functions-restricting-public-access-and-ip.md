---
layout: post
title: Blocking all external access and securing Azure Functions
date: '2018-01-06T08:48:00.000+05:30'
comments: true
author: aks
categories: [Azure]
tags: [Security, Azure, Azure Functions]
modified_time: '2018-01-16T08:53:47.160+05:30'
thumbnail: https://1.bp.blogspot.com/-rx72y9R2tDs/Wl1v80xPwXI/AAAAAAAAJRQ/bYes1l6Zg3kKQNDChsB3vUKnaVzCXkplACLcBGAs/s72-c/azure-functions-02.PNG
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-5910727056765322260
blogger_orig_url: http://techyfreak.blogspot.com/2018/01/restricting-public-access-and-ip.html
---

If you're developing a solution with micro services architecture with Azure, there are chances you've been using Azure Functions to develop a service. For example, If you're solution can involving sending email alerts. One of the microservices here can be Email Service. This can further be broken down into two micro services. One to enqueue mail message to Azure storage queue. This can be a Http Triggered function. Second one can be a Queue trigerred function that prcesses these messages and actually sends the email, using a third party API. 


However, it's a good idea to restrict public access to Azure Function if it's meant to be consumed only internally. 


One way to do this is to make use of the [built-in keys](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#working-with-keys). By default all `HttpTrigerred` functions require a key. You can make this required for other functions and share the secret code only with the resources that are meant to access it. This is a good solution, but if your keys are leaked (and your function's URL is known to public), then someone could access it. You can roll the keys and generate again. To manage the keys, access your function from portal, and click Manage. 


On the networking side, if you publish your azure function as hosted function, in an App Service Plan, then you can setup IP restrictions. To do so, open your hosted function app in Azure portal, and go to `Platform Features` tab. Once there access `networking -> IP restrictions`. On this screen you can add the IP addresses that are allowed to access the app. You can add single IPs or use subnet mask for range. Please note that as this is done at App Service level, all functions within the app, would follow the same rules. You can always separate your functions to different app service. 


IP restrictions allow you to define a static list of IP addresses that are allowed access to your app. The requests to this app from an IP address not in this list will get an HTTP 403 Forbidden response. If no rules are defined, your app will accept traffic from any IP. 

[<img 
border="0" data-original-height="618" data-original-width="1600" height="246" 
src="https://1.bp.blogspot.com/-rx72y9R2tDs/Wl1v80xPwXI/AAAAAAAAJRQ/bYes1l6Zg3kKQNDChsB3vUKnaVzCXkplACLcBGAs/s640/azure-functions-02.PNG" 
width="640" 
/>](https://1.bp.blogspot.com/-rx72y9R2tDs/Wl1v80xPwXI/AAAAAAAAJRQ/bYes1l6Zg3kKQNDChsB3vUKnaVzCXkplACLcBGAs/s1600/azure-functions-02.PNG) 


[<img 
border="0" data-original-height="1600" data-original-width="755" height="400" 
src="https://4.bp.blogspot.com/-udb5N-k2AXU/Wl1v9EAheqI/AAAAAAAAJRU/4tZCEyTpjsMvyVeBnpuf2nwO3nIiY28OQCLcBGAs/s400/azure-functions-03.PNG" 
width="187" 
/>](https://4.bp.blogspot.com/-udb5N-k2AXU/Wl1v9EAheqI/AAAAAAAAJRU/4tZCEyTpjsMvyVeBnpuf2nwO3nIiY28OQCLcBGAs/s1600/azure-functions-03.PNG) 


[<img 
border="0" data-original-height="700" data-original-width="1600" height="280" 
src="https://4.bp.blogspot.com/-rsmnGFTYsMg/Wl1v9Lg3UrI/AAAAAAAAJRM/nI7iaQmAOgQ5ohPqKaSBENbdE_RPRZaTgCLcBGAs/s640/azure-functions-04.PNG" 
width="640" 
/>](https://4.bp.blogspot.com/-rsmnGFTYsMg/Wl1v9Lg3UrI/AAAAAAAAJRM/nI7iaQmAOgQ5ohPqKaSBENbdE_RPRZaTgCLcBGAs/s1600/azure-functions-04.PNG)
