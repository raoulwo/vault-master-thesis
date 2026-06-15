---
author: Malazi H.T., Farahani R., Mohan N., Dustdar S.
year: 2026
tags:
  - serverless
  - 3d-continuum
pdf_link: "[[Orchestrating_Serverless_Applications_in_the_Edge-Cloud-Space_Continuum__What_Breaks_and_Whats_Next.pdf|Open PDF]]"
---
# Orchestrating Serverless Applications in the Edge-Cloud-Space Continuum: What Breaks and What’s Next?

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The article identifies ten broken assumptions in existing serverless orchestration and organizes them into three core challenges: 1) spatiotemporal execution over dynamic graphs, 2) constraint-aware function placement and scaling, and 3) correctness and progress under decentralized and delayed state.

## Thoughts

%% Thoughts and questions about the paper. %%

- It can't be assumed that worker handoff overhead is negligible, maybe focusing in on handoff mechanisms, virtual stationarity -> reducing number of handoffs, using Wasm as runtime (AoT precompiled) reduces overhead

## Related Work

%% Existing state-of-the-art research. %%

Three tightly coupled requirements exist for serverless orchestrators:

- Time-dependent execution
- State-dependent execution 
- Policy-constrained execution

Takeaway: The 3D continuum changes the nature of serverless orchestration. It introduces the following:

- Temporal constraints
- State locality challenges
- Policy coupling

## Contributions

%% Research gap and novel contributions. %%

Orchestration strategies valid in the cloud become infeasible in the 3D continuum. 10 *broken assumptions* are identified and mapped to 3 *core orchestration challenges*.

Challenges:

- Spatiotemporal execution over dynamic graphs
- Constraint-aware function placement and scaling
- Correctness and progress under decentralized and delayed state

### Spatiotemporal Execution over Dynamic Graphs

Broken assumptions:

- Instantaneous reachability implies execution feasibility
- Connectivity implies path feasibility
- Execution binding is stable

Research questions:

- How should an orchestrator place and schedule workflow stages over a time-varying space-ground graph such that computation and data exchange remain feasible throughout execution under SLO constraints?

Design implications:

- Contact window-aware scheduling
- Time-indexed planning
- Path-aware feasibility analysis

### Constraint-Aware Function Placement and Scaling

Broken assumptions:

- Elasticity implies available headroom
- Execution configuration is static

Research questions:

- How should an orchestrator control scaling and choose execution configurations so that workflow stages remain feasible under energy, onboard thermal, memory, and accelerator constraints?

Design implications:

- Energy- and thermal-aware admission control
- Time-aware execution planning
- Dynamic execution-mode selection
- Accelerator-aware placement

### Correctness and Progress under Decentralized and Delayed State

Broken assumptions:

- Remote state is close enough
- Workflow handoff overhead is negligible
- Cloud-style failure and recovery assumptions
- Centralized control assumes timely and consistent state
- Single-domain uniform policy and governance

Research questions:

- How can an orchestrator ensure correctness and forward progress by managing state, composition, recovery, and policy under intermittent connectivity and stale system state?

Design implications:

- State-aware placement and propagation
- Composition-aware optimization
- Partition-tolerant progress and recovery
- Decentralized control under stale telemetry
- Policy-aware orchestration across domains

### Future Work

Future work includes taking the proposed architecture and turning the design into portable orchestration interfaces.

## Methodology

%% Architecture of the solution. %%

The architecture separates concerns into three layers:

- Continuum control layer for global, constraint-aware decision making
	- Placement engine
	- Energy planning engine
	- Migration engine
	- Policy manager
	- Federation engine
- Service orchestration layer for workflow coordination under dynamic conditions
	- Workflow engine
	- Connectivity manager
	- State manager
	- Data locality manager
	- Trust manager
	- Event dispatcher
- Function execution layer for on-node execution of serverless functions
	- FaaS runtime
	- Execution controller
	- Resource abstraction
	- Telemetry agent

## Implementation

%% Actual implementation of the solution. %%

Challenges:

- Contact-window and path abstraction
- Feasibility-aware resource model
- Time-indexed orchestration APIs
- State and data locality primitives
- State telemetry and uncertainty handling
- Cross-domain policy and trust interface
- Failure semantics under intermittent connectivity

## Evaluation

%% The evaluation of the solution. %%

---

## Links

%% Links to related literature. %%