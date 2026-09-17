---
author: Denby B., Lucia B.
year: 2020
tags:
  - leo-satellites
  - orbital-edge-computing
pdf_link: "[[Orbital_Edge_Computing__Nanosatellite_Constellations_as_a_New_Class_of_Computer_System.pdf|Open PDF]]"
---
# Orbital Edge Computing: Nanosatellite Constellations as a New Class of Computer System

## TLDR

%% One-sentence summary of the core/claim contribution. %%

So called "bent-pipe" architectures are inefficient as LEO constellation size increases. An OEC architecture is proposed which supports edge computing in scenarios where downlinking is not possible. The paper shows that this architecture can reduce ground infrastructure over 24x compared to bent-pipe architecture and reduce edge processing latency by over 617x.

%% Notes about the paper. %%

- Bent-pipe architecture:
	- ground stations send commands to orbit and satellites reply with raw data
	- time-varying relationship between GS and satellites can lead to high downlink latency
	- downlinks can be unreliable
	- downlink bitrate is limited, prevents extreme data volumes
	- breaks down as number of edge-sensed data increases because of downlink and bitrate
		- OEC solves this bottleneck: processing sensing data in orbit, thus need to downlink less data
- Terrestrial edge-cloud:
	- Cloud benefits from edge accelerating computing
	- Benefits of edge computing depend on backhaul network availability
	- High data-rate sensors deployed across large environments face network bottleneck as data-rate exceeds network bandwidth
	- Edge processing avoids privacy and security risks of multi-tenancy
- Applying edge computing to space:
	- Energy must be harvested from environment
		- Satellite size limits solar panel surface area, thus limits power
	- Communication bitrate dictates amount of data satellites buffer between downlinks
	- Communication bitrate affected by:
		- Orbit params
		- GS capability
		- GS location



---

## Links

%% Links to related literature. %%