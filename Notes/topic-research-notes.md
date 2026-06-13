## Search Criteria

- "Serverless" AND "Orbital"
- "Serverless" AND "3D Continuum"
- "Serverless" AND "WebAssembly"

Found *12 papers* via Google Scholar (see *Zotero*).

TODO: Research about shared state management in orbital systems.
TODO: Research about satellite stationarity and hand-off models.
TODO: Read up on virtual stationarity and the challenges/issues/bottlenecks of migrating compute services in large-scale LEO satellite systems. Does WebAssembly (AoT-precompiled) solve any of these bottlenecks?

## Ideas

- So one topic that comes up with serverless platforms is state management. Existing literature regarding WebAssembly runtime integration into serverless platforms are mostly about cold-start latency reduction and evaluations regarding CPU-bound, I/O-bound, or mixed workloads. Stateful functions aren't really handled here, probably because it's a problem that's pretty orthogonal to the issue of used runtime for serverless I guess? Maybe it's a thread to follow though.
- Mobility of satellite systems necessitates the migration of compute services (containers, VMs, serverless functions) to keep proximity to nearby clients (virtual stationarity): Maybe AoT-precompiled Wasm modules might be the solution to reduce the cold-start latency in presence of high number of migrations?
- Function deployment orchestration, state replication orchestration for virtual stationarity

## Questions

- Are there existing solutions to state-management in orbital edge systems?
	- Is it as simple as an in-memory key-value store? Because stationarity needs to be accounted for as well right? Distributed state, eventual consistency, temporary ISLs?
- What's stationarity in satellite systems? How is code shared, updated, migrated?
	- What are hand-off models?
	- How to manage high-availability in context of stationarity?
	- 