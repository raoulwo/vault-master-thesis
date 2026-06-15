---
author: Marcelino C., Nastic S.
year: 2024
tags:
  - serverless
pdf_link: "[[Truffle__Efficient_Data_Passing_for_Data-Intensive_Serverless_Workflows_in_the_Edge-Cloud_Continuum.pdf|Open PDF]]"
---
# Truffle: Efficient Data Passing for Data-Intensive Serverless Workflows in the Edge-Cloud Continuum

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper introduces *Truffle*, an architecture that enables efficient inter-function data passing in the Edge-Cloud continuum by introducing mechanisms that separate computation and I/O.

## Thoughts

%% Thoughts and questions about the paper. %%

- Serverless functions lack direct addressability, one thing that's addressed by Marcelino's other paper regarding serverless agents
- The question is, how important is this for OEC specifically, are there any issues/challenges specific to that domain other than "regular" serverless constraints
- This work addresses data-passing as a bottleneck during serverless cold-start times and proposes an architecture for efficient inter-function data passing -> reduced e2e function execution times, maybe combine with Wasm runtimes for better performance overall.

## Related Work

%% Existing state-of-the-art research. %%

Stateless serverless design necessitates external services for data exchange, such as message brokers, key-value stores, and object storage. This leads to increased latency and end-to-end function execution time. Improvements for data-fetching are needed to improve the data passing bottleneck between serverless functions.

Data passing approaches:

- Remote storage services (key-value store, object storage, cache)
- Local storage (disk, shared memory, local cache)

Local storage approaches leverage data locality to reduce network overhead, they don't account for cold-start latency however.

Approaches that address cold-start latency:

- Caching: Full or partial function sandbox snapshots in memory for a certain time frame.
- Sandbox sharing: Multiple functions share the same sandbox, leads to single cold-start for multiple function invocations (what's with security though?).
- Sandbox techniques (tiny VM, Wasm VM) for minimal sandboxes.

However, existing approaches don't consider the data transfer of the function lifecycle which impacts cold start significantly.

## Contributions

%% Research gap and novel contributions. %%

Contributions are summarized as follows:

- *Truffle*: A model and architecture that separates computation and I/O to enable functions to execute tasks in parallel to the cold starts for optimized e2e execution time.
- *Smart Data Prefetch* (SDP): Is a function input data fetching mechanism that abstracts the retrieval of input data.
- *Cold Start Pass* (CSP): An inter-function data passing mechanism that optimizes data exchange between functions.

## Methodology

%% Architecture of the solution. %%

Components:

- Listener
- Ingress
- Data Engine
- Watcher
- Buffer

## Implementation

%% Actual implementation of the solution. %%

Part of the *Polaris SLO CLoud*, implemented in Go using Kubernetes for orchestration, based on Knative as serverless platform.

## Evaluation

%% The evaluation of the solution. %%

Truffle reduces the I/O latency impact on the total function execution time by up to 77%, improving the function execution time by up to 46% compared to the state-of-the-art data passing approaches. The profit gained from Truffle shows especially for applications with long cold start times >= 10s than one's with short cold start times <= 2s (30% increased benefit).

---

## Links

%% Links to related literature. %%