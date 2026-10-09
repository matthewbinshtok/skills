---
name: pragmatic-go-review
description: Use when reviewing Go code, diffs, or pull requests for correctness, maintainability, and explicit, pragmatic design. Trace operations end to end and produce grounded findings about dependencies, lifecycle, abstractions, errors, concurrency, and tests.
---

# Pragmatic Go Review

Review the requested scope. Propose changes as findings unless implementation is also requested.

## Workflow

1. Read repository instructions and establish scope and intended behavior. Read relevant callers, dependency construction, implementations, and tests alongside the code under review.
2. Trace success and failure paths, including errors, cancellation, shared state, background work, and cleanup. Inspect test coverage; run focused checks when useful to resolve a concern.
3. Apply the standard and abstraction check below. Ground findings in code and concrete consequences. For clarity or maintainability findings, identify ownership ambiguity, unnecessary navigation, scattered invariants, or caller complexity. For missing-test findings, identify the specific unprotected behavior.

## Style standard

Write explicit, pragmatic Go. Optimize for a reader following an operation from entry point to result.

Respect established repository contracts and conventions. Recommend changing an existing design only to address a concrete problem in scope, not solely to match these defaults.

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

## Abstraction check

For each interface, wrapper, helper, or package in scope, inspect its consumers, implementations, invariants, and tests, then ask:

1. What current problem or meaningful complexity does this abstraction address?
2. Does it make the operation easier to understand than a direct implementation?

An external I/O boundary or a need to test failures can justify an interface with a single implementation. A finding needs more evidence than the presence of a particular pattern.

## Default findings format

Use the requested format, or list concise findings in impact order with a priority (high, medium, or low), title, `file:line`, evidence and consequence, and the smallest useful improvement.

Distinguish facts from assumptions, resolve uncertain claims where possible, state material open questions, and consolidate findings with the same root cause.

If there are no actionable findings, say so. Briefly report verification performed and any material behavior that remains unverified.
