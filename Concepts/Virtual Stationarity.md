The dynamic nature of [[Low-Earth Orbit (LEO)]] constellations poses a challenge:

Each satellite is only reachable by ground stations for a few minutes, after which a communication *hand-off* from ground station to another satellite is required. This hand-off is different from cellular or WiFi hand-offs due to the mobility of the network infrastructure.

For applications that need to persist state, *migrations* of application state is required. If this can be done, LEO constellations could benefit from an abstraction similar to [[Geostationary-Earth Orbit (GEO)]] constellations: *stationary operation* at (roughly) the same location over time.

The naive approach for selecting a satellite picks the *latency-optimal* satellite at each instant (called *MinMax*). The *MinMax* algorithm has the following two problems:

- Potentially *frequent hand-offs*
- Potentially *large latency for hand-offs*