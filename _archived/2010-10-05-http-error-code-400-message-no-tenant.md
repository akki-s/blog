---
layout: post
title: 'HTTP Error Code:  400  Message:  No tenant signing key of type X509 certificate
  is provisioned.'
date: '2010-10-05T00:24:00.000+05:30'
author: aks
tags:
- Azure
- ACS
modified_time: '2010-10-05T00:24:16.349+05:30'
thumbnail: http://1.bp.blogspot.com/_S92PTW6g_pk/TKoijF_v44I/AAAAAAAADGA/yA8tvGo7rgY/s72-c/token-signing.PNG
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-4328359692197347976
blogger_orig_url: http://techyfreak.blogspot.com/2010/10/http-error-code-400-message-no-tenant.html
---

After the September release if you are configuring your service namespace as 
per old method you might get following error: 

<blockquote>HTTP Error Code:  400 
Message:  No tenant signing key of type X509 certificate is provisioned. 
Trace ID:  2c46fa55-8ae8-443b-9f8a-ab885593c3fb 
Timestamp</blockquote> 
<span class="fullpost">This is caused because your token signing certificate 
is not configured properly. In order for Federation Metadata to work, this 
signing certificate should be configured for Service Namespace. 

You have to do this by selecting the value in "Used For" set to "Service 
namespace". To perform this under your Service Namespace, select 'Certificate 
and Keys' and then in "Token signing Key/Certificate" under Used for select 
"Service namespace". This will solve the issue. 

<div class="separator" style="clear: both; text-align: center;">[<img 
border="0" 
src="http://1.bp.blogspot.com/_S92PTW6g_pk/TKoijF_v44I/AAAAAAAADGA/yA8tvGo7rgY/s1600/token-signing.PNG" 
alt="No tenant signing key of type X509 certificate is provisioned" 
/>](http://1.bp.blogspot.com/_S92PTW6g_pk/TKoijF_v44I/AAAAAAAADGA/yA8tvGo7rgY/s1600/token-signing.PNG)<span 
class="fullpost"> 

<span class="fullpost"> Access Control Service will use a Service Namespace 
certificate or key to sign tokens if none are present for a specific relying 
party application. Service Namespace certificates are also used to sign 
WS-Federation metadata. 

For SAML tokens, ACS uses an X.509 certificate to sign the token.  ACS will 
use a relying party's certificate, if the relying party has its own 
certificate.  Otherwise, the service namespace certificate is used as a 
fallback.  If there isn't one, an error is shown. 

The Appfabric ACS needs a service namespace certificate configured in order to 
sign Fed metadata. Without this, the Fed metadata cannot be signed and 
attempting to view it will fail. 