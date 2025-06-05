---
layout: post
title: 'Sending Mails in .NET framework 2.0 : new namespace System.Net.Mail'
date: '2007-03-13T19:26:00.000+05:30'
author: aks
tags:
- ".Net"
modified_time: '2007-03-13T19:27:47.374+05:30'
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-6917061444900921845
blogger_orig_url: http://techyfreak.blogspot.com/2007/03/sending-mails-in-net-framework-20-new.html
---

<p class="MsoNormal"><span style="font-family:Verdana;font-size:85%;"><span 
style="font-size: 10pt; font-family: Verdana;">If you have used 
System.Web.Mail  namespace in .NET 1.x for sending emails programmatically, 
expect a surprise.   All classes within this namespace have been deprecated in 
favor of the new  System.Net.Mail namespace. System.Net.Mail contains classes 
such as MailMessage,  Attachment, MailAddress, SmtpClient, etc to help us send 
emails in the 2.0  world.  The features provided by this namespace, in a 
nutshell, are given  below.<o:p></o:p></p> <p class="MsoNormal"><span 
style="font-family:Verdana;font-size:85%;"><span style="font-size: 10pt; 
font-family: Verdana;"><o:p> </o:p></p> <ul style="margin-top: 0in;" 
type="disc"><li class="MsoNormal" style=""><span 
style="font-family:Verdana;font-size:85%;"><span style="font-size: 10pt; 
font-family: Verdana;">MailMessage is the  main class that represents an email 
message.<o:p></o:p>  </li><li class="MsoNormal" style=""><span 
style="font-family:Verdana;font-size:85%;"><span style="font-size: 10pt; 
font-family: Verdana;">Use MailAddress class  to represent the sender and each 
recipient.  <o:p></o:p> </li><li class="MsoNormal" style=""><span 
style="font-family:Verdana;font-size:85%;"><span style="font-size: 10pt; 
font-family: Verdana;">Use SmtpClient class  to connect to the SMTP server and 
send the email, both synchronously and  asynchronously.<o:p></o:p>  </li><li 
class="MsoNormal" style=""><span 
style="font-family:Verdana;font-size:85%;"><span style="font-size: 10pt; 
font-family: Verdana;">Use AlternateView  class to create the email content in 
alternate formats, say one in HTML and  other plain text, to support different 
recipient types.<o:p></o:p>   </li><li class="MsoNormal" style=""><span 
style="font-family:Verdana;font-size:85%;"><span style="font-size: 10pt; 
font-family: Verdana;">Use LinkedResource  class to associate an image with 
the email content.<o:p></o:p>  </li><li class="MsoNormal" style=""><span 
style="font-family:Verdana;font-size:85%;"><span style="font-size: 10pt; 
font-family: Verdana;">SmtpPermission class  and SmtpPermissionAttribute can 
be used for code access  security.<o:p></o:p> </li> <p class="MsoNormal"><span 
style="font-family:Verdana;font-size:85%;"><span style="font-size: 10pt; 
font-family: Verdana;"><o:p> </o:p></p> <p class="MsoNormal"><span 
style="font-family:Verdana;font-size:85%;"><span style="font-size: 10pt; 
font-family: Verdana;">To know more about some of these  classes, [read this  
article](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).<o:p></o:p></p> 