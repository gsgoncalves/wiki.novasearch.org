---
title: Condor
permalink: wiki/Condor/
layout: wiki
---

Condor is an workload management system for compute-intensive tasks. It
is used in production environments for more than 15 years, so it is
considered a stable and mature project.

Highlights
----------

-   Preserves user environment on remote machine
-   Users need not make files available or have access to remote machine

Condor Components
-----------------

-   Job queuing
-   Scheduling policy
-   Priority mechanism
-   Resource monitoring
-   Resource management

NovaSeach Condor setup
----------------------

This page details the Condor setup on our cluster.

We have four [nodes](/wiki/Cluster#Hardware "wikilink") with 6 real cores (12
HT threads).

All threads are available for condor, meaning we have 48 Condor slots,
named slot<number>@<machine>.novasearch.org.

### Important information

Each user has a **48GB memory limit** on each machine; each slot has a
**5GB memory limit**.

You must take these limits into account, so that your jobs don't get
killed as you reach the limit.

Usage
-----

### Create a submit file

$ vi sim.submit

`   Executable = sim`  
`   Input = sim.in`  
`   Output = sim.out`  
`   Log = sim.log`  
`   Queue`

### Submit the job

$ condor\_submit sim.submit

### Watch progress

$ condor\_q

### Running many processes

The real benefit of Condor comes from managing 1000s of jobs.

$ vi sim.submit

`   Executable = sim`  
`   Input = sim.$(PROCESS)`  
`   Output = sim.$(PROCESS)`  
`   Log = sim.log`  
`   Queue 1000`

### Running many processes with different arguments

`   Executable = sim`  
`   Arguments = $(PROCESS)`  
`   Output = sim.$(PROCESS)`  
`   Log = sim.log`  
`   Queue 1000`

Will execute 1000 processes with the process id as a parameter: sim 0,
sim 1, sim 2, ... , sim 999

If you need to change multiple arguments, you can set the shared
parameters at the beginning and change the required parameters

`   Executable = sim `  
`   Output = sim.$(PROCESS)`  
`   Log = sim.log`  
`   `  
`   Arguments = a    `  
`   Output = a.out`  
`   Queue`  
`   `  
`   Arguments = b`  
`   Output = b.out`  
`   Queue`  
`   `  
`   Arguments = c`  
`   Output = c.out`  
`   Queue`

Will execute 3 processes with the selected parameters: sim a, sim b, sim
c and will output to a.out, b.out, c.out respectively.

If you have many parameter combinations, we suggest you generate this
condor file with a script.

### Other useful parameters

`   getenv = True`

This parameter shares the local environmental variables with the remote
environment. This is useful if you want to share global library paths.

`   initialdir = `<path>

Sets the base execution path of the execution. Useful to reference files
with relative paths.

`   Requirements = (Machine == "arthur.novasearch.org")`

Restricts the job to run on machines that satisfy the requirement. On
this example, the jobs will only be deployed to **arthur**.
