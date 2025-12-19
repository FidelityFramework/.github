# Fidelity Framework

**Native F# compilation with preserved type and memory safety.**

<p align="center">
🚧 <strong>Under Active Development</strong> 🚧<br>
<em>These projects are in early development and not intended for production use.</em>
</p>

## About

F# has several compilation targets. The standard compiler produces .NET assemblies for the Common Language Runtime. [Fable](https://fable.io/) transpiles F# to JavaScript, TypeScript, Rust, Python, and other languages for web and cross-platform development. [WebSharper](https://websharper.com/) provides full-stack F# web applications. Fidelity adds another option: native binaries without a runtime or garbage collector.

Where other compilers target managed runtimes or transpile to languages with their own memory models, Fidelity produces standalone executables with deterministic memory management and compile-time safety guarantees. The name "Fidelity" reflects this mission: preserving type and memory safety from source code through compilation to native execution. The properties you rely on in your F# code remain true in the generated binary.

Firefly, the AOT compiler at the heart of Fidelity, uses a continuation-passing style (CPS) transformation and nanopass architecture. Rather than large monolithic compiler phases, the pipeline consists of many small, composable passes that each perform a single transformation. This design enables verification, optimization, and targeting flexibility. For deeper discussion of the architectural thinking behind Fidelity, see the [SpeakEZ blog](https://speakez.tech/blog/).

## Projects

| Repository | Description |
|------------|-------------|
| **[Firefly](https://github.com/FidelityFramework/Firefly)** | AOT compiler orchestrating the pipeline from F# source through MLIR to native binaries |
| **[Alloy](https://github.com/FidelityFramework/Alloy)** | Native standard library providing BCL-sympathetic APIs without runtime dependencies |
| **[BAREWire](https://github.com/FidelityFramework/BAREWire)** | Type-safe binary encoding, zero-copy memory operations, and IPC |
| **[Farscape](https://github.com/FidelityFramework/Farscape)** | C/C++ header parsing for generating native library bindings |
| **[fsnative](https://github.com/FidelityFramework/fsnative)** | F# Native Compiler Services (FNCS) providing native type resolution |
| **[fsnative-spec](https://github.com/FidelityFramework/fsnative-spec)** | Normative specification for native F# type semantics |
| **[FStar](https://github.com/FidelityFramework/FStar)** | Proof-oriented language with adaptations for fsnative integration |
| **[fsil](https://github.com/FidelityFramework/fsil)** | F# inline generic library |
| **[FSharp.UMX](https://github.com/FidelityFramework/FSharp.UMX)** | Units of measure for primitive non-numeric types |

## How It Works

```
F# Source Code
    |
    v
FNCS (fsnative)          Type checking with native type resolution
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

**Platform targeting.** The same F# code can target Linux, macOS, Windows, embedded systems, or WebAssembly. Platform-specific details are defined by you in the `.fidproj` file and handled downstream by the compiler.

## Origins

Fidelity is developed by [SpeakEZ Technologies](https://speakez.tech). The framework builds on several established foundations:

- **F# Compiler Services** from Microsoft provides parsing and type checking infrastructure
- **MLIR/LLVM** from the LLVM project provides the code generation backend
- **F\*** from Microsoft Research and INRIA informs the verification integration roadmap

## Licensing

Projects in the Fidelity Framework use different licenses appropriate to their nature:

| Project | License |
|---------|---------|
| Firefly, Alloy, BAREWire, Farscape | Apache 2.0 + Commercial dual license |
| fsnative | MIT (hard fork of dotnet/fsharp) |
| fsnative-spec | MIT (hard fork of fsprojects/fsharp-spec) |
| FStar | Apache 2.0 (fork of FStarLang/FStar) |
| fsil, FSharp.UMX | MIT |

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
