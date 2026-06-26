---
author: Bhattacherjee D., Kassing S., Licciardello M., Singla A.
year: 2020
tags:
  - leo-satellites
  - orbital-edge-computing
pdf_link: "[[In-orbit_Computing__An_Outlandish_thought_Experiment.pdf|Open PDF]]"
---
# In-orbit Computing: An Outlandish thought Experiment?

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper examines the challenges and opportunities of in-orbit computing.

## Thoughts

%% Thoughts and questions about the paper. %%

- Idea: LEO constellations as servers for online matchmaking, rollback netcode, worldwide playability

## Related Work

%% Existing state-of-the-art research. %%

## Contributions

%% Research gap and novel contributions. %%

The paper presents the following thought:

- What if mega-constellations could offer *in-orbit compute as a service*, much like cloud computing in from terrestrial data centers?
- The in-orbit computing service would be more expensive and more limited than terrestrial cloud computing, thus would only have specific use cases.
- It would extend computing *whenever* you want to computing *wherever* you want. Even CDN edge locations have latencies of 100+ ms.

A key technical challenge arises from the high velocity of LEO satellites. Each ground station sees an LEO satellite only for a few minutes. This is followed by a hand-off to another satellite. This is especially challenging for stateful applications (e.g. multiplayer games). To provide persistence needed for such applications, picking a suitable satellite-server ahead of time and migrating state to a suitable successor would be necessary. If this is possible, LEO constellations can offer a powerful abstraction:

- GEO-like stationarity
- ~65x lower latency

The paper discusses LEO constellations' potential use case as a CDN:

- Motivation: In some parts of the world, latencies to the CDN still exceed 100ms.
- Equipping every satellite of Starlink (~40,000 satellites) with a CDN server would make it only 7x smaller than the world's currently largest CDN
	- In-orbit compute could meet the needs of sparsely populated areas not covered by CDNs

Another use case of these constellations are multi-user applications with real-time requirements such as online games, collaboratively playing music, shared online classrooms, or shared VR environments:

- Motivation: Collaborative application used at remote locations without nearby data center leads to large RTT, latencies would be reduced using LEO constellation
- Use cases:
	- No nearby data center (South America, Africa, parts of Asia)
	- No data center that minimizes latency for all users

In-orbit computing is more limited in resources and more expensive than terrestrial cloud resources:

- Moving general computations up there isn't feasible
- However, applications that generate data in space already (earth imagery, weather observations, remote sensing) could benefit from LEO constellations which could additionally to providing ISP services or computing, generate data in space

Feasibility of in-orbit compute:

- Power-draw could be a burden
- Cost of compute is several times higher than terrestrial data centers
- If operators still see enough value for the use cases, these barriers are not showstoppers

[[Virtual Stationarity]]:

The *MinMax* algorithm is naive and can lead to frequent hand-offs or high-latency hand-offs. Instead, a *Sticky* heuristic is proposed:

- Idea: Prioritizes stationarity by planning ahead leveraging predictable satellite motions
	1. Compute the set of servers with latency within 10% of MinMax
	2. For each of these candidates, compute the time until next hand-off
		- Select the top 5 with longest time until hand-off
	3. Pick the one with lowest latency from these 5

State migrations every few minutes are a substantial overhead, however high inter-satellite bandwidth might lessen this.

From application perspective:

- Differentiate between session-specific state and general application state
- Session-specific state will be migrated live
- General application state will be replicated ahead-of-time

## Methodology

%% Architecture of the solution. %%

## Implementation

%% Actual implementation of the solution. %%

## Evaluation

%% The evaluation of the solution. %%

---

## Links

%% Links to related literature. %%