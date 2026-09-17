---
author: Jiang B. et al
year: 2010
tags:
  - live-migration
pdf_link: "[[Lightweight_Live_Migration_for_High_Availability_Cluster_Service.pdf|Open PDF]]"
---
# Lightweight Live Migration for High Availability Cluster Service

## TLDR

%% One-sentence summary of the core/claim contribution. %%

Paper proposes lightweight live migration (LLM) mechanism to reduce overhead and offer availability. The proposed LLM is evaluated and compared to Remus (state-of-the-art system).

%% Notes about the paper. %%

- Remus based on checkpoint/restore
	- Data migration at high frequency contributes to CPU and network delay bottlenecks
- Input replay is alternative approach, also suitable for high availability
- Paper combines whole system checkpointing with input replay
- Checkpointing most commonly used for fault tolerance
- Evaluation shows:
	- LLM performs better for high network loads
	- Remus performs better for high system loads
	- Reason: frequency of checkpoint transfers (duplicated transmission by Remus)
	- LLM has lower network delay overall, with exception of spikes at beginning and end of migration period

---

## Links

%% Links to related literature. %%