---
author: Fujii D., Matsubara K., Nakata Y.
year: 2024
tags:
  - webassembly
  - live-migration
pdf_link: "[[Stateful_VM_Migration_Among_Heterogeneous_WebAssembly_Runtimes_for_Efficient_Edge-cloud_Collaborations.pdf|Open PDF]]"
---
# Stateful VM Migration Among Heterogeneous WebAssembly Runtimes for Efficient Edge-cloud Collaborations

## TLDR

%% One-sentence summary of the core/claim contribution. %%

The paper aims to investigate the challenges and determine the feasibility of implementing runtime-neutral VM migration.

## Thoughts

%% Thoughts and questions about the paper. %%

- While migration of Wasm binaries across different runtimes is challenging, LEO satellite networks are said to be homogeneous -> Therefore having the same runtime on these devices seems feasible

## Related Work

%% Existing state-of-the-art research. %%

## Contributions

%% Research gap and novel contributions. %%

Motivation: 

- VM migration allows seamless and flexible workload offloading among edge devices and clouds.

Challenges:

1. VM migration across heterogeneous hardware (in general)
2. Reduce file size and migration time

WebAssembly seems suitable for addressing both these challenges:

1. It abstracts away heterogeneous platforms
2. It is more lightweight than container and VM runtimes

**However**, migrating [[Virtual Machines (VMs)|VMs]] across different [[WebAssembly]] runtimes is difficult since the standard doesn't specify the implementation design of internal runtimes very well.

Contributions:

- Introduce workload offloading among heterogeneous Wasm runtimes
- Clarify the feasibility of runtime-agnostic Wasm VM migration
- Discuss technical issues related to implementation
- Discuss how to improve runtime neutrality on checkpointing/restoring (C/R) method
- Evaluate the effectiveness of workload offloading with runtime switching
- Migration prototype between WAMR (edge device) and WasmEdge (cloud) runtimes

### Feasibility of Runtime-Neutral VM Migration

1. Prototypical implementation and discussion of technical challenges
2. Investigation on how to improve the runtime neutrality

#### Prototyping

VM checkpointing and translation:

- Program state of Wasm consists of components that change during execution in the Wasm runtime structure
	- These components need to be written to the state checkpoint
	- The program state implementation differs depending on Wasm runtime
		- Paper checkpointed the program state in runtime-independent format
			- Allows for migration across different runtimes
- Wasm runtime structure consists of (see Wasm core specs):
	- Module instance
		- Collects definitions for:
			- Types
			- Functions
			- Tables
				- Data structures that store the host environment contents outside of Wasm
				- Not subject to Wasm migration
				- Not considered by the paper
			- Memories
			- Globals
	- Program counter
	- Stacks
		- Value stack
		- Control stack
		- Frame stack

Memories and globals:

- For WasmEdge and WAMR there is no difference in implementation for memories and globals
	- Paper checkpoints them without translation between runtimes
- According to Wasm spec, memories are linear and increase page by page
	- Memory can be restored by extracting only used contiguous pages (not the whole region)
- Globals are an ordered list capable of storing 32-bit, 64-bit, 128-bit types
	- The type is specified for each index: List can be restored entirely
- Paper only checkpoints areas of memory that have actually been written
	- That way we have smaller checkpoint size, reducing transmission/restoration overheads
	- Done by using OS kernel dirty memory detection feature
		- Dirty memory is memory written to at least once

Program counter:

-  Program counter holds the address of the next Wasm instruction to be executed
	- It's an absolute address within the process
- Two challenges:
	- Enable restoration of absolute process addresses in different OSs and processes
		- Because program counter is absolute address, it can't be restored directly during migration
			- Paper address this by adding a feature to translate the instruction address into a pair of (function index, offset from beginning address of function)
				- Function index is a unique value that's assigned to each function in Wasm
			- Idea is to calculate difference from beginning address of function to see if it can be restored
	- Accommodate the differences in instruction implementation pointed to by the program counter between heterogeneous runtimes
		- WasmEdge: Instructions are objects that contain values like opcode, args, ...
			- Instruction sequence consists only of instruction objects
		- WAMR: Instructions aren't objectified
			- Instruction sequence consists of mixed instructions and args
		- Because of runtime-specific differences, offsets pointing to the same instruction assume different values depending on the runtime
		- Solution: Add functionality to translate each offset mutually
			- That way, the translation can be computed in advance
			- Enables implementation independent from the runtime

Value stack:

- Stores Wasm values
- Wasm spec specifies value types tobe 32, 64, 128 bit size
- WasmEdge: All elements have 128 bits so that all types of values can be stored in single element
	- 0 padding with < 128 bit type
- WAMR: 32 bits for each element, values with > 32 bits are stored in multiple 32bit increments
- To restore different layouts for migration, need to keep internal stack type information
	- If we know the value types, we can work with them instead of the implementation specific element sizes
		- Problem: Wasm runtime doesn't have access to internal types of value stack
		- Solution: Type stack was implemented that manages types of values in value stack
			- Note: Execution overhead because value and type stacks are processed a lot
			- Implementation features generated type table for runtime type information

Control stack:

- Manages labels (for jump instructions)
- Goto instructions (jumps, branches, loops) can only move to labelled addresses
- Labels can be generated by specific instructions (such as block instruction)
	- Label is a block instruction
	- Jumps executed inside a block can move to the end of the block
- WasmEdge: Doesn't implement control stack, instead pre-calculates instruction addresses to which all jumps move and stores them in an instruction object
	- Enables jumps without control stack
- WAMR: Implements control stack
- Migration:
	- Challenge: We don't want to implement control stack for WasmEdge because overhead
	- Solution: Utilize program counter to restore control stack
		- Analyze Wasm code in function sequentially from start to program counter
			- Check if instruction is a block instruction and resolve it using push/pop operations
			- Enables stack migration without overhead

Frame stack:

- Holds context of function for each call, this includes:
	- Value stack
	- Control stack
	- Return address (program counter)
- Migrating the frame stack is done by combining value stack, control stack and return address

#### Discussion

- Saving execution states to memory or globals is achievable by using Wasm-instructions for saving
- Can also implement function outside runtime to abstract away implementation differences
- Using a type table the value stack can be abstracted
	- Wasm has two instructions store/load to store/retrieve elements of the value stack to/from memory
- Control stack can also be made agnostic using program counter and return address (instruction address)
	- So, if instruction address can be made runtime-agnostic -> so can the control stack
- Frame stack consists of value stack, control stack, return address
	- If all three can be made agnostic -> so can the frame stack

### Future Work

- For now, VM migration depending on runtime-internal state was performed
	- VM state might depend on external state (e.g. network sockets kept in OS kernel)
		- Migration of runtime external state not tackled
- Migration of stateful VMs optimized with proprietary code optimization and converted with JIT/AoT

## Methodology

%% Architecture of the solution. %%

## Implementation

%% Actual implementation of the solution. %%

## Evaluation

%% The evaluation of the solution. %%

---

## Links

%% Links to related literature. %%