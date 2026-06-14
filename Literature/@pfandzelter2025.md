---
author: Pfandzelter T. et al
year: 2025
tags:
  - serverless
  - orbital-edge-computing
pdf_link: "[[Trabant__A_Serverless_Architecture_for_Multi-Tenant_Orbital_Edge_Computing.pdf|Open PDF]]"
---
# @pfandzelter2025

## TLDR

%% One-sentence summary of the core/claim contribution. %%

*Trabant*, a serverless architecture for shared satellite platforms, leveraging the FaaS paradigm and time-shifted computing is proposed.

## Thoughts

%% Thoughts and questions about the paper. %%

- Basically, Satellite-as-a-Service (or Constellation-as-a-Service) is proposed -> Create a serverless platform based on Wasm for secure isolation in multi-tenant context with good cold-start times?
- This paper focuses on a multi-tenant serverless platform, specifically for Earth observation. Are there any other use cases one might find (or rather for general applications)?
- Multi-tenancy as a way of better utilizing satellites even though they are constantly handing off data/services <- Maybe that's an idea?
- Trust model in multi-tenant sandboxed environments -> establish isolated security boundaries for clients
- Trabant utilizes docker containers for isolated sandboxes -> Wasm for efficiency 

## Related Work

%% Existing state-of-the-art research. %%

## Contributions

%% Research gap and novel contributions. %%

Shared Earth observation satellites are proposed, owned and operated by a satellite operator, but simultaneously used by different tenants to flexibly execute their Earth observation missions.

Key insights for architecture:

- Earth observation satellites are often unused -> resources are not fully utilized
- Mission planning and actual operation mismatch
- OEC provides a flexible platform for sharing resources

FaaS is suitable for overcoming the challenges of shared satellite platforms:

- High level of abstraction allow scheduling/interruption of OEC function calls
- Abstraction reduces development costs for clients, no infrastructure management
- Shared runtimes reduce size of deployment packages
- FaaS execution model (event-triggered) aligns with continuous Earth observation events

Future work:

- Deployment optimization
- Real-world experiments
- Multiple-service assignments
- Isolation
- Resource pricing
- Base resource consumption (hibernation)

## Methodology

%% Architecture of the solution. %%

Limitations of non-multi-tenant satellite systems:

- Mission cost -> Shared platforms could distribute these costs between tenants
- Mission lead times -> Decoupling software from hardware development can reduce lead times
- Mission/hardware/software alignment -> Running multiple apps on a shared platform allows flexible (re)allocation of resources
- Development cost for constrained software -> A shared satellite platform can provide a software platform tailored to satellite resource constraints, providing an abstraction for clients
- LEO satellites spend little time over their RoIs -> When serving multiple tenants means that a satellite can provide more value, even as it moves away from one tenants' geo-location
- Environmental impact -> Even though a shared platform will be larger than application-specific platforms, reducing the overall number and mass of satellites in Earth orbit can reduce the environmental impact

## Implementation

%% Actual implementation of the solution. %%

Prototypical proof-of-concept implemented in Go, loosely based on tinyFaaS

## Evaluation

%% The evaluation of the solution. %%

The findings suggest that Trabant reduces mission planning overheads, offering a scalable and efficient platform for Earth observation missions.

---

## Links

%% Links to related literature. %%