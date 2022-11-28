---
layout: ../../layouts/BlogPost.astro
title: Application Insights and OpenTelemetry
description: "High level look at application insgihts vs OpenTelemetry"
author: "Cam Beatty"
pubDate: 'September 07 2022'  
heroImage: "/stock/header-1.jpg"
tags:
- Logging
- Azure
- Telemetry
- .NET
---
As someone who has worked in Azure, I have used Application Insights to provide logging, metrics, and analysis on my web applications. It has been an excellent tool for me and the teams I have been a part of. In the past couple of months, I have heard more information about OpenTelemetry. OpenTelemetry is slowly becoming the industry standard for telemetry, so it seemed appropriate to evaluate the combination of Application Insights and OpenTelemetry.

### Application Insights
For those of you that are new to Application Insights, I’ll give a brief synopsis. For more information, I suggest reviewing the Microsoft created [documentation](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview). Application Insights (AI) is part of Azure Monitor and provides application performance management for web applications. The best use case for App Insights is for web apps, but it also supports non-web-based apps as it has an ingestion point for logs. Application Insights is linked to a Log Analytics workspace which can query the ingested logs. The piece I find most helpful is live metrics. I use it to verify an app is running and pinpoint an exact error when reproduced. For web apps, AI provides additional powerful tools like performance telemetry and usage analytics.

### Adding App Insights to a .Net Web App
#### Prerequisites
- Azure Portal Account
- Application Insights resource

This web app is the default web app created by the `dotnet` template, with the addition of application insights. Create the app by either using your favorite C# IDE or running

```shell
dotnet new web --name <Name Of Project>
```

Add `Microsoft.ApplicationInsights.AspNetCore` NuGet package by using your IDE or running

```shell
dotnet add package Microsoft.ApplicationInsights.AspNetCore
```

Grab your connection string from your Application Insights resource, this can be done by going to the portal, finding the resource, and clicking the copy button in the top right corner with the connection string label.

![Application Insights Connection String](/app-insights-connection-string.png)

Add connection string to `appsettings.json` or environment variable named `APPLICATIONINSIGHTS_CONNECTION_STRING`
```json
{
  "ApplicationInsights": {
    "ConnectionString" : "Copy connection string from Application Insights Resource Overview"
  }
}
```
Add the bellow line the enable app insights. This will look at the `appsettings.json` for the section we added before.
```csharp
builder.Services.AddApplicationInsightsTelemetry();
```

### OpenTelemetry
[OpenTelemetry (OTel)](https://opentelemetry.io/) is an open-source observability framework. It can instrument, generate, collect, and export telemetry data. As mentioned previously, OTel is the industry standard for telemetry data. OpenTelemetry has gained that position in the industry by providing vendor-agnostic libraries for a large set of programming languages. They also provide a collector for all telemetry data; this is especially useful when your systems become increasingly more distributed. The signals that OpenTelemetry supports currently are [traces](https://opentelemetry.io/docs/concepts/signals/traces/), [metrics](https://opentelemetry.io/docs/concepts/signals/metrics/), [logs](https://opentelemetry.io/docs/concepts/signals/logs/), and [baggage](https://opentelemetry.io/docs/concepts/signals/baggage/). These can all help inform developers what is happening in their applications and deduce any issues.

### Adding OTel to a .Net Web App
This application is doing the exact same as the App Insights example, with the single difference being that OpenTelemetry is the one generating, emitting, and exporting the telemetry data. The telemetry data still exports to App Insights, as exploring observability back-ends is not the purpose of this post.

*One thing of note is the current [limitations](https://docs.microsoft.com/en-us/azure/azure-monitor/app/opentelemetry-enable?tabs=net#limitations-of-the-preview-release) of App Insights when combined with applications insights*

Create the app the same way

```shell
dotnet new web --name <Name Of Project>
```

This time, we need to add more packages, run these commands
```shell
dotnet add pacakge Azure.Monitor.OpenTelemetry.Exporter --prerelease
dotnet add package OpenTelemetry.Extensions.Hosting --prerelease
dotnet add package OpenTelemetry.Exporter.Console
dotnet add package OpenTelemetry.Instrumentation.AspNetCore --prerelease
dotnet add package OpenTelemetry.Instrumentation.Http --prerelease
dotnet add package OpenTelemetry.Instrumentation.SqlClient --prerelease #Optional
```

Next, we will add a section to the `appsettings.json
```json
{
  "ServiceInfo": {
    "Name": "OTel-Sample",
    "Version": "0.0.1"
  },
  "ApplicationInsights": {
    "ConnectionString": "Copy connection string from Application Insights Resource Overview"
  }
}
````

Lastly, let's build the Telemetry pipeline and export it to Application Insights
```csharp
builder.Services.AddOpenTelemetryTracing(tpb =>
{
    var serviceInfo = builder.Configuration.GetSection("ServiceInfo");
    var serviceName = serviceInfo.GetValue<string>("Name");
    var serviceVersion = serviceInfo.GetValue<string>("Version");
    tpb
        .AddConsoleExporter()
        .AddSource(serviceName)
        .SetResourceBuilder(
            ResourceBuilder.CreateDefault()
                .AddService(serviceName: serviceName, serviceVersion: serviceVersion))
        .AddHttpClientInstrumentation()
        .AddAspNetCoreInstrumentation()
        .AddAzureMonitorTraceExporter(o =>
        {
            var AiConnectionString =builder.Configuration.GetValue<string>("ApplicationInsights:ConnectionString");
            o.ConnectionString = AiConnectionString;
        });
});
```

### Conclusion
For this example, using OpenTelemetry doesn’t seem it provides any value over using App Insights. A few things that make me give the pass on this one
The current limitations of exporting the telemetry data to App Insights
A single web app
Increased setup time

OpenTelemetry is built for distributed systems and microservice architecture. These examples were for a single app by itself. If you develop apps in a microservice architecture, and accept the current limitations of App Insights or use a different observability back-end, OpenTelemetry is a good option to explore. I think Microsoft will decrease the limitations between App Insights and OpenTelemetry, but until that occurs, I will reach for App Insights as my default.
