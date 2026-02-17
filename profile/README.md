# Fidelity Framework

**Native F# compilation with preserved type and memory safety.**

<p align="center">
🚧 <strong>Under Active Development</strong> 🚧<br>
<em>These projects are in early development and may see significant changes to supported features and APIs.</em>
</p>

## About

The Fidelity Framework was created by [Houston Haynes](https://www.linkedin.com/in/houstonhaynes/), founder and CEO of [SpeakEZ Technologies](https://speakez.tech). The vision:

> provide maximum degrees of freedom in producing intelligent hardware and software products that elevate performance, safety, and efficiency.

On a technical level it means direct native compilation without runtime dependencies. For our purposes, it translates to massive advantages in elegant design and ergonomics that yields deterministic memory without garbage collection and other negative aspects of managed runtime environments. The element that separates this framework from other tool chains is its memory safety that persists from source through compilation, as far as can be carried to the final binary.

Where other frameworks target managed runtimes or transpile to languages with their own memory models, Fidelity is designed to produce processor targeted applications and libraries with coherent memory management and compile-time safety guarantees. The name "Fidelity" reflects this mission: preserving type and memory safety from source code through compilation to native execution. The properties that made F# a reliable .NET platform language remains true when recast to generate native applications and libraries.

Composer, the native AOT compiler at the heart of the Fidelity framework, uses a unique continuation-passing style (CPS) transformation and nanopass architecture. Rather than large monolithic compiler phases, the pipeline consists of many small, composable passes that each perform a single transformation. This design enables verification, optimization, and targeting flexibility.

### Learn More

For deeper discussion of the architectural thinking behind Fidelity, see the [The Clef Language Site](https://clef-lang.com). Key articles include:

- [The Fidelity Framework: A Primer](https://speakez.tech/blog/fidelity-framework-a-primer/) — Overview of the framework's goals, components, and design philosophy
- [Delimited Continuations: Fidelity's Turning Point](https://speakez.tech/blog/delimited-continuations-fidelitys-turning-point/) — How CPS unifies async, actors, and native compilation
- [The WREN Stack](https://speakez.tech/blog/wren-stack/) — WebView + Reactive + Embedded + Native desktop applications
- [Dimensional Type Safety](https://speakez.tech/blog/dimensional-type-safety/) — How intrinsic units of measure enable multi-architecture targeting

## Core Projects

| Repository | Description |
|------------|-------------|
| **[Composer](https://github.com/FidelityFramework/Composer)** | AOT compiler orchestrating the pipeline from Clef through MLIR to native binaries |
| **[Clef](https://github.com/FidelityFramework/clef)** | Clef Compiler Services (CCS) |
| **[clef-lang-spec](https://github.com/FidelityFramework/clef-lang-spec)** | Normative specification for native Clef type and memory semantics |
| **[BAREWire](https://github.com/FidelityFramework/BAREWire)** | Type-safe binary encoding, zero-copy memory operations, and IPC |
| **[Farscape](https://github.com/FidelityFramework/Farscape)** | C/C++ header parsing for generating native library bindings |

## WREN Stack & Tooling

| Repository | Description |
|------------|-------------|
| **[Atelier](https://github.com/FidelityFramework/Atelier)** | Purpose-built editor for the Fidelity ecosystem, built on the WREN Stack |
| **[Fidelity.Toml](https://github.com/FidelityFramework/Fidelity.Toml)** | TOML 1.0 compliant parser built with XParsec |
| **[FStarHelloWorld](https://github.com/FidelityFramework/FStarHelloWorld)** | Sample F*/F# project proof of concept |

## IDE Support (Lattice)

Lattice IDE tooling for Clef development (evolved from Ionide):

| Repository | Description |
|------------|-------------|
| **[ClefAutoComplete](https://github.com/FidelityFramework/ClefAutoComplete)** | Clef language server using Language Server Protocol |
| **[lattice-vscode](https://github.com/FidelityFramework/lattice-vscode)** | VS Code plugin for Clef development |
| **[lattice-vim](https://github.com/FidelityFramework/lattice-vim)** | Vim plugin for Clef based on LSP |
| **[lattice-vscode-helpers](https://github.com/FidelityFramework/lattice-vscode-helpers)** | Common helpers for VS Code plugins |
| **[lattice-analyzers](https://github.com/FidelityFramework/lattice-analyzers)** | Native code analyzers for Lattice |
| **[Clef.Analyzers.SDK](https://github.com/FidelityFramework/FSharp.Native.Analyzers.SDK)** | SDK for building custom analyzers for Clef / CAC |

## Library Forks

Libraries integrated into the fsnative ecosystem as either compiler intrinsics or deeply integrated tools:

| Repository | Description |
|------------|-------------|
| **[fsil](https://github.com/FidelityFramework/fsil)** | F# inline generic library — default inlining semantics for fsnative |
| **[FSharp.UMX](https://github.com/FidelityFramework/FSharp.UMX)** | Units of measure for primitive non-numeric types — intrinsic to fsnative's type universe |
| **[XParsec](https://github.com/FidelityFramework/XParsec)** | Parser combinator library for F# |
| **[FStar](https://github.com/FidelityFramework/FStar)** | Proof-oriented language that contributes ideas on how to use SMT with Clef |

## How It Works

```
Clef Source Code
    |
    v
CCS (clef)          Type checking with native type resolution
    |
    v
Program Semantic Graph   Rich intermediate representation
    |
    v
Alex (in Firefly)        Platform-aware code generation
    |
    v
MLIR                     Multi-Level IR with dialect flexibility
    |
    v
Backend                  LLVM, or other targets as the ecosystem matures
    |
    v
Native Binary            Standalone executable, library, unikernel
```

Firefly orchestrates this pipeline. FNCS handles parsing and type checking, resolving familiar F# types to their native representations. The Program Semantic Graph captures the full semantics of your program. Alex generates MLIR, and this is where degrees of freedom emerge. MLIR's dialect system allows targeting different backends without changing the upstream pipeline. In the early stages of platform maturity, we're focusing on LLVM to produce binaries. But MLIR opens the door to other backends as the ecosystem evolves. Tell us what you'd like to see!

## What Makes Fidelity Different

**Native types from the start.** When you write `string`, FNCS resolves it to a UTF-8 native string, not `System.String`. When you write `Some x`, you get a stack-allocated value option, not a heap-allocated reference. The type system understands native semantics.

**Deterministic memory.** No garbage collector decides when resources are freed. Memory lifetimes are explicit. When a value goes out of scope, its resources are released immediately.

**Preserved safety.** The compile-time guarantees of F# carry through to the native binary. Type safety, memory safety, and resource management are enforced at compile time, not deferred to a runtime.

**Platform targeting.** The same F# code can target Linux, macOS, Windows, embedded systems, or WebAssembly. Platform-specific details are defined by you in the `.fidproj` file and handled downstream by the compiler. And we have plans on our roadmap to address GPU, TPU and other accelerators and specialty processors.

**Intrinsic units of measure.** Non-numeric dimensional types are built into the compiler, not a library. Dimensional constraints flow through the entire compilation pipeline, informing code generation for CPUs, GPUs, FPGAs, and CGRAs.

## Origins

Fidelity is developed by [SpeakEZ Technologies](https://speakez.tech). The framework builds on several established foundations:

- **F# Compiler Services** from Microsoft provides parsing and type checking infrastructure
- **MLIR/LLVM** from the LLVM project provides the code generation backend
- **F\*** from Microsoft Research and INRIA informs the verification integration roadmap

## Licensing

While SpeakEZ Technologies intends to develop this platform for commercialization, we provide open licensing for several reasons: to make source available for inspection and learning, to support further research in compiler architecture and formal verification, and to enable community contributions back to the systems from which we draw inspiration.

Projects in the Fidelity Framework use different licenses appropriate to their nature:

| Project | License |
|---------|---------|
| Firefly, BAREWire, Farscape, Atelier | Apache 2.0 + Commercial dual license |
| fsnative | MIT (hard fork of dotnet/fsharp) |
| fsnative-spec | MIT (hard fork of fsprojects/fsharp-spec) |
| FStar | Apache 2.0 (fork of FStarLang/FStar) |
| fsil, FSharp.UMX, XParsec | MIT |
| Lattice forks | MIT |

The dual-licensed projects are available under Apache 2.0 for open source use. Commercial licenses are available for organizations requiring additional terms.

Certain compilation techniques in Firefly are covered by pending patent US 63/786,264: "System and Method for Verification-Preserving Compilation Using Formal Certificate Guided Optimization."

## Contributing

We welcome community contributions. Each repository contains its own contribution guidelines. General principles:

- Issues and discussions help shape the framework's direction
- Pull requests should target specific, well-defined improvements
- Documentation improvements are always appreciated
- Please review the license terms before contributing

For substantial changes, please open an issue first to discuss the approach.

## Status

All Fidelity projects are under active development. APIs are unstable. The framework is not yet suitable for production use. We're building in the open and appreciate patience as the work progresses.

---

<p align="center">
Copyright 2025 SpeakEZ Technologies, Inc. All rights reserved.
</p>
