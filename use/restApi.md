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

### Plugin Configuration
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
**Note**: Please refer to the [MSDN Documentation for HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener(v=vs.110).aspx#Anchor_6) for the supported URI prefixes depending on your operating system and .NET Runtime version.

## Rest API Reference

### /jobs Endpoint
You could either configure your Jobs by the RepositoryBuilder in C# code or create them by using the APi, or both. The followig APIs are made for managing Jobs

#### Get all Jobs
The following endpoint retuns all registered jobs as an array:

    GET http://localhost:8765/api/jobs HTTP/1.1


##### Successful Response

Is indicated by 

    HTTP/1.1 200 OK
    Content-Type: application/json

and the followig example payload

```json
[
    {
        "id": 1,
        "uniqueName": "ProgressJob",
        "type": "Demo.MyJobs.ProgressJob",
        "parameters": { 
            "param1" : "test", 
            "param2" : "anothervalue" 
        },
        "createdDateTimeUtc": "2015-03-04T17:40:00",
        "createdDateTimeUtc": "2017-07-30T13:40:00"
    }
]
```

#### Single Job details
Details about a single job (including triggers) can be retreived by accessing the detail route of a job

    GET http://localhost:8765/api/jobs/1 HTTP/1.1


##### Sucessful Response

If the job is found, the response is indicated by

    HTTP/1.1 200 OK
    Content-Type: application/json

and the following example data is returned. Note the addition of **Triggers**

```json
[
    {
        "id": 1,
        "uniqueName": "ProgressJob",
        "type": "Demo.MyJobs.ProgressJob",
        "parameters": { 
            "param1" : "test", 
            "param2" : "anothervalue" 
        },
        "createdDateTimeUtc": "2015-03-04T17:40:00",
        "createdDateTimeUtc": "2017-07-30T13:40:00",
        "trigger": [
            {
                "triggerType": "Recurring",
                "id": 1,
                "isActive": true
            }    
        ]
    }
]        
```

##### Error Responses
If no job exists with the given id, an error will be returned

#### Add a new Job

Besides the RepositoryBuilder in C#, there's also a posibility to create jobs by the Rest Api.

**Parameters**
* `uniqueName`: Unique name if the job (**Required**)
* `type` : Name of the CLR Type that should be used (**Required**)
* `parameters`: Object that will be used as JobParameters

##### Sample Request

    POST http://localhost:8765/api/jobs HTTP/1.1
    Content-Type: application/json

```json
[
    {
        "id": 1,
        "uniqueName": "ProgressJob",
        "type": "Demo.MyJobs.ProgressJob",
        "parameters": { 
            "param1" : "jobParameter1", 
            "param2" : "anothervalue" 
        }
    },
]
```    

##### Successful Response

    201 Created 
    Location: /api/jobs/2
    Content-Type: application/json

and the payload

```json
{
    "uniqueName": "ProgressJob2",
    "type": "Demo.MyJobs.ProgressJob"
}    
```
##### Error Responses

If there is already a job with the same UniqueName, the error `409 CONFLICT` will be returned.

#### JobRuns for Job
If you wan't to get all runs for a specifc job, you can use the subresource `runs` on every job. A job **can be identified** by its internal `id` or the user specific `uniqueName`.

**Url Parameters**
* `id` : Internal Id of the Job (**Required**)
* `uniqueName`: Unique name if the job (**Required**)


    GET http://localhost:8765/api/jobs/1/runs

    GET http://localhost:8765/api/jobs/progressJob/runs

##### Successful Response

which will be replied with

```json
[
    {
        "jobId": 1,
        "triggerId": 1,
        "jobRunId": 1,
        "state": "Completed",
        "progress": 100,
        "plannedStartUtc": "2017-07-30T10:53:00Z",
        "auctualStartUtc": "2017-07-30T10:53:00.5132852Z",
        "auctualEndUtc": "2017-07-30T10:53:19.335589Z"
    },
]
```

*Note:*: Only single entry shown.

### /trigger Endpoint
Triggers are used to either add future or instant triggers to a job. The trigger is causig a job to run.

### /jobruns Endpoint


### Generic Endpoints
#### /status
The status endpoint is for testing purposes only. Even if does not perform health checking it can be used to determine the HTTP bindings and general availability of the Http-Endpoint.

    GET http://localhost:8765/api/status HTTP/1.1

Does usually just return:

    HTTP/1.1 200 OK

    "Fine"

#### /configuration
If you need to check the current configuration, use the `/configuration` endpoint.

    GET http://localhost:8765/api/status

Which will return a object that represents the current Web-API configuration.

    HTTP/1.1 200 OK 

    {
        "backendAddress": "http://localhost:8765/api"
    }

#### /fail
If you are curious if logging is setup correctly or if any reverse proxies are interfering with HTTP500 error codes, you can use this endpoint

    GET /fail HTTP/1.1

Will raise an unhandled exception and usually shows an error response similar to 

    HTTP/1.1 500 Internal Server Error

    {
    "message": "An error has occurred.",
    "exceptionMessage": "This has failed!",
    "exceptionType": "System.Exception",
    "stackTrace": "   at Jobbr.Server.WebAPI.Core.Controller.DefaultController.Fail() in DefaultController.cs:line 42
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