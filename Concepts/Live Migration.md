---
tags:
  - live-migration
---
# Live Migration

Live migration is a *seamless transfer* of a running computational entity (e.g. [[Virtual Machine]]) between various hosts while the *continuous operation* and *client connectivity* is maintained.

## Migration Objectives

Migration objectives fall into four categories:

- Resource efficiency
	- VMs are packed into minimum number of physical hosts
	- Allows idle machines to be powered off or put to sleep
	- Enables dynamic load balancing
		- Through continuous monitoring, hotspots and cold spots are recognized
		- Can proactively distribute workloads
		- That way, [[Service Level Agreements (SLAs)]] are met
	- Implications for data centers:
		- Traditionally, energy efficiency is hardware-centric (focus on physical)
		- Live migrations shift paradigm to software-centric
		- Decoupling of workloads from hardware
- Reliability and fault tolerance
	- Ensures proactive fault tolerance
- Network optimization
	- Modern strategies focus on network and bandwidth awareness
- Security
	- Ensures survival of services during cyber threats

Success of migration strictly defined by *specific sensitivity* of the application being migrated.
There is no universal standard for *liveness*. Requirements vary depending on field:

- [[High Performance Computing (HPC)]]
- [[Big Data]]
- [[Internet of Things (IoT)]]

The difference between *live* and *cold* migrations isn't rigid, it is instead dictated by the *acceptable downtime* for a use case.

- *Live migration*: Seamless transition of a running computational entity
- *Cold migration*: Allows service disruption during migration from source to destination

Measuring live migration performance:

- *Migration time*: Total duration from start to end of the migration process
- *Migration downtime*: Period of service unavailability
	- Determines whether migration is live or cold, depending on acceptable thresholds

## Migration Techniques

Live migration relies on *shared storage* because of its efficiency in transferring only process memory and state between hosts. Alternative *shared nothing* strategies the whole VM storage in addition to memory and state needs to be transmitted. Migrating data across hosts can be achieved via two primary approaches, distinguished by the order in which key components are transferred:

- *Pre-copy*: Process memory content is transferred first before other elements
- *Post-copy*: Process state is transferred first before other elements

Pre-copy:

- Begins by transferring all memory pages and required files to the destination host while the process remains operational
- Then, iteratively transfers modified data until termination condition
- Then, process is paused and the remaining dirty pages and state are migrated
- Challenges:
	- Limited parallelism during data-dumping
	- Non-convergence of pre-copy strategy
	- Limited parallelism in recovery

Post-copy:

- Process state to target host and resume it there before transferring memory
- Then, memory pages are pushed to target, missing pages are fetched on demand
- Challenges:
	- High risk of data loss if failure before all memory pages transferred
	- Better performance than pre-copy, however not done in practice because of poor reliability

NOTE: Hybrid approaches exist that combine pre-copy and post-copy
NOTE: Boundaries of live migration can only be defined in context of a concrete use case

Checkpoint/restore tools:

- *Checkpointing*: Capturing the state of a running process
- *Restoring*: Recreate it as a new running process
- Can operate on different levels:
	- System-level
	- User-level
	- Application-level

## Migration Unit

The migration unit is the *computational entity* transferred between hosts during a migration:

- Container
- Pod
- VM

## Infrastructure Characteristics

Network conditions and resource availability have a direct effect on live migration performance, efficiency, and reliability.

---

## Links

%% Links to related concepts. %%

- [[@attar-khorasani2026]]
- [[@tinto2025]]