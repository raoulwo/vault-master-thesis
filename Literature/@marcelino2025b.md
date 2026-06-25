---
author: Marcelino C., Gollhofer-Berger S., Pusztai T., Nastic S.
year: 2025
tags:
  - serverless
  - 3d-continuum
pdf_link: "[[Cosmos_A_Cost_Model_for_Serverless_Workflows_in_the_3D_Compute_Continuum.pdf|Open PDF]]"
---
# Cosmos: A Cost Model for Serverless Workflows in the 3D Compute Continuum

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper presents *Cosmos*, a cost- and a performance-cost-tradeoff model for serverless workflows that identifies key factors that affect cost changes across different workloads and cloud providers.

## Thoughts

%% Thoughts and questions about the paper. %%

- Paper says existing cost simulators aren't fine-grained enough to accurately simulate costs for serverless workflows in the 3D-continuum -> maybe that's something, however I'm not really excited by this direction

## Related Work

%% Existing state-of-the-art research. %%

Common approaches for serverless cost estimation:

- Predictions
- Simulations

Existing software simulation tools often lack fine-grained parameters to identify which aspects contribute to higher costs.

## Contributions

%% Research gap and novel contributions. %%

The contributions include:

- *Cosmos*: Cost and performance-cost-tradeoff model for serverless workflows
- A cost taxonomy that classifies the main cost drivers
- A case study on different commercial cloud providers

Research questions:

- How can serverless workflows in the 3D Continuum be optimized according to their workload characteristics?
- How can cost models accurately capture and predict serverless execution costs for heterogeneous environments?
- How to evaluate and benchmark cost drivers for serverless functions in the heterogeneous 3D Continuum?

Serverless main cost drivers in the 3D continuum:

- Invocation
- Compute
- State management
- Data transfer
- BaaS

Future work:

- Introduce optimization mechanisms and compare them to state-of-the-art approaches
- Develop an intelligent framework capable of predicting costs for individual functions and dynamically selecting the most suitable layer and provider

## Methodology

%% Architecture of the solution. %%

A case study is performed, where a simple serverless workflow is implemented and evaluated according to the designed cost model.

## Implementation

%% Actual implementation of the solution. %%

## Evaluation

%% The evaluation of the solution. %%

Results:

- Data transfer and state management costs significantly account for 75% costs of of I/O-intensive functions
- BaaS costs dominate CPU-intensive functions, up to 97%
- Processing data closer to the function reduces latency, increases costs by up to 35%

---

## Links

%% Links to related literature. %%