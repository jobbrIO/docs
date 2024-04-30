# SD 2024-02-13 "Mono Repo Approach"

## Current Situation

There are currently around 18 different repositories for all the different Jobbr components and component models.

The idea behind this setup is to be very flexible and extensible, as all the components are decoupled and if needed only reference the necessary NuGet packages.

Unfortunately this causes a lot of repetitive and unnecessary work, when trying to bring a change to multiple or all repositories.

## Target Situation

Even though mono repo can be a bit controversial, it seems to be about the only solution to the problem of having to run the same git operations on 18 different repositories.
There might be workarounds for solutions that spawn multiple repos and condition-based MSBuild includes, etc. but it would still leave us with a mess of managing all the repos themselves.

### Requirements

For a mono repo solution to work, it needs to be able to handle the following requirements:

- Retain the original Git history in some way
- Allow for individual versioning of the separate components
- Keep components isolate through NuGet packages
- Allow for quick iterations with references to local projects

### Git History

We can "merge" the commit Git history by using `git subtree`, which essentially checks out the whole repository inside a sub-directory.

```
git subtree add --prefix src/Server https://github.com/jobbrIO/jobbr-server.git develop
git subtree add --prefix src/Runtime https://github.com/jobbrIO/jobbr-runtime.git develop
git subtree add --prefix src/ComponentModel/Registration https://github.com/jobbrIO/jobbr-cm-registration.git develop
git subtree add --prefix src/ComponentModel/Management https://github.com/jobbrIO/jobbr-cm-management.git develop
git subtree add --prefix src/ComponentModel/JobStorage https://github.com/jobbrIO/jobbr-cm-jobstorage.git develop
git subtree add --prefix src/ComponentModel/Execution https://github.com/jobbrIO/jobbr-cm-execution.git develop
git subtree add --prefix src/ComponentModel/ArtefactStorage https://github.com/jobbrIO/jobbr-cm-artefactstorage.git develop
git subtree add --prefix src/Execution/Forked https://github.com/jobbrIO/jobbr-execution-forked.git develop
git subtree add --prefix src/Execution/InProcess https://github.com/jobbrIO/jobbr-execution-inprocess.git develop
git subtree add --prefix src/Storage/MsSql https://github.com/jobbrIO/jobbr-storage-mssql.git develop
git subtree add --prefix src/Storage/RavenDB https://github.com/jobbrIO/jobbr-storage-ravendb.git develop
git subtree add --prefix src/ArtefactStorage/FileSystem https://github.com/jobbrIO/jobbr-artefactstorage-filesystem.git develop
git subtree add --prefix src/ArtefactStorage/RavenFS https://github.com/jobbrIO/jobbr-artefactstorage-ravenfs.git develop
git subtree add --prefix src/WebAPI https://github.com/jobbrIO/jobbr-webapi.git develop
git subtree add --prefix src/Dashboard https://github.com/jobbrIO/jobbr-dashboard.git develop
git subtree add --prefix src/DevSupport https://github.com/jobbrIO/devsupport.git master
```

### Multi Versioning

GitVersion only really works for a single version in a repository and/or maybe with heavy tag usage.

[Nerdbank.GitVersioning](https://github.com/dotnet/Nerdbank.GitVersioning) does allow to have different version [per path](https://github.com/dotnet/Nerdbank.GitVersioning/blob/main/doc/pathFilters.md) and doesn't rely on crazy git tag systems.

### NuGet Isolation

One of the design goals of Jobbr was and is to have independent modules that you can put together at will. You can best ensure this isolation by having no project references and only depend on published NuGet packages.

This is already the status quo, so we don't have to do anything to fulfill this requirement.

### Quick Iterations

While the NuGet Isolation is great for low coupling, picking the wanted components, and easier extendability, it does make it harder to develop any larger feature, as you'd have to publish NuGet packages for each component, then either reference them locally or even worse, publish them to NuGet.org.

A simpler solution would be to have relative project references for certain build configurations and if the project exists. Having everything in a mono repo should make this fairly stable, as the relative paths are fixed and always known.

### Shared Configurations

This is probably where the mono repo approach will shine the most. Currently, we're the `devsupport` repo as git submodule in every repository to share our configurations and some basic infrastructure test facilities. This is very cumbersome to manage, as every change in the devsupport repository requires 17 updates in all the other repositories.

Being able to easily share a single StyleCop configuration or having one infrastructure test project, will be a lot more pleasant.

### Continuous Integration

Currently, we're using Appveyor to build, run tests and publish NuGet packages. While we've added GitHub Actions builds and test executions, the NuGet publishing has remained in Appveyor.

Going forward Appveyor will not be used anymore and the publishing of NuGet packages is moved to GitHub Actions as well.

In a mono repo, we'd need separate builds for each component, so we're not building the whole Jobbr monstrosity on every commit, but just the parts that changed. This can be achieved with path filters:

```yaml
on:
  push:
    paths:
      - "folder1/**"
  pull_request:
    paths:
      - "folder1/**"
```

### Development

A `git subtree` test has already been tested with the [Jobbr repository](https://github.com/jobbrIO/jobbr).

- 🟨 Make sure all the work has been merged to all the `develop` branches
- 🟨 `git subtree add` all the repos into a single repository
- 🟨 Create a shared README
- 🟨 Use share configurations
- 🟨 Set up a multi-component versioning system and set all the version correctly
- 🟨 Set up multi-component CI pipelines with NuGet publish support
  - 🟨 Test NuGet publishing without actually publishing anything
- 🟨 Set up quick iteration possibilities
- 🟨 Clean up the documentation
- 🟨 Move open issues & component labels
- 🟨 Archive all the other repositories
