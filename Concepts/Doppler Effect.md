---
tags:
  - orbital-edge-computing
---
# Doppler Effect

The *Doppler effect* is the change of frequency of a wave in relation to a moving observer relative to the source of the wave.

Relevance for [[Orbital Edge Computing (OEC)]]:

- Communication between ground stations and [[Low-Earth Orbit (LEO)]] satellites:
	- LEO satellites move at roughly 28,000km/h relative to a fixed ground station
	- Physical frequencies change as the satellites move:
		- Higher frequency as they approach the ground station
		- Lower frequency as they move away from the ground station
- In terrestrial systems, the Doppler effect is negligible
- In satellite systems, the shift degrade the system considerably:
	- Receiver blindness: radio receivers use tight digital filters to listen to specific channels, if the frequencies fall outside this channel because of Doppler shift, [[Packet Loss]] occurs
	- To prevent this receiver blindness, satellite systems must track the frequency shift in real-time -> this results in overhead because packets must send a larger preamble

Doppler shift depending on satellite position relative to ground station:

- Horizon ingress (satellite appears):
	- High relative velocity, thus max positive Doppler shift
- Zenith (directly above):
	- Relative velocity drops to 0, thus 0 Doppler shift
- Horizon egress (satellite disappears):
	- Low relative velocity, thus max negative Doppler shift

---

## Links

%% Links to related concepts. %%

- https://en.wikipedia.org/wiki/Doppler_effect