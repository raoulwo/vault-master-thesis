---
author: Reisecker M. et al
year: 2025
tags:
  - serverless
  - leo-satellites
  - orbital-edge-computing
  - 3d-continuum
pdf_link: "[[Gaia__Hybrid_Hardware_Acceleration_for_Serverless_AI_in_the_3D_Compute_Continuum.pdf|Open PDF]]"
---
# Gaia: Hybrid Hardware Acceleration for Serverless AI in the 3D Compute Continuum

## TLDR

%% One-sentence summary of the core/claim contribution. %%

*Gaia* is presented, a GPU-as-a-service model and architecture that makes hardware acceleration a platform concern.

## Thoughts

%% Thoughts and questions about the paper. %%

- Seems to be the work with least overlap with what Wasm brings to the table, don't think it's of relevance if I want to stay in the Wasm track

## Related Work

%% Existing state-of-the-art research. %%

Hardware acceleration in serverless platforms remains challenging. Functions are typically deployed with knowledge of the underlying hardware, developers must manually decide between running functions on CPU or GPU. In context of 3D continuum, hardware acceleration becomes critical. Due to the dynamic nature of satellite connectivity, static assignments for GPU or CPU are insufficient. Overprovisioning of GPU resources can furthermore lead to performance degradation under workflow drift.

Existing hardware acceleration approaches:

- Letting the developer decide between CPU and GPU
- Dynamic approaches that migrate to GPUs based on current function performance. These *one-off* device choices are not revisited as load changes, can lead to performance degradation due to over-provisioning.

## Contributions

%% Research gap and novel contributions. %%

*Gaia* enables the platform to automatically identify whether a function requires CPU or GPU execution and to adjust execution dynamically at runtime, while ensuring SLO compliance.

Contributions include:

- *Gaia*: Hybrid acceleration model and architecture
- *Execution Mode Identifier*: Identifies functions with GPU-affine operations at runtime
- *Dynamic Function Runtime Reconfiguration*: Dynamically adjusts hardware allocation during execution

Future work:

- Incorporate predictive, learning-based policies and workflow-level objectives for runtime decision making
- Integrate 3D continuum dynamics, including LEO handovers, power and thermal constraints, intermittent links, into our placement and mode decisions

## Methodology

%% Architecture of the solution. %%

Research Challenges:

- How can serverless AI functions maintain optimal execution performance under resource constraints and dynamic conditions of the 3D Compute Continuum?
- How to seamlessly identify functions’ hardware requirements, such as GPU?
- How to dynamically change function runtime between CPU and GPU to balance performance and cost SLOs?

Design principles:

- Hardware-agnostic
- Zero developer friction
- Intelligent start and dynamic switching
- Observability by design

Components:

- Code analyzer
- Build and deploy
- Controller
- Function runtime manager
- Function runtime (container-based)
- Telemetry

## Implementation

%% Actual implementation of the solution. %%

- Dynamic runtime adaptation: Python, Static code analyzer: Go
- Knative serverless platform and Kubernetes are used

## Evaluation

%% The evaluation of the solution. %%

Across matrix multiplication, image classification, LLM inference, and idle baseline, Gaia promotes accelerable workloads and demotes non-accelerable ones -> E2E latency is reduced by up to 95%.

---

## Links

%% Links to related literature. %%