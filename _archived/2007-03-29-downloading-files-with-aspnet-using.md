---
layout: post
title: Downloading files with ASP.NET using Save As Dialog
date: '2007-03-29T18:37:00.000+05:30'
author: aks
tags:
- ".Net"
modified_time: '2007-03-29T22:09:11.178+05:30'
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-8896849269275192446
blogger_orig_url: http://techyfreak.blogspot.com/2007/03/downloading-files-with-aspnet-using.html
---

<p style="margin-left: 0.5in;"><span style="font-size:100%;">This article 
explains  how to download a file that is stored in the database with the 
possibility of  forcing the browser to open a “Save As” dialog box to save 
the file on to the client system. <o:p></o:p></p> <p style="margin-left: 
0.5in;"><span style="font-size:100%;">The content of the  file is stored in a 
column of a table of image data type.<o:p></o:p></p> <p style="margin-left: 
0.5in;"><span style="font-size:100%;">Such a dialog-box  for saving a file can 
be displayed by using <span class="SpellE"  
style="font-size:100%;">HttpContext.Current.Response<span 
style="font-size:100%;"> property.  <o:p></o:p></p> <p style="margin-left: 
0.5in;"><span class="SpellE"  style="font-size:100%;">HttpContext<span 
style="font-size:100%;"> is a class that  encapsulates all HTTP-specific 
information about an individual HTTP request. The  property <span 
class="SpellE">HttpContext.Current gets the <span class="SpellE">HttpContext 
object for the current HTTP request.<span style="font-size:100%;">  
<o:p></o:p></p> <p style="margin-left: 0.5in;"><span style="font-size:100%;"> 
Here is an example:  <span class="fullpost"><span 
class="fullpost"><o:p></o:p></p> <table class="MsoTableGrid" style="border: 
medium none ; background: rgb(255, 255, 204) none repeat scroll 0% 50%; 
-moz-background-clip: -moz-initial; -moz-background-origin: -moz-initial; 
-moz-background-inline-policy: -moz-initial; margin-left: 0.5in; 
border-collapse: collapse;" border="1" cellpadding="0" cellspacing="0">  <tr 
style=""> <td style="border: 1pt solid windowtext; padding: 0in 5.4pt; width: 
487.8pt;" valign="top" width="650"> <p class="MsoNormal" style="margin-left: 
0.5in; text-indent: -0.5in;"><span style=";font-family:'Courier 
New';font-size:100%;"  ><span style="">1.<span style="font-style: normal; 
font-variant: normal; font-weight: normal; line-height: normal; 
font-size-adjust: none; font-stretch: normal;font-family:'Times New Roman';" > 
            <span class="SpellE"  style="font-size:100%;"><span 
style="font-family:Verdana;">HttpContext.Current.Response.Clear<span 
style=";font-family:Verdana;font-size:100%;"  >();<o:p></o:p></p> <p 
class="MsoNormal" style="margin-left: 0.5in; text-indent: -0.5in;"><span 
style=";font-family:'Courier New';font-size:100%;"  ><span style="">2.<span 
style="font-style: normal; font-variant: normal; font-weight: normal; 
line-height: normal; font-size-adjust: none; font-stretch: 
normal;font-family:'Times New Roman';" >             <span class="SpellE"  
style="font-size:100%;"><span 
style="font-family:Verdana;">HttpContext.Current.Response.ContentType<span 
style=";font-family:Verdana;font-size:100%;"  >  = "file";<span style="">      
   <o:p></o:p></p> <p class="MsoNormal" style="margin-left: 0.5in; 
text-indent: -0.5in;"><span style=";font-family:'Courier New';font-size:100%;" 
 ><span style="">3.<span style="font-style: normal; font-variant: normal; 
font-weight: normal; line-height: normal; font-size-adjust: none; 
font-stretch: normal;font-family:'Times New Roman';" >             <span 
class="SpellE"  style="font-size:100%;"><span 
style="font-family:Verdana;">HttpContext.Current.Response.AddHeader<span 
style=";font-family:Verdana;font-size:100%;"  >("content-disposition",<span 
style="">    <span style="">                                     "<span 
class="SpellE">attachment;filename=" + <span 
class="SpellE">fileName);<o:p></o:p></p> <p class="MsoNormal" 
style="margin-left: 0.5in; text-indent: -0.5in;"><span 
style=";font-family:'Courier New';font-size:100%;"  ><span style="">4.<span 
style="font-style: normal; font-variant: normal; font-weight: normal; 
line-height: normal; font-size-adjust: none; font-stretch: 
normal;font-family:'Times New Roman';" >                <span 
style=";font-family:Verdana;font-size:100%;color:green;"   >//  Remove the 
<span class="SpellE">charset from the Content-Type  header.<o:p></o:p></p> <p 
class="MsoNormal" style="margin-left: 0.5in; text-indent: -0.5in;"><span 
style=";font-family:'Courier New';font-size:100%;"  ><span style="">5.<span 
style="font-style: normal; font-variant: normal; font-weight: normal; 
line-height: normal; font-size-adjust: none; font-stretch: 
normal;font-family:'Times New Roman';" >                <span class="SpellE"  
style="font-size:100%;"><span 
style="font-family:Verdana;">HttpContext.Current.Response.Charset<span 
style=";font-family:Verdana;font-size:100%;"  >  = "";<o:p></o:p></p> <p 
class="MsoNormal" style="margin-left: 0.5in; text-indent: -0.5in;"><span 
style=";font-family:'Courier New';font-size:100%;"  ><span style="">6.<span 
style="font-style: normal; font-variant: normal; font-weight: normal; 
line-height: normal; font-size-adjust: none; font-stretch: 
normal;font-family:'Times New Roman';" >                <span 
style=";font-family:Verdana;font-size:100%;color:blue;"   >byte<span 
style=";font-family:Verdana;font-size:100%;"  >  [] buffer = (<span 
style="color:blue;">byte[])(<span 
class="SpellE">dsFile.Tables[0].Rows[0]["FILE_CONTENT"]);<o:p></o:p></p> <p 
class="MsoNormal" style="margin-left: 0.5in; text-indent: -0.5in;"><span 
style=";font-family:'Courier New';font-size:100%;"  ><span style="">7.<span 
style="font-style: normal; font-variant: normal; font-weight: normal; 
line-height: normal; font-size-adjust: none; font-stretch: 
normal;font-family:'Times New Roman';" >                <span class="SpellE"  
style="font-size:100%;"><span 
style="font-family:Verdana;">HttpContext.Current.Response.BinaryWrite<span 
style=";font-family:Verdana;font-size:100%;"  >(buffer);<o:p></o:p></p> <p 
class="MsoNormal" style="margin-left: 0.5in; text-indent: -0.5in;"><span 
style=";font-family:'Courier New';font-size:100%;"  ><span style="">8.<span 
style="font-style: normal; font-variant: normal; font-weight: normal; 
line-height: normal; font-size-adjust: none; font-stretch: 
normal;font-family:'Times New Roman';" >                <span 
style=";font-family:Verdana;font-size:100%;"  >//  End the 
response.<o:p></o:p></p> <p class="MsoNormal" style="margin-left: 0.5in; 
text-indent: -0.5in;"><span style=";font-family:'Courier New';font-size:100%;" 
 ><span style="">9.<span style="font-style: normal; font-variant: normal; 
font-weight: normal; line-height: normal; font-size-adjust: none; 
font-stretch: normal;font-family:'Times New Roman';" >                <span 
class="SpellE"  style="font-size:100%;"><span 
style="font-family:Verdana;">HttpContext.Current.Response.End<span 
style=";font-family:Verdana;font-size:100%;"  >();<span 
style=";font-family:Verdana;font-size:100%;"  ><o:p></o:p></p>   <p 
class="MsoNormal" style="margin-left: 0.5in;"><span 
style=";font-family:Verdana;font-size:100%;"  ><o:p>  </o:p> 
Line 1 clears all content output  from the buffer stream.<o:p></o:p></p>     
<p class="MsoNormal" style="margin-left: 0.5in;"><span 
style=";font-family:Verdana;font-size:100%;"  ><o:p></o:p>Line 2 sets the HTTP 
MIME type for  the output stream. The default value is "**text/html**". As we 
want to store  it as a file to the client system we specify it as 
“**file**”. 
<o:p></o:p>The <span class="SpellE"><span class="GramE">**AddHeader****(****) 
**method in Line 3 is used to add an  HTTP header to the output stream. It 
accepts two parameters. First parameter is  the name of the HTTP header to add 
value to and second is the value itself. In  our case the parameter name is 
“**content-disposition**” and value is sent  as  **"****attachment****"** 
along with the file  name. This is what that causes the opening of save file 
dialog. If we provide  value of content-disposition as “inline” instead of 
save file dialog the file  will be opened in the associated application.<o:p> 
</o:p></p>  <p class="MsoNormal" style="margin-left: 0.5in;"><span 
style=";font-family:Verdana;font-size:100%;"  >In the Line 7 we are actually  
writing the contents of the file, as binary characters, to the HTTP output  
stream. Method <span class="SpellE"><span class="GramE">**BinaryWrite****()** 
is used for this purpose. This method  takes a parameter buffer which is a 
byte array containing bytes to be written to  the HTTP output stream. In our 
case the content of this buffer are fetched from  the database into a dataset 
<span class="SpellE">**dsFile** with column “**FILE_CONTENT**” 
corresponding to the  bytes to be written.<o:p> 
</o:p></p>   <p class="MsoNormal" style="margin-left: 0.5in;"><span 
style=";font-family:Verdana;font-size:100%;"  >The **End()** method in Line 9, 
sends all  currently buffered output to the client, stops execution of the 
page, and raises  the <span class="SpellE">**Application_EndRequest** event. 
This is when  we see a message asking us to save or open the file, displaying 
the information  about the file, like file type, file name.<o:p></o:p></p><p 
class="MsoNormal" style="margin-left: 0.5in;"><span 
style=";font-family:Verdana;font-size:100%;"  ><o:p></o:p>On clicking save, 
the save file  dialog is opened where we can save the file to our system with 
the name that we  have provided in the line 3 or we can provide a new name  
also.<o:p></o:p></p><span> 