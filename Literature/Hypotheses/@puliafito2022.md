---
author: Puliafito C., Conforti L., Virdis A., Mingozzi E.
year: 2022
tags:
  - live-migration
  - edge-computing
  - internet-of-things
  - quic
pdf_link: "[[Server-Side_QUIC_Connection_Migration_to_Support_Microservice_Deployment_at_the_Edge.pdf|Open PDF]]"
---
# Server-Side QUIC Connection Migration to Support Microservice Deployment at the Edge

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper proposes an extension of the [[QUIC]] protocol to support server-side connection migration when a container is migrated between servers.

%% Notes about the paper. %%

## Introduction

- Microservices are loosely coupled and can be scaled and deployed independently
- They are mostly packaged using containers
- [[Containers]] more lightweight, faster to boot up, and quicker to migrate than [[Virtual Machines (VMs)|VMs]]
- [[Edge Computing]] emerging as extension of [[Cloud Computing]]
	- Enables low latency, high bandwidth, improved privacy
- Mobility of end users necessitates migration of edge-microservices
	- Done by performing *stateful migration*
- Stateful migration transfers the state of the container from source to destination server
	- Migration state includes memory pages, CPU state, disk etc.
- Container migration across different data centers typically results in changed IP address of server-side endpoint of communication between user client and microservice
	- Requires mechanism to re-establish connection between them
- Existing solutions ...
	- leverage network virtualization
	- introduce ad-hoc protocols and dedicated forwarding functions
	- lean on client to discover the service and re-establish [[Transmission Control Protocol (TCP)]] connections
- Leads to increased overhead and adds network complexity

## Contributions

- Propose a [[QUIC]]-based solution to support server-side container migration
- Extend QUIC to support server-side connection migration using three strategies:
	- *Reactive-Explicit*
	- *Proactive-Explicit*
	- *Pool-of-Addresses*
- Evaluate performance through experiments performed on edge testbed
- Quantitative comparison with solution based on TCP and DNS
- Qualitative comparison with solution based on MPTCP

## Background

### Stateful Container Migration

State consists of two parts:

- Container image: includes guest OS file system and idle application, also application-specific data
- In-memory state: consists of all resources currently loaded into memory (memory pages, CPU state, register contents)
- Migrating the whole container state is often impractical
- To optimize the migration process, the paper follows the approach of [[@machen2018]]
- Proactively distribute container images based on predicted demand
- Because images are already distributed, only in-memory state needs to be transferred next
- Four approaches:
	- Cold migration
		- downtime equals migration time
	- Pre-copy migration
		- shorter downtime than cold, higher migration time
	- Post-copy migration
		- improves on pre-copy downtime, prolonged migration time
	- Hybrid migration
		- shortest downtime, shares pre- and post-copy cons
- As of writing the paper [[Checkpoint;Restore in Userspace (CRIU)]] is a tool that can be used to implement all the techniques above

### [[QUIC]] Protocol

- Described in the concept notes (link above)

## QUIC Extension Design

### Reference Architecture and General Design

- End-user devices run client application and QUIC instance
- Server (cloud or edge) have two components:
	- Migration management: handles container migration
	- Container: encapsulates a microservice, consists of:
		- Server application
		- Server-side QUIC instance
	- NOTE: QUIC server is part of container and migrated with it
- Three strategies proposed for server-side migration of containers
	- i) Reactive-explicit
	- ii) Proactive-explicit
	- iii) Pool-of-addresses
- All strategies are based on the idea that server-side connection migration can be achieved by introducing minimal and non-breaking additions to both QUIC client and server
	- Client and server are then migration-aware
- Difference between 3 strategies:
	- Both i) and ii):
		- QUIC server is informed at runtime of container migration and notifies QUIC client with destination address (IP and/or Port) before migration starts
		- Speeds up migration as destination address is known
		- Difference between reactive and proactive once packets are sent
	- iii): 
		- Assumes that container can be migrated to a server part of a set of servers known in advance
		- When establishing the connection, the QUIC server notifies the QUIC client of the pool of possible destination addresses
		- QUIC doesn't deterministically know the next destination address, worsens performance
			- However, this allows iii) to pre-provision the pool of known addresses without any runtime interaction of migration runtime
- The three strategies can't be compared equally, i) and ii) outperform iii)
	- i) and ii) should be used when there exists a central entity orchestrating the migration
		- The *orchestrator* knows the new IP address dynamically and can interact with the QUIC server
	- iii) is suitable for cases where there is no central *orchestrator* for migration
- The proposed solution involves extension of QUIC on both client- and server-side
- Only involves QUIC logic, application-logic untouched, migration-agnostic

### Reactive and Proactive Explicit

- Reactive: Client considers old server active until packet loss occurs (no response)
	- THOUGHTS: Ground-to-satellite connection can have transmission loss
		- Or do they mean no response despite being a reliable protocol ensuring packets arrive
- Proactive: As soon as migration occurs, client considers new server active

### Pool-of-Addresses

- Suitable for mobility scenarios in which the area where the user's device moves is known
	- THOUGHTS:
		- Area for LEO satellites in vicinity of ground stations is known
		- However, LEO satellites leave and enter this area in their orbit
- Pool of addresses:
	- Either configured in container base image
		- All containers from the same base image have the same pool
	- Or provided as parameter when container is launched
	- THOUGHTS: 
		- Doesn't work right, since satellites in range constantly change
		- Need to dynamically update the pool
- QUIC server can migrate to any of the addresses without notifying the client first
- When the QUIC client can't reach the server, it'll probe addresses from the pool to find where the QUIC server has migrated
	- Server itself will be probed again since it may have not answered because of congestion
	- Probing the servers happens cyclically
		- Reason: Client may probe the correct server before the container becomes available, thus falsely considering it unavailable
		- Authors note that the way the servers are probed could be done using a chosen criteria
			- e.g. Priority-based criteria
			- THOUGHTS: Definitely relevant for LEO satellites since you want to consider satellites that remain in the GS window long enough for migration
	- THOUGHTS: 
		- Ground-to-satellite RTT significant?
		- How many LEO satellites typically in window of a ground-station at a given time?

## Implementation

- Implemented by extending `aioquic` using Python
- GitHub links below

## Evaluation

- Comparison of extended QUIC versus TCP-based system that closes TCP connections at the end of each request
	- CRIU requires TCP connections to be closed before migration to different subnets
- Read up on exact experiment setup

Metrics:

- *Service time*. Time between beginning of client request and reception of server response.
- *Container migration time*. Time to complete container migration.
- *Container downtime*. Time during which container is not up and running.
- *Container migration overhead*. Amount of data transmitted during migration.

Scenario 1 - Sporadic requests:

- TCP performs worse after migration than any QUIC strategy

Scenario 2 - Continuous back-to-back requests:

- Service time significantly higher because of container migration
	- Packet loss doubles delay between retransmissions
- iii) performs significantly worse than other strategies

## Hypotheses

The paper states that the [[QUIC]] manages to reduce delay when establishing a connection from 3 required RTTs using [[TCP]] to a single RTT for an unknown destination server and zero RTTs for a known destination server.

*Hypothesis 1*. Due to significant propagation delay in ground-to-satellite communication, the use of QUIC as a transport protocol as opposed to TCP can improve communication performance.

The paper introduces two *explicit* migration strategies, *reactive-explicit (R-explicit)* and *proactive-explicit (P-explicit)*. The P-explicit strategy operates in a way where the client drops the connection to the source server once the migration to the destination server starts. The R-explicit strategy on the other hand involves the client continuously sending packets to the source server, only switching to the destination server once packet loss occurs. Both strategies work under the assumption that the source server is notified by an imminent migration and the client is informed of the IP address of the destination server. These strategies are suitable for use with a *central orchestrator*.

*Hypothesis 2*. Because both explicit migration strategies involve notifying the QUIC client of the exact destination address, the address is deterministically known, thus speeding up the connection migration. However, this requires an *orchestrator*, a central orchestrator deployed via cloud being infeasible for LEO constellations as its usage would introduce substantial propagation delay. 
Instead, distributed orchestration strategies would have to be considered.

NOTE: Falls under scheduling more than actual live migration mechanisms.

The paper also introduces a third migration strategy, *Pool-of-addresses*. The containers manage a set of addresses for potential destination servers. This set either being baked into the underlying image or passed as parameter to the startup of a container. Following this strategy, the source server notifies the client of possible server addresses to migrate to, the client storing those. Migration can then happen at any moment without the client being notified. Once the QUIC client can't reach the source server anymore, it then needs to cyclically probe all addresses in the pool to connect to the destination server. The paper states that the client could be configured to probe the candidate destination servers using a given criterion.

*Hypothesis 3*. The *pool-of-addresses* migration strategy is unsuitable for ground-to-satellite communication due to the inherently high RTT and packet loss (e.g. through atmospheric absorption) and the inefficient cyclical probing.

---

## Links

%% Links to related literature. %%

- [Explicit Implementation (GitHub)](https://github.com/kruviser/aioquic-explicit_UniPisa)
- [Pool-of-Addresses Implementation (GitHub)](https://github.com/kruviser/aioquic-PoA_UniPisa)