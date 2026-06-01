---
author: Casteigts A. and Flocchini P. and Quattrociocchi W. and Santoro N.
year: 2021
tags:
  - time-varying-graphs
pdf_link: "[[Time-varying_graphs_and_dynamic_networks.pdf|Open PDF]]"
---
# Time-Varying Graphs and Dynamic Networks

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper unifies the formalisms found in literature (i.e. *delay-tolerant networks*, *opportunistic-mobility networks*, and *social networks*) into a unified framework called *time-varying graphs* (TVGs).

## Related Work

%% Existing state-of-the-art research. %%

A common point of existing works is that the *network topology* varies in time. In these systems, changes are not unexpected anomalies, but rather an integral part of the nature of the system. Since the network topologies change dynamically, (static) graphs are not suitable for representing such systems, instead dynamic graphs are better suited.

Existing works propose similar dynamic graphs: 46, 54, 29, 31, 67, 50.

Three research areas which display dynamical aspects are explained as motivation for TVGs:

- *Delay-tolerant networks* (DTNs). They are highly dynamic, infrastructure-less networks with possible absence of end-to-end communication routes at any instant.
- *Opportunistic-mobility networks*. It's possible to exploit DTNs for uses that are external to the carriers. Other entities called *agents* can move on the network for their own purposes by using the mobility of the carriers (or ferries) as a transport mechanism.
- *Real-world complex networks*. This research area addresses the analysis of real complex dynamic networks, ranging from neuroscience and biology to transportation networks and social studies.

## Contributions

%% Research gap and novel contributions. %%

The paper formalizes existing works into a unified framework by defining *time-varying graphs*.

### Time-Varying Graphs (TVGs)

- $V$: Set of *vertices*
- $E$  with $E \subseteq V \times V \times L$: Set of *edges*
- $L$: Alphabet for edge *labels*

Because TVGs address dynamic systems, the relations between vertices are not assumed to be static. They take place over a *lifetime* $\mathcal{T} \subseteq \mathbb{T}$ where $\mathbb{T}$  is the *temporal domain* that is assumed to be either $\mathbb{N}$ for *discrete-time systems* or $\mathbb{R}^+$ for *continuous-time systems*.

A TVG called $\mathcal{G}$ can be defined as a five-tuple $\mathcal{G} = (V,E,\mathcal{T},\rho,\zeta)$ with:

- *Presence function* $\rho: E\times\mathcal{T} \to \{0,1\}$. Indicates whether a given edge is available at a given time.
- *Latency function* $\zeta: E\times\mathcal{T} \to \mathbb{T}$. Indicates the time it takes to cross a given edge at a given point in time.

NOTE: The model can be extended with a *node presence function* and *node latency function*.

### Definition of TVG Concepts

#### The Underlying Graph $G$

Given a TVG $\mathcal{G}=(V,E,\mathcal{T},\rho,\zeta)$, the graph $G = (V,E)$ is called the *underlying graph* of $\mathcal{G}$. This static graph can be seen as the *footprint* of $\mathcal{G}$.

#### Point of Views

Depending on the problem under consideration, it may be convenient to look at the evolution of the system from different perspectives:

- *Edge-centric evolution*
- *Vertex-centric evolution*
- *Graph-centric evolution*

#### Subgraphs of a TVG

It is possible to define classical subgraphs by restricting the set of vertices $V$ or edges $E$ of a TVG $\mathcal{G}$. More interesting is the definition of a *temporal subgraph* by restricting the *lifetime* $\mathcal{T}$, resulting in the subgraph $\mathcal{G}'=(V,E',\mathcal{T}',\rho',\zeta')$ with the following properties:

- $\mathcal{T}' \subseteq \mathcal{T}$
- $E' = \{e\in E:\exists t\in\mathcal{T}': \rho(e,t) = 1 \land t+\zeta(e,t)\in \mathcal{T}'\}$
- $\rho': E'\times \mathcal{T}' \to \{0,1\}$, where $\rho'(e,t) = \rho(e,t)$
- $\zeta': E'\times \mathcal{T}' \to \mathbb{T}$, where $\zeta'(e,t) = \zeta(e,t)$

#### Journeys

A sequence of couples $\mathcal{J}=\{(e_1,t_1),(e_2,t_2),...,(e_k,t_k)\}$ with $\{e_1,e_2,...,e_k\}$ being a *walk* in the underlying graph $G$ is a *journey* in TVG $\mathcal{G}$ **if and only if** $\rho(e_i,t_i) = 1$ and $t_{i+1}\ge t_i + \zeta(e_i,t_i)$ for all $i < k$.

Furthermore we define:

- $\text{departure}(\mathcal{J})$: starting date $t_1$ of a journey $\mathcal{J}$
- $\text{arrival}(\mathcal{J})$: last date $t_k+\zeta(e_k,t_k)$ of a journey $\mathcal{J}$

A journey can be thought of as *paths over time* from a source to destination. Journeys therefore have *topological* and *temporal* lengths:

- *Topological length* $|\mathcal{J}| = k$ (number of hops).
- *Temporal length* $\text{arrival}(\mathcal{J}) - \text{departure}(\mathcal{J})$ (end-to-end duration).

$\mathcal{J}^{*}_{\mathcal{G}}$ denotes the *set of all possible journeys* $\mathcal{J}$ of TVG $\mathcal{G}$.

#### Distance

The length of a journey can be measured in terms of *hops* and *time*. For this reason, there are two distinct definitions of *distance* in a TVG:

- *Topological distance*.
- *Temporal distance*.

## Methodology

%% Architecture of the solution. %%

## Implementation

%% Actual implementation of the solution. %%

## Evaluation

%% The evaluation of the solution. %%

---

## Links

%% Links to related topics. %%