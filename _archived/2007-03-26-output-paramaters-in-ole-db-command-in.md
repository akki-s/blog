---
layout: post
title: Output Paramaters in OLE DB Command in SSIS
date: '2007-03-26T21:53:00.000+05:30'
author: aks
tags:
- SQL Server
modified_time: '2007-03-29T23:55:54.489+05:30'
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-3833375770898623270
blogger_orig_url: http://techyfreak.blogspot.com/2007/03/output-paramaters-in-ole-db-command-in.html
---

Today when i was nearly at the verge of finishing my SSIS package i just found 
out that i really can not complete it at the moment. I am having lot of 
validations on the input data, lot of look ups to get the reference values and 
after that i need to insert the rows into a <span class="fullpost">SQL server 
database. It is not simple insertion, there is a big logic behind inserting 
rows, checking if the rows containing individual numbers form a sequence so 
that those can be inserted into a range rather than individual entries. Not 
only that i need to check if that number is already in the table in some 
range, if yes i need to discard it, redirect that row to an error output file. 

So ultimately, i have to use an OLE DB command component, write my SQL in it 
to call the stored procedure handling the logic to insert in the database. I 
was able to map the input columns of the stored procedure with my source input 
columns without any difficulty. But the problem was how to know which row 
already existed and how to redirect that row to error output with proper 
custom message. Well i just did it by hit and trial by having two output 
parameters in my stored procedure, added two columns to my input and mapped 
them with the out parameters in the OLE DB command. Then the task was simple, 
just to have a conditional split after the OLE DB command, check if indicator 
(one of the OUT parameters) is TRUE, suggesting insertion failed and then 
copying those rows to an error file that i am maintaining with the other out 
parameter @Message. 

If you want to log general SQL errors that may occur during the execution of 
the OLE DB command component you can simply configure its error outputs and 
log the ErrorMessage. 

However i am still not able to figure out how can we use the return parameters 
in a stored procedure to map them using an OLE DB command. 