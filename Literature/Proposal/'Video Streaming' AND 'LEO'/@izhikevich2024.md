---
author: Izhikevich L. et al
year: 2024
tags:
  - video-streaming
  - leo-satellites
pdf_link: "[[A_Global_Perspective_on_the_Past_Present_and_Future_of_Video_Streaming_over_Starlink.pdf|Open PDF]]"
---
# A Global Perspective on the Past, Present, and Future of Video Streaming over Starlink

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper presents a global analysis of on-demand video streaming over LEO networks.

## Thoughts

%% Thoughts and questions about the paper. %%

## Related Work

%% Existing state-of-the-art research. %%

## Contributions

%% Research gap and novel contributions. %%

Main contributions:

- Global analysis of on-demand video streaming over LEO networks
	- Insights:
		- LEO is a key network for video delivery
		- Starlink's global presence is unique compared to other ISPs:
			- Responsible for most unique countries streaming video content
		- Video quality over LEO is similar to non-LEO networks in well-served territories
		- Video quality over LEO is better than non-LEO networks in underserved territories
			- Challenge: Variable throughput and packet loss lead to marginal increase in bitrate switches and rebuffers
- Simulations are performed where congestion control and adaptive bitrate streaming algorithms algorithms are manipulated
- A/B tests are deployed
	- Insights:
		- Existing design principles not completely adaptable to LEO
		- Simply boosting metrics like throughput isn't enough for overall increase in QoE


Future work:

- Solutions must treat LEO-constellations as *consistently variable networks* that comes with constantly changing:
	- throughput
	- latency
	- packet loss
	- availability
- Generalizations are needed to move LEO-specific solutions to be applicable across multiple network types:
	- LEO
	- GEO
	- Mobile
	- Terrestrial

## Methodology

%% Architecture of the solution. %%

## Implementation

%% Actual implementation of the solution. %%

## Evaluation

%% The evaluation of the solution. %%

---

## Links

%% Links to related literature. %%