---
author: Gundall M. et al
year: 2022
tags:
  - live-migration
pdf_link: "[[Downtime_Optimized_Live_Migration_of_Industrial_Real-Time_Control_Services.pdf|Open PDF]]"
---
# Downtime Optimized Live Migration of Industrial Real-Time Control Services

## TLDR

%% One-sentence summary of the core/claim contribution. %%

A concept is proposed that builds on existing migration approaches that aims at minimizing service downtime for industrial real-time applications.

%% Notes about the paper. %%

## Contributions

- Analysis of existing live migration approaches for application in industrial applications
- Proposal of downtime optimized live migration concept

## Migration Goals

- Efficiency and energy consumption
- Hardware replacement
- Availability and resilience
- Mobility
- Reconfiguration and updates

## Metrics

- Process downtime
- Migration time
- Transmitted data volume
- Resource load

## Existing Approaches

- Checkpoint/Restore (C/R)
	 - Inter-copy C/R
	 - Pre-copy C/R
	 - Post-copy C/R
	 - Hybrid C/R
	 - NOTE: One process active at a time
- Parallel post migration (PPM)
	- NOTE: Process is already active at target and gets supplied with data

### Analysis

- Goal of PPM is to reduce process downtime
- Suitable for live migration of industrial control services

## Novel Concept

- Source host (SRC) and destination host (DST)
	- Migration controller
	- Network controller
	- Control containers
- Cyber-physical system (CPS) sends data to control container of source host

- Live migration triggered
- Connection between source and destination established
- Packet buffer started on destination
- All packets addressed from CPS to source are copied, timestamped, redirected to DST
- DST caches packets in its buffer

## Hypotheses

- Paper has similar approach to [[@govindaraj2018]] where packets sent to one host are forwarded and buffered in another
	- Here, client (CPS) sends to SRC and it forwards to DST where they're buffered
	- [[@govindaraj2018]]: client sends to DST, it buffers them and it forwards to SRC
- Feel like for this approach, the same assumptions as in [[@govindaraj2018]] break in space:
	- Can't just store packets for migration in a buffer if transmission latency of GS is shorter than they can be processed, buffered and further transmitted on SRC node

---

## Links

%% Links to related literature. %%
