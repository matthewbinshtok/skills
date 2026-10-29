---
name: pragmatic-docs
description: >-
  Use when writing or reviewing durable developer documentation, including READMEs,
  package guides, API usage documentation, and runbooks. Focus on purpose, practical
  usage, public contracts, and operational behavior.
---

# Pragmatic Docs

Write for future readers who have not seen the conversation. Treat task requirements
as acceptance criteria; include them in documentation when they help the reader use,
operate, or understand the component.

## Write for the reader's task

Identify the reader and what they need to accomplish. For usage documentation, explain
the purpose, show a minimal useful example, and describe non-obvious constraints.

Cover relevant ownership, lifecycle, failure behavior, configuration, defaults, and
limits. Keep caller-specific setup with the caller. Prefer a focused example over an
implementation tour.

## Put knowledge where it belongs

Use READMEs and guides to explain usage and how the pieces fit together. Keep
symbol-level contracts near their declarations; prefer Go doc for Go APIs.

Keep implementation journals, design negotiations, extraction checklists, and one-off
verification results in work notes or PRs. Retain lasting rationale and reusable
operational procedures when they support future decisions or actions.

## Check the result

Verify examples and claims against the actual API and behavior. Preserve important
constraints when shortening prose.

Remove content whose only purpose is demonstrating that a task requirement was
followed. Each section should answer a future reader's question.
