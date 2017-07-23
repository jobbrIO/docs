# Restful API
The WebApi extension from [jobbrIO/jobbr-webapi](https://github.com/jobbrIO/jobbr-webapi) adds a Rest-style management interface to jobbr. 

## Installation

Install the NuGet `Jobbr.Server.WebAPI` to the project where you host you Jobbr-Server. The extension already comes with a small webserver based on OWIN/Katana. The referenced HttpListenr will be installed by NuGet automatically.

	Install-Package Jobbr.Server.WebAPI


### Registration
The Library comes with an extension method for the `JobbrBuilder`. To add the Web API to a Jobbr-Server you need to register it prior start as you see below. Please note that this is not ASP.NET WebAPI when registering it to an OWIN Pipeline, allthough we're using the same principle. (In fact, we're using WebAPI internally)

```c#
using Jobbr.Server.WebAPI;

// ....

// Create a new builder which helps to setup your JobbrServer
var builder = new JobbrBuilder();

// Register the extension
builder.AddWebApi();

// Create a new instance of the JobbrServer
var server = builder.Create();

// Start
server.Start();
```

### Configuration
If you don't specify any value for `BackendAddress` the server will try to find a free port automatically and binds to all available interfaces. The endpoint is logged and usually shown in the console, but this approach is not suggested in production scenarios, see below:

	[WARN]  (Jobbr.Server.WebAPI.Core.WebHost) There was no BackendAdress specified. Falling back to random port, which is not guaranteed to work in production scenarios
	....
	[INFO]  (Jobbr.Server.JobbrServer) The configuration was validated and seems ok. Final configuration below:
	JobbrWebApiConfiguration = [BackendAddress: "http://localhost:1903"]

You can override this behavior, by explicitly providing your own URI prefix, for instance `http://localhost:8765/api`. See example below:

```c#
builder.AddWebApi(config => 
{
	config.BaseUrl = "http://localhost:8765/api";
});
```
**Note**: Please refer to [https://msdn.microsoft.com/en-us/library/system.net.httplistener(v=vs.110).aspx](https://msdn.microsoft.com/en-us/library/system.net.httplistener(v=vs.110).aspx) for the supported URI prefixes depending on your operating system and .NET Runtime version.

#### Configuration Overview
Additional configuration is not available at time of writing.

## Rest API Reference

### /status Endpoint
The status endpoint is for testing purposes only. Even if does not perform health checking it can be used to determine the HTTP bindings and general availability of the Http-Endpoint.

    GET http://localhost:8765/api/status

Does usually just return:

    HTTP/1.1 200 OK

    "Fine"

### /configuration Endpoint
If you need to check the current configuration, use the `/configuration` endpoint.

    GET http://localhost:8765/api/status

Which will return a object that represents the current Web-API configuration.

    {
        "backendAddress": "http://localhost:8765/api"
    }

### /fail Endpoint
If you are curious if logging is setup correctly or if any reverse proxies are interfering with HTTP500 error codes, you can use this endpoint

    GET /fail

Will raise an unhandled exception and usually shows an error response similar to 

    HTTP/1.1 500 Internal Server Error

    {
    "message": "An error has occurred.",
    "exceptionMessage": "This has failed!",
    "exceptionType": "System.Exception",
    "stackTrace": "   at Jobbr.Server.WebAPI.Core.Controller.DefaultController.Fail() in C:\\projects\\jobbr-webapi\\source\\Jobbr.Server.WebAPI\\Core\\Controller\\DefaultController.cs:line 42\r\n   at lambda_method(Closure , Object , Object[] )\r\n   at 
    ...
    }

## Static Typed C#-Client
There is also a static typed client available which you can use to interact with any Jobbr Rest Api. Install the client by using the following commands

	Install-Package Jobbr.Client

After installation, you might provide the base url where the api can be found. See example below

```c#
using Jobbr.Client;
using Jobbr.WebAPI.Common.Models;

// ...

var jobbrClient = new JobbrClient("http://localhost:8765/api");

var allJobs = jobbrClient.GetAllJobs();

```

## Limitations

### Authentication
Currently there is no authentication middleware available and the API should therefore not be exposed to the public. However this feature is planned for future