# JavaScript: WebAssembly

Date: Dec 18, 2020 12:15 AM
Property: https://blog.sessionstack.com/how-javascript-works-a-comparison-with-webassembly-why-in-certain-cases-its-better-to-use-it-d80945172d79
Tags: Javascript, Log, Notes, Study

## Webassambly (wasm)

- WebAssembly is an efficient, low-level bytecode for the web.
- WASM allows you to use languages other than Javascript (C, C++, Rust or other languages)
- Write program -> compile (ahead of time) -> WebAssembly

## Loading time

- To load Javascript, browser has to load all the .js files (textual)
- WebAssembly is faster to load because only the already compiled wasm files have two be transported.
- it is very compact binary format

## V8 Pipleline

![JavaScript%20WebAssembly%2068633e25d72441f5958be81ac246d036/Untitled.png](JavaScript%20WebAssembly%2068633e25d72441f5958be81ac246d036/Untitled.png)

## WASM

![JavaScript%20WebAssembly%2068633e25d72441f5958be81ac246d036/Untitled%201.png](JavaScript%20WebAssembly%2068633e25d72441f5958be81ac246d036/Untitled%201.png)

- WASM has already gone through optimization during compilation.
- Parsing no need -> Optimized binary that can directly hook into the backend that can generate machine code.

## Memory model

![JavaScript%20WebAssembly%2068633e25d72441f5958be81ac246d036/Untitled%202.png](JavaScript%20WebAssembly%2068633e25d72441f5958be81ac246d036/Untitled%202.png)

- Memory of a C++ program (compiled into WebAseembly), is a contiguous block of memory with no holes it it.
- Execution stack is separate from the linear memory.