---
author: Huang Y. et al
year: 2025
tags:
  - vr/ar
  - quality-of-service
pdf_link: "[[Model_Between_Network_Quality_and_XR_Users_Experience_in_Terms_of_Video_Stalling_for_Cloud_Rendering_XR.pdf|Open PDF]]"
---
# Model Between Network Quality and XR User’s Experience in Terms of Video Stalling for Cloud Rendering XR

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper proposes a frame-level data transmission framework with two-layer mapping model.

## Thoughts

%% Thoughts and questions about the paper. %%

- Cloud rendering XR reduces local rendering, storage, and processing requirements
- Cloud rendering XR can be viewed as *video streaming with high network transmission requirements*
- *[[Video Stalling]]* is directly affected by network performance and directly impacts UX

## Related Work

%% Existing state-of-the-art research. %%

## Contributions

%% Research gap and novel contributions. %%

## Methodology

%% Architecture of the solution. %%

## Implementation

%% Actual implementation of the solution. %%

## Evaluation

%% The evaluation of the solution. %%

Re-transmission latency: 2.5ms
Encoding rate: 60 Mbps

| Packet Loss (%) | Stalling Probability (%) |
|-----------------|--------------------------|
| 0.25            | 0.06                     |
| 0.5             | 6.74                     |
| 0.75            | 50.8                     |
| 1               | 95                       |
| 1.25            | nearly 100               |
| 1.5             | nearly 100               |

- With packet loss >= 2%, probabilities of stalling almost 100%
- Packet loss of <1% is a typical network performance requirement
- In *high-capacity scenarios* and *weak coverage scenarios*, <1% packet loss is difficult to achieve

RESULT: Data packet loss < 1.4% to for cloud rendering VR UX without stalling or freezing

---

## Links

%% Links to related literature. %%

- [[Extended Reality (XR)]]