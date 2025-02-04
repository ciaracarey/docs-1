---
title: Run multiple processes on a target simultaneously
description: How to run multiple processes on a deployment target simultaneously.
position: 70
---

By default, Octopus will only run one process on each [deployment target](/docs/infrastructure/deployment-targets/index.md) at a time, queuing the rest. There may be reasons that you need to run multiple, and that's okay we have a setting for that!

![](images/bypass-deployment-mutex.png "width=500")

`OctopusBypassDeploymentMutex` must be set at the project variable stage. It will allow for multiple processes to run at once on the target. Having said that, _deployments of the same project to the same environment (and, if applicable, the same tenant)_ are not able to be run in parallel even when using this variable.

## Multiple projects

If you require multiple steps to run on a target, by multiple Projects in parallel, you need to add the `OctopusBypassDeploymentMutex` variable to **ALL** of your projects.

:::problem
**Caution**
When this variable is enabled, Octopus will be able to run multiple deployments simultaneously on the same machine. This can cause deployments to fail if the same file is modified more than once at the same time.

If you use `OctopusBypassDeploymentMutex`, make sure that your projects will not conflict with each other on the same machine.
:::

## Max Parallelism

When enabling `OctopusBypassDeploymentMutex` there are a couple of special variables that may impact the number of parallel tasks that are run.

* `Octopus.Acquire.MaxParallelism`:
    * This variable limits the maximum number of packages that can be concurrently deployed to multiple targets.
    *  By default, this is set to `10`.
* `Octopus.Action.MaxParallelism`:
    * This variable limits the maximum number of machines on which the action will concurrently execute.
    * By default, this is set to `10`.
    * **Note:** Some built-in steps have their own concurrent limit and will ignore this value if set. For example the [health-check step](/docs/projects/built-in-step-templates/health-check.md).

Given five projects with the **OctopusBypassDeploymentMutex** set as follows:

- Project 1: `True`
- Project 2: `True`
- Project 3: `False`
- Project 4: `True` 
- Project 5: `True`

Assuming the deployments for these projects are started in that order, the first two will run in parallel, but the third will wait until they have finished. The last two will then also be blocked until _project three completes_ at which point they both will run in parallel.

## Named mutex for shared resources

!include <powershell-named-mutex>
