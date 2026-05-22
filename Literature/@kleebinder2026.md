---
author: Kleebinder, D.
year: 2026
tags:
  - orbital-edge-computing
  - vector-symbolic-architecture
  - time-varying-graphs
pdf_link: "[[Semantic_Pulse__A_Neuro_Symbolic_Middleware_for_Selective_State_Hydration_on_the_Orbital_Edge.pdf|Open PDF]]"
---
# Semantic Pulse: A Neuro-Symbolic Middleware for Selective State Hydration on the Orbital Edge

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper proposes the concept of *Ambient Holographic State*, which aims to create an ambient (static and location-aware) global data field, that incorporates holographic data representations to
achieve increased fault tolerance, resiliency and robustness against packet loss.

## Related Work

%% Existing state-of-the-art research. %%

## Contributions

%% Research gap and novel contributions. %%

The paper aims to answer the following research questions:

- `RQ1` How must an ambient holographic middleware be architected to enable a decentralized ambient data field that balances spatial resolution with information persistence in the 3D continuum?
- `RQ2` How do orbital topology dynamics and gossip frequency influence the stochastic convergence of holographic state signals across a highly dynamic time-varying topology?
- `RQ3` To what extent can an ambient holographic representation maintain state consistency under high degrees of packet loss and node churn?
- `RQ4` How can the threshold `τ_prewarm` be dynamically tuned to balance the trade-off between hydration success rate and energy waste in power-constrained LEO nodes?

The paper describes the following limitations and trade-offs of the proposed solution:

- **Communication Overhead**: HDC trades resiliency for payload size through high-dimensional hypervectors. Evaluation must be performed to compare HPC approach with traditional approaches in terms of packet loss.
- **Decay Factor α**: The decay factor `α` determines the weight of old *Ambient Holographic State* when computing the current state.  Implications of different values for the decay factor must be studied.
- **Serverless Workflow Execution**: The execution of serverless workflows executed partially on the satellite edge must be studied with respect to the threshold `τ_prewarm` to prepare serverless execution. This approach must be compared to existing scheduling strategies.

## Methodology

%% Architecture of the solution. %%

*Ambient Holographic Events* and *Ambient Holographic State* are defined via hypervectors and their operations. *Hyperdimensional Computing* (HDC) is naturally resilient to bit-flips due to
its property of *smearing* information across a hypervector.

Using *cosine similarity* `δ` (or alternatively *Hamming distance*), satellites can update their local representation of *Ambient Holographic State* using a spatial- and time-aware query. Should
`δ` exceed the `τ_prewarm` threshold, serverless workflows can be prewarmed by the satellite.

*Vector Symbolic Architecture* requires discrete binary representation for efficient data transmission and binary operations. For this reason *Fractional Power Encoding* (FPE) is used to encode fractional numbers like latitude and longitude in a state hypervector.

## Implementation

%% Actual implementation of the solution. %%

## Evaluation

%% The evaluation of the solution. %%

---

## Links

%% Links to related topics. %%