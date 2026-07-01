---
author: Tinto E., Marchiori L., Vardanega T.
year: 2025
tags:
  - webassembly
  - live-migration
pdf_link: "[[Live_Migration_of_Compiled_Wasm_Modules_Across_the_Compute_Continuum.pdf|Open PDF]]"
---
# Live migration of compiled Wasm modules across the Compute Continuum

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper discusses live migration of compiled Wasm in heterogeneous environments.

## Thoughts

%% Thoughts and questions about the paper. %%

- Scenarios that require for computations at the edge to relocate:
	- When local resources become insufficient
	- When real-world displacements require computations to move
- WebAssembly provides portability, isolation
- Abstraction of software from heterogeneous hardware wanted
- Live migration -> Seamless continuation of computation
- While similar, difference between migration and offloading:
	 - Trigger of offloading is fault in the sending node
		- Computation is then redeployed on a node with sufficient resources
	- On the other hand, migrations occur for *convenience*
		- Done not because it's required, but to be even more efficient
			- Has to be inexpensive for resources and more efficient
- Coordination between nodes becomes crucial at the edge
	- Orchestration should happen at two places
		- Infrastructure level
		- Application level
- Note on containers:
	- Deployment of same code as container or Wasm:
		- Deployment of Wasm bytecode is faster
		- Wasm bytecode is smaller
		- However container performs better
			- Idea: Incorporate both Wasm and containers to handle dynamic workloads
				- Requires orchestrator that can handle Wasm and static containers
- NOTE: Paper only discusses JIT-compiled Wasm, not AoT compiled Wasm
 
## Related Work

%% Existing state-of-the-art research. %%

Motivation:

- Heterogeneity and need for relocating computations at the edge motivates migration
- Live migration using Wasm is not new, however existing solutions focus on:
	- Non-standard runtimes
	- Migration of interpreted Wasm

Live migration support for compiled modules across heterogeneous nodes is an open challenge.

Live migration:

- Checkpoint and Restore In Userspace (CRIU)
- (13) CRIU-based approach for live migration across homogeneous architectures
- (14) CRIU-based cross-architecture approach
- (15) (CRIU-based) Focus on latency-critical approach
- (16) (CRIU-based) Improve live migration efficiency for containers
- (17) Wasm computation migration across runtimes

Wasm live migration:

- (18) Live migration where computation is HTML5 mobile web worker
- (19) Mechanism for live migration of interpreted Wasm (no WASI support discussed)
- (21) Live migration of interpreted Wasm which integrates trusted execution environment and experimental Wasi support
- (22) Migration of idle components, not engaged in a function call
- (23) Migration of Wasm computations to mitigate security risks
- (24) Live migration of Wasm modules using a tailored runtime (less attractive)
- (25) Wanco open-source project: Checkpoint and restore of Wasm functions across POSIX-compliant heterogeneous architectures

## Contributions

%% Research gap and novel contributions. %%

Work presents approach to support live migration of compiled Wasm by:
	
- partitioning target functions within a Wasm module into regions
- injecting run-time procedures in them to perform regular checkpoints at region boundaries

Works on bytecode level:

- works for any Wasm runtime (even interpreted)

To the best of the authors knowledge: Live migration support for compiled modules is an open question.

Three contributions:

- System design to decouple infrastructure from application
	- Enriched with Checkpoint and restore mechanism for computations
		- Runs as functions within Wasm module
		- (Required for live migration)
- Software instrument capable of injecting runtime procedures into existing Wasm module
	- Allows performing occasional checkpoint and restore procedures
	- Paves the way for more efficient strategies (!!!)
	-  Approach can be applied as-is to any Wasm runtime
- Preliminary evaluation of run-time penalty by periodic checkpoints
	- Done using established benchmark

## Methodology

%% Architecture of the solution. %%

Key technologies:

- Wasm is portable and lightweight runtime environment
	- Portable bytecode is not a new idea (Java)
		- However, considerably lighter
		- Minimal set of functionalities
		- regulates the interactions with the host
- Wasm executes in a sandbox
	- Accesses only statically defined set of imports from the host
	- Accesses only specific portions of continuous memory -> linear memory
		- Import requirements for instantiating and running a Wasm computation can be determined entirely by declared imports and memory
- Wasmtime used as runtime
	- Developed in Rust
	- Embeds JIT compiler called Cranelift
- Question is how to interact with hardware interfaces (underlying OS), two existing approaches:
	- (33) Use WASI component model to split drivers into two components
		- First component runs as Wasm component
		- Other is deployed directly into hosting node
		- A *WIT* (Wasm Interface Type) is language used to define the interfaces of a Wasm component which regulates the interaction between to pieces of software
	- (34) Proposes a method for Wasm modules to handle hardware interrupts directly

Live migration:

- Must be fast and minimally disruptive
	- Else it would be outperformed by a restart
- Problem: Preserving state across different architectures
	- CPU registers and systems APIs are different
	- Containers, unikernels must transmit other software components as well
		- Wasm doesn't have to, lighter memory footprint
		- More flexible for live migration
- (38) Architectural requirements for running Wasm are few:
	- single- and double-precision floating points
	- little endianness

### Design of the Solution
	
Intermittent computing (IC):

- Checkpoint/restore is a familiar topic in IC
- Energy harvesting systems (EHSp)
	- Problem: Executing a region between checkpoints without being stuck at re-execution
	- (41) Promising checkpointing solution -> distributed checkpointing
		- Motivation:
			- All registers are checkpointed in memory
			- *Live-in* registers hold little info for upcoming regions
				- They are unnecessary
			- Only registers that have been updated within the current region and *Live-out* registers are checkpointed
				- This concept of partitioning execution into regions that can be resumed is repurposed for live migration of compiled Wasm modules

Implications for Wasm:

- Distributed checkpointing should occur at function level
- Injecting the checkpoint and restore procedures directly into bytecode
	- Done once, ahead of execution time; this has two advantages:
		- Leaves room for optimization without time constraints
			- Moving the step before JIT compilation decouples it from the compilation step
		- By injecting runtime procedures directly into Wasm bytecode it can run in any Wasm runtime

#### Injection Mechanism

1. First step: Partitioning selected functions into regions
	- (Approach chosen in this work is simple -> Maybe i can refine this)
	- Construct a graph from the blocks in the function bytecode
		- Each non-nested (outermost) block becomes a region
	- In the current implementation checkpoints only occur for empty value stacks
		- Why? What are the implications of this?
2. Producing a set of *live* variables to store during checkpointing
	- NOTE: How are variables organized in Wasm modules?
		- Variables can be locals or globals, both can be mutable
			- Locals can be params and function-locals
	- For constructing checkpoint, mutable globals, locals, params where considered
	- Because of simple partitioning strategy
		- regions are sequential
		- data flow is deterministic
	- At the end of each region, only variables modified since last checkpoint are stored
		- $V_M(r_i)$ ... set of modified variables since last checkpoints
		- $r_i$ ... $i$-th region of function body
	- After computing the sets for each region $V_M(r_i)$
		- Map each variable to a corresponding memory address 
			- (Each variable has one address)
			- These addresses are used to:
				- store selected variables during checkpoint
				- restore the variables when resuming
	- WebAssembly module interaction with host memory via *linear memories*
		- These are continuous address spaces:
			- accessed through offsets
			- statically declared in module bytecode
			- sizes are statically defined (dynamic changes are allowed)
		- A module can interact with multiple linear memories
		- Wasm runtime takes care of allocation
		- Requirement: Host runtime needs to support dynamic allocation
			- e.g. via Rust `alloc`
	- Each function checkpoint:
		- stored in its own dedicated linear memory
		- added to the bytecode memory section during injection of checkpoint/restore
		- (this prevents faulty or malicious apps from altering checkpoint state)
3. After $V_M(r_i)$ is computed and each of these variables are mapped to memory addresses
	- 3 runtime procedures are inserted at different stages:
		1. Initial procedure:
			- Loads index of region $r_i$ from memory where computation should resume
				- For this, `i32` variable, which represents $r_i$, added to the locals
					- updated at completion of each region
			- Called once after function instantiation and before executing the function body
		2. Second procedure:
			- Injected at entrance of each region $r_i$
			- Checks the following three states:
				- (TODO: Understand the pseudo-code in figure 5)
					- What the hell is `\=` is this division-assignment or not-equals
				- Computation is resuming from a later region:
					- Restore each variable in $V_M(r_i)$
					- Jump execution to end of region
				- Computation is resuming from this region:
					- Continue execution without restoring any variables
				- Computation is not resuming:
					- Continue execution without restoring any variables
		3. Final procedure:
			- Injected at the end of each region $r_i$
			- Stores all variables $V_M(r_i)$ at their corresponding memory addresses
				- After this, a host-imported function is invoked
					- NOTE: Implementation is host-specific
					- Used to signal pending migration request!
						- A migration request is pending:
							- Execution traps so that normal termination and migration-triggered termination are distinguishable
							- Termination of computation always happens after checkpoint completed
								- Prevents situation where computation has to resume from partial checkpoint
						- No migration request is pending:
							- Continue execution 
	- Procedures are injected as bytecode
		- Injected functions are compiled to target architecture together with rest of module
	- Resuming the state of linear memories requires care:
		- e.g. `foo` calls `bar` (which has checkpoints) and both access linear memory `m`
			- `foo` expects `m` to be blank slate -> `bar` has initialized it already
		- Solution: Linear memories should be restored at each function entrance
			- Done via second host-defined imported function

To checkpoint multiple functions -> Run the algorithm above for multiple function indexes, each function reserves a dedicated linear memory

Centralized checkpointing:

- Motivation: Distributed checkpointing could even double execution time
- Centralized checkpoint
	- Computed at most once during function execution on a node, only when migration request is pending
	- Resuming performed as a single procedure at beginning of a region after migration
		- Slight altering of three procedures discussed prior
	- Can also reduce the number of variables to be checkpointed of $V_M(r_i)$:
		- Select a subset of modified variables in a region which is also subset of *live-out* vars
			- For region $r_i$ -> $V_{LO}(r_i)$ ... set of variables that are *live*
				- Meaning: they are read in any successor region of $r_i$
				- To compute $V_{LO}(r_i)$, we need the set of *live-in* vars
			- $V_{LI}(r_i)$ ... set of live-in vars
				- Meaning: Set of all variables read in $r_i$
			- $V_{M,0}(r_i)$ ... set of modified variables from beginning of function $0$ until end of the current region $r_i$
			- $\text{next}(r_i)$ ... set of regions after $r_i$
				- $V_{LO}(r_i) = \bigcup_{r_j \in \text{next}(r_i)} V_{LI}(r_j)$
				- live-out vars of $r_i$ are equivalent to all live-in vars of regions after $r_i$
			- $V_C(r_i)$ ... set of all variables required to restore the computation state:
				- $V_C(r_i) = V_{M,0}(r_i) \cap V_{LO}(r_i) = V_{M,0}(r_i) \cap \bigcup_{r_j \in \text{next}(r_i)} V_{LI}(r_j)$
				- Meaning: To restore computation state for region $r_i$, we need the variables that will be read after $r_i$
					- However not all, only those that have been mutated until the end of $r_i$ -> these we need to restore
						- The ones that are mutated after $r_i$ will be computed again anyways

Modifications of the three runtime procedures:

1. Initial procedure:
	- Unchanged
2. Second procedure at entrance of each region $r_i$:
	- Computation is resuming from a later region:
		- Execution jumps to end of region *without* resuming variables
	- Computation is resuming from the current region $r_i$:
		- restore variables $V_C(r_{i-1})$ from memory and resume execution
			- $r_{i-1}$ ... region before $r_i$
	- Computation isn't resuming:
		- Resume execution without restoring	
3. Final procedure at end of each region $r_i$:
	- Check if there is a pending migration request:
		- There is a pending migration request:
			- Execution stores $V_C(r_i)$ in linear memory
			- Execution traps
		- There is no pending migration request:
			- Execution resumes *without* storing any variables

Distributed vs. centralized checkpointing:

- Distributed approach *always* performs checkpoints at end of a region
	- Consequence: Computations that aren't migrated will still have checkpointing overhead
- Centralized approach has lesser runtime penalty

## Implementation

%% Actual implementation of the solution. %%

- *wasm-tools*: toolset for parsing Wasm bytecode
	- has `validate` command to ensure module is well formed after injection
- used to implement *wasm-migrate* to inject runtime procedures
	- handles injection of two host-imported functions
		- handles re-indexing of local functions
	- offers two algorithms for checkpoint and restore
		- distributed
		- centralized
	- has extension that enables support of Wasm components
		- component consists of one or more Wasm modules
		- this requires integration of specification language Wit (WebAssembly Interface Type)


## Evaluation

%% The evaluation of the solution. %%

- Exhaustive evaluation of migration orchestration falls outside scope of this work
- Unaware of other solutions for live-migration of compiled Wasm -> no comparisons
- Use open-source benchmark to measure extra time taken for computation with checkpointing
	- Benchmark used is *PolyBenchC*

Setup:

- ThinkPad E15 running Ubuntu 22.04.01
- Disabled hyperthreading, execution bound to a specific core
	- Prevents Linux scheduler from scheduling computations on another core
- Each experiment ran with highest priority (Linux scheduling FIFO policy)
- Each computation performed 5-20 times (small sample size)

Results:

- Significant runtime overhead using distributed checkpoints -> often more than doubles
- Insight: Reducing checkpoint size would reduce incurred overhead
	- So, reducing $V_M(r_i)$ would reduce checkpoint size which in turn reduces the overhead
	- Fine-grained regions might improve performance by limiting the state size

End-to-end execution time evaluation:

1. Set affinity to a specific core, start Wasm computation with injected checkpoint/restore procedures
2. Computation traps and terminates, program saves its linear memory and secondary checkpoint memory
3. Computation changes affinity program is moved to different core (doesn't share L2 cache)
4. Wasm computation resumes and terminates regularly

NOTE: To get data for key procedures `rustix` was used

INSIGHT:
	- Restoring memory incurs significant overhead as well (!!!)
		- Current solution restores entire linear memory
	- Future work might include selective/more efficient approaches for restoring linear memory
	- There's also room for improvement in the region formation algorithm
		- Use same as (41) or
		- Dispose of autonomy requirement when creating regions
			- See here: [[Live_Migration_of_Compiled_Wasm_Modules_Across_the_Compute_Continuum.pdf#page=10&selection=484,0,688,9|Live_Migration_of_Compiled_Wasm_Modules_Across_the_Compute_Continuum, page 10]]
	- Two kinds of orchestration required:
		- Infrastructural
		- Application
		- No industrial orchestrator exists that suffices
	- No representative benchmark for Wasm computation exists
		- Previous works argued that PolyBenchC produces positive results applied to Wasm
			- Part of future work to perform more realistic benchmark
	- Proposed solution could also be extended for *stack migration*
		 - Wasm is based on a stack machine, where instructions manipulate an operand stack
			 - In this implementation, checkpoints occur between regions
				 - Meaning, only when this stack is empty
	- Proposed solution could also be applied to Wasi-compliant components

---

## Links

%% Links to related literature. %%