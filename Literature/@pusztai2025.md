---
author: Pusztai T. and Hisberger J. and Marcelino C. and Nastic S.
year: 2025
tags:
  - orbital-edge-computing
  - 3d-continuum
  - edge-cloud-continuum
  - leo-satellites
pdf_link: "[[Stardust__A_Scalable_and_Extensible_Simulator_for_the_3D_Continuum.pdf|Open PDF]]"
---
# Stardust: A Scalable and Extensible Simulator for the 3D Continuum

## TLDR

%% One-sentence summary of the core/claim contribution. %%

*Stardust*, a scalable and extensible simulator for the *3D continuum*, is presented in the paper.

## Thoughts

- Resource to task allocation is handled via Kubernetes.
- Future work involves implementing a lightweight emulation mode based on containerization -> maybe an alternative would be Wasm as runtime with scalability in mind

## Related Work

%% Existing state-of-the-art research. %%

%% NOTE: Introduction %%

Launching satellites is expensive, and computing capabilities anticipated for the near future aren't yet available in space. Therefore, evaluation of real satellite systems is not feasible, instead simulators or emulators must be used.

Existing simulators/emulators are the following:

- `Kassing, S., Bhattacherjee, D., Águas, A.B., Saethre, J.E. and Singla, A., 2020, October. Exploring the" Internet from space" with Hypatia. In Proceedings of the ACM Internet Measurement conference (pp. 214-229).`
- `Pfandzelter, T. and Bermbach, D., 2022, November. Celestial: Virtual software system testbeds for the leo edge. In Proceedings of the 23rd ACM/IFIP International Middleware Conference (pp. 69-81).`
- `Lai, Z., Li, H., Deng, Y., Wu, Q., Liu, J., Li, Y., Li, J., Liu, L., Liu, W. and Wu, J., 2023. {StarryNet}: Empowering researchers to evaluate futuristic integrated space and terrestrial networks. In 20th USENIX Symposium on Networked Systems Design and Implementation (NSDI 23) (pp. 1309-1324).`

The simulator *Hypatia* does is used for network simulation only and does not provide capabilities for software testing that requires the position of a node. Furthermore, the emulators *Celestial* and *StarryNet* execute microVMs or containers per node of the 3D continuum, thus limiting the maximum infrastructure size that can be evaluated.

%% NOTE: Related Work %%

Simulators for the 3D continuum can be divided into two categories:

- *Network-only simulators* - Focus on simulating the network of a satellite mega constellation and their connections to ground stations.
- *Emulators* - Simulate the network of a satellite constellation and provision containers or VMs for nodes to execute workloads.

%% NOTE: Paper mentions additional simulators/emulators here. %%

## Contributions

%% Research gap and novel contributions. %%

The papers' main contributions are:

1. *Stardust*, a scalable and extensible simulator for the 3D continuum. It enables experiments for evaluating networking and orchestration algorithms for mega constellations up to three times the size of the currently largest LEO constellation, with almost 7000 satellites on a single machine.
2. A *dynamic routing mechanism* that makes the ISL routing protocol and network path computation interchangeable.
3. *SimPlugin*, a plugin mechanism which serves as an integration point for custom logic that should be executed by the simulator.

These contributions alleviate shortcomings of existing simulators by making *Stardust* **scalable** and **extensible**.

Future work:

- Implement approximation of perturbed orbits
- Introduce emulator capabilities (allow choosing between simulator and emulator mode)
- Introduce lightweight container emulation mode in addition to 3D continuum emulation mode based on Kubernetes

## Methodology

%% Architecture of the solution. %%

The following requirements for a next-generation simulator for the 3D continuum are defined:

- *Simulate entire 3D continuum*. Edge, cloud, and LEO nodes must be supported.
- *Configurable simulation steps*. The amount of time elapsed between simulated steps must be configurable.
- *Extensibility*. The simulator must be extendable with custom logic performed at each simulator step.
- *Information availability*. Custom logic must have access to relevant data, including node positions, resources, network routes.
- *Choice between simulation and emulation*. Users must be able to choose between executing in simulation or emulation mode.

### Architecture

These are the core abstractions of the simulator:

- `Node` - Every node has computational capabilities, communication links to other nodes, and the ability to route traffic.
- `GroundStation` extends `Node` - Implements simplified movement along the latitude every 24 hours.
- `Satellite` extends `Node` - Calculates its position by computing an unperturbed orbit.
- `Computing` - Tracks available and used computing resources.
- `ComputingType` - Differentiates `Computing` between routing, scheduling, or other algorithms.
- `ILink` - Models a direct physical connection between two nodes.
- `IRouter` - Abstracts the proposed dynamic routing mechanism.

*SimPlugin* extensibility via `ISimPlugin` interface.

## Implementation

%% Actual implementation of the solution. %%

The simulator is implemented in C# using .NET 8.

## Evaluation

%% The evaluation of the solution. %%

There are three categories of experiments performed to validate the simulator:

- *Simulator performance with respect to the infrastructure*. Measure simulator performance (resource usage, end-to-end runtime) while increasing number of simulated satellites.
- *Scheduling performance with respect to the infrastructure*. Same as first, one workflow instance is scheduled every simulation step.
- *Scheduling performance with respect to the workload*. Scheduling time is measured while increasing the number of workflow instances per simulation step.

Experiment results:

- Shorter end-to-end runtime compared to *StarryNet* and *Celestial*, suggesting more efficient computation of simulation state.
- Performance of simulator state and deployment APIs scale almost linearly with infrastructure size and are not a bottleneck for *SimPlugin* plugins.
- The scheduling time scales linearly with number of workflow instances, showing that the simulator state API does not present any bottleneck.

---

## Links

%% Links to related topics. %%