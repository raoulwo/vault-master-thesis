---
tags:
  - webassembly
---
# WebAssembly

WebAssembly (Wasm) was initially proposed for the web to improve the performance of JavaScript and enable high-performance web applications. It's a *binary instruction format* for a *stack-based virtual machine* (VM).

Although initially proposed for the web, it expanded to other domains, such as serverless computing. To develop Wasm applications, developers can write code in high-level programming languages and compile the programs into Wasm binaries to be executed. Outside the web, Wasm relies on a *Wasm runtime* for execution. Wasm binaries run in a *sandbox* environment, they can't directly interact with the underlying operating system. Instead, the programs interact via the *WebAssembly System Interface* (WASI).

Two types of Wasm compilers:

- Just-in-Time (JIT)
- Ahead-of-Time (AoT)

---

## Links

%% Links to related concepts. %%

- [[@zhang2025]]