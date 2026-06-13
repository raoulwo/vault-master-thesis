---
author: Marcelino C., Shahhoud J., Nastic S.
year: 2024
tags:
  - serverless
  - webassembly
  - edge-computing
pdf_link: "[[GoldFish__Serverless_Actors_with_Short-Term_Memory_State_for_the_Edge-Cloud_Continuum.pdf|Open PDF]]"
---
# GoldFish: Serverless Actors with Short-Term Memory State for the Edge-Cloud Continuum

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper introduces a serverless lifecycle model that allows *short-term stateful actors*, enabling actors to maintain their state between executions.

## Thoughts

%% Thoughts and questions about the paper. %%

- Serverless functions require external services for state management (KVS, object storage, message brokers) -> Increase latency
- Serverless actors have emerged to solve this issue
- Immediate question: leverage the actor model for state-management in context of orbital computing -> Can this work in tandem with migrations for virtual stationarity?
- Custom sandboxes seem a promising solution for stateful management, however inoperable with existing FaaS platforms -> Standalone platform with lightweight footprint?
- This short-term state is only local -> What's the relevance for distributed satellite systems; shouldn't distributed stores be more important, or is it a combination of both?
- The relevance of Wasm in this paper is as the mechanism used for lightweight sandboxed isolation.

## Related Work

%% Existing state-of-the-art research. %%

*Actors* are isolated entities that can:

- Create other actors
- Communicate with other actors
- Influence the processing or state for the next received message

*Serverless functions* are:

- Stateless
- Non-addressable
- Event-triggered

To enable stateful serverless actors, two approaches are used:

- Lifecycle
- Event-trigger invocation

Existing approaches that enable stateful serverless functions are:

- *Programming models*. These models abstract function state handling from the developer by leveraging external services. Latency overhead might be introduced.
- *Sidecars*. These systems act as proxies and manage state interactions transparently, however consume additional CPU and memory resources (challenging at edge)
- *Custom sandboxes*. Ensure that functions can access and modify shared states in a controlled manner, providing isolation (important for multi-tenancy).

Related works concerning the *Serverless Actor Model* and *Stateful Serverless* exist.

Serverless Actor Model: Existing approaches are not interoperable with existing serverless platforms based on Docker runtimes.

Stateful Serverless:

Faasm is not interoperable with OCI specs, however introduces a Wasm-based runtime for serverless functions

Motivation:

By softening the statelessness requirement of serverless functions state can be managed while keeping scale-to-zero properties and still reducing additional network overhead by communication with external storage services. This tradeoff enhances performance without compromising the innate benefits of serverless computing.

## Contributions

%% Research gap and novel contributions. %%

The paper presents *GoldFish*, a lightweight Wasm short-term stateful serverless actor platform which provides a serverless actor lifecycle and invocation model. Wasm is leveraged for lightweight sandbox isolation.

The contributions of the paper include:

- *LCM*: A novel *Serverless Lifecycle Model* that natively executes functions as actors. Supports maintaining short-term state -> Not necessary to spawn multiple functions for multiple requests
- *SIM*: A novel *Serverless Invocation Model* that allows actors to influence processing future messages (discard, queue).
- *GoldFish*: A *WebAssembly Serverless Actor Platform* that provides a lightweight isolation. A dedicated message middleware is leveraged for communication.

Future works include:

- Expanding *GoldFish* into the 3D Edge Cloud Space Continuum by integration orbital edge computing requirements (satellite positioning).
- Implement a smart and serialization-free actor state.
- Integration into ML pipelines to facilitate data-intensive workloads.

Research challenges:

The following research challenges are identified:

- How to enable short-term stateful Serverless actors in the Edge-Cloud Continuum while preserving Serverless characteristics such as scale-to-zero?
- How can we enable direct communication between actors while allowing them to influence the processing of future messages?
- How to provide lightweight isolation while enabling the full potential of Serverless actors in the Edge-Cloud Continuum?

## Methodology

%% Architecture of the solution. %%

Serverless Lifecycle Model (LCM):

- Actor consists of the following components: Channel, Wasm Host Interface, Handler
- LCM consists of multiple phases (CREATED, SUSPENDED, ERROR, RUNNING, COMPLETED, TERMINATED)

Serverless Invocation Model (SIM):

- An actor handles requests sequentially (like Elixir/Erlang) -> Concurrency is enabled via multiple agents

Architecture Overview - GoldFish consists of the following components:

- Middleware
- Registry
- Buffer
- Actor Dispatcher

## Implementation

%% Actual implementation of the solution. %%

- Deployed on Polaris SLO Cloud
- WasmEdge runtime
- Docker used to run actors
- Rust wasmedge-sdk for host functions
- Middleware communicates with Redis via GRPC
- OCI Bundle encapsulates actors
- Dispatchers implemented in Rust

## Evaluation

%% The evaluation of the solution. %%

- GoldFish optimizes the data exchange latency by up to 92%
- GoldFish increases the throughput by up to 10x compared with OpenFaaS and Spin

---

## Links

%% Links to related literature. %%