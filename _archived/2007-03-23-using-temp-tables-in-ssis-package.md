---
layout: post
title: Using Temp Tables in SSIS Package Development
date: '2007-03-23T12:11:00.001+05:30'
author: aks
tags:
- SQL Server
modified_time: '2008-04-18T03:50:15.078+05:30'
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-7085827903316712531
blogger_orig_url: http://techyfreak.blogspot.com/2007/03/using-temp-tables-in-ssis-package.html
---

Often while working in a **SSIS package** you will require to temporary hold 
your data in a staging table in one of the Data Flow Tasks and then in another 
task you will require to fetch data from the staging table, perform 
transformations and load it and delete the staging table. 

It means you create a physical table in your production database to stage 
data. But in a production environment, you may not want to create and destroy 
objects in the production database and might prefer to use temp tables 
instead. This seems easy, in fact it is, but it requires a trick and to modify 
the default properties of the components. Let us see what to do in this 
regard. 

<img src="http://img300.imageshack.us/img300/7987/temp1no0.jpg" /> 

In the figure you have <span class="fullpost">two Execute SQL tasks. The 
Create Temp Table task executes a SQL command to create a temporary table 
named *#tmpMyData*. The Drop Temp Table task executes a SQL command to drop 
table *#tmpMyData*. 

If you execute this package, you will notice that the drop portion of the 
package failed. The package progress tab will report the error message that 
the table doesn't exist. This is because both of these Execute SQL tasks do 
not share the same connection, rather they just share the same connection 
manager. Each task builds its own connection from the connection manager. So 
when the first task is finished, temp table is destroyed and the second task 
creates a new connection. 

To fix this in the regular property window of the OLE DB connection there is a 
property <span style="font-weight: bold; font-style: 
italic;">RetainSameConnection that is set to "FALSE" as a default. Changing it 
to "TRUE" is our trick and will solve the problem. 

<img src="http://img244.imageshack.us/img244/9019/tempce4.jpg" /> 

By changing this property to "TRUE," both Execute SQL tasks will share the 
same connection and both will be able to use the temp table. 

You can use this trick for performance in SSIS packages also in the case you 
are going to be performing a task requiring a connection within a loop. 
Otherwise, imagine how many openings and closings are going to occur during 
that loop. 