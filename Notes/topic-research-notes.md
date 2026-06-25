## Search Criteria

- "Serverless" AND "Orbital"
- "Serverless" AND "3D Continuum"
- "Serverless" AND "WebAssembly"

Found *13 papers* via Google Scholar (see *Zotero*).

TODO: Research about shared state management in orbital systems.
TODO: Research about satellite stationarity and hand-off models.
TODO: Read up on virtual stationarity and the challenges/issues/bottlenecks of migrating compute services in large-scale LEO satellite systems. Does WebAssembly (AoT-precompiled) solve any of these bottlenecks?
TODO: Research migration mechanisms to achieve virtual stationarity -> Migrating a stateful container is more difficult than migrating a serverless function I imagine

## Ideas

- So one topic that comes up with serverless platforms is state management. Existing literature regarding WebAssembly runtime integration into serverless platforms are mostly about cold-start latency reduction and evaluations regarding CPU-bound, I/O-bound, or mixed workloads. Stateful functions aren't really handled here, probably because it's a problem that's pretty orthogonal to the issue of used runtime for serverless I guess? Maybe it's a thread to follow though.
- Mobility of satellite systems necessitates the migration of compute services (containers, VMs, serverless functions) to keep proximity to nearby clients (virtual stationarity): Maybe AoT-precompiled Wasm modules might be the solution to reduce the cold-start latency in presence of high number of migrations?
- Function deployment orchestration, state replication orchestration for virtual stationarity
- Leverage serverless actors for communication in LEO satellite networks and migrations
- (*Komet*) Client hand-offs happen every 4-5 minutes, meaning migrations are required for virtual stationarity -> how can we minimize the overhead for the migrations -> is this where wasm comes in? Kubernetes mandates downtime for frequent migrations -> serverless
- *Komet* investigates trade-offs between SLOs in context of varying migration frequencies -> anything to add here in research?
- Maybe a combination of AoT pre-compilation for Wasm in combination with migrations for virtual stationarity make up a suitable mechanism for serverless functions in LEO satellite systems?
- Do container-based runtimes fail to meet demands when it comes to scalability of satellite networks (since they are in the number of thousands), that's where serverless comes in -> WebAssembly as a more efficient runtime, especially at the edge
- Trade-off in complexity of migrating stateful serverless functions: Is it worth the additional migration overhead for having stateful serverless functions in orbital edge systems?
- Serverless edge can't handle continuous data streams (audio transcoding, video conferencing well) -> how can this be improved?
- A centralized scheduler is proposed -> What if we have decentralized schedulers? Scheduler location: Authors believe that existence of centralized scheduler is good enough, however future research for different scheduler architectures may be possible -> distributed per-satellite autonomous schedulers + intelligent scheduling algorithms?
- LEO satellite systems are growing, therefore a need for scalability exists. Maybe this necessitates the existence of lightweight serverless platforms with a focus on scalability; that's why *HyperDrive* uses a distributed scheduler as well for the 3D continuum
- Is a combination of serverless agents with distributed schedulers and WebAssembly something to be desired? Don't know if that would contribute anything
- *Trabant*: Basically, Satellite-as-a-Service (or Constellation-as-a-Service) is proposed -> Create a serverless platform based on Wasm for secure isolation in multi-tenant context with good cold-start times? Maybe address the scalability, or elasticity aspect?
- Satellite-as-a-Service -> Basically satellites as Cloudflare workers?
- *Stardust* Future work involves implementing a lightweight emulation mode based on containerization -> maybe an alternative would be Wasm as runtime with scalability in mind
- Most important question: Is Wasm **alone** good enough yet, should we instead go for a hybrid approach with Docker, similar to how Wasm was integrated into OpenWhisk?

Simple extensions:

- Move existing container-based runtime to Wasm
- Create hybrid Wasm/Container-based runtime platform
- Extend existing OEC solution to cover whole 3D-continuum

## Questions

- Are there existing solutions to state-management in orbital edge systems?
	- Is it as simple as an in-memory key-value store? Because stationarity needs to be accounted for as well right? Distributed state, eventual consistency, temporary ISLs?
- What's stationarity in satellite systems? How is code shared, updated, migrated?
	- What are hand-off models?
	- How to manage high-availability in context of stationarity?
	- Is Wasm in any way related/relevant in context of virtual stationarity/migrations/hand-offs?
- What should be our goal here? Compatibility with existing, non-Wasm (Docker-based) platforms, or (like Faasm) standalone Wasm stuff that doesn't interoperate with containers? Do novel approaches like Faasm make sense if not applicable via modern serverless platforms like AWS Lambda (improvements or real-world applicability)?
- Are there any latency-critical use cases for satellite networks -> That's where the improved cold-start latency and scale-to-zero characteristic of Wasm serverless would shine
- What's the *Sticky* heuristic for migration in context of virtual stationarity?

## For Meeting on the 15th

Papers I found to be most relevant:

- @gackstatter2022: Wasm runtime platform integrated into OpenWhisk
- @marcelino2024a: Actor model for serverless functions
- @marcelino2025a: Performance characterization for Wasm
- @pfandzelter2021: Characteristics, requirements for LEO edge
- @pfandzelter2023: Serverless abstractions for LEO edge applications
- @pfandzelter2024: Serverless platform for LEO edge
- @pfandzelter2025: Serverless architecture for multi-tenant LEO platforms
- @pusztai2024: Scheduler for 3D continuum
- @pusztai2025: Simulator for 3D continuum

Things I considered:

- Standalone-Wasm runtime vs. hybrid runtime
- Focus on data-intensive applications / data replication aspect (seems unrelated to Wasm)
- Handoff mechanisms for virtual stationarity, optimizing migrations via Wasm runtime
- Serverless LEO with focus on:
	- Scalability
	- Orchestration
	- Multi-tenancy xxx
	- Migration/Handoffs/Live-migration xxx
	- Data-replication
- Addition to existing works by:
	- Extension from LEO to 3D continuum
	- Moving from container-runtimes to Wasm or hybrid runtimes
- Wasm-based emulator

Live-migrations: Streaming, real-time applications, extending runtimes, creating snapshots (checkpoints/restoration) which are transferred, maybe using TCP-sockets, Wasm runtime on TCP-level; From JIT -> on-stack replacement: 1) at function begin, 2) in loop header, how do you work
with streamed data from TCP sockets? Context of real-time applications
TCP-specific or UDP-specific or even agnostic to underlying L4 protocol

TODO:

Until next meeting -> Festlegen auf ein Thema, das ist mein Themengebiet
Zu diesem Thema schon ein paar paper lesen, ein paper bei hand haben -> das ist richtig cool, da will ich weiter arbeiten
Wenn ich dieses Paper gefunden habe -> Genau durchlesen, nicht nur ueberfliegen; kritisch hinterfragen, evaluierung gegen baseline oft indiz fuer luecken im ansatz