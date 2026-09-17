---
author: Ahmadpanah S.H. et al
year: 2025
tags:
  - live-migration
  - containers
pdf_link: "[[FlexiMigrate__Enhancing_Live_Container_Migration_in_Heterogeneous_Computing_Environments.pdf|Open PDF]]"
---
# FlexiMigrate: Enhancing Live Container Migration in Heterogeneous Computing Environments

## TLDR

%% One-sentence summary of the core/claim contribution. %%

Paper introduces *FlexiMigrate*, a framework to address challenges of live container migration in heterogeneous computing environments.

%% Notes about the paper. %%

## Introduction

### Research Motivation

- Managing containers across diverse infrastructures remains a challenge due to dependence on container orchestration platforms
	- vendor lock-in, portability constraints, operational complexity
- Heterogeneity in orchestration platforms complicates container migration across cloud/edge environments

### Research Problem

Key challenges that impede live migration of containers in heterogeneous cloud-infrastructure:

- Orchestration platform dependency
- Orchestration platform heterogeneity
- State transfer overhead
- Network disruptions and service downtime
- Suboptimal migration decision-making

### Research Scope

- Hardware/software heterogeneity not considered
- Cross-platform portability is also excluded

### Research Contributions

- FlexiMigrate live migration framework including:
	- Orchestrator-agnostic migration
		- nested container architecture
	- Efficient state transfer mechanism
	- Resilient network adaption
	- Intelligent migration decision-making
- Comprehensive evaluation of FlexiMigrate

## FlexiMigrate Framework

### Framework Overview

- Framework as bridge between two separate cloud/edge infrastructures
- Components:
	- Migration manager
	- Resource monitor
	- Decision engine
	- Container manager
	- Network manager
	- State synchronizer
- Two external components:
	- Container orchestrator
	- SDN controller

### Nested Container Architecture

- transparent and minimally disruptive migration
- leverages Linux kernel features
- enables migration of entire container stack without modifications to app container
- algorithms described in text

### SDN-based Network Management

- IP address reassignment with DNS updates
- Implements:
	- OpenFlow protocol
	- Intent-based networking (IBN)
	- Virtual extensible LAN (VXLAN)

## Components and Modules in FlexiMigrate

- Describes the modules of FlexiMigrate's components and their responsibilities
- Read paper for detail

### State Synchronizer

Components:

- Checkpoint controller
	- copy-on-write mechanism for incremental checkpointing
	- memory page deduplication to eliminate redundant data
	- compression to reduce checkpoint size
- Delta tracker
	- monitors incremental changes between checkpoints
- State restoration
	- reconstructs container state on target host by applying checkpoint deltas

## Performance Evaluation

- See paper

## Hypotheses

NOTE: This paper goes over how to build a complete system supporting live migration in heterogeneous environments across different container orchestration platforms. However, the actual migration aspect itself seems to be discussed on surface level only, don't think it's that relevant.

---

## Links

%% Links to related literature. %%