---
name: pragmatic-review
description: >-
  Use when reviewing a change in any language or configuration: a diff, a pull request, or a stack
  of them. Decide whether it is correct, safe to roll out, and clear to the next reader, and report
  grounded findings. Language-specific review guidance adds its own standard.
---

# Pragmatic Review

Review as a staff engineer would: decide whether the change should merge in this shape, and what
would have to be true for it to. Report findings to the person who asked. Edit the code, post
comments, or approve only when asked.

## Workflow

1. Establish the scope and the claim. Read the description and every changed file; for a stack,
   read every change in order. Read the revision under review, since a local checkout may be
   behind it.
2. Check the claim against the code. Trace each statement the description relies on to the caller,
   configuration, or generated output that makes it true. Note anything the change does that the
   description leaves out.
3. Apply the standard below with the design and language guidance that fits the files.
4. Resolve uncertainty with a focused check before reporting it. Report what stays unverified.

## Standard

Judge consequences for the system, its operators, and the next reader. Respect repository
conventions; a preference is not a finding.

- **Fit the design.** Ask what would have been built had this requirement been there from the
  start. A branch, flag, or second copy added beside an existing design is a finding when a
  simpler shape exists. Prefer the version that removes something first.

- **Give each decision one owner.** A rule, default, or prerequisite repeated in several places
  drifts. Name where it belongs. Validate data where it enters and rely on it afterwards.

- **Keep it easy to follow.** Count the layers between a question and its answer, and the state a
  reader must hold. Wrappers that only forward and state that could be derived are findings when
  they cost the reader something concrete.

- **Test what callers observe.** A useful test fails when the behavior breaks and passes when only
  the internals change. Check that the cases a change claims to handle have one.

- **Make the rollout safe.** Consider mixed versions, partial deployment, rollback, and the
  callers and data already in place. Name the signal that shows the change working and the one
  that shows it failing; a change nobody would notice breaking is a finding.

- **Serve the reviewer.** In a stack, each change should be understandable and verifiable alone,
  in an order that proves the work. Its description says why it exists and names the gaps a later
  change fills.

## Findings

Lead with a verdict for each change: approve, approve with follow-ups, or request changes. Then
list findings in impact order with priority (high, medium, low), title, `file:line`, evidence,
consequence, and the smallest useful improvement.

Ground each finding in the code and what goes wrong because of it. Consolidate findings that share
a root cause, separate facts from assumptions, and leave out nits unless asked. If nothing is
actionable, say so.

Close with what was verified and what was not. When reviewing a revision again, start from the
earlier findings and check each against the new revision.
