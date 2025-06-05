---
layout: post
title: How to Test Azure Logic Apps
date: '2018-02-18T08:48:00.000+10:00'
comments: true
author: aakash
categories: [Azure]
tags: [Azure, Azure Logic Apps, Testing]
date: '2018-02-18T08:48:00.000+10:00'
---

Azure Logic Apps are a great solution to write workflows of the business processes, and that too without writing any code. As a result, unit tests can not be written because there's no code to write unit tests against. How do one go around testing a logic app then?

If you've been developing logic apps on Azure Portal, you should've noticed code view of your logic app. The code view is nothing but an underlying JSON which is used by designer to display the workflow visually. This JSON is in Azure Resource Managee (ARM) template. First thing, I'll suggest is to stop using Azure Portal for Logic Apps development, and instead use Visual Studio. 

You can install [Visual Studio Tools for Azure Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-deploy-from-vs), and start developing from within visual studio in the same way as you do on portal. The process involves creating a new project using the project template "Azure Resource Group", and selecting Logic App as the template. This results in a JSON file, which you can either edit as text, or with Logic App Designer. The way it helps is that you can deploy logic app in the same way you would deploy any other resource via ARM template. 

Coming back to testing your Logic App. As currently there's no way we can run Azure Logic App locally as compared to an Azure Function, we need to deploy a logic app to dev/test environment, and then run end to end tests. This can be done as part of the build script that gets trigerred on your build server such as VSTS, Jenkins etc. This will be triggered wheneve you commit changes made to your Logic App's JSON in to your repository. As part of the build script, end to end tests can be run using Cucumber, Specflow or Postman. 

However, you must be careful that your tests don't hit the real workflows, so you need to figure out which actions to mock in your logic apps. You can maintain different ARM template parameter files for dev, test and production environments. For example, dev/test version of parameter files will point to dev/test API connections, dev/test Azure functions, dev/test email addresses etc.
