---
author: Pfandzelter T.
year: 2023
tags:
  - leo-satellites
  - orbital-edge-computing
  - serverless
pdf_link: "[[Serverless_Abstractions_for_Edge_Computing_in_Large_Low-Earth_Orbit_Satellite_Networks.pdf|Open PDF]]"
---
# Serverless Abstractions for Edge Computing in Large Low-Earth Orbit Satellite Networks

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper introduces serverless abstractions for LEO edge applications.

## Thoughts

- LEO networks have the potential to extend 6G mobile networks to remote/rural areas.
- Distributed edge systems face challenges such as data management and scalability.
- Large-scale LEO networks need to operate (Starlink has more than 4000 satellites) across thousands of shared edge servers while satellites have high mobility
- Idea of building serverless abstractions for developers hiding these constraints/challenges to provide a flexible, scalable, and elastic development platform.
- Again, mentioning of data replication across edge-networks. Not really a problem specific to orbital-edge systems however. Also seems to be orthogonal to WebAssembly and cold-start latencies.
- Serverless edge compute platform specifically for satellite networks sounds interesting, creating a more lightweight environment than Kubernetes, based on Wasm for shorter cold-start latencies? One thing to address as always is state management which cannot be done on the cloud-side, else the benefits from edge deployment would be for nothing.
- *Virtual stationarity*: Migrating logic to nodes to stay in proximity in clients seems to be an LEO-specific mechanism addressing the issue of satellite mobility. While WebAssemlby has use for serverless computing and serverless being identified as a great underlying platform for orbital edge systems, i don't see how Wasm addresses issues specific to the orbital edge, not including terrestrial systems. -> I mean, in terms of migrating systems: AoT-precompiled Wasm could address cold-start latencies for frequently migrated functions; as compared to container-based serverless functions.
- It is mentioned that ISL topologies have unique 2D torus shape on which algorithms can be based for intelligent migration

## Related Work

%% Existing state-of-the-art research. %%

## Contributions

%% Research gap and novel contributions. %%

Large scale LEO-networks face challenges regarding the operation of thousands of *mobile* satellites with continuously changing links to nearby edge servers. Serverless abstractions are suitable for hiding the underlying system and providing a development platform that is scalable, flexible, and elastic.

The paper makes the following contributions:

- A *virtual testbed tool* is designed to evaluate LEO edge platform abstractions.
- A *data management platform* for geo-distributed edge computing with application-controlled replica placement is proposed.
- A *computing platform* for the LEO-edge based on the FaaS paradigm is proposed.
- Possible LEO edge applications are discussed.

Virtual testbed tool:

Barrier to entry for testing real-world satellite systems (especially in scale) is high, possible infeasible. For this reason, simulations are used instead. Existing simulation software falls short in terms of satellite mobility and scalability of networks. The *Celestial* virtual testbed tool is proposed which combines high-performance LEO satellite network tooling with Firecracker microVMs. Evaluations show that it has scalability issues in the thousands of satellites.

Geo-distributed data store:

Basic functionality of edge networks is to provide low-latency, and high-bandwidth access to local data replicas (similar to a CDN). Managing data replication is challenging for edge applications, regardless of whether they run on terrestrial edge infrastructure, or distributed geo nodes.

A serverless geo-distributed data store is proposed -> The data store itself is ran by some infrastructure provider, applications only configure their replication policies. *FReD* implements this data replication mechanism via *keygroups*, applications need only specify at which location they want keygroup data replicated.

Serverless edge compute platform:

Edge server resources have higher limitations than the cloud. That's why resource allocation at the edge has requirements for more fine-grained and elastic sharing. The FaaS paradigm is a good fit for addressing these challenges due to its scale-to-zero characteristic.

Existing compute platforms (like Kubernetes) are heavyweight since they focus on cloud-scale deployments. More lightweight FaaS platforms like *tinyFaaS* could achieve a smaller footprint, increase throughput, and decrease request-response latency (most important for latency constrained edge applications).

Local edge databases are better suited than cloud-managed databases, however data replication is cumbersome. *Enoki* integrates *tinyFaaS* into *FReD* for a combined serverless platform, supporting stateful apps.

Future work: This approach is also usable for LEO edge networks. Using *virtual stationarity* (migrating compute services to counteract the mobility of satellites), edge services can remain in proximity to their clients without explicit support by the application.

## Methodology

%% Architecture of the solution. %%

## Implementation

%% Actual implementation of the solution. %%

## Evaluation

%% The evaluation of the solution. %%

---

## Links

%% Links to related literature. %%