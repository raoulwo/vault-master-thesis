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

## Thoughts

- Wasm helps with cold-start latencies, with acceptable loss in warm-start performance. We need a use case in which cold-start latencies are the bottleneck, not the warm-start performance, else a hybrid approach might be more suitable.
- The serverless model is great for unpredictable and bursty workloads, however the cold-start latencies are challenging especially for latency-critical applications (computations are moved closer to the edge to achieve shorter latencies).
- The serverless model is also used for its pay-as-you-go model, auto-scaling behavior, and scale-to-zero principle. Question: Is the *scale-to-zero* principle *that* important for real-life use cases that we actually want to avoid prewarming or similar mechanisms to combat cold-start latencies?
- I think scale-to-zero and shorter cold-starts are for keeping costs low.
- Isolation in multi-tentant environments is required.
- Cold-start latencies are in the *hundreds of milliseconds*.
- Wasm is a promising solution for cold-start latencies, Wasm functions can be created/destroyed in *microseconds*, they are portable, multiple languages can be compiled to Wasm. A variety of runtimes exist for Wasm execution on the edge, Wasm supports near-native execution speeds
- Question: Standalone Wasm runtime vs. Integration into existing serverless platforms (if so: wasm-only or hybrid for better warm-start performance)?
- Serverless used for mixed workloads: CPU-bound and I/O-bound
- Interpreted Wasm is found to execute significantly slower than native code, JIT and AoT compilation are supported however for near-native execution.
- Wasm uses software-based fault isolation techniques to sandbox an executing Wasm module.
- Wasm interacts with the host system via the *WebAssembly System Interface* (WASI). Wasm modules cannot use syscalls directly, everything goes through WASI function calls (slower performance, better security and portability).
- Tradeoff: Shorter response times vs. cost and resource saving (think prewarming or keeping containers running) -> Reducing cold-start times would benefit both sides at once
- Wasm has characteristics suitable for serverless edge runtimes: programming language-agnostic, multi-tenancy support, portable (for heterogeneous edge hardware)
- The usual steps for code execution in serverless platforms are the following: 1) upload code to the platform, 2) inintialize the executing env, 3) run it -> note that Wasm has significantly slower execution speed if interpreted: JIT, AoT compilation helps with that; JIT compilation after uploading the code however is the greatest chunk of cold-start time, therefore even before the execution, AoT (pre)compilation of the Wasm module is preferred to prevent the largest cold-start delay due to compilation
- Selection of Wasm runtime is important, following criteria for selecting the right one exist: JIT, AoT compilation support, architecture support, ease of embedding, WASI standard compatibility
- Integration into existing OpenWhisk platform: Either implement compatible executor that still uses container-semantics for interfacing with Wasm modules, or integrate using Kubernetes
- This work does not address stateful function invocations, sharing state across functions
- No mention of serverless function execution for distributed networks (such as satellite networks) are made specifically; sharing state over multiple nodes; live-updating code (heterogeneous environment)
- Wasm is promising: Cold-starts are made cheap, C/C++ and Rust are supported well, Scale-to-zero (not fully realized, however getting way closer); limitations are CPU-bound workloads

## Related Work

%% Existing state-of-the-art research. %%

Existing approaches to combat cold-start latency:

- Pre-warming (problematic at the edge)
- Pre-creation
- Prediction
- Client-centric approaches

It's possible to combine AI-based prediction for keep-alive times with Wasm.

WebAssembly in serverless computing:

- Execution in Node.js (CPU-bound performance is bad because JIT)
- Cloudflare Workers (Avoids Node.js startup overhead, still needs to parse and compile code before execution)
- Fastly's Lucet (Similar approach used for AoT compilation, thus reaching good results)
- FAASM (Tackles a different problem: state-sharing across functions)
- Sledge (Focuses on single-host function execution)

## Contributions

%% Research gap and novel contributions. %%

Serverless computing is a popular cloud computing model that can abstract away infrastructure management by enabling developers to write functions that can auto-scale, while only paying for the used compute time. This model is ideal for *unpredictable* and *bursty* workloads, it however suffers from cold-start latencies in the range of *hundreds of milliseconds*. The reason for this are the underlying runtime environments used for serverless functions: most often virtual machines or containerization technologies like Docker. Especially in context of latency critical applications, a lightweight alternative to these existing runtime environments is needed. For this purpose, the paper examines *WebAssembly* for its suitability as a serverless container runtime.

The paper aims to answer the following research question: *What is the viability of WebAssembly in serverless edge computing and what are the performance benefits it can bring about?*

The paper presents a prototypical runtime environment for WebAssembly execution in Apache OpenWhisk (WOW) that reduces cold-start latency by up to 99.5%, can improve on memory consumption by more than 5x, and increases function execution throughput by up to 4.2x on low-end edge computing hardware.

The following three contributions are made:

- An execution environment for WebAssembly serverless functions is designed.
- The prototypical WebAssembly runtime environment WOW is presented.
- Extensive experiments on WOW are performed.

Limitations and open challenges:

- Wasm features (multi-threading, atomics...)
- Stateful functions (WOW is stateless, caching for stateful functions could be implemented)
- Cold-start latencies (Are still high sometimes, combination with complementary approaches would be useful)

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

Mixed workload evaluation:

Throughput (requests/s) increased for Wasm functions, this increase is higher for edge-constrained environments (tested with Raspberry PIs).

I/O-bound workload:

Throughput higher for Wasm functions, this is limited however by *Executor* implementation and number of concurrent threads that can be spawned.

CPU-bound workload:

Here, Docker runtime outperforms Wasm runtimes:

- 39% (wamr)
- 57% (wasmtime)
- 88% (wasmer)

NOTE: 88% (wasmer) can be noted as acceptible performance compared to Docker with better cold-start times.

Results:

- The WebAssembly runtimes have around 0.5% of the cold-start time of Docker.
- The average throughput gain of Wasm executors on a Raspberry Pi are from 2.4-4.2x.
- The average throughput gain of Wasm executors on the server are from 1.6-3.0x.
- The memory consumption gain of Wasm executors over Docker per warm container range from 3.5-7.6x.

---

## Links

%% Links to related literature. %%