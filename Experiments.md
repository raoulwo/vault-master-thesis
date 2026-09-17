
I feel like I need to pivot my topic a bit, else it feels like I just risk doing the same thing Daniel does already but worse, right?

## Todo

Finding a direction:

- [x] Study all live migration papers I know and look at the measurements and plots they make to motivate their novel approaches
- [ ] Find out what I want to show with my experiments
- [ ] Look at existing meeting notes and at what different directions Daniel recommended to go into

Continuing current experiments:

- [ ] Set up NTP (network time protocol) to prevent diverging times using *chrony*
- [ ] Send ACK responses from servers to client for measuring packet loss
- [ ] Measure network traffic using *tcpdump*

## Notes

- Combining checkpoint/restore with buffering + forwarding input, and replay
	- checkpoint/restore inefficient -> show checkpoint, transfer, restore times
		- scales with memory size, how do i tweak this with the `worker.py` script?
	- buffer size increases linearly during migration window
		- what can i do with that?
		- buffer size correlates to checkpoint/restore time, as packets are buffered during that period
			- maybe show chart correlating buffer growth / size to memory size of migrated service?
	- replay happens for buffered packets during migration
		- buffer size correlates to memory size of migrated service, therefore ...
			- ... replay duration correlates to memory size of migrated service
				- find edge case in which replay of a lot of packets is inefficient
					- CPU-heavy, memory-heavy, network-heavy ???
						- Instinctively should be CPU-heavy right?

- One thing I can definitely show is: CRIU checkpoint and restore times with scaling memory size
	- Measure downtime -> show that CRIU takes too long -> Wasm would be more efficient
- Redundancy migration -> Show latency spikes due to additional hop during migration window
- Redundancy migration -> Show packet loss over migration phases -> should theoretically be unaffected by migration right?
- Parallel migration -> downtime zero, however state synchronization can be an issue, especially in regards to migration time, how can i show that it can become a problem?
	- CRIU checkpoint/restore takes longer for bigger memory sizes
	- therefore longer migration window and more packets are buffered in the meantime
	- more packets are buffered -> replay takes longer
		- question: what causes replay to go bad (CPU-heavy "long" computations?)
			- take VR/AR or video streaming applications as use case

Question: Is the packet replay even part of migration time?

## Measurements of Live Migration Papers

[[@ahmadpanah2025]]:

- Comparison of *FlexiMigrate* framework to existing frameworks:
	- Metrics measured
		- Total migration time (s)
		- Downtime during migration (ms)
		- Network overhead (MB)
		- Avg. CPU usage (%)
		- Peak memory usage (MB)
	- Performed for single container migration and multiple containers
	- Performed under normal network conditions and network congestion
- Measuring the overhead of the framework's nested container architecture
	- Comparison of framework with regular deployment
	- Overhead (%)
		- Avg. CPU usage (%)
		- Avg. memory usage (MB)
		- Disk I/O throughput (ops/s)
		- Application throughput (req/s for NGINX, TPS for PostgreSQL)
		- Application latency (ms)

[[@choi2023]]:

- Experiments of TCP connections with simulated migration overhead ranging from 0-500 microseconds -> 99th percentile tail latency increases significantly above 200 microseconds overhead
- Comparison of the solution with alternative
	- Measuring total latency overhead, subdivided in TCP handshake flow

[[@fujii2024]]:

- Comparison between WasmEdge and WAMR runtimes using `sqlite-bench` benchmark
	- Different tasks measured in execution time per query (s/op)
		- WasmEdge lower execution time across all tasks
	- Memory usage (RSS) measured by duration (s)
		- WAMR lower memory usage overall
- Comparison between WasmEdge/WAMR migration using the paper's prototype and CRIU checkpoint/restore
	- Programs migrated
		- `binary-trees` and `n-body` of *The Computer Language Benchmarks Game*
		- `sqlite`
	- Measured
		- Checkpoint time
			- Overall way faster for Wasm runtimes compared to CRIU
		- Restore time
			- Both WasmEdge and WAMR faster for `binary-trees` and `n-body`
			- Slower for `sqlite`
		- Checkpoint size (KB)
			- Wasm way smaller than CRIU

[[@govindaraj2018]]:

- Comparison of redundancy migration (using replay) with LXD migration:
	- Avg. downtime over 30 runs: redundancy migration lower downtime
	- Avg migration time over 30 runs: LXD migration lower migration time
- Checkpoint/restore times scale logarithmically with increasing state size
- Reason for longer migration time is buffer size, thus replay duration
- Redundancy migration adds additional overhead due to packet retransmission
	- Additional latency can violate latency requirements

[[@gundall2022]]:

- Qualitative study of migration methods
	- Compared migration approaches
		- C/R
		- Pre-copy
		- Post-copy
		- Hybrid-copy
		- Parallel migration
			- Issue -> state synchronization and network overhead
- Testbed evaluation shows downtime of ~0.2ms and migration time of 1-2s
- Migration time:
	- Option 1 (starting blank container, then sending memory over) ~1s
	- Option 2 (C/R of container) ~2s
		- Biggest fraction of time used for C/R

[[@jiang2010]]:

- Comparison of their lightweight live migration (LLM) with state-of-the-art approach Remus:
	- Downtime comparison under high network load:
		- LLM performs significantly better than Remus
	- Downtime comparison under high system load:
		- Remus performs significantly better than LLM
	- Average network delay per checkpointing period
		- Both under high network and system load: LLM performs way better
			- Detailed:
				- LLM has delay spikes at beginning and end of checkpointing period
				- Remus consistently higher delay during migration period, no spikes

[[@kim2021]]:

- Request/Response times over sequence numbers (comparison between 3 migrations)
	- Average line, max and min response times for each request
		- How are these created -> has to be done over multiple experiment runs right?
- Also objective value compared between 3 migration types
- Correlation between migration time and response time for three migration strategies

[[@kleebinder2026b]]:

- e2e latency (ms) during satellite pass over time (s)
	- graph shows latency curve as satellite moves over GS with minimum directly above
	- after a handover -> constant latency increase because of additional hop
- packet loss (%) over time (s) for C/R, pre-copy, post-copy of UDP workload
	- shows handover window
	- p5-p95 band
	- p50 line
- showing feasibility of migration membrane by ...
	- plotting migration time, CPU consumption, memory stability over 300 handovers
	- also table that shows operational stability and longevity metrics across handovers
- migration convergence bounds w.r.t. memory writes (MB/s)
	- shows pre-copy migration time fails to converge, while migration membrane has constant time
- migration convergence bounds w.r.t. system calls (syscall/s)
	- shows migration membrane fails to converge, pre-copy has constant time
- e2e latency (ms) during continuous migration (multiple handovers over ~9 minutes) over time (s)
	- with 6ms, 10ms, 14ms latency SLO targets
	- shows p50 latency line
	- shows p5-p95 latency band

[[@lei2017]]:

- Two benchmarks
	- SPEC VIRT_SC 2013 measures e2e performance of all system components (write-intensive)
	- Linux Kernel Compile (intensive workload)
- For the two benchmarks, three measures are taken for different hyperparameter values alpha:
	- Total migration time
	- Total number of transferred pages
	- Total number of page faults

[[@machen2018]]:

- Layered framework for migrating VMs or containers
- Table showing layered migration (2-layer, 3-layer) comparison for both LXC (containers), KVM (VMs)
	- Measuring
		- Total migration time
		- Total data transferred
		- Service downtime
	- Various applications
		- No app
		- game server
		- RAM simulation
		- Video streaming
		- Face detection
	- Layers:
		- Two layers
		- Three layers app not found
		- Three layers app found
- Migration time for LXC and KVM under:
	- Increasing RAM usage (MB) -> linear increase
	- Increasing bandwidth (Mbps) -> 1/x curve (hyperbola)

[[@puliafito2022]]:

- Comparison of service time impacted (s) by connection migration:
	- Question: Do they mean service downtime???
	Different strategies:
		- TCP baseline
		- QUIC baseline
		- P-explicit
		- R-explicit
		- Pool of addresses
		- ...
- Figures contrasting cold (C/R???), pre-copy, post-copy, hybrid-copy migration:
	- For each connection strategy:
		- TCP baseline
		- PoA
		- R-Explicit
		- P-Explicit
	- Following comparisons are made:
		- Done for two types of containers:
			1. Small container
			2. Large container
		- Service time (s) for each migration mechanism and connection strategy
			- Again, what's service time???
		- Migration time (s)
			- With following sub-divisions
				- Pre-dump
				- Pre-dump Tx Time
				- Dump
				- Dump Tx Time
				- Restore
				- Fault Page Tx Time
		- Migration overhead (MB)
			- With following sub-divisions
				- Pre-dump Tx size
				- Dump Tx size
				- Faulted pages

[[@rong2023]]:

- Three example apps
	- Vehicle counter (CNN-based detector, pytorch)
	- Object tracking (CNN-based detector, tensorflow)
	- Person detection (CNN-based classifier, tensorflow)
- Table showing snapshot sizes (GB) and downtime (s) of three apps using CRIU C/R
- Table showing downtime (s), migration time (s) and number of processed video frames during migration window using post-copy migration
- Plots shows dirty page rates (Mbps) compared to constant edge bandwidth (Mbps) over iteration rounds
	- Both for compressed and non-compressed video streams
	- dirty page rate exceeds constant edge bandwidth
- Plot showing the subdivision of three memory categories (GB) for three apps
	- memory types: permanent, ephemeral, crucial
- Plot showing the migration time (s) subdivided into defined 4 migration phases for three apps
	- phases:
		- pod bootup
		- warm-up
		- sync
		- replay
- Plot showing latency (s) for 3 applications and baseline over frames before, during, and after migration window
	- 3 different frame rates: 2, 3, 5 per seconds
- Plot showing CPU usage (%) over time (s) for three apps for both source and destination servers
- Tables showing read/write performance (ms) and data size (Kb) for Deep Sort and SURF (object tracking methods) for variable params alpha and beta (which are method specific)

[[@tinto2025]]:

- Table showing run-time cost (ms) of instantiating a JIT Wasm module
- Table showing run-time cost (ms) of deploying Linux Alpine Docker container
- Table showing memory footprint comparison (Mb) between docker image and Wasm module
- Execution times (s) of three PolyBenchC benchmarks -> comparing Wasm with container
- Measured times (microseconds) of operations connected to migrating a Wasm module
	- of `3mm` and `correlation` PolyBenchC workload
	- Overhead of C/R procedures < 30% of execution time of non-migrating computation
- Plot showing run-time cost of performing regular checkpoints (no migration, only checkpointing):
	- done for each workload of PolyBenchC benchmark
	- overhead of checkpointing shown as factor of original computation
	- green line shows avg. number of variables checkpointed
	- Comparing distributed checkpointing (regularly) vs. centralized checkpointing
- Plot showing proportions of memory regions to blocks per benchmark workload
- 3D plot of checkpointing overhead (s), scaling with the two axes:
	- number of checkpoints
	- number of variables per checkpoint
	- Makes sense, the more variables per checkpoint the more memory, the more checkpoints the more overhead