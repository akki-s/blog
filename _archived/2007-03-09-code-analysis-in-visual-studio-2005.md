---
layout: post
title: Code Analysis in Visual Studio 2005 Team Suite
date: '2007-03-09T16:24:00.000+05:30'
author: aks
tags:
- ".Net"
- Tips
modified_time: '2007-03-30T00:03:57.044+05:30'
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-8009630016216267218
blogger_orig_url: http://techyfreak.blogspot.com/2007/03/code-analysis-in-visual-studio-2005.html
---

If you are using VS.NET 2005 Team Suite, code analysis is built into the IDE 
itself.  In older version of VS.NET, you might have used FxCop externally for 
comparing against pre-defined rules or would have integrated with the IDE by 
adding FxCop as an Add-In. 

To enable code analysis, open project properties, <span 
class="fullpost">navigate to Code Analysis tab, select “Enable Code 
Analysis” and choose the different rules or categories that you want to run. 
 When you do so, code that does not conform to these rules would be reported 
during build as warnings.  Based on project needs, you can customize some of 
them to report Errors instead of warnings.  This level of granular control 
helps in strict conformance to rules. For example, any violation of a design 
rule can be considered as a bad practice and hence should be configured to 
throw an Error rather than just a warning. 