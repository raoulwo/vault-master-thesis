---
author: Govindaraj K., Artemenko A.
year: 2018
tags:
  - live-migration
  - edge-computing
  - containers
  - latency
pdf_link: "[[Container_Live_Migration_for_Latency_Critical_Industrial_Applications_on_Edge_Computing.pdf|Open PDF]]"
---
# Container Live Migration for Latency Critical Industrial Applications on Edge Computing

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper proposes a live migration scheme called *redudancy migration* that reduces the downtime by a factor of *1.8* compared to stock migration in linux containers.

## Thoughts

%% Thoughts and questions about the paper. %%

## Related Work

%% Existing state-of-the-art research. %%

## Contributions

%% Research gap and novel contributions. %%

Core contributions:

- Overview of requirements and challenges of edge computing for factory automation applications
- Redundancy live migration scheme to reduce service downtime
- Evaluation of benefits of redundancy migration scheme over stock migration scheme

Factory automation applications:

- Collaborative robotics
- [[Extended Reality (XR)|Augmented reality]]
- Autonomous transport system
- Predictive maintenance

Factory automation challenges:

- System requirements
- Infrastructure requirements
- Operating environment requirements
- Service migration

[[Live Migration]]:

- Live migration optimization schemes
	- Optimization of basic live migration schemes
	- State reproduction schemes for migration

Future work:

- Optimizing CRIU to reduce overall migration time

## Methodology

%% Architecture of the solution. %%

### Redundancy Migration

Characteristics:

- Unlike basic migration schemes, **no** *stop-and-copy* phase
- Application agnostic
- Environment independent
- Suitable for TCP and UDP applications

Assumptions:

- Application is deterministic
- Processing speed of network packets on destination server is *higher than*:
	- Packet generation rate
	- Transmission rate of the packets on the client

### Basic Idea

- Trigger for live migration can be:
	- Mobility of devices
	- Need for server maintenance
	- Load balancing requirement
- Destination server is chosen by *edge controller* using management scheme
	- Scheme not discussed in paper (out of scope)
- Client transmits packet streams to destination server
- Destination server buffers the packets and forwards the same to source server
- Source server starts to iteratively transmit files relevant to the service to be migrated to destination server
	- Also transmits checkpoint of CPU, memory, network state to destination server
	- Service keeps running meanwhile
- Destination server restores checkpoint provided by source server
- Once service is running, destination server replays buffered packets
	- States of service running on source and destination servers diverges
	- Once destination server packet buffer is empty, application states are the same

## Implementation

%% Actual implementation of the solution. %%

- Evaluated using LinuX Containers (LXC)
	- Lightweight virtualization because low overhead
	- Use cgroups to monitor resource utilization
	- Interface to [[Checkpoint;Restore in Userspace (CRIU)]]

Four phases:

1. Buffer and routing initialization phase
2. Copy and restore phase
3. Replay phase
4. Switch phase

## Evaluation

%% The evaluation of the solution. %%

- Comparison of *redundancy migration* versus stock LXD live migration
	- LXD: Open-source solution for managing VMs, containers
- Metrics: Migration time, downtime
- Main bottleneck for downtime in redundancy migration is CRIU
- Benchmark tool `lookbusy` used to control state size to quantify benefit

- Downtime: redundancy migration 1.8x lower downtime
- Migration time: redundancy migration 1.7x higher migration time
	- Long replay phase is main reason for increased migration time
		- Directly proportional to number of packets buffered during copy and restore phase
		- Best returns: Optimizing the copy and restore phase

- Rerouting overhead:
	- Latency added to each packet during redundancy migration
	- Overhead for processing times 30-45 microseconds
	- Overhead results in e2e latency of 100 microseconds
		- Minimal effect on latency requirements

---

## Links

%% Links to related literature. %%