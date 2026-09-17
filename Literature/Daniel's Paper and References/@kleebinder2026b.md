---
author: Kleebinder D. et al
year: 2026
tags:
  - leo-satellites
  - live-migration
  - virtual-stationarity
pdf_link: "[[Migration_as_a_Steady-State_Execution_Primitive__A_Runtime_for_Virtual_Stationarity_in_Low-Earth_Orbit.pdf|Open PDF]]"
---
# Migration as a Steady-State Execution Primitive: A Runtime for Virtual Stationarity in Low-Earth Orbit

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper introduces a novel virtual stationarity runtime paradigm. A virtual stationarity runtime (VSR) framework is defined and subsequently evaluated on heterogeneous hosts, as well as via simulations.

%% Notes about the paper. %%

## Introduction

- Satellites as compute nodes (not only relays), interconnected via:
	- Inter-satellite links (ISLs)
	- Ground-to-satellite links (GSLs)
- LEO satellites (altitudes from 80 to 2000km) yield *single-digit-millisecond propagation delays*
	- Can therefore host latency-critical workloads in physical proximity to satisfy SLOs
- LEO satellite velocities of ~27'000km/h reduce line-of-sight contact windows to minutes
	- To maintain logical proximity, applications must continuously migrate

Contributions:

- Formalize system model and feasibility conditions
- Design virtual stationarity runtime (VSR) framework
	- Separates external orchestration from runtime handover
	- Defines 4 phase protocol (prepare, reconstruct, project, commit)
- Introduce *migration membrane* which enables two mechanisms
	- *Capability virtualization* to dynamically rebind host-dependent resources
	- *Continuous state projection* to prepare successor nodes while source executes
- Implement the VSR framework and migration membrane using WAMR runtime
- Evaluate VSR on a physical Raspberry Pi 5 testbed

## Motivation

- GSLs can achieve near-optimal propagation latencies of under 10ms
- XR-assisted offshore maintenance use case (motivation for migrating applications):
	- maintenance window is longer than satellite line-of-sight contact window
	- static deployment of application on a satellite causes latency to increase due to increasing number of ISL hops

Migrating the application:

- Extending WAMR to support three canonical migration paradigms
	- Checkpoint/restore
	- Pre-copy
	- Post-copy
- Omit CRIU because it requires OS configurations not supported on edge hardware
- UDP telemetry data from VR gaming dataset trace (~4000 packets/s = ~5MB/s)
- Checkpoint/restore:
	- Halts execution
	- Serialized memory and CPU state
	- Transfers over network
	- Restores
	- Complete service blackout for duration (means 100% packet loss)
- Pre-copy
	- Initial memory snapshot is created and transferred
	- Dirty memory pages are then iteratively transferred
	- Low packet loss, however high memory write rates require higher transfer rates
		- Can prevent iterative convergence
- Post-copy
	- Pauses source instance to transfer minimal execution registers
	- Spawns the process on the target host without its memory
	- Missing memory pages are then fetched from the source host
	- Initial downtime is negligible, however severe packet loss and latency jitter

## Related Work

- *ReSync*: couples CRIU checkpoint/restore with buffered input replay
- *Voyager*: combines CRIU memory transfer with networked union filesystem
- *dOS/RecShim*: enforces deterministic process groups and records external non-determinism for replay
- However, all three require on OS/kernel-level mechanisms; problem -> migration across heterogeneous platforms

- *ReVirt*: logs non-deterministic events beneath guest OS for instruction-level VM replay
- Liu et al. combine copy-on-write VM checkpoint with iterative transfer and replay
	- Runs at VM boundary and targets single handover
- Yu et al. adapt iterative logging and replay above a shared Docker image

- Bhattacherjee et al. define virtual stationarity and evaluate policies to keep a service near ground
- Wu et al. jointly optimize task offloading and service migration
- Databelt constructs a data path for orbital serverless workflows
- Komet repeatedly instantiates stateless functions and proactively replicates application-managed state

TODO: The rest of this paper is information-dense, requires time to understand everything.

---

## Links

%% Links to related literature. %%

### References

- [[@bhattacherjee2020]]
- [[@denby2020]]
- [[@pfandzelter2024]]
- [[@rong2023]]
