# Features

## Job Execution

### Parameterization

Jobs can receive parameters per default in the job definition or dynamically from the trigger itself.

### Forked Execution

Each job is executed in a separate process through JobRunner, guaranteeing a process-level isolation.

### Progress Reporting

Every job can inform the job server of its current status through a progress reporting feature.

### Output Collection

In case a job produces any kind of output, the data can be attached to a job run, as to keep the information on the execution and the output together.

## Management

### Trigger Types

Three types of triggers are provided:

- Instant: A one-time instantaneously executed trigger
- Scheduled: A one-time execution that is scheduled to trigger at a specific date and time
- Recurring: A recurring execution triggering at a specific interval defined as cron expression

### REST API

The Jobbr.Server.WebAPI provides a clear interface to communicate with the JobServer in any form.

### Fully Typed Client

There is also a static typed client available which you can use to interact with any Jobbr REST API, without having to manually make your own HTTP calls.

## Persistence

### Filesystem Store

For any job artifacts you want to store, you can use a filesystem store to have them written to server's disk.

### MSSQL JobStore

By default Jobbr operates with an In-Memory storage model that isn't persist anywhere. For any productive system, it's however recommended to use the MSSQL JobStore to ensure data is retained across shutdown and reboots.

## Extendability

We think it's important to define clear responsibilities for such an important thing like a JobServer. By having this in mind, we were able to cut the core components into different extensions so that you can choose which features you need and which complexity you need to add. Furthermore this makes the development more stable and predictable. You will notice that by less frequent version updates for the server itself, but evolving feature components.
