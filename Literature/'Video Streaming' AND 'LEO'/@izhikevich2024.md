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

The paper presents a global analysis of on-demand video streaming over LEO networks. Findings show that variable LEO conditions (i.e. throughput fluctuations and packet loss) leads to an increase in bitrate switches and rebuffers (video freezes mid-stream, loading spinner because pre-loaded data has run out).

## Introduction

- LEO requires video delivery to adapt frequently
- Using data from Netflix, they analyze over a million LEO households
- As of 2024 Starlink is the 29th ISP providing the most seconds of Netflix video to end users
- Perceptual video quality streaming over LEO is similar to non-LEO networks in well-served areas
	- ... and better than non-LEO networks in underserved areas
- Variable throughput and non-congestive packet loss lead to marginal increase in bitrate switches and rebuffers
- Existing congestion control and bitrate streaming algorithms are manipulated through simulations
	- ... and real A/B tests are deployed to millions of households to improve QoE
- Existing design principles *are not completely adaptable* to LEO, present undesirable tradeoffs
- Boosting Starlink performance metrics such as throughput is not enough for better QoE

## Quality of Experience

- Starlink suffers from higher rates of bitrate switches (60% more likely) -> prevents rebuffers, however negatively affects QoE
	- Likely due to lower, oscillating throughput
- >= 95% of Starlink throughput is lower than other ISPs
- 80% of observed throughput in Starlink varies with higher magnitude than other ISPs
- Rebuffers are 216% more likely to happen over Starlink than top 10 ISPs
- Rebuffers are 40% more likely to happen over Starlink than any non-Starlink network

## Improving Congestion Control for Video Delivery over LEO

- High RTT and retransmit rates contribute to low TCP congestion windows, and hence low throughput
- Modified congestion control -> single TCP connections insufficient for fully utilizing Starlink
	- Emulate the use of three concurrent connections
- Results:
	- Avg. throughput increased by 30-40%
	- Rebuffers/h decrease by 7%
	- Play delay decrease by 2%
	- Bitrate switches decrease by 5%
	- Retransmit rates increase by 300% (three connections instead of one)

## Adaptive Bitrate Streaming for Starlink

- Adaptive bitrate (ABR) goal is to maximize quality while minimizing rebuffers
	- Maximizing quality requires sending more data (meaning buffers fill up faster)
	- Minimizing rebuffers requires larger buffers
- Results
	- Throughput smoothing is more likely to overestimate throughput leading to increased rebuffers

---

## Links

%% Links to related literature. %%