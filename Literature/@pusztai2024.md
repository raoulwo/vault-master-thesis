---
author: Pusztai T., Marcelino C., Nastic S.
year: 2024
tags:
  - serverless
  - leo-satellites
  - orbital-edge-computing
  - 3d-continuum
pdf_link: "[[HyperDrive__Scheduling_Serverless_Functions_in_the_Edge-Cloud-Space_3D_Continuum.pdf|Open PDF]]"
---
# HyperDrive: Scheduling Serverless Functions in the Edge-Cloud-Space 3D Continuum

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper presents *HyperDrive*, an SLO-aware scheduler for serverless functions specifically designed for the 3D continuum.

## Thoughts

%% Thoughts and questions about the paper. %%

- The scheduler as defined in the core platform layer is distributed for better scalability in context of 3D continuum, maybe that's an angle for my master thesis?
- The paper doesn't address virtual stationarity or migrations in any way, is this implied? It talks about data transfer, not really about serverless functions / services that need to be migrated
- Does the paper take number of migrations and service uptime into account? Downtime for migrations are not discussed I believe.
- A unique selling point of HyperDrive is the combination of space and terrestrial edge nodes into the 3D continuum, maybe that's an angle as well?

## Related Work

%% Existing state-of-the-art research. %%

Scheduling mechanisms are required for serverless platforms to address the environmental heterogeneity of the Edge-Cloud-Space Continuum. Most common scheduling strategies focus on meeting requirements for the following factors: *resources*, *network*, *application*, and *energy*.

- *Resource-aware schedulers*: They dynamically allocate functions considering resource properties such as CPU, GPU, and memory. For orbital edge computing, schedulers take resource and energy costs of transfering data between satellites or to ground stations into account, they fail to consider heat/temperature of satellites, potentially leading to overheating.
- *Network-aware schedulers*: They consider network characteristics like end-to-end latency, bandwidth availability, and link reliability. For OEC, the position of satellites is important information required to know about communication channels necessary for enabling serverless workflows.
- *Application and SLO-aware schedulers*: SLO-aware schedulers do not only consider resource properties, but also workload characteristics to guarantee workload requirements.
- *Energy-aware schedulers*: Are crucial for preventing battery-powered devices from running out of power. They ensure prolonged operation by optimizing energy usage. In context of OEC, energy-aware schedulers take energy needed for transferring data into account. Again, satellite position is not considered.

Emerging services in space:

- Constellation-as-a-Service
- Satellite-as-a-Service
- Payload-as-a-Service

## Contributions

%% Research gap and novel contributions. %%

The paper introduces *HyperDrive*, a serverless platform that creates a 3D continuum. It's part of the Polaris SIG (special interests group), an open-source platform for building unified and highly scalable public or private distributed Cloud and Edge systems.

The main contributions include:

- The architecture of the *HyperDrive* serverless platform
- The *HyperDrive* scheduling model
- Heuristic scheduling algorithms for the 3D continuum

Future work includes:

- Improveming the coordination between function execution and satellite orbits, enabling low-latency hand-offs
- Reduce scheduling complexity for large infrastructure sizes using hypergraphs (or precomputation and caching)
- Lightweight framework for serverless-native development of next generation Earth-orbit

## Methodology

%% Architecture of the solution. %%

Research challenges for scheduling:

- *Satellite availability*: Must factor in satellite mobility.
- *Power supply*: Satellite's power states must be considered.
- *Computing capacity and heat generation*: Prolonged high temperatures in combination with increased computation can lead to overheating.
- *Scalability*: Horizontal scaling of satellites is a significant challenge regarding auto-scaling.
- *SLO awareness*: SLOs must be met, this is already difficult on the ground and even more challenging in orbital edge systems.
- *Workflow dependencies*: In mixed environments, serverless functions can either be executed on satellite nodes or ground nodes, this factor needs to be taken into account by the scheduler.

### Architecture

The platform is composed of *6 layers*:

- Infrastructure layer
- Core platform layer
- Function runtime layer
- Function model
- Monitoring and tracing for real-time insights
- Stewardship layer


Core platform layer: The scheduler is distributed for better scalability, information needs to be synched between scheduler instances for this to be possible

Function runtime layer: WebAssembly is used for lightweight isolation, safety, and low-latency communication, short-term state management is also handled (maybe via mechanisms of the short-term state paper i read?)

The heuristic scheduling algorithm is based on a multi-criteria decision making (MCDM) approach:

- Vicinity selection
- Resource checking
- Network SLOs enforcement
- Temperature optimization
- Multi commit

## Implementation

%% Actual implementation of the solution. %%

Implement open-source as a Python prototype.

## Evaluation

%% The evaluation of the solution. %%

Experiments performed on StarryNet satellite constellation simulator.

Experiments examine two quality objectives:

- Scheduling quality (latency, temperature management)
- Scalability

Evaluations in context of a wildfire disaster response use case with Earth Observation data are performed:

- HyperDrive achieves 71% lower network latency compared to the next best baseline scheduler
- HyperDrive never exceeds the overheating temperature threshold, next best baseline exceeds it 33% of the cases
- HyperDrive scales linearly with infrastructure size

---

## Links

%% Links to related literature. %%