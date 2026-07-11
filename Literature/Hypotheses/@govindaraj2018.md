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

### Hypotheses

The paper assumes that the *processing of network packets on the destination server* is higher than the *packet generation and transmission rates* of packets on the client. For live migration *from ground station to satellite* this assumption does not hold since the computational capabilities of satellites are quite limited. This will presumably lead to a buffer flow on the satellite's side since the ground station will send packets at a much higher rate than can be processed:

*Hypothesis 1*. Terrestrial live migration methods fail when sending data from a ground station (GS) to a satellite because the source server transmits data faster than it can be processed by the destination server. 

The paper assumes that the *destination server can smoothly buffer and forward incoming client data to the source server*. For live migration from a ground station to a satellite constellation, this assumption does not hold because the *multi-hop network chain introduces compounding network jitter, asynchronous frequencies, and severe Doppler shifts*. If the ground-to-satellite uplink remains stable while the inter-satellite link (ISL) degrades, the destination satellite will receive data from the ground faster than it can forward it. This will presumably lead to a buffer overflow and data loss on the destination satellite:

*Hypothesis 2*. Terrestrial redundancy-based live migration methods fail in orbit because variable inter-satellite link (ISL) bandwidth restricts destination satellite's ability to forward data, causing rapid memory buffer exhaustion when paired with a high-speed ground station uplink.

Comment by Daniel: ISLs have throughput of roughly 10-100 Gbit/s, way higher than ground-to-satellite links that also face the issue of transmission loss due to atmospheric absorption. So, this hypothesis is quite unlikely to be true.

The paper assumes that the *application is deterministic*, meaning the *destination server can replay buffered network packets* to catch up to the source server's state. For live migration in orbit, this assumption does not hold because the space environment forces hardware into non-deterministic execution loops. Cosmic radiation causes Single Event Upsets (SEUs) that force the CPU to inject arbitrary processing pauses for error correction, while passive thermal stagnation triggers aggressive CPU clock throttling. This will presumably prevent the destination satellite from emptying its packet buffer, causing the migration loop to fail to converge before the ground station visibility window closes:

*Hypothesis 3*. Live migration methods relying on packet replay fail to achieve state convergence in orbit because radiation-induced hardware interrupts and thermal throttling destroy computational determinism, preventing the destination satellite from processing its backlog before losing ground station connectivity.

Comment by Daniel: Would need to confirm the Gemini statement of applications not being deterministic in space, literature to confirms these claims were recommended by LLM.

## Related Work

%% Existing state-of-the-art research. %%

### Introduction

- Mobility and availability requirements in Industry 4.0 pose unique requirements for existing live migration schemes in terms of downtime

## Contributions

%% Research gap and novel contributions. %%

Core contributions:

- Overview of requirements and challenges of edge computing for factory automation applications
- Redundancy live migration scheme to reduce service downtime
- Evaluation of benefits of redundancy migration scheme over stock migration scheme

### Factory Automation Applications

This section presents most prominent applications and their requirements.

#### Collaborative Robotics

- Helps workers perform mundane, strenuous tasks
- Human-robot collaboration
- High safety requirements
- Sensing, processing, reacting must occur in real-time (millisecond cycles)

#### [[Extended Reality (XR)|Augmented reality]]

- AR helps workers with visual aids
- Processing is computationally expensive
- To avoid perceptual issues, e2e latency <= 16ms

#### Autonomous Transport System

- Transports objects without human intervention
- Complex algorithms need to work in real-time

#### Predictive Maintenance

- Extends maintenance to prediction, optimization, and prevention
- Requires massive amounts of data
	- Has impact on required communication and computation resources

#### Predictive Maintenance

### Special Challenges of Edge Computing for Factory Automation

- Main challenge of edge computing is *deterministic operation* of industrial applications
- Strict *latency* and *reliability* requirements

#### System Requirements

- Applications require *safe and reliable* operation
- In closed-loop control applications, control cycle period must be ~0.5ms
- Also for closed-loop control applications, packet loss rate must be below $10^{-9}$
- Huge amount of data processing is required

#### Infrastructure Requirements

- Size of industry plants vary
	- Complexity of applications scale with size
- Applications that generate huge amounts of data can cause a bottleneck in centralized data clouds
	- Restructuring complete factory networks is expensive and time consuming
	- Infrastructure of edge servers is required


#### Operating Environment Requirements

- Virtualization supports hardware flexibility, isolation, and scalability
	- Add additional delay

#### Service Migration

- Dynamic factory requirements demand flexible reconfiguration and load balancing

### [[Live Migration]]

- Concept of moving a service from one server to another without losing state of the running application
- For this, CPU, network, and memory state need to be migrated
- Two important metrics:
	- Migration time
	- Downtime
- Multiple live migrations with long ongoing migration times lead to reduced resource usage efficiency

#### Basic Live Migration Schemes

##### Pre-Copy Migration

- Consists of two phases
	- Push phase
	- Stop-and-copy-phase

1. Push phase:
	- whenever a memory page is modified, the source server copies it iteratively and sends it to destination server
	- this happens until number of altered memory pages falls under a threshold
2. Stop-and-Copy Phase:
	- Source server freezes the service to achieve consistent state
		- This results in downtime
	- The destination server then restores and resumes the service in consistent state

##### Post-Copy Migration

- Opposite approach to pre-copy migration to reduce the downtime during *stop-and-copy phase*
- Consists of two phases:
	- Stop-and-copy phase
	- Pull phase

1. Stop-and-Copy Phase:
	- Source server freezes service to transfer CPU state only
		- This results in downtime
	- Once destination server receives transmitted CPU state, the destination server resumes the service
		- However, because memory state isn't transferred yet, it'll experience pagefaults on every memory page request
		- This starts the pull phase
2. Pull Phase
	- Pulls the requested memory pages from source server
	- Leads to jitter and unpredictable response times during this phase

##### Hybrid Migration

- Combination of *pre-copy* and *post-copy* migration strategies
- Three phases
	- Push phase
	- Stop-and-copy phase
	- Pull phase

1. Push phase
	- Source server transfers image of all states to destination server
2. Stop-and-copy phase
	- Source server freezes the service
		- This leads to downtime
3. Pull phase
	- Destination server restores the service
	- Requests missing memory pages

- Live migration optimization schemes
	- Optimization of basic live migration schemes
	- State reproduction schemes for migration

##### Discussion

- All of these schemes have the goal of minimizing downtime to minimal level
- Migration time depends on threshold limits
- Post-copy migration improves on downtime of pre-copy
	- However, introduces jitter and unpredictable response times
- Hybrid approaches downtime and migration time, however stop-and-copy phase still exists


Future work:

- Optimizing CRIU to reduce overall migration time


#### Live Migration Optimization Schemes

##### Optimization of Basic Live Migration Schemes

- (27) propose three phase optimization for pre-copy
	- identify the writable working set (WWS) to prevent sending the same memory page multiple times
- (25) propose optimization for post-copy to reduce jitter
	- predict which memory pages will be needed for prefetching

##### State Reproduction Schemes for Migration

- Basic live migration schemes require *post-and-copy* phase to keep state consistent
	- Leads to downtime
- To achieve lower downtime, new approaches are required
	- e.g. trace and replay method
		- (28) proposes combination of checkpoint/restore, trace and replay
			- builds upon (29)
	- (30) uses checkpoint and replay

## Methodology

%% Architecture of the solution. %%

### Redundancy Migration

#### Characteristics

- Unlike basic migration schemes, **no** *stop-and-copy* phase
- Application agnostic
- Environment independent
- Suitable for TCP and UDP applications

#### Assumptions

- Application is deterministic
- Processing speed of network packets on destination server is *higher than*:
	- Packet generation rate of the client
	- Transmission rate of the packets on the client

### Basic Idea

Trigger for live migration can be:

- Mobility of devices
- Need for server maintenance
- Load balancing requirement

- Destination server is chosen by *edge controller* 
	- using management scheme
	- Scheme not discussed in paper (out of scope)
- Client transmits packet streams to destination server
	- Destination server buffers the packets and forwards the same to source server
- Source server starts to iteratively transmit files relevant to the service to be migrated to destination server
	- Also transmits checkpoint of CPU, memory, network state to destination server
		- Destination server restores checkpoint provided by source server
	- Service keeps running meanwhile
- Once service is running, destination server replays buffered packets
	- States of service running on source and destination servers diverges
	- Once destination server packet buffer is empty, application states are the same

Thoughts on this scheme in LEO context:

- Client is ground station (GS)
- Source and destination servers are satellites
- Requires ground-to-satellite connection
	- This is limited due to LEO satellite mobility (connection exists for couple of minutes)
- Requires inter-satellite connection
	- QUESTION: How long do two LEO satellites of the same constellation see each other?
		- Depends on whether they are on the same orbital plane (intra-plane) or different orbital planes (inter-plane):
			- intra-plane: they travel behind one another in the same ring, always see each other
			- inter-plane: they pass each other briefly, might be visible for a couple of minutes
- Requires destination server to have large enough packet buffer to store application data from client
- Destination server acts as relay node between client and source server
	- Asynchronous frequencies and doppler shift between those three compound
		- Lead to higher jitter
	- If ground-to-satellite communication is good and ISL connection degrades with high packet loss rate, the buffer will overflow
- Limited bandwidth of ISL connections, is data forwarding from destination to source server possible
- Assumes constant communication links between client, source server, destination server
	- Packet loss results in retransmission which between ground station and satellite leads to high latency 
	- Question: How far apart are LEO satellites, how costly is retransmission between satellites?
		- In Starlink, neighboring satellites are roughly 1000-2000km apart
			- At speed of light estimate this would be roughly 3-7ms latency
			- TCP RTT would be ~14ms, 3 hops would be 40-50ms
- The migration scheme assumes the application to be deterministic so that diverging application states can converge after replaying the buffer
	- Space environments however inject hardware non-determinism
		- e.g. radiation-induced ECC memory pauses, thermal throttling
	- For this reason, replay doesn't guarantee converging application states

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