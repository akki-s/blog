---
layout: post
title: 'AppFabric ACS Exception: A potentially dangerous Request.Form value was detected
  from the client (wresult="<t:RequestSecurityTo...")'
date: '2010-09-20T16:59:00.002+05:30'
author: aks
tags:
- ".Net"
- Azure
- ACS
modified_time: '2010-09-20T17:00:29.405+05:30'
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-3820821594814219259
blogger_orig_url: http://techyfreak.blogspot.com/2010/09/detected-from-client-wresult.html
---

AppFabric ACS Exception : A potentially dangerous Request.Form value was 

When you are working with AppFabric ACS labs and implement identity providers 
such as Windows Livefollowing error might show up when you try to run your 
application 

<blockquote>A potentially dangerous Request.Form value was detected from the 
client (wresult="&lt;t:RequestSecurityTo...").</blockquote> 
This error occurs because <span class="fullpost">ACS sends you a SAML in a 
POST request, as the wresult value token. ASP.NET considers this as if a user 
typed some XML content in a textbox called "wresult" which is considered to be 
unsafe by ASP.NET. ASP.NET considers this kind of values as potentially 
dangerous, as some kind of script injection. 

Therefore, if in your application Request Validation is enables this exception 
is thrown. 

As a solution, you need to add ValidateRequest="false" in your page or in you 
web.config. This is a required step in case you want to integrate AppFabric 
ACS. 