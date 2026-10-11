---
name: pragmatic-go-review
description: Use when reviewing Go code, diffs, or pull requests for correctness, maintainability, and explicit, pragmatic design. Trace operations end to end and produce grounded findings about dependencies, lifecycle, abstractions, errors, concurrency, and tests.
---

# Pragmatic Go Review

Review the requested scope. Propose changes as findings unless implementation is also requested.

## Workflow

1. Establish scope and intended behavior from repository instructions, callers, dependency construction, implementations, and tests.
2. Trace success, failure, cancellation, shared state, background work, and cleanup. Run focused checks to resolve uncertainty.
3. Apply the standard below. Ground findings in code and consequences: ambiguous ownership, needless navigation, scattered invariants, caller complexity, or a specific unprotected behavior. Recommend the smallest useful improvement; avoid arbitrary style thresholds.

## Standard

Make an operation easy to follow from entry point to result. Respect repository contracts and conventions; change a design to solve a concrete problem in scope.

- **Make ownership visible.** Wire dependencies at `main` or `run` with constructors and struct literals. Organize packages by capability and files by concern. Keep related policy, caching, and bookkeeping behind a cohesive API. Make it clear who starts background work, cancels it, waits, and closes resources.

- **Make abstractions earn their place.** Use concrete types by default. A boundary should simplify callers, own an invariant, isolate external I/O, or support a needed substitution or failure test. One implementation can be enough. Keep interfaces small and consumer-owned; cleanup methods belong with the cleanup owner. Prefer standard interfaces or functions when they fit.

- **Remove empty layers.** Let implementations satisfy consumer interfaces directly; align contracts you own. Retain adapters for meaningful conversion or external constraints. Use decorators for independently composable behavior. Share operations and invariants, not similar syntax; modest repetition is fine. Use generics for concrete reuse.

- **Keep the operation visible.** Preserve decision order. Use descriptive values, early error returns, blank lines, and short phase comments to expose execution and ownership. Prefer local clarity before extracting readability-only helpers; extract meaningful complexity. Expand lifecycle goroutines so starting work and waiting are distinct.

- **Preserve contracts.** Wrap errors with `%w` when adding context; present transport errors at the transport boundary. Explain non-obvious precedence, absence, freshness, and operational tradeoffs. Keep inferable execution flow in code rather than repeating it in documentation. Prefer the standard library; dependencies need a concrete correctness, maintenance, or measured performance benefit.

- **Make concurrency locally understandable.** Keep synchronization with the state it protects. Honor caller cancellation and deadlines.

- **Test the promised behavior.** Cover observable results, failures, and invariants. Use small boundary fakes, standard test servers, and focused fixtures; reuse suitable test doubles. Consolidate overlap while preserving distinct regressions. Synchronize deterministically or use `testing/synctest` where available. Check call counts when they prove a contract, such as avoiding reads on cache hits.

## Default findings format

Use the requested format, or list findings in impact order with priority (high, medium, low), title, `file:line`, evidence, consequence, and the smallest useful improvement.

Distinguish facts from assumptions, resolve uncertain claims, state material open questions, and consolidate shared root causes. A pattern alone is not a finding; inspect its consumers, invariants, and tests.

If there are no actionable findings, say so. Report verification and material behavior that remains unverified.
