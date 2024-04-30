# MN 2022-10-26 "Knowledge Exchange"

**Topic**: Determine the current state of Jobbr and how to move forward

## Participants

- Lukas Dürrenberger (@eXpl0it3r)
- Oliver Zuercher (@olibanjoli)

## Decision

- Lukas got access to the GitHub organization, the NuGet organization, ReadTheDocs, and Appveyor
- Lukas takes over development, especially for the .NET 6 migration

---

## History

Jobbr was originally created for a Zühlke project that experienced issues with memory leaks in jobs leading to application terminations, because Quartz ran everything in the same process and thus the memory space remains limited and all the job executions accumulate the leaks over time. As such one of the main pillars for Jobbr was to use isolated processes to prevent one execution to affect the rest of the application.

## Organisational

Nobody from the original team is actively maintaining Jobbr anymore.  
As Jobbr is still in use on at least one project Lukas works on, he'll take over the development, especially to migrate the whole project to .NET 6.

## Infrastructure

- GitFlow is used as git branching model
- GitVersion with SemVer is used for versioning all the components individually
- Appveyor is used as CI/CD with automatic NuGet publishing
- ReadTheDocs is the host for the documentation
- All packages are publishing to NuGet.org

## .NET 6 Migration

- It might make sense to pull together all the repositories, instead of having 18 different repos for each component.
- There shouldn't be that much code that needs migration. The biggest part will be APS.NET for the Dashboard.
