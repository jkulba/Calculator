# Calculator

### Calculator is a Azure Serverless Function

[Azure Functions documentation](https://docs.microsoft.com/en-us/azure/azure-functions/)

**Software System Requirements**

1. As a developer, I want to create a new HTTP serverless function and test my new function locally, so that it is ready to be deployed to the Azure Cloud.
1. As a developer, I want to add dependency injection to my function, so that I can utilize services or 3rd party libraries.
1. As a DevOps Engineer, I want to package and deploy my serverless function to the Azure cloud, so that I allow my team to perform integration tests.

## Pre-requisites

&nbsp;&nbsp;&nbsp;&nbsp;Azure Subscription

&nbsp;&nbsp;&nbsp;&nbsp;.Net 6 SDK

&nbsp;&nbsp;&nbsp;&nbsp;Visual Studio Code

&nbsp;&nbsp;&nbsp;&nbsp;Azure Function Core Tools 4x -
[Azure Functions Core Tools](https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=v4%2Clinux%2Ccsharp%2Cportal%2Cbash#v2)

&nbsp;&nbsp;&nbsp;&nbsp;Azure CLI -
[Azure Command-line Interface (CLI)](https://docs.microsoft.com/en-us/cli/azure/)

## Steps to create the Azure Function Project and function types

1. Create new project. I created a new project in GitHub named Calculator. I cloned my new Calculator project to my local desktop.
1. Navigate into the new Calculator project directory .
1. **Optional**: Initialize git flow. From the Calculator directory, I initialized Git Flow using the following command:

```shell
git flow init
```

4. Generate new Azure Function App. I used the Azure Function Core CLI to generate the new function app.

```shell
func init Kulba.Function.Calculator --dotnet
```

5. Create a new HTTP function type. Navigate into the new directory Kulba.Function.Calculator that was created in the previous step. Here is another example of the Azure Function Core CLI command to generate the HTTP function. The generator uses the name value to create the new class and to name the function. The template value describes the name of the function. And the authlevel describes the execution security scope.

```shell
func new --name SubmitItemHttpFunction --template "HTTP trigger" --authlevel "function"
```

6. Add Serilog packages to project.

```shell
dotnet add package Microsoft.Azure.Functions.Extensions
dotnet add package Serilog
dotnet add package Serilog.Extensions.Logging
dotnet add package Serilog.Sinks.Console
dotnet add package Serilog.Sinks.File
```

7. Initialize Serilog in the Program.cs

```c#
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Serilog;

var host = new HostBuilder()
    .ConfigureFunctionsWorkerDefaults()

    .ConfigureServices(services =>
    {
        var logger = new LoggerConfiguration()
        .WriteTo.Console()
        .WriteTo.File("log.txt", rollingInterval: RollingInterval.Day)
        .CreateLogger();
        services.AddLogging(lb => lb.AddSerilog(logger));
    })
    .Build();

host.Run();
```

## Additional Azure Function Resources

[Offical Microsoft Azure Functions](https://azure.microsoft.com/en-us/services/functions/#overview)

### DotNet SDK CLI Templates

1. Visual Studio 2022 has the Azure Functions project templates already installed.
1. To install the additional Azure Function templates. This gives you the developer the awesome power to create new function apps and function types from the DotNet SDK CLI. This is important if you want to use Visual Studio Code.

### Function Project Templates

```shell
dotnet new --install Microsoft.Azure.Functions.Worker.ProjectTemplates
```
