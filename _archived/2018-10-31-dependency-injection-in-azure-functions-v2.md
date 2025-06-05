---
layout: post
title: Dependency Injection In Azure Functions V2
date: '2018-10-31T18:48:00.000+10:00'
comments: true
author: aakash
categories: [Azure]
tags: [Azure, Azure Functions]
---

With the Azure Functions v2 runtime, supporting .NET Core it has become easier to do dependency injection. It can be done in a similar way that ASP.NET Core does via `Microsoft.Extensions.DependencyInjection`. 

ASP.NET Core encourages the use of dependency injection by the built-in DI container. This feature of ASP.NET Core is very handy as many extensions such as logging and configuration via IOptions pattern are registered using during startup in `Startup.cs`. ASP.NET Core registers these services, along with any custom services you need using the built-in DI container via `IServiceCollection`.

This feature of ASP.NET Core can be utilized for dependency injection in Azure Functions V2 as it comes with .NET Core. We can create a container by creating an instance of `IServiceCollection` and then registering the services we need. 

Let's see this in action with an example. We'll create an Azure Function that returns a list of time zones. These time zones are maintained in a JSON file uploaded to Azure blob storage. Even though it is a very simple scenario, my solution consists of three different projects as below:

- **Function app project**. `Azure.Functions.v2.DI.FunctionApp`. This is the project that contains an HTTP-triggered Azure function, which returns the list of time zones via an HTTP GET.
- **Helper project**. `Azure.Functions.v2.DI.Helpers`. As the name suggests this contains helper class to read from Azure blob storage. The blob file containing a JSON array of time zones is stored in a file named `timezones.json` in a container `azfnv2demo`.
- **Services project**. `Azure.Functions.v2.DI.Services`. This project contains the `TimeZoneService` class that returns data to the function project by reading data from the Azure blob via the helper project.

Complete source code is available at: [https://github.com/akki-s/azure-functions-v2-dependency-injection](https://github.com/akki-s/azure-functions-v2-dependency-injection)

This means that the Function app project has a dependency on the services project, and services project has a dependency on the helper project. 

At this stage `TimeZoneService.cs` looks like below:

![TimeZoneService.cs]({{ site.baseurl }}/assets/images/posts/2018/azfnv2di/01.PNG)

As you would have noticed in the code above, a new object of `AzureBlobStorageHelper` is created which is then used to get the data from blob storage. However, this makes my `TimeZoneService.cs` code difficult to unit test. What we need is to inject an instance of `IAzureBlobStorageHelper`, when an instance of `TimeZoneService` is created.

The first thing we do is make a change in `TimeZoneService.cs` in Services project to remove the instantiation of `AzureBlobStorageHelper` and have it injected in the constructor. The constructor of `TimeZoneService` can also include other dependencies such as ILogger for logging, HttpClient for REST calls etc, but those are left out for the sake of this example.

The next step is to register the services and build the `ServiceProvider`. This is done by creating an instance of `IServiceCollection`, registering the services and then making a call to the `BuildServiceProvider` method on the ServiceCollection. This returns the ServiceProvider that has all the registrations and other configuration if we used it, such as adding logging. Below is the class that has a static method called `ConfigureServices` that does all this work. This is what ASP.NET Core does out of the box and I'm utilizing same here for Azure functions.

<script src="https://gist.github.com/akki-s/fb78c62b7fc6adc3fc8f23058c0dede4.js"></script>

Note that the above method registers `TimeZoneService` and `AzureBlobStorageHelper`.

Now all that's left to be done is to call `ConfigureServices` from within the Azure Function, to configure the service collection, and get an instance of TimeZoneService from the ServiceProvider (You may like to call the serviceProvider something else, such as `container` as it is in other DI frameworks like AutoFac).

<script src="https://gist.github.com/akki-s/d7d0f87869276b3c8463b0f9e89efc56.js"></script>

In order to use the generic method `GetService<T>` above we need to add a reference to `Microsoft.Extensions.DependencyInjection` in our functions project:

<script src="https://gist.github.com/akki-s/32ad397225d655e7835836c0da3d1dfa.js"></script>

`TimeZoneService.cs` now injects `IAzureBlobStorageHelper` in its constructor:

<script src="https://gist.github.com/akki-s/923728e1332cf510d13f53d8ad12ac74.js"></script>

This is it. This might not be the best solution for DI in Azure functions, but it certainly is very simple. 

**Note:** It is worth noting that, at the time of writing this post, Microsoft is working on integrating DI in Azure functions v2 as can be tracked [here](https://github.com/Azure/Azure-Functions/issues/299). But till then above approach works great.

Complete source code is available at [GitHub](https://github.com/akki-s/azure-functions-v2-dependency-injection)