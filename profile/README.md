# Fidelity Framework

**Native Clef Language compilation with preserved type and memory safety.**

<p align="center">
🚧 <strong>Early, active development</strong> — APIs and supported features may change significantly. 🚧<br>
<em>Active development now lives on the <a href="https://forge.spkez.dev/FidelityFramework">Forgejo server at forge.spkez.dev</a>. The repositories on GitHub are read-only mirrors, kept open for public review. We're not soliciting open contributions, but the work remains available to follow and inspect.</em>
</p>

## About

The Fidelity Framework was created by [Houston Haynes](https://www.linkedin.com/in/houstonhaynes/), founder and CEO of [SpeakEZ Technologies](https://speakez.tech). The vision:

> provide maximum degrees of freedom in producing intelligent hardware and software products that elevate performance, safety, and efficiency.

On a technical level it means direct native compilation without runtime dependencies. For our purposes, it translates to massive advantages in elegant design and ergonomics that yields deterministic memory without garbage collection and other negative aspects of managed runtime environments. The element that separates this framework from other tool chains is its memory safety that persists from source through compilation, as far as can be carried to the final binary.

Where other frameworks target managed runtimes or transpile to languages with their own memory models, Fidelity is designed to produce processor targeted applications and libraries with coherent memory management and compile-time safety guarantees. The name "Fidelity" reflects this mission: preserving type and memory safety from source code through compilation to native execution. The properties that made F# a reliable .NET platform language remain true when recast in the new "Clef" language to generate native applications and libraries.

Composer, the native AOT compiler at the heart of the Fidelity framework, uses a unique continuation-passing style (CPS) transformation and nanopass architecture. Rather than large monolithic compiler phases, the pipeline consists of many small, composable passes that each perform a single transformation. This design enables verification, optimization, and targeting flexibility.

### Learn More

For deeper discussion of the architectural thinking behind Fidelity, see [The Clef Language Site](https://clef-lang.com). Key articles include:

- [The Fidelity Framework: A Primer](https://speakez.tech/blog/fidelity-framework-a-primer/) — Overview of the framework's goals, components, and design philosophy
- [Delimited Continuations: Fidelity's Turning Point](https://speakez.tech/blog/delimited-continuations-fidelitys-turning-point/) — How CPS unifies async, actors, and native compilation
- [The WREN Stack](https://speakez.tech/blog/wren-stack/) — WebView + Reactive + Embedded + Native desktop applications
- [Dimensional Type Safety](https://speakez.tech/blog/dimensional-type-safety/) — How intrinsic units of measure enable multi-architecture targeting

## Projects

The framework spans the native compiler and its core libraries, the platform and runtime bindings, the WREN UI stack, and the Lattice editor tooling. Rather than mirror a repository list that drifts out of date as the framework grows, the canonical and always-current set lives on the Forgejo server:

### ▸ [Browse all Fidelity Framework projects on Forgejo →](https://forge.spkez.dev/FidelityFramework)

Good places to start:

| Project | Description |
|---------|-------------|
| **[Composer](https://forge.spkez.dev/FidelityFramework/Composer)** | The AOT compiler — from Clef through MLIR to native binaries |
| **[Clef](https://forge.spkez.dev/FidelityFramework/clef)** | The Clef compiler, core library, and language service (CCS) |
| **[BAREWire](https://forge.spkez.dev/FidelityFramework/BAREWire)** | Type-safe binary encoding, zero-copy memory operations, and IPC |
| **[Atelier](https://forge.spkez.dev/FidelityFramework/Atelier)** | The purpose-built editor for the ecosystem, built on the WREN Stack |

Each project name links to its canonical home on Forgejo, where development is active. The GitHub copies are read-only mirrors maintained for public review.

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
Alex (in Composer)       Platform-aware code generation
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

Composer orchestrates this pipeline. CCS handles parsing and type checking, resolving familiar F# types to their native representations. The Program Semantic Graph captures the full semantics of your program. Alex generates MLIR, and this is where degrees of freedom emerge. MLIR's dialect system allows targeting different backends without changing the upstream pipeline. In the early stages of platform maturity, we're focusing on LLVM to produce binaries. But MLIR opens the door to other backends as the ecosystem evolves. Tell us what you'd like to see!

## What Makes Fidelity Different

**Native types from the start.** When you write `string`, CCS resolves it to a UTF-8 native string, not `System.String`. When you write `Some x`, you get a stack-allocated value option, not a heap-allocated reference. The type system understands native semantics.

**Deterministic memory.** No garbage collector decides when resources are freed. Memory lifetimes are explicit. When a value goes out of scope, its resources are released immediately.

**Preserved safety.** The compile-time guarantees of F# carry through to the native binary. Type safety, memory safety, and resource management are enforced at compile time, not deferred to a runtime.

**Platform targeting.** The same source code can target Linux, macOS, Windows, embedded systems, or WebAssembly. Platform-specific details are defined by you in the `.fidproj` file and handled downstream by the compiler. And we have plans on our roadmap to address GPU, TPU and other accelerators and specialty processors.

**Intrinsic units of measure.** Non-numeric dimensional types are built into the compiler, not a library. Dimensional constraints flow through the entire compilation pipeline, informing code generation for CPUs, GPUs, FPGAs, and CGRAs.

## Origins

Fidelity is developed by [SpeakEZ Technologies](https://speakez.tech). The framework builds on several established foundations:

- **F# Compiler Services** from Microsoft provides parsing and type checking infrastructure
- **MLIR/LLVM** from the LLVM project provides the code generation backend
- **F\*** from Microsoft Research and INRIA informs the verification integration roadmap

## Licensing

SpeakEZ Technologies develops Fidelity toward commercialization while keeping the source open for inspection, learning, research, and curated community contribution. Licenses vary by project:

- **Dual-licensed (Apache 2.0 + Commercial).** The commercial-core components — Composer, BAREWire, Farscape, and Atelier — are available under Apache 2.0 for open-source use, with commercial licenses available for organizations requiring additional terms.
- **MIT.** Clef and its language specification (hard forks of dotnet/fsharp and the F# spec), XParsec, and the Lattice tooling.

Each repository declares its own license; refer to the repository for authoritative terms.

Certain compilation techniques in Composer are covered by pending patent US 63/786,264: "System and Method for Verification-Preserving Compilation Using Formal Certificate Guided Optimization."

## Contributing

Active development happens on the [Forgejo server](https://forge.spkez.dev/FidelityFramework); the GitHub repositories are read-only mirrors. We are not currently soliciting open contributions — a small number of contributors are invited to specific projects at our discretion. If the work interests you, the best way to engage is to follow it on Forgejo and start a discussion there. Documentation feedback and issue reports are always welcome.

## Status

All Fidelity projects are under active development. APIs are unstable. The framework is not yet suitable for production use. We're building in the open and appreciate patience as the work progresses.

---

<p align="center">
Copyright 2025–2026 SpeakEZ Technologies, Inc. All rights reserved.
</p>
