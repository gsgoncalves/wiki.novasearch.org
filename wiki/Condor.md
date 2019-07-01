---
title: Condor
permalink: wiki/Condor/
layout: wiki
---

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
