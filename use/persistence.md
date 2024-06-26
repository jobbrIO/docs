# Persistence

There are two different categories of information that need to be stored

- **JobStorage**: Job Information, Triggers, and Runs
- **Artefacts**: Files created during Run by the job

:::{note}
You can combine different technologies to persist job related information (and triggers, runs, etc.) and Artifacts created by the jobruns.
:::

The following implementations provide adapters for one or two of these categories

| Technology   | JobStorage                                                                          | Artefact storage                                                                                          |
| ------------ | ----------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| MSSQL Server | Yes, with [Jobbr.Storage.MsSQL](https://github.com/jobbrIO/jobbr-storage-mssql)     | N / A                                                                                                     |
| FileSystem   | N/A                                                                                 | Yes, with [Jobbr.ArtefactStorage.FileSystem](https://github.com/jobbrIO/jobbr-artefactstorage-filesystem) |
| Raven        | Yes, with [Jobbr.Storage.RavenDB](https://github.com/jobbrIO/jobbr-storage-ravendb) | Yes, with [Jobbr.ArtefactStorage.RavenFS](https://github.com/jobbrIO/jobbr-artefactstorage-ravenfs)       |

:::{note}
**InMemory**: Without any extensions, the jobserver will fall back to InMemory versions shipped with with the `Jobbr.Server`-Package.
The InMemory implementations are a perfect fit for testing purposes but we don't recommend them for production scenarios.
Using them will write `WARN` log messages in your log.
:::

## MsSQL Server

## Filesystem

## RavenDB / RavenFS

In contrast to the MsSQL Server and the Filestore implementation, Raven can be used to both store Job Information and Artefact.
