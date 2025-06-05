---
layout: post
title: Active Directory Domain Join of Azure Roles Using Connect
date: '2010-11-16T12:09:00.001+05:30'
author: aks
tags:
- Azure
- Connect
modified_time: '2010-11-16T12:10:30.425+05:30'
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-1456604010716340513
blogger_orig_url: http://techyfreak.blogspot.com/2010/11/active-directory-domain-join-of-azure.html
---

We already have seen how Windows Azure connect can help us in connecting our 
windows azure role instances to our local computers in the [previous 
post](http://techyfreak.blogspot.com/2010/11/guide-to-windows-azure-connect.html#connect-scenarios). 
We also saw in the [use cases/scenarios for azure 
connect](http://techyfreak.blogspot.com/2010/11/guide-to-windows-azure-connect.html#connect-scenarios), 
that we can domain join our windows azure roles with our on-premise Active 
Directory, and this is possible by using connect plug-in. 

This is a very useful feature provided by Windows Azure Connect. Active 
Directory Domain Join might help you in <span class="fullpost">following 
regards: 
1. You can now control access to your azure role instances based on domain 
accounts. 
1. You can now provide access control using windows authentication along with 
on-premise SQL server. 
1. In general, as customers migrate existing Line of Business applications to 
cloud; many of those applications today are written or assume domain joined 
environment. And with this capability of domain joining your azure roles to 
on-premise AD, this process of migration can be made simpler. 

The process to **setup, enable and configure Active Directory Domain Join 
using connect**, involves following steps: 

<li>Enable one of your domain controller/DNS servers for connectivity by 
installing Windows Azure Connect Agent on that machine. 
Many customers with multiple Domain Controller environment, will have many 
DCs. For such scenario it is recommended to create a dedicated AD site to be 
used for domain joining of your azure roles.</li><li>Configure your Windows 
Azure connect plug-in to automatically domain join your azure role instances 
to active directory. For domain joining there are specific settings to be done 
in Service Configuration file (.cscfg ) 
– credentials (domain account that has permission to domain join these new 
instances coming online) 
- target organizational unit (OU) for where your azure role instances SHOULD 
be located within your AD 
- you can specify list of domain users or groups that will automatically be 
added to local admin groups for your azure role.</li>1. Configure your network 
policy. This will specify which roles will connect to what Active Directory 
servers. This is done from admin portal. 
1. New Windows Azure Role instances will automatically be domain-joined 