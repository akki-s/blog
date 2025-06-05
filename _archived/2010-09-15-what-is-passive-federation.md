---
layout: post
title: What is Passive Federation
date: '2010-09-15T16:04:00.005+05:30'
author: aks
tags:
- What Is
- Security
- ACS
modified_time: '2010-09-22T01:04:42.032+05:30'
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-8387047104045135839
blogger_orig_url: http://techyfreak.blogspot.com/2010/09/what-is-passive-federation.html
---

When i started working with ACS on Appfabric I kept on encountering various 
terminologies, one of them was Passive Federation and it was widely used. I 
could understand the term Federation but passive was not clear to me ever. 

So, I did some research and finally got an answer <span class="fullpost">by 
[Vittorio](http://blogs.msdn.com/members/vibro/) at 
[Vibro.net](http://blogs.msdn.com/b/vbertocci/archive/2008/06/05/active-passive-and-passive-aggressive.aspx) 
. 

The term Passive refers to all those requests that are made by a requestor 
which is not capable of producing proper SOAP request, for example Web 
Clients. This means all Web clients are Passive Requestor. Passive Federation 
emulates WS-Trust on top of GET, POST and cookies. This involves multiple 
redirects involving browser, resources, requestor and cookies. Check the 
[diagram by 
Vittorio](http://blogs.msdn.com/blogfiles/vbertocci/WindowsLiveWriter/ActivePassiveandPassiveAggressive_C481/image_b7c24359-5827-4f7f-a6fa-686d497f4af1.png) 
for details. 

At Present whenever someone refers to WS-Federation it simply means Passive 
Federation, as Active requestors are capable of handling WS-Trust directly. 

So now if we are implementing Windows Live ID single sign on in a our 
application, may be by using Appfabric ACS Labs we essentially mean Passive 
Federation. I will be shortly talking about my experiences with AppFabric ACS 
Labs. 