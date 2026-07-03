---
tags:
	- networks
---
# Packet Loss

- *Packet loss* occurs when one or more packets of data travelling across a computer network fail to reach their destination
- Reasons for packet loss:
	- Data transmission errors
	- [[Network Congestion]]
- Packet loss is measured as *percentage of packets lost with respect to packets sent*
- [[Transmission Control Protocol (TCP)]] detects packet loss and retransmits data
	- Packet loss in TCP connections is also used to avoid congestion
		- Produces an intentionally reduced throughput for the connection
- In real-time applications, packet loss can negatively affect a user's [[Quality of Experience (QoE)]]

## Causes for Packet Loss

- [[Internet Protocol (IP)]] is designed as *best-effort delivery* service to keep router logic simple
- Network doesn't make *reliable delivery* guarantees
	- Requires a *store and forward* infrastructure
	- Once a router fails the delivery guarantees can't be maintained anymore
	- Not all applications require reliable delivery
		- e.g. it instead would be a bottleneck for streaming applications
- [[Internet Protocol (IP)]] allows routers to *drop packets* if it's too busy
	- Not ideal for speedy and efficient data transmission
	- Not expected to happen in uncongested networks
	- Is an indicator for possible network congestion

---

## Links

%% Links to related concepts. %%

- https://en.wikipedia.org/wiki/Packet_loss