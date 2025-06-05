---
layout: post
title: Guide to Windows Azure Connect
date: '2010-11-15T20:53:00.003+05:30'
author: aks
tags:
- Azure
- Connect
modified_time: '2010-11-16T12:09:59.618+05:30'
thumbnail: http://2.bp.blogspot.com/_S92PTW6g_pk/TOFN9bVPzmI/AAAAAAAADHo/YfH9BydwcBo/s72-c/connect1.png
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-5946834650974426908
blogger_orig_url: http://techyfreak.blogspot.com/2010/11/guide-to-windows-azure-connect.html
---

Building applications for cloud and hosting them on cloud is one of the great 
things that happened in recent times. However, you might be having number of 
existing applications that you wish to migrate to cloud, but you do not want 
to move your database server to the cloud. Or you want to create a new 
application and host it in the cloud, but this new application needs to 
communicate with your existing on-premise applications hosted in your 
enterprise's network. Other case might be that your new application that you 
wish to host in cloud will rely for its authentication on your enterprise's 
Active Directory. 

What options do you have? You can think of re-writing your on-premise 
applications for azure and then host them in azure, or in case of Database 
servers, you can move your DB servers to SQL Azure in cloud. But you have 
other easier option too, that has been announced in PDC10 and which will be 
released by the end of this year. What we are talking about is **Windows Azure 
Connect**. 

Later in this post we will talk about [How to setup windows azure 
connect](http://techyfreak.blogspot.com/2010/11/guide-to-windows-azure-connect.html#connect-setup) 
and [Azure Connect Use Cases &amp; 
Scenarios](http://techyfreak.blogspot.com/2010/11/guide-to-windows-azure-connect.html#connect-scenarios). 
But first let's go through what connect is and why do we need it. 

<span class="fullpost"> 

## What is Windows Azure ConnectWindows Azure Connect provides secure network 
connectivity between your on-premises environments and Windows Azure through 
standard IP protocols such as TCP and UDP. Connect provides IP-level 
connectivity between a Windows Azure application and machines running outside 
the Microsoft cloud. 

<table align="center" cellpadding="0" cellspacing="0" 
class="tr-caption-container" style="margin-left: auto; margin-right: auto; 
text-align: center;"><td style="text-align: center;">[<img border="0" 
height="158" 
src="http://2.bp.blogspot.com/_S92PTW6g_pk/TOFN9bVPzmI/AAAAAAAADHo/YfH9BydwcBo/s320/connect1.png" 
width="320" 
/>](http://2.bp.blogspot.com/_S92PTW6g_pk/TOFN9bVPzmI/AAAAAAAADHo/YfH9BydwcBo/s1600/connect1.png)<td 
class="tr-caption" style="text-align: center;">Figure 1: Windows Azure Connect 


This combination can be of different scenarios, which we will see later in the 
section of Use Case and Scenarios. 
In first CTP release we can access Azure resources by installing connect agent 
on the non-azure resources. The upcoming release will support Windows Server 
2008 R2, Windows Server 2008, Windows 7, Windows Vista SP1, and up. A point to 
be noted here is that this isn’t a full-fledged virtual private network 
(VPN). However, future plans are to extend current functionality which will 
enable connectivity using your already existing on premise VPN devices that 
you use within your organization today. 

<h2 id="connect-setup">Setup Azure Connect</h2>Windows Azure Connect is a 
simple solution. Setting it up doesn’t require contacting your network 
administrator. All that’s required is the ability to install the endpoint 
agent on the local machine. Process for setting up Azure Connect involves 3 
steps: 

1. [Enable Windows Azure Roles for external 
connectivity](http://techyfreak.blogspot.com/2010/11/guide-to-windows-azure-connect.html#configure-roles). 
This is done using service model. 
2. [Enable your on-premise/local computers for 
connectivity](http://techyfreak.blogspot.com/2010/11/guide-to-windows-azure-connect.html#configure-premise) 
by installing windows azure connect agent. 
3. [Manage Network 
Policy](http://techyfreak.blogspot.com/2010/11/guide-to-windows-azure-connect.html#manage-policy) 
through Windows Azure Portal. 

Final step is to configure and define network policy. This defines which WA 
roles and local computers that you have enabled for connect are able to 
communicate with each other. This is done using WA admin portal. It provides 
very granular level of control. 

<h3 id="configure-roles">1. Enable Windows Azure Roles for external 
connectivity</h3>To use Connect with a Windows Azure service, we need to 
enable one or more of its Roles. These roles can be Web/worker role or a new 
role Vm Role announced at PDC10. 
For web/worker role, the only thing we need to do is to add an entry in your 
.csdef. You simply add a line of xml in your .csdef specifying to import or 
include windows connect plugin. Then, you need to specify your ActivationToken 
in ServiceConfiguration (.cscfg) file. ActivationToken is a unique 
per-subscription token, which means if you have two different Azure 
subscriptions then you will have individual Activation Token for each 
subscription. This token is direclty accessed from Admin UI. 

For VM role, install the Connect Agent in VHD image using the Connect VM 
install package. This package is available through Windows Azure Admin Portal 
and it contains the ActivationToken within itself. 

Also, in our .cscfg file you can specify optional settings for managing AD 
domain-join and service availability. 

Once these configuration are done for a role, connect agent will automatically 
be deployed for each new role instance that starts up. This means, if 
tomorrow, for example, you add more instances to the roles then each new 
instances will automatilcally be provisioned to use Azure Connect. 

<h3 id="configure-premise">2. Enable your on-premise/local computers for 
connectivity </h3>Your Local on-premise computers are enabled for connectivity 
with Azure Services by installing &amp; activating the Connect agent. The 
connect agent can be installed to your local computers in two ways: 
1. **Web-based installation**: From your Windows Azure Admin portal you get a 
web-based installation link. This link is per-subscription basis and it has 
the activation token embedded within the URL. 
2. S**tandalone install package**: Other option you have is to use standalone 
installation package. You can run this installation package using any standard 
software distribution tool installed in your system, as you do it for other 
programs. It will add activation token into registry and read its value from 
there. 

Once Connect agent is installed, you will also have a client UI on your system 
as well as connect agent tray icon in your system tray. Connect agent tray 
icon &amp; client UI let you view the current status, both the activation 
status as well as network connectivity status, of azure connect agent. Also it 
provides basic tasks such as network refresh policy. 

Connect agent automatically manages network connectivity between your local 
computers and Azure services/apps. To do this it does several things 
including: 

1. Setting up virtual network adapter 
1. “Auto-connects” to Connect relay service as needed 
1. Configures IPSec policy based on network policy 
1. Enables DNS name resolution 
1. Automatically syncs latest network policies 


<h3 id="manage-policy">3. Manage Network Policy </h3>Once you have identified 
your Azure Roles that need to connect to on-premise comouters, and also you 
have installed and actiated Connect Agents on your local computers, you need 
to configure which roles connect to which of the configured local computers. 
To do this we specify Network Policy and it is managed through Windows Azure 
admin portal. Again, this is done on a per-subscription basis. 

Management model for connect is pretty simple. There are 3 different type of 
operations you can do: 

1. You can take your local computers, that have been enabled and activated for 
Windows Azure Connect by installing connect agents on them, and organize them 
into groups. e.g. you might create a group that contain your SQL Server 
computers that one of your azure role needs to connect to, and call this "SQL 
Server Group". Or you might put all you developer laptops into My Laptops 
group or you might put computers related to a given project into a group. 
However, there are two constraints - first, a computer can only belong to a 
single group at a time and second, when you have a new computer where you just 
installed Windows Azure Connect agent on, the newly activated computer is 
unassigned by default meaning it doesn’t belong to any group, and therefore 
they wont have connectivity. 

<table align="center" cellpadding="0" cellspacing="0" 
class="tr-caption-container" style="margin-left: auto; margin-right: auto; 
text-align: center;"><td style="text-align: center;">[<img border="0" 
height="302" id="figure-2" 
src="http://2.bp.blogspot.com/_S92PTW6g_pk/TOFN-IQKIXI/AAAAAAAADHs/BbSZh8vtY1c/s320/connect2.png" 
width="320" 
/>](http://2.bp.blogspot.com/_S92PTW6g_pk/TOFN-IQKIXI/AAAAAAAADHs/BbSZh8vtY1c/s1600/connect2.png)<td 
class="tr-caption" style="text-align: center;">Figure 2: Connect Groups and 
Hosted Relay 
As shown in the Figure 2 above, we can have groups of local Development 
machines, and call this group “Development Computers” and our Windows 
Azure Role – “Role A” will connect to this group. Similarly, we can have 
a group of our DB servers and name this group as “Database Servers” and 
have our Role B connect to this group. All these configurations will be done 
from Azure Admin Portal. 

2. Windows Azure roles can be connected to the group of these local computers 
from admin UI. It is done for all of the instances that make up that azure 
role and all the local computers in that group. One thing to note is that 
**WINDOWS AZURE CONNECT DOES NOT CONTROL CONNECTIVITY WITHIN AZURE**. In other 
words you can’t use WA connect to connect between two roles which are part 
of a service or two instances that are part of the same role. The reason for 
this there are already existing mechanisms for doing this. WA connect is all 
about connecting azure roles to computers outside of azure. 

3. Additionally, it has the ability of connecting group of local computers to 
other groups of local computers. This enables network connectivity between 
computers in each of these groups. Additionally, we can have interconnectivity 
for a given group. This allows all computers within that group to have 
connectivity with each other. This functionality is useful in ad-hoc or 
roaming scenarios e.g. if you like to have your developer laptop to have a 
secure connectivity back to a server that resides in your corporate network. 

<h2 id="network">Networking Behavior</h2>Once you have defined network policy 
, Azure connect service will automatically setup secure IP level network 
connection between all of those role instances that you enabled for 
connectivity and local computers, based upon the network policy you specified. 

All of the traffic is tunneled through Hosted relay service. This ensures 
Windows Azure Connect will allow you to connect to resources regardless of 
firewalls and NAT configuration of the environment they are residing in. Your 
Azure Role Instances and your local computers, all will have, through WA 
Connect, secure IP level connectivity. This connectivity holder is established 
regardless of the physical network topology of those resources. Due to any 
firewalls or NATS, many local computers might not have direct public IP 
addresses. But using Windows Azure Connect you can establish connectivity 
through all of those firewalls and NATs, as long as those machines have 
outbound HTTPS access to the hosted relay service. 

[Figure 
2](http://techyfreak.blogspot.com/2010/11/guide-to-windows-azure-connect.html#figure-2) 
above shows how local on-premise computers are connected to web role instances 
through Hosted Relay Service. 

Other point to note here is that network connectivity that Windows Azure 
Connect establishes is secured fully in an end to end fashion using IPSec, and 
Azure Connect agent takes care of setting this up automatically. 

Each connected machine has a routable IPv6 address. Important point to note is 
even being part of Windows Azure Connect network, any of your existing network 
behavior remains unchanged. Connect doesn't alter the behavior of you existing 
network configuration. It just joins you to an additional network with 
connect. 

Once the network connectivity is established, applications running either in 
azure or on local system will be able to resolve the names to IP address using 
DNS name resolutions that connect provides. 

<h2 id="connect-scenarios">Use Cases/Scenarios</h2>Let's go through few of the 
scenarios where Windows Azure Connect might be our solution: 

1. Enterprise app migrated to Windows Azure that requires access to on-premise 
SQL Server. This is most widely seen scenario. You have your on-premise 
application which connects to your on-premise database server. Now you are 
willing to move your application to Azure, but due to some business reasons 
the database this application uses needs to stay on premises. Using Windows 
Azure connect, the web role, converted from on-premise ASP.NET application, 
can access the on-premises database directly. The wonderful thing here is that 
it does not even require to change the connection string, as both web-role and 
database server are in the same network. 

2. Domain-joined to the on-premises environment. This scenario opens up 
various other use cases. One such case is letting the application use existing 
Active Directory accounts and groups for access control to your Azure 
application. Theefore, you should not be bothered about exposing your AD to 
internet to provide access control in cloud. Using Windows Azure Connect your 
azure services/apps are domain joined with your Active Directory domain. This 
enables to control who can access azure services based on your existing 
windows user or group accounts. 

Also, domain-joining allows single sign-on to the cloud application by 
on-premises users, or it can allow your azure services to connect to 
on-premise SQL server using Windows Integrated Authentication. 

We will see in a seperate post [How to Domain Join Azure Roles with Active 
Directory using 
Connect](http://techyfreak.blogspot.com/2010/11/active-directory-domain-join-of-azure.html). 

3. Since you have direct connectivity to VMs running in cloud, you can 
**remotely administer and trouble-shoot your Windows Azure Roles** 