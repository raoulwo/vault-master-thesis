---
author: Marcelino C., Krennmair N., Pusztai T.W., Nastic S.
year: 2025
tags:
  - serverless
  - webassembly
  - benchmark
pdf_link: "[[Lumos__Performance_Characterization_of_WebAssembly_as_a_Serverless_Runtime_in_the_Edge-Cloud_Continuum.pdf|Open PDF]]"
---
# Lumos: Performance Characterization of WebAssembly as a Serverless Runtime in the Edge-Cloud Continuum

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The work presents *Lumos*, a performance model and benchmarking tool for characterizing serverless runtimes.

## Thoughts

- The paper differentiates between CPU-intensive and data-intensive work, what's the difference between data-intensive work and I/O-intensive work?
- Lumos makes it possible to evaluate diverse workload characteristics for serverless runtimes, what's missing in context of OEC?
	- Migrations or handoffs
	- Satellite position / Temperature
	- Distributed workflows including transmission latency?
- Impacting cold-start time: data I/O, auto-scaling, scheduling, device heterogeneity, network congestion
- Binary size (which is smaller for Wasm) directly impacts cold-start time
- Only WasmEdge as runtime is evaluated, the benchmarking is extensible to other Wasm runtimes as well however
- Wasm up to 30x smaller image size -> benefits limited network, for handoffs in OEC perhaps?
- Cold-start Wasm good, Warm-start Wasm 1.1x slower (maybe negligible) -> I/O and serialization overhead 2x (how to reduce this)
- Wasm performance degradation under high load -> For high concurrency, scalability, container-runtimes are preferred

## Related Work

%% Existing state-of-the-art research. %%

## Contributions

%% Research gap and novel contributions. %%

Performance characteristics in the edge-cloud continuum remain insufficiently understood. Existing performance characterization of Wasm includes:

- *Standalone runtime analysis*. Focuses on low-level Wasm runtime behavior, however fails to compare against state-of-the-art Docker runtime.
- *Serverless runtimes*. Cold-start latency, execution time and warm-start performance are measured, however I/O-serialization overhead is overlooked.

The work presents *Lumos*, a performance model and benchmarking tool for characterizing serverless runtimes.

The contributions are summarized as follows:

- *Lumos: A Performance Model for serverless functions in the edge-cloud continuum*.
- *A benchmarking tool for evaluating serverless runtimes*.
- *A performance characterization of WebAssembly as serverless runtime*.

Possible future work includes extending Lumos to enable performance characterization of serverless functions with *real-world traces* and in different environments, such as the 3D computing continuum that unifies edge-cloud and space.

## Methodology

%% Architecture of the solution. %%

### Research Challenges

The following research challenges of serverless function performance in the edge-cloud continuum are identified:

- *How do workload, system, and environmental factors influence the performance of serverless functions across the Edge-Cloud Continuum?* Environmental factors such as *device heterogeneity* and *network congestion* can cause increased execution queue and/or failures.
- *How do serverless runtimes impact function performance in the Edge-Cloud Continuum?* Since Wasm executes within a sandboxed environment, it relies on WASI to enable interactions with the underlying host. This leads to increased I/O overhead, especially in data-intensive applications. Interpreted Wasm-execution enables portability, has increased runtime overhead compared to JIT-compiled or AoT-compiled binaries.
- *How to characterize serverless function performance under different workload characteristics and heterogeneous environments, such as the Edge-Cloud Continuum?*  Evaluating serverless runtimes requires the ability to simulate concurrency, diverse workload characteristics, cold vs. warm execution, and BaaS access.

### Performance Model

A performance model is introduced that categorizes the main performance factors of serverless functions into *workload*, *system*, and *environment*.

#### Workload Performance Factors

- *Function Heterogeneity*.
- *Workflow Dependencies*.
- *Function State and BaaS*.
- *Serialization Overhead*.

#### System Performance Factors

- *Performance Isolation*.
- *Function Isolation*.
- *Function Invocation*.
- *Auto Scaling and Cold Starts*.

#### Environment Performance Factors

- *Nodes and Devices Capabilities*.
- *Network Variability*.
- *Devices Volatility*.

### Lumos Architecture Overview

The Lumos benchmarking tool is defined across three planes:

- *User plane*. Functions are prepared and reproducible traces are created.
- *Control plane*. Scheduling and routing are observed and control metadata is persisted.
- *Data plane*. State I/O is instrumented to capture compute and BaaS effects.

Lumos includes a set of representative serverless workloads to capture diverse performance behaviors.

Lumos is comprised of the following components:

- *Build and Deploy*
- *Trace Generator*
- *Observer*
- *Storage*
- *Telemetry*

The following performance metrics are provided by Lumos:

- *Total Function Execution Time*
- *I/O Latency*
- *Serialization*
- *Compute Time*
- *Image Size*
- *Cold-Start Time*
- *CPU Usage*
- *Memory Usage*

## Implementation

%% Actual implementation of the solution. %%

The system is implemented using a combination of Bash, Python, and Rust.

## Evaluation

%% The evaluation of the solution. %%

State-of-the-art containers and the *Wasm* runtime (interpreted mode and AoT compiled) are benchmarked. AoT-compiled Wasm images are shown to be *up to 30x smaller* and *decrease cold-start latency by up to 16%* compared to containers. Interpreted Wasm suffers 30-55x higher warm latency and 7-10x I/O-serialization overhead.

---

## Links

%% Links to related literature. %%