---
name: pragmatic-go
description: Use when writing, implementing, fixing, or refactoring Go code. Apply explicit dependency wiring, cohesive components, justified abstractions, linear control flow, clear lifecycle ownership, and behavior-focused tests.
---

# Pragmatic Go

## Workflow

1. Read repository instructions, nearby code, `go.mod`, and relevant tests. Establish required behavior and trace dependency construction, resource ownership, and background work.
2. Implement a scoped change using the standard below. Preserve observable behavior when refactoring; update tests where existing coverage is insufficient.
3. Apply the abstraction check, run `gofmt`, required checks, and relevant tests. Use race-enabled tests for synchronization or goroutine lifecycle changes. Report behavior and verification performed.

## Style standard

Write explicit, pragmatic Go. Optimize for a reader following an operation from entry point to result.

Respect established repository contracts and conventions. Change an existing design only to address a concrete problem in scope, not solely to match these defaults.

### Structure and composition

- Wire dependencies explicitly at the composition root, usually `main` or `run`, using ordinary constructors and struct literals. Keep the dependency graph visible.
- Give components meaningful responsibilities. A store can own source access, caching, and associated bookkeeping behind a small API. Extract a layer when it makes the caller simpler or supports an actual substitution.
- Organize by capability. Use packages such as `authentication`, `keys`, and `cryptography`. Within a cohesive package, split files by operation or concern; a file boundary does not require another interface or architectural layer.
- Keep lifecycle ownership explicit. Make it clear who starts background work, cancels it, waits for completion, and closes resources.

### Interfaces and abstractions

- Use concrete types by default. Introduce small interfaces where a consumer needs a replaceable capability—for example, a database reader, key provider, or external client.
- Define interfaces near their consumers, with only the methods those consumers need. Prefer an existing standard-library interface or a function when it fits.
- Require a present-day reason for each abstraction. Good reasons include isolating external I/O, supporting existing implementations, centralizing a meaningful invariant, or testing failure behavior.
- Use decorators selectively. They fit independently composable behavior. Local caching or instrumentation can remain inside the component that owns it when that produces clearer code.
- Allow modest repetition. Keep distinct business paths recognizable. Extract shared code when it represents the same operation or invariant, rather than merely similar syntax.
- Use generics for concrete reuse. Keep type parameters out of code that becomes clearer with ordinary types and functions.

### Implementation

- Write operations as linear sequences. Use descriptive intermediate values, early validation, and early error returns. Keep the successful path easy to scan.
- Prefer standard-library solutions. Add dependencies when they materially improve correctness, maintainability, or measured performance.
- Preserve useful error identity. Add context with `%w` when wrapping errors; handle transport-specific error presentation at the transport boundary.
- Use comments to make the code navigable. Introduce meaningful phases in longer functions, explain component responsibilities, and document behavioral contracts, invariants, and operational reasoning. Brief "what happens next" comments are useful when they orient the reader; avoid narrating individual statements or merely repeating names.
- Make concurrency understandable locally. Keep synchronization and the state it protects together. Honor caller cancellation and deadlines.

### Tests

- Prefer small handwritten fakes at dependency boundaries; reuse suitable existing test doubles.
- Test observable behavior, failure paths, and meaningful invariants.
- Assert dependency call counts when they establish a contract, such as avoiding a source read on a cache hit.

## Before finishing

For each new interface, wrapper, helper, or package, ask:

1. What current problem or meaningful complexity does this abstraction address?
2. Does it make the operation easier to understand than a direct implementation?

Remove abstractions that cannot justify their existence.
