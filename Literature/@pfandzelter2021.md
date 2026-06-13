---
author: Pfandzelter T. and Hasenburg J. and Bermbach D.
year: 2021
tags:
  - orbital-edge-computing
  - leo-satellites
pdf_link: "[[Towards_a_Computing_Platform_for_the_LEO_Edge.pdf|Open PDF]]"
---
# Towards a Computing Platform for the LEO Edge

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper describes the characteristics of the LEO edge, derives requirements for implementing LEO edge applications and compares three organization paradigms for applications (OPAs) for how suitable they are for meeting the identified requirements: i) virtual machines, ii) containers, and iii) serverless functions.

## Thoughts

- High availability, thus resilience and reliability are a requirement -> abstracting away underlying heterogeneous systems for better fault-tolerance
- Isolation, elastic scalability, and flexible deployment
- Centralized orchestration might hinder workflows -> decentralized approach?
- Migrations for overcoming stationarity -> migrating VM, container, serverless function to another node; how are stateful systems handled here?
- Is state managed at the edge or cloud? In any case replication of stateful applications seems to be difficult compared to stateless systems.

## Related Work

%% Existing state-of-the-art research. %%

- Bhosale, V., Bhardwaj, K. and Gavrilovska, A., 2020. Toward Loosely Coupled Orchestration for the {LEO} Satellite Edge. In 3rd USENIX Workshop on Hot Topics in Edge Computing (HotEdge 20).
- Bhattacherjee, D., Kassing, S., Licciardello, M. and Singla, A., 2020, November. In-orbit computing: An outlandish thought experiment?. In Proceedings of the 19th ACM Workshop on Hot Topics in Networks (pp. 197-204).
- Pfandzelter, T. and Bermbach, D., 2021, May. Edge (of the earth) replication: Optimizing content delivery in large LEO satellite communication networks. In 2021 IEEE/ACM 21st International Symposium on Cluster, Cloud and Internet Computing (CCGrid) (pp. 565-575). IEEE.

## Contributions

%% Research gap and novel contributions. %%

- Description of characteristics unique to the LEO edge
- Requirements for implementations of LEO edge applications
- Comparison of three OPAs in context of LEO edge application requirements

### LEO Edge Characteristics

- *C1: Mobile Server Infrastructure*. LEO satellites orbit the earth at high speeds. For example, a satellite at altitude of 550km must maintain a speed of 27000km/h to maintain its orbit. This means for the static ground equipment that they must **frequently change their communication partner**.
- *C2: Same-model Servers*. Satellites in a constellation eventually cover each part of the earth. Thus, **using different kinds of satellites for different regions is not possible**.
- *C3: Homogeneously Distributed Servers*. Satellites are homogeneously distributed across the globe, for this reason **each ground station has access to more or less the same amount of equally equipped satellites at all times**.
- *C4: Heterogeneous Demand*. **Demand is not distributed equally across the globe**. Urban areas have higher client density which increases resource demand compared to rural areas or oceans.
- *C5: Limited Compute Capabilities*. Energy consumption and heat generation must be kept low for economical reasons. For this reason, **a satellite servers' capabilities must be limited**.
- *C6: No Physical Access*. **Satellites placed in LEO cannot be accessed for maintenance**.
- *C7: Fixed Server Capabilities*. As a direct consequence of C6, **satellite servers cannot be upgraded**.
- *C8: Fixed Number of Servers*. The size of an LEO constellation cannot be changed easily. Because servers can only be placed on satellites part of an existing constellation, **horizontal scalability is limited**.

### LEO Edge Application Requirements

- *R1: Deployment Close to Clients*. If an application is deployed on a LEO edge platform, the platform should make sure that **individual services are placed on parts of infrastructure close to clients**.
- *R2: High Availability*. Similar to cloud applications, high availability is expected for LEO edge applications. Consequently, **LEO edge platforms must abstract from the widely distributed and heterogenous underlying infrastructure to provide fault-tolerance**.
- *R3: Isolation*. A single LEO edge platform can host services for multiple developers. This multi-tenancy should be hidden, as **services should not interfere with each other**. This comprises two dimensions: i) security, and ii) performance.
- *R4: Familiar, Open Technology Stack*. In order to transform cloud-client applications to cloud-edge-client applications, **the usage of widely used technologies is preferred for easier adoption of the platform**.
- *R5: Flexible Deployment*. **A similar deployment model to a cloud "pay-as-you-go" model will likely increase adoption of a LEO edge platform**.
- *R6: Elastic Scalability*. **A LEO edge platform should provide elastic scalability features to meet increasing demands**.

### Suitability of OPA Options to LEO Edge Scenarios

- *R1: Deployment Close to Clients*. Two deploy services close to clients, first the client locations need to be identified, and second, infrastructure in proximity to clients needs to be identified and allocated so that the service can be deployed. Scheduling on the LEO edge proves to be difficult. Centralized cluster orchestrators like Kubernetes prove to be a hindrance in this regard. Since satellites are mobile, applications need to move across LEO edge infrastructure to remain stationary. This is possible for all of virtual machines, containers and serverless functions. Migrating virtual machines incurs great overhead, especially with frequent migrations. Stateless serverless functions benefit from concurrency. The functions can be preemptively propagated to nearby satellites, alternatively they can be deployed on all satellites concurrently. They don't have to be instantiated at all times, instead they can start once the first request arrives (cold start). To manage state in stateless serverless environments, additional stateful services such as database systems are needed.
- *R2: High Availability*. To provide high availability in presence of server or satellite failure, services need to be migrated since they cannot easily be repaired or replaced. This is problematic for stateful VMs and containers, as only one instance can be live at one time. Stateless VMs and containers suffer from the same migration overheads as described above. Serverless OPA is a better fit for this requirements as backup functions can be deployed easily across satellites.
- *R3: Isolation*. All OPAs provide a level of security isolation, performance isolation is a more pressing issue. Resource allocation for VMs is static and required resources are quite large.
- For containers, resources can be limited to prevent leaking into other services. Resources for static functions can either be limited or statically allocated.
- *R4: Familiar, Open Technology Stack*. All three OPAs are widely used in cloud technology and reflect similar technology stacks.
- *R5: Flexible Deployment*. Shipping VMs, container images or serverless functions to satellite servers is simple. All OPAs can quickly start or stop services. The efficacy of pay-as-you-go models improves with granularity of allocated resources. For this reason, serverless functions outperform containers and VMs.
- *R6: Elastic Scalability*. Offloading is required to meet demand if one service exceeds its limits. Individual service requests or full services may need to be offloaded. For individual request offloading, stateless serverless functions are suitable.

Future work: 

- Planning and evaluation of a serverless platform for the LEO edge.
- *Stationarity* needs to be explored more; different *hand-off* models for satellites

## Methodology

%% Architecture of the solution. %%

## Implementation

%% Actual implementation of the solution. %%

## Evaluation

%% The evaluation of the solution. %%

---

## Links

%% Links to related topics. %%
