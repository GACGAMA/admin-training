layout: true
class: inverse
---
class: special, center, middle

# All these Clouds
## It's positively meterological..

.normal[
.footnote[\#usegalaxy \#PennState2019 / @galaxyproject] ]
---
class: left, middle, center

# Please interrupt

*We are here to answer questions!*

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Overview

.large[
* Galaxy in the Clouds?
* AWS and other Clouds
* CloudMan and CloudLaunch (& cloud agnosticisim)
* Genomics Virtual Lab
* Architecture
* Persistence
* Integration with the cloud: CloudBridge
]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Help!

.large[
* Galaxy server flat out?
* Queue longer than a Grateful Dead concert?
* An urgent job to run?

What do you do now?
]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Help!

.large[
* Galaxy server flat out?
* Queue longer than a Grateful Dead concert?
* An urgent job to run?

What do you do now?

.special[Use the cloud man!]
]
.center[![cloudman](images/cloudman_logo.png)]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Clouds?

**Cloud computing** ... is a model for enabling ubiquitous, **on-demand** access to a **shared pool** of configurable **computing resources** ... which can be **rapidly provisioned and released** with minimal management effort. Cloud computing and storage ... may be located far from the user – ranging in distance from **across a city to across the world.** - Wikipedia, Cloud Computing.

.center[![aws_logo.png](images/aws_logo.png) ![OpenStack_logo.png](images/OpenStack-logo.png)
<br /> ![gce-logo.png](images/gce-logo.png) ![azure-logo.png](images/Azure-logo.png)]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Available Clouds

.large[
* Commercial: Amazon Web Services, Google Compute Engine, Microsoft Azure
  * Pay-per-time/machine etc.
  * Reasonably priced, but keep an eye on the costs
  * Large range of machine (i.e., instance) types
  * Education grants
* Academic:
  * OpenStack: open source community project
  * NeCTAR in Australia, Jetstream in USA, CLIMB in UK, lots of others
  * Some free for researchers (NeCTAR, CLIMB), some with project grants (Jetstream)
]
.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Why Clouds?

.large[
* Elastic compute!
  * Can dynamically resource analyses
* No need to maintain the hardware
  * Provider takes on cost of hardware and maintenance
  * Cost is shared between all users
* Move the compute to the data
  * Data on East Coast?
  * Start compute there. Save on data transfer.
]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Galaxy on the Cloud

.large[
* There are cloud images (VM blueprints) available
  * with Galaxy pre-installed
  * with different sets of tools installed
  * with access to reference data
  * for different clouds (AWS globally, Jetstream, NeCTAR, CLIMB etc.)
* You just need credentials for the cloud you want to "launch" on.
  * Credentials are generally strings
  * An access key and a secret key or username and password with project details
  * They are obtained from the cloud account admin page you want to use
]
.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Taking it Further: Genomics Virtual Lab (GVL)

![GVL-evolution.png](images/GVL-evolution.png)

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# CloudLaunch

.large[
* CloudLaunch is a system for launching Galaxy (and other applications) on cloud resources
  * Public server available at [launch.usegalaxy.org](https://launch.usegalaxy.org)
  * You can also install and run your own
  * Fill in the credential details, choose a location and machine size
  * Press go!
* CloudLaunch will now provision you a computer in the cloud with Galaxy installed and ready to go.
  * Depending on your choices and availability you will also have access to reference data and various tools
  * It should only take 2-3 minutes for everything to be set up.
]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Launch Demo

.large[
**End-user** perspective
* Create your own machine with Galaxy in the cloud

https://launch.usegalaxy.org/
]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Cloud Manager

.center[![cloudman](images/cloudman_logo.png)]

.large[
* Cloud manager: middleware to control cloud clusters
* Can be used to control system and application services, such as Galaxy
* Can mount filesystems, dynamically add/remove worker nodes, start/stop services
]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# CloudMan

.center[<img src='images/cloudman-main.png' width=85%>]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# CloudMan Admin

.center[<img src='images/cloudman-admin.png' width=80%>]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Cluster on the Cloud?

.large[
* Your CloudMan instance is a single machine
  * It is the "Head node" of a cluster
* CloudMan can start "worker" nodes.
  * More cloud instances (of any size)
  * Automatically connects to file system
  * Are registered in Slurm setup
  * A node will take ~2-3 minutes to start and configure.
]
.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Auto-Scaling

.large[
* Can set up dynamic scaling to respond to the system load
  * Upper and lower node numbers
  * When queue is full and jobs wait certain time, new nodes are launched.
]

.center[<img src='images/cloudman-scaling.png' width=60%>]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# CloudMan Galaxy

.large[
* Your Galaxy server is set up and ready to go!
  * Includes large list of pre-installed tools.
  * Includes access to reference data files
  * Zero-to-go in less than 10 minutes.
* Toolsets and reference sets can be tailored to suit needs
  * Will be discussed in architecture section
]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# CloudMan Galaxy

.pull-left[
<img src='images/Cloudman-tools.png' height=85% />
]
.pull-right[
<img src='images/reference_data.png'/>
]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# CloudMan Galaxy

* Configured for Slurm out of the box
```xml
...
    <destinations default="default_dynamic_job_wrapper">
        <destination id="slurm_cluster" runner="slurm"/>
        <destination id="slurm_4slots" runner="slurm">
            <param id="nativeSpecification">--ntasks=4</param>
        </destination>
        <destination id="default_dynamic_job_wrapper" runner="dynamic">
          <param id="type">python</param>
          <param id="function">default_dynamic_job_wrapper</param>
        </destination>
    ...
    </destinations>
    <tools>
        <tool id="toolshed.g2.bx.psu.edu/repos/devteam/bwa/bwa/0.3.1" destination="slurm_4slots" />
...
```
``` ini
# COMPUTE NODES
NodeName=master NodeAddr=45.113.232.91 CPUs=15 RealMemory=64431 Weight=10 State=UNKNOWN
NodeName=w1 NodeAddr=45.113.232.83 CPUs=16 RealMemory=64431 Weight=5 State=UNKNOWN
NodeName=w2 NodeAddr=45.113.232.92 CPUs=8 RealMemory=32176 Weight=5 State=UNKNOWN
NodeName=w3 NodeAddr=45.113.232.93 CPUs=8 RealMemory=32176 Weight=5 State=UNKNOWN
```

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Persistence

.large[
* Cloud instances are typically transient
  * Can be terminated and resources returned to the pool
* However, user data and cluster configuration can be persisted
  * Then can be attached to new instance when they start
* CloudMan stores an instance's set up in an object store container for persistence
]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]

---
# Looking to the future

.large[
* Cloudlaunch has a Pulsar appliance
* As of Galaxy version 19.01
    * You can make dynamic rules to look for your pulsar appliances in the cloud
    * Automatically connect to them
    * Send jobs to them
]

.footnote[\#usegalaxy \#PennState2019 / @galaxyproject]
