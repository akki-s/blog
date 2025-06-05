---
layout: post
title: Azure Event Grid events across Azure subscriptions
date: '2018-08-11T22:18:30.000+10:00'
comments: true
author: aakash
categories: [Azure]
tags: [Azure, Azure Functions, Azure Event Grid]
---

Consider a scenario where you need to listen to Azure resource events happening in one Azure subscription from another Azure subscription. A use case for such a scenario can be when you are developing a solution where you listen to events happening in your customers' Azure subscriptions, and then you need to handle those events from an Azure Function or Logic App running in your subscription. A solution for such a scenario could be: 
1. Create an Azure Function in your subscription that will handle Azure resource events received from Azure Event Grid. 
2. Handle event validation in the above function, which is required to perform a handshake with Event Grid.
3. Create an Azure Event Grid subscription in the customers' Azure subscriptions. 

Before, I go into details let's have a brief overview of Azure Event Grid. Azure Event Grid is a routing service based on a publish/subscribe model, which is used for developing event-based applications. Event sources publish events, and event handlers can subscribe to these events via Event Grid subscriptions.

![event-grids]({{ site.baseurl }}/assets/images/posts/2018/event-grids.png) 
Figure 1. Azure event grid publishers and handlers

Azure Event Grid subscriptions can be used to subscribe to system topics as well as custom topics. Various Azure services automatically send events to Event Grid. The system-level event sources that currently send events to Event Grid are Azure subscriptions, resource groups, Event Hubs, IOT Hubs, Azure Media Services, Service Bus, and blob storage.

You can listen to these events by creating an event handler. Azure Event Grid supports several Azure Services and custom webhooks for event handlers. There are number of Azure services that can be used as event handlers, including Azure Functions, Logic Apps, Event Hubs, Azure Automation, Hybrid Connections, and storage queues.

In this post I'll focus on using Azure Functions as an event handler to which an Event Grid subscription will send events to whenever an event occurs at the whole Azure subscription level. You can also create an Event Grid subscription at a resource group level to be notified only for the resources belonging to a particular resource group. The figure 1 posted above, shows various event sources that can publish events, and various supported event handlers. As per our solution Azure subscriptions and Azure Functions are marked.

## Create an Azure Function in your subscription and handle the validation event from Event Grid

If our Event Grid subscription and function were in the same subscription, then we could have simply created an Event Grid-triggered Azure Function. Using that you can simply specify the Event Grid subscription details with this function specified as an endpoint in the Event Grid subscription. However, in our case this cannot be done as we need to have the Event Grid subscription in the customer subscription, and the Azure Function in our subscription. Therefore, we will simply create a HTTP-triggered function or a webhook function.

Because we’re not selecting an Event Grid triggered function, we need us to do an extra validation step. At the time of creating a new Azure Event Grid subscription, Event Grid requires the endpoint to prove the ownership of the webhook, so that Event Grid can deliver the events to that endpoint. For built-in event handlers such as Logic Apps, Azure Automation, and Event Grid triggered functions, this process of validation is not necessary. However, in our scenario where we are using a HTTP-triggered function we need to handle the validation handshake When an Event Grid subscription is created, it sends a subscription validation event in a POST request to the endpoint. All we need to do is to handle this event, read the request body, read the `validationCode` property in the data object in the request, and send it back in the response. Once Event Grid receives the same validation code back it knows that endpoint is validated, and it will start delivering events to our function. Following is an example of a POST request that Event Grid sends to the endpoint for validation. 

```json
[{

  "id": "512c81af-3a4c-c7b8-bd0c-f43d91da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "2d1788b6-4d7c-40c8-89fe-f57e9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-08-55T12:32:29.5458618Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Our function can check if the `eventType` is `Microsoft.EventGrid.SubscriptionValidationEvent` , which indicates it is meant for validation, and send back the value in `data.validationCode`. In all other scenarios, eventType will be based on the resource on which the event occurred, and the function can process those events accordingly. Also, the resource validation event contains a header `aeg-event-type` with value `SubscriptionValidation`. You should also validate this header. Following is the sample code for a Node.js function to handle the validation event and send back the validation code and hence completing the validation handshake. 

```javascript
module.exports = function (context, req) {
    for (var events in req.body) {
        var body = req.body[events];        

        if (body.data && body.eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
            context.log("event grid validation event, validation code: " + body.data.validationCode);

            //validate header
            if(req.headers['aeg-event-type'] && req.headers['aeg-event-type'] == 'SubscriptionValidation')
            {            
                // Do any additional validation (as required) and then return back the below response
                var code = body.data.validationCode;
                context.res = { status: 200, body: { "ValidationResponse": code } };
            }
            else{
                context.res = { status: 400, body: "Validation header is missing." };
            }
        }
        else{
            //for all other events, do the processing you need to do, such as call another azure function
            // or a logic app, send alerts, or emails etc.        

            // create a response body, with the properties you're interested in, or just send the event in response.
            let responseBody = {                 
                    "eventType": body.eventType,
                    "operationName": body.data.operationName,
                    "resourceProvider": body.data.resourceProvider,
                    "resourceUri": body.data.resourceUri,
                    "status": body.data.status,  
                    "subject": body.subject
                    "subscriptionId": body.data.subscriptionId,  
                    "tenantId": body.data.tenantId,
                };
           
            context.log("Resoonse body", responseBody);
            context.res = { status: 200, body: responseBody };
        }
    }
    context.done();
};
```

## Processing Resource Events

To process the resource events, you can filter them on the `resourceProvider` or `operationName` properties. For example, the operationName property for a VM create event is set to `Microsoft.Compute/virtualMachines/write`. The event payload follows a fixed schema as described [here](https://docs.microsoft.com/en-us/azure/event-grid/event-schema-subscriptions). An event for a virtual machine creation looks like below:

```json
{
  "subject": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/resource-group/providers/Microsoft.Compute/virtualMachines/vmName",
  "eventType": "Microsoft.Resources.ResourceWriteSuccess",
  "eventTime": "2018-08-05T14:48:32.945Z",
  "id": "0605b293-e06e-48ac-a16d-9322fe8e24dd",
  "data":
   { 
      "authorization":
      { 
        "scope": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/resource-group/providers/Microsoft.Compute/virtualMachines/vmName",
        "action": "Microsoft.Compute/virtualMachines/write",
        "evidence": []
      },
      "claims": {},
      "correlationId": "8a2da39d-ef93-42dd-8ce6-97ac802ecc6d",
      "resourceProvider": "Microsoft.Compute",
      "resourceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/resource-group/providers/Microsoft.Compute/virtualMachines/vmName",
      "operationName": "Microsoft.Compute/virtualMachines/write",
      "status": "Succeeded",
      "subscriptionId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" 
    },
    "dataVersion": "2",
    "metadataVersion": "1",
    "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

### Authentication

While creating the Event Grid subscription, detailed in next section, it should be created with the endpoint URL pointing to function URL including the function key.. Also, event validation done for the handshake acts as another means of authentication. To add an extra layer of authentication, you can generate your own access token, and append it to your function URL when specifying the endpoint for the Event Grid subscription. Your function can now also validate this access token before further processing.

## Create an Azure Event Grid Subscription in customer’s subscription

A subscription owner/administrator should be able to run an Azure CLI or PowerShell command for creating the Event Grid subscription in customer subscription. 
**Important**: This step must be done after the above step of creating the Azure Function is done. Otherwise, when you try to create an Event Grid subscription, and it raises the subscription validation event, Event Grid will not get a valid response back, and the creation of the Event Grid subscription will fail. You can add filters to your Event Grid subscription to filter the events by subject. Currently, events can only be filtered with text comparison of the subject property value starting with or ending with some text. The subject filter doesn’t support a wildcard or regex search.

### Azure CLI or PowerShell

An example Azure CLI command to create an Event Grid Subscription, which receives all the events occurring at subscription level is as below: 
```cmd
az eventgrid event-subscription create --name eg-subscription-test --endpoint https://myhttptriggerfunction.azurewebsites.net/api/f1?code=
```

Here `https://myhttptriggerfunction.azurewebsites.net/api/f1?code=` is the URL of the function app.

### Azure REST API

Instead of asking customer to run a CLI or PowerShell script to create the Event Grid subscription, you can automate this process by writing another Azure Function that calls Azure REST API. The API call can be invoked using service principal with rights on the customer’s subscription. To create an Event Grid subscription for the customer’s Azure Subscription, you submit the following PUT request: 

**Request**
```
PUT https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx /providers/Microsoft.EventGrid/eventSubscriptions/ eg-subscription-test?api-version=2018-01-01` 
```
**Request Body:**
```json
{ 
    "properties": 
    { 
        "destination": 
        { 
            "endpointType": "WebHook", 
            "properties": 
            { 
                "endpointUrl": " https://myhttptriggerfunction.azurewebsites.net/api/f1?code=" 
            } 
        }, 
        "filter": { "isSubjectCaseSensitive": false } 
    } 
}
```
