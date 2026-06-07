---
author: Kjorvesiroski V., Filiposka S.
year: 2023
tags:
  - webassembly
  - serverless
pdf_link: "[[WebAssembly_as_an_Enabler_for_Next_Generation_Serverless_Computing.pdf|Open PDF]]"
---
# WebAssembly as an Enabler for Next Generation Serverless Computing

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper proposed a novel benchmarking suite containing 13 different functions, compatible with WebAssembly.

## Related Work

%% Existing state-of-the-art research. %%

## Contributions

%% Research gap and novel contributions. %%

The paper proposed a novel benchmarking suite containing 13 different functions, compatible with WebAssembly.

The main contributions of the work are:

- Description of a strategy for integrating WebAssembly runtimes with container runtimes.
- Creation of WebAssembly serverless benchmarking functions.
- Comparison of how cold start times impact execution performance.
- Making results publicly available.

To the knowledge of the authors, it's the first work comparing performance of WASM runtimes through integration with a container runtime.

WASM doesn't shine in sustained computation because as of time writing the paper, it doesn't support multi-threading.

WASM has clear advantages in terms of cold-start times and artifact (image) size.

WASM cannot be seen as a complete replacement for container runtimes, as they provide better performance for sustained resource-intensive computations. By integrating WASM into existing container runtimes it's possible to achieve the best of both worlds.

## Methodology

%% Architecture of the solution. %%

## Implementation

%% Actual implementation of the solution. %%

## Evaluation

%% The evaluation of the solution. %%

Results show that WASM runtimes achieve better execution times in 10/13 tests when comparing WASM AoT execution with *distroless* and *debian:bullseye* container images.

Wasmtime performs the best out of all runtimes, second is WasmEdge, third being Wasmer.

---

## Links

%% Links to related literature. %%