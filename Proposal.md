## Notes

- LEO live migration orchestration middleware
	- Can formally define the migration model via TVGs (see [[@casteigts2012]])

## Title Ideas

- A transparent middleware for orchestrating replay-based live migrations in low-earth orbit networks

## Core Message Ideas

## Motivation for Middleware

- Blue/Green deployment
	- Switch 100% traffic to node B, if any errors switch back to A
- Canary deployment
	- Start with small fraction of traffic (1-5%) to node B
		- Verify connection stability through performance metrics
	- Increase traffic gradually

### Positives

- Client awareness
	- Client doesn't need to know about which node to communicate with or sending packets after migration handover to the new authority (transparent)
- Orchestrator works independently of scheduler, receives migration event and initiates it
- Encapsulation of state synchronization
- Zero perceived downtime live migration
- Can perform sequential replay on node B, before switching authority to it
- Middleware can act as *circuit breaker* and have node A as fallback if B doesn't work after
- Can also perform *canary* deployment and route traffic gradually for load testing

### Negatives

- Additional hop because of middleware/proxy
- Introducing a single point of failure (SPOF)
- State synchronization overhead (buffer/replay)

### Possible Experiments

Idea:

- No middleware, no buffer: Packet loss during migration
- No middleware, with buffer:
	- Handover, then packet replay parallel to accepting requests leads to race conditions
	- Handover, then sequential packet before accepting requests replay leads to latency spikes
	- Handover after synchronization has overhead doing it decentralized (???)

We got motivations for: 1) buffering, 2) warm-up, 3) replay and fail-over strategies

- Do hard cutoff CRIU migration from A to B, continue sending write requests
	- Client sends data to B, however either packet loss or long latency since B still warming up or replaying
	- Measure data loss over write rates during migration and replay of buffered packets
	- Motivates buffering the data for replay
- Compare the latency spike of node B after handoff with the initial node A latencies
	- Measure how long it takes for B to arrive at A latencies
		- ... based on varying memory footprint, size of working set (cached/buffered data)
	- Could motivate the need for warming up node B (e.g. traffic shadowing)
- Stop node A and start node B for handover
	- Figure out how the client reacts, are there any timeouts, what's the packet loss for an abrupt cutoff
	- Measure the RTO (recovery time objective) to make perceived downtime quantifiable
	- Motivates client-transparent request buffering and failure tolerance mechanisms through middleware fallbacks

## Links

- [[2026-09-16]]