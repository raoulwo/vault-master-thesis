---
author: Kjorveziroski V., Filiposka S.
year: 2023
tags:
  - webassembly
  - serverless
  - kubernetes
pdf_link: "[[WebAssembly_Orchestration_in_the_Context_of_Serverless_Computing.pdf|Open PDF]]"
---
# WebAssembly Orchestration in the Context of Serverless Computing

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper presents a Kubernetes extension for orchestration of natively executed WebAssembly modules.

## Related Work

%% Existing state-of-the-art research. %%

%% Cites 21, 23 %%
WASM's inherent advantages such as portability, fast instantiation and security through isolation identified it as a potential runtime environment for execution of serverless functions.

%% Cites 35, 36, 37 %%
Existing works have focused on optimizing orchestration engines for running serverless workloads in resource-constrained environments.

## Contributions

%% Research gap and novel contributions. %%

Existing serverless runtimes (containerization, virtual machines) suffer from long cold-start times. A potential solution to this is the use of WebAssembly runtimes. Serverless WASM applications are made possible through the *WebAssembly System Interface* (WASI), which allows them to leverage underlying OS functionality, similar to the POSIX interface. Server-side WebAssembly runtimes already exist, the issue of WebAssembly module orchestration at scale has not been answered before however.

WASM does not offer complete feature parity to more mature execution environments which is expected to be resolved in the future (implementation of new proposals).

The papers' goal is to propose and evaluate a concrete approach with which WebAssembly modules can be orchestrated in serverless scenarios. These are the concrete contributions:

- Development of a Kubernetes *operator* to schedule and run WASM serverless functions.
- Extension of a WASM *software shim* for the *containerd* runtime to run WASM modules.
- Benchmarking of the proposed solution as compared to OpenFaaS runtime.
- Description of how Kubernetes orchestrator can be extended to support high resolution metrics.

WASM has a great potential to be used as a runtime environment for *mulit-tenant serverless functions* which are frequently invoked, require elasticity, and are not computationally intensive.

## Methodology

%% Architecture of the solution. %%

### Extending Container Runtimes

The WASM runtime environment *Spin* (based on *Wasmtime*) was selected. It supports multiple programming languages including *Go* and *Rust*. It supports remote invocation of WASM modules via HTTP and Redis triggers. *containerd* is used as an interface for the *Spin* runtime. For this reason the distribution medium of WASM modules is via *Open Container Initiative* (OCI) images. These images contain the WASM binaries as well as a *Spin file* describing the runtime behavior.

### WebAssembly Support in Kubernetes

A dedicated Kubernetes WASM operator was implemented. Such a Kubernetes operator is a third-party component which has access to the Kubernetes API server and can monitor the occurrence of certain events which are of interest, and can react based on those events.

## Implementation

%% Actual implementation of the solution. %%

## Evaluation

%% The evaluation of the solution. %%

An existing set of serverless benchmarking functions was extended to evaluate the performance of the proposed solution. The benchmarking test-suite is comprised of 5 microbenchmarks and 4 real-world scenarios. 4 different execution scenarios were defined for the evaluated environments.

OpenFaaS beats WASM in 6/9 function executions when executed in a serial manner.

WASM function instantiation is faster across all benchmarks.

The results show that OpenFaaS (based on containers) offers better concurrent execution performance compared to WASM when using already warm container instances. however, WASM offers true *scale to zero* behavior without cold start delay.

OpenFaaS images are an order of magnitude larger than WASM counterparts.

---

## Links

%% Links to related literature. %%