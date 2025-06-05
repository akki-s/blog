---
layout: post
title: Create a dotnet core Azure Function
date: '2018-01-07T08:48:00.000+10:00'
comments: true
author: aakash
categories: [Azure]
tags: [Azure, Azure Functions, .Net Core]
date: '2018-01-07T08:48:00.000+10:00'
---

Latest version of Visual Studio Extension [Azure Functions and Web tools](https://marketplace.visualstudio.com/items?itemName=VisualStudioWebandAzureTools.AzureFunctionsandWebJobsTools) helps you create pre-compiled C# Azure Functions, and lets you write functions as a C# library. Being a pre-compiled function it has better performance while starting up. Along with that, it lets you use Webjobs attributes for declaring bindings, thus not needing a functions.json file. And, it lets you debug and run azure functions locally.

Moreover, being a C# library, it makes it possible to write unit tests, and use code analysis for Azure Function. In this post we'll limit ourselves to creating a dotnet core Azure Function, and then later posts will cover unit testing an Azure Function, and how to do CI/CD for Azure Functions.

{::comment}
## Intended use case

For the purpose of this exercise, we will be creating an Azure Function for a very simple scenario. Reason for having a simple scenario? Well, to keep things simple.

Our intended Azure Function will be responsible for following:

{% highlight gherkin %}
Scenario: Validate a driving license number
When driving license number is valid
Then return true

When driving license number is not valid
Then return false
{% endhighlight %}

For this solution we'll validate driving license number just using a Regular Expression. In real world you'll be calling an ID verification API.
{:/comment}

## Creating Azure Function in Visual Studio

- Make sure you've installed latest version of Visual Studio 2017 (atleast 15.5.3), and Azure workload is already installed. This would've installed base version of the Azure Functions extension. Now download and update the extension [Azure Functions and Web tools](https://marketplace.visualstudio.com/items?itemName=VisualStudioWebandAzureTools.AzureFunctionsandWebJobsTools).
- Above template lets you create Azure Function v2 version which is in dotnet core. You must understand that currently Azure Functions v2 is in preview, and this is not fit for production.
- Create a new project in Visual Studio and select Azure Functions template. 
- Ignore the first screen for creating Azure Function as it still shows .Net Framework versions. Don't get confused with that, and be patient, and go to the next step.

	![azure functions and webjobs tools template]({{ site.baseurl }}/assets/images/posts/af/azure-functions-dotnet-core-01.PNG)

- At the next step you can select to use Azure Functions v2. The new template lets you select a storage account and access rights. You can select any trigger type based on your need. You can even select empty, and then add a new function to your project later.
	
	![azure functions dotnet core]({{ site.baseurl }}/assets/images/posts/af/azure-functions-dotnet-core-02.PNG)

- I chose Empty function, so as to demonstrate how to add a new function after creating project. Project will be created now, and if you see it's properties you'll see its target framework as .NET Standard 2.0.

	![azure functions dotnet core]({{ site.baseurl }}/assets/images/posts/af/azure-functions-dotnet-core-03.PNG)

- Right click on your project, and select Add -> New Azure Function. Give your function a name, and click Add.

	![azure functions dotnet core]({{ site.baseurl }}/assets/images/posts/af/azure-functions-dotnet-core-04.PNG)

- At next step, I need a Http Trigger, function, so I selected that, and hit Ok.
- At this point of time, you'll have a fully functional Azure function. Notice that there's no function.json file. This will be generated when the function is built. Note that function name is specified with `FunctionName` attribute.
- The default function created here is a fully functional one. Hit F5 in visual studio, and it will deploy it locally and run it. You can even set a breakpoint to help with debugging.
- When you run the project Azure Function runtime will show the endpoints of all the function in your project. For example, in this case, there's only one function

	![azure functions dotnet core]({{ site.baseurl }}/assets/images/posts/af/azure-functions-dotnet-core-05.PNG)

- The default function created here, supports both GET and POST, and expects `name` property. You can either test it from browser by adding a querystring called `name` such as http://localhost:7071/api/DrivingLicenseValidator?name=aakash, or using POSTMan.
- If you go to your bin\debug folder, you'll notice that there's a folder created for your Azure function, which indludes a function.json file. This would be done for each Azure Function in your project. When your function project id built, a subfolder would be created for each function with function.json file. This function.json file has information related to bindings, points to the compiled dll to be used in the `scriptFile` property, and `entryPoint` which is different for each function that points to where the Run method is located.


	{% highlight javascript %}
	{
	  "generatedBy": "Microsoft.NET.Sdk.Functions.Generator-1.0.6",
	  "configurationSource": "attributes",
	  "bindings": [
	    {
	      "type": "httpTrigger",
	      "methods": [
	        "get",
	        "post"
	      ],
	      "authLevel": "function",
	      "name": "req"
	    }
	  ],
	  "disabled": false,
	  "scriptFile": "../bin/Demos.Functions.dll",
	  "entryPoint": "Demos.Functions.DrivingLicenseValidator.Run"
	}
	{% endhighlight %}

