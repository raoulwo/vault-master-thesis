---
tags:
  - orbital-edge-computing
  - edge-computing
---
# Orbital Edge Computing (OEC)

## Constraints

- High mobility of satellites
	- Time-varying nature of networks
	- A ground station can communicate with a passing satellite for a couple of minutes
- Limited weight and size of satellites
- Limited computational resources
- Limited energy consumption, thermal dissipation
- Harsh nature of space environment
	- Hardware and software must be resilient, reliable, and fault tolerant
- Limited bandwidth for data transmission
- Various causes for satellite transmission loss
- Protocols
	- QUESTION: Do satellites communicate via TCP/IP or specialized protocols?
- Heterogeneous hardware:
	- Between satellites, can assume that homogeneous hardware
	- However, between ground-station and satellite probably different
- [[Propagation Delay]]: At LEO altitudes from 500-2000km, propagation delay even at speed of light is significant (2-7ms)
	- If a packet is dropped, due to propagation delay it takes considerable time before GS notices for retransmission
	- Propagation delay changes depending on relative position of satellite to GS
		- RTT is constantly changing
			- Different period times -> [[Jitter]]
			- The different RTTs can mess up [[Transmission Control Protocol (TCP)]] timeout calculations (TCP RTO)
- [[Transmission Delay]] is asymmetric:
	- Downlink transmission latency is lower because of limited computational resources on satellites, therefore rate at which bits are pushed into the wire is lower ([[Shannon-Hartley Theorem]])

---

## Links

%% Links to related concepts. %%
