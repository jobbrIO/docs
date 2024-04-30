Home of the Jobbr Documentation
===============================

Thank you for your interest in Jobbr, the .NET JobServer!  
You can find all the source code of the main repository and extensions over at the GitHub Organisation page: https://github.com/JobbrIO

We created a small set of articles to start with learning how Jobbr will help you implement background jobs more efficently,
the motivation behind creating an(other) embeddable job server in C#, and who's behind all of this.

.. toctree::
   :maxdepth: 1

   intro/welcome
   intro/features
   intro/why
   intro/architecture
   intro/status

If you consider using Jobbr in your .NET project, we recommend reading the following articles, which cover everything from the C# API to the Restful Web API.
It also highlights the available options and contains suggestions on how to deal with certain requirements.
  
.. toctree::
   :maxdepth: 2
   :caption: Library Documentation

   use/getting-started
   use/createJob
   use/triggers
   use/execution
   use/rest-api
   use/persistence
   use/recommondations

If you happen to miss a specific feature and plan to extend Jobbr, or ran into an issue which you want to help fix,
it's important to know where and what the architectural decisions were in the past so that you can understand and align with them.

This section also convers tips on contributions as well as how we release and how version bumps are made.

.. toctree::
   :maxdepth: 2
   :caption: Developer Documentation

   dev/contribution
   dev/tooling
   dev/extend
   dev/knowledgebase

.. toctree::
   :maxdepth: 2
   :caption: Project

   proj/project

.. toctree::
   :maxdepth: 1
   :glob:
   :reversed:
   :caption: Meeting Notes

   proj/meetings/*
