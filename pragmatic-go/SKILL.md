---
name: pragmatic-go
description: Use when writing, implementing, fixing, or refactoring Go code. Apply explicit dependency wiring, cohesive components, justified abstractions, linear control flow, clear lifecycle ownership, and behavior-focused tests.
---

# Pragmatic Go

## Workflow

1. Read repository instructions, nearby code, `go.mod`, and tests. Establish the required behavior, dependency construction, and lifecycle ownership.
2. Implement the scoped change using the standard below. Preserve observable behavior when refactoring; cover meaningful gaps in existing tests.
3. Check that added abstractions earn their place. Run `gofmt`, required checks, and relevant tests; use race-enabled tests for synchronization or goroutine lifecycle changes. Report behavior and verification.

## Standard

Make an operation easy to follow from entry point to result. Respect repository contracts and conventions; change a design to solve a concrete problem in scope.

- **Make ownership visible.** Wire dependencies at `main` or `run` with constructors and struct literals. Organize packages by capability and files by concern. Keep related policy, caching, and bookkeeping behind a cohesive API. Make it clear who starts background work, cancels it, waits, and closes resources.

- **Make abstractions earn their place.** Use concrete types by default. A boundary should simplify callers, own an invariant, isolate external I/O, or support a needed substitution or failure test. One implementation can be enough. Keep interfaces small and consumer-owned; cleanup methods belong with the cleanup owner. Prefer standard interfaces or functions when they fit.

- **Remove empty layers.** Let implementations satisfy consumer interfaces directly; align contracts you own. Retain adapters for meaningful conversion or external constraints. Use decorators for independently composable behavior. Share operations and invariants, not similar syntax; modest repetition is fine. Use generics for concrete reuse.

- **Keep the operation visible.** Preserve decision order. Use descriptive values, early error returns, blank lines, and short phase comments to expose execution and ownership. Prefer local clarity before extracting readability-only helpers; extract meaningful complexity. Expand lifecycle goroutines so starting work and waiting are distinct.

- **Preserve contracts.** Wrap errors with `%w` when adding context; present transport errors at the transport boundary. Explain non-obvious precedence, absence, freshness, and operational tradeoffs. Keep inferable execution flow in code rather than repeating it in documentation. Prefer the standard library; dependencies need a concrete correctness, maintenance, or measured performance benefit.

- **Make concurrency locally understandable.** Keep synchronization with the state it protects. Honor caller cancellation and deadlines.

- **Test the promised behavior.** Cover observable results, failures, and invariants. Use small boundary fakes, standard test servers, and focused fixtures; reuse suitable test doubles. Consolidate overlap while preserving distinct regressions. Synchronize deterministically or use `testing/synctest` where available. Check call counts when they prove a contract, such as avoiding reads on cache hits.
