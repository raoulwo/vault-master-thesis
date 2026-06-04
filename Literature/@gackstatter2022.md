---
author: Gackstatter P., Frangoudis P.A., Dustdar S.
year: 2022
tags:
  - webassembly
  - edge-computing
  - serverless
pdf_link: "[[Pushing_Serverless_to_the_Edge_with_WebAssembly_Runtimes.pdf|Open PDF]]"
---
# Pushing Serverless to the Edge with WebAssembly Runtimes

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper presents a prototypical runtime environment for WebAssembly execution in Apache OpenWhisk (WOW) that reduces cold-start latency by up to 99.5%, can improve on memory consumption by more than 5x, and increases function execution throughput by up to 4.2x on low-end edge computing hardware.

## Related Work

%% Existing state-of-the-art research. %%

Existing approaches to combat cold-start latency:

- Pre-warming
- Pre-creation
- Prediction
- Client-centric approaches

WebAssembly in serverless computing:

- Execution in Node.js
- Cloudflare Workers
- Fastly's Lucet
- FAASM
- Sledge

## Contributions

%% Research gap and novel contributions. %%

Serverless computing is a popular cloud computing model that can abstract away infrastructure management by enabling developers to write functions that can auto-scale, while only paying for the used compute time. This model is ideal for *unpredictable* and *bursty* workloads, it however suffers from cold-start latencies in the range of *hundreds of milliseconds*. The reason for this are the underlying runtime environments used for serverless functions: most often virtual machines or containerization technologies like Docker. Especially in context of latency critical applications, a lightweight alternative to these existing runtime environments is needed. For this purpose, the paper examines *WebAssembly* for its suitability as a serverless container runtime.

The paper aims to answer the following research question: *What is the viability of WebAssembly in serverless edge computing and what are the performance benefits it can bring about?*

The paper presents a prototypical runtime environment for WebAssembly execution in Apache OpenWhisk (WOW) that reduces cold-start latency by up to 99.5%, can improve on memory consumption by more than 5x, and increases function execution throughput by up to 4.2x on low-end edge computing hardware.

The following three contributions are made:

- An execution environment for WebAssembly serverless functions is designed.
- The prototypical WebAssembly runtime environment WOW is presented.
- Extensive experiments on WOW are performed.

## Methodology

%% Architecture of the solution. %%

### Architecture of a Wasm-Based FaaS Platform

Here are some design requirements for a Wasm runtime environment to be a viable alternative to existing runtimes such as Docker:

- Programming-language agnostic runtime
- Cheap sandboxing to provide multi-tenancy
- Easy integration into existing serverless frameworks
- As close to native speed as possible

Here are some popular open-source serverless frameworks:

- Open-FaaS
- Apache OpenWhisk
- OpenLambda

These serverless frameworks generally follow the same architecture:

- Users interact with the system via a single entry-point, the *API Gateway*.
- A *Controller* implements logic for resource allocation, authorization and scheduling.
- Users manage serverless functions via *CRUD*.
- A load balancer selects one of multiple *Invokers* to execute a function on a host if invoked.
- Each host runs a *sandboxing execution engine* which runs the functions in an isolated manner.
- Multiple programming languages for writing serverless functions are supported.

To support integrate different runtimes, a common protocol is needed so that the *Controller* can communicate with each of them. For this reason, a runtime *implements* and *exposes* a number of endpoints:

- *Instantiation endpoint* creates a new container and returns its address.
- *Initialization endpoint* prepares the function for execution.
- *Execution endpoint* invokes the function.

The paper introduces a *lightweight runtime management layer* that manages the execution of Wasm functions. It consists of the following components:

- *Executor*. Is in charge of the actual Wasm function execution on the underlying host.
- *Invoker*. Manages the lifecycle of Wasm functions and interacts with the *Executor*.
- *Wasm Module*. Is the actual Wasm code of the serverless function. It can be compiled *ahead-of-time* (AoT) or *just-in-time* (JIT).


## Implementation

%% Actual implementation of the solution. %%

For the impementation of the serverless function runtime environment the following technologies need to be selected:

- *Serverless platform*: Production-ready, commercial use, suitable for edge, extensibility
- *WebAssembly runtime*: AoT/JIT compilation support, compiler support

For the serverless platform Apache OpenWhisk was chosen, for WebAssembly runtimes Wasmtime, Wasmer, and WebAssembly Micro Runtime (wamr) where selected.

To interact with OpenWhisk more easily, the authors implemented their own *Executor* from scratch in Rust. Its architecture is described in more detail in the paper.

## Evaluation

%% The evaluation of the solution. %%

Experiments for both *CPU-bound* and *I/O-bound* workloads where performed. The metrics measured where latency and cold-start time (methodology of measurement explained in detail).

### Results

- The WebAssembly runtimes have around 0.5% of the cold-start time of Docker.
- The average throughput gain of Wasm executors on a Raspberry Pi are from 2.4-4.2x.
- The average throughput gain of Wasm executors on the server are from 1.6-3.0x.
- The memory consumption gain of Wasm executors over Docker per warm container range from 3.5-7.6x.

---

## Links

%% Links to related literature. %%