---
title: Development branches
permalink: /components/development-branches/
---

* TOC
{:toc}
  
The Development branches feature allows you to modify [component configurations](http://localhost:4000/components/) 
without interfering with the running configurations or entire [orchestrated pipelines](http://localhost:4000/orchestrator/).

Typically, you would use branches when making bigger changes to a project or when you need to perform the changes safely. Imagine, 
that you have an ordinary orchestration extracting, transforming and writing the data to a target system. And you need to remove a 
column from the source. Such change involves modification of several configurations, ideally with a dry run to check that the 
data in the target system are ok. When such orchestration runs every ten minutes, this is impossible to achieve without 
an outage of data. This is where Branches are the most useful.

## Private Beta Warning
Development branches are currently in private beta. Some features may not work as expected. Please bear
with us while we polish all necessities. Any feedback is welcome at [https://ideas.keboola.com](https://ideas.keboola.com).

## How Branches work
When you create a (*development*) Branch in your project, you obtain a *shallow copy* of the project. A *shallow copy* means that
you'll get a copy of all the current configurations in the project. You can then modify these configurations without 
touching the original (*production*) ones (and these can keep being run by orchestrations).

When you run a configuration in a Branch, it is able to **read** the [tables]() and [files]() from the Storage as if it
were a normal configuration. However when a Branch configuration attempts to **write** data (be it tables or files) to Storage,
they are stored separately so as they do not overwrite the original (production) data and interfere with the running configurations.

To be able to run chains of configurations -- e.g. an extractor followed by a transformation -- one more rule is applied.
If you run a branch configuration and branch data is available, it is used. Only if the branch data is not present, the 
production data is used. Therefore when you run a branch transformation, it will read the production data and write branch output data.
When you run an extractor on which the transformation depends in the branch, it too will create branch output data.
When you run the transformation again, it will use the output of the branch extractor and write branch output data.

There are two ways how a Branch can end. Either it can be simply deleted - i.e. you do not wish to use the changes and simply discard them.
Or, in a more useful case, it can be **Merged**. Merging the branch means that all changes in configurations are brought back 
to their original configurations. All the changes are applied at once (after you approve them) and produce new 
[versions](https://help.keboola.com/components/#configuration-versions) of the respective configurations. The branch data are discarded.

It is important to note that all of this happens in a same project. Allowing you to collaborate on the modifications 
with other members of the project.

## Before You Start
To request to be a beta tester for this feature, please ask via the [support button](http://localhost:4000/management/support/) in your project.

## Tutorial
To see how you can take advantage of branches, you can go through the following tutorial. You will configure various components that demonstrate the different aspects of branches.

* Part 1 -- Working with tables
    * [Preparing](/components/development-branches/tables/)
    * [Working in Branch](/components/development-branches/tables/todo)
* Part 2 -- Working with files (optional)
    * [Preparing](/components/development-branches/files/)
    * [Working in Branch](/components/development-branches/files/todo)
* [Part 3 -- Merging branches](/components/development-branches/diff-and-merge/)
