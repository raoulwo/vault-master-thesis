
## TCP/UDP Socket Migration

- OSI L4 migration system based on TCP/UDP
	- How should data be transferred?
		- Maybe I can motivate both replay approaches as well as pre- and post-copy approaches.
	- TCP/UDP/QUIC-specific or protocol agnostic?
- Find concrete use case: e.g. VR/AR or Gaming
- QUIC protocol transparently supports connection migration:
	- https://quic-go.net/docs/quic/connection-migration/
- Alternative topics
	- Migration orchestrator
	- Migration heuristic

Replay migration:

- Overflowing in-memory buffer to show replay failure
- Migration time is relevant since line-of-sight contact windows are limited (4-5 minutes)
- Downtime ideally near-zero
- Showing insufficient QoS (e.g. latency, packet loss, availability, jitter)

- Pre- and post-copy migrations:
	- Showing that pre-copy has prolonged migration time
	- Showing that post-copy has packet loss during migration window

Docker container migration with CRIU:

- Show checkpoint/restore times with increasing memory size
	- With the goal of showing that they are way too high

Migrating multiple connections:

- Show UDP/TCP socket migration overhead
	- How does it scale with number of migrated sockets, where is the point of failure
	- How do I measure the migration overhead however?
