---
author: Tripathi R.D., Lyu M., Sivaraman V.
year: 2024
tags:
  - vr/ar
  - quality-of-service
pdf_link: "[[Assessing_the_Impact_of_Network_Quality-of-Service_on_Metaverse_Virtual_Reality_User_Experience.pdf|Open PDF]]"
---
# Assessing the Impact of Network Quality-of-Service on Metaverse Virtual Reality User Experience

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper empirically measures user QoE of metaverse VR caused by network QoS.

## Thoughts

%% Thoughts and questions about the paper. %%

## Related Work

%% Existing state-of-the-art research. %%

Motivation:

- Degraded network [[Quality of Service (QoS)]] can lead to suboptimal [[Quality of Experience (QoE)]]
- Poor UX leads to feeling physically unwell: dizziness and nausea

## Contributions

%% Research gap and novel contributions. %%

Main contributions:

- Empirical evaluation of perceived QoE within metaverse VR impacted by degraded network QoS.
- Three QoE metrics are formally defined:
	- Freeze
	- Content Load
	- Control Response Time
- UX measured through conditions of three QoS network parameters:
	- Bandwidth
	- Latency
	- Packet Loss

## Methodology

%% Architecture of the solution. %%

## Implementation

%% Actual implementation of the solution. %%

- *Linux traffic control (TC)* functions are used to adjust available QoS parameters

## Evaluation

%% The evaluation of the solution. %%

NOTE: In the *metaverse*, we differentiate between *public* and *private* rooms

Impact of bandwidth on QoE:

- Public Social Hub: 1 Mbps - 700 kbps for excellent UX
- Private Events: 1.5 Mbps - 1.3 Mbps for excellent UX

Impact of packet loss on QoE:

- Public Social Hub: <= 1% packet loss for excellent UX
- Private Events: <= 1% packet loss for excellent UX

Impact of latency on QoE:

- Public Social Hub: 100ms - 300ms latency for excellent UX
	- Is less detrimental than gaming, thus higher latency
- Private Events: ~0ms latency for excellent UX

---

## Links

%% Links to related literature. %%