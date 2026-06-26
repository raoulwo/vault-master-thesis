---
tags:
  - leo-satellites
---
# Low-Earth Orbit (LEO)

LEO satellite constellations operate below 2,000km (unlike [[Geostationary-Earth Orbit (GEO)]] constellations). This allows networks to offer *low latency*, with *round-trip times* between satellites and ground stations potentially in *single-digit milliseconds*. With their *scale*, the networks can provide *global Internet coverage*.

For example, Starlink operates at an altitude of 550km. At this height, the satellites travel at 27,306km/h, completing each orbit in 95m39s. By operating so much lower than GEO constellations, LEO satellites offer a much lower propagation latency (65x lower at 550km). However, LEO constellations lose the stationarity. Furthermore, GEO satellites offer a much higher coverage, a handful of them being enough for global coverage. Global coverage in LEO requires a lot more satellites.

The coverage and velocity characteristics of LEO mean that a ground station sees a particular LEO satellite for only a few minutes. After this time, the ground station must perform a connection hand-off to another LEO satellite for continuous communication.

Besides satellites connecting to nearby ground stations, networks feature *inter-satellite links* (ISLs). A connection between two different ground stations traverses the following:

- Uplink from ground station to satellite
- A series of ISLs
- Downlink from satellite to ground station

---

## Links

%% Links to related concepts. %%

- [[@bhattacherjee2020]]