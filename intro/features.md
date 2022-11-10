# Features

## Job Execution

### Parameterization

Both Job & Trigger (JobRun)

### Forked Execution

Isolation of Jobs on process-level (JobRunner)

### Progress Reporting

### Output Collection

### DI for Jobs

Optional

### Logging


## Management

### Trigger Types

### Non-Invasive

### Rest API

### Fully Typed Client


## Persistence

### RavenFS Store

### Filesystem Store

### MSSQL JobStore

### RavenDB JobStore


## Extendability

We think it's important to define clear responsibilities for such an important thing like a JobServer. By having this in mind, we were able to cut the core components into different extensions so that you can choose which features you need and which complexity you need to add. Furthermore this makes the development more stable and preditcable. You will notice that by less frequent version updates for the server itself, but evolving feature componenents.