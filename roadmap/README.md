# Roadmap

*Status: draft. Phases ordered by dependency. No dates, by design — each
phase states what becomes true and how you will know it is done.*

## Why this order

A verification ecosystem whose own gates are untrustworthy is a joke at its
own expense — so trust comes first. Witnesses come before new capabilities,
because a failure you cannot act on is noise. The design-to-production chain
closes before the semantic layer deepens, because the chain is the thesis.
Services and self-improvement come last because they stand on everything
else.

## Phase 1 — Make the gates trustworthy
Continuous integration and zero-flake suites across every package;
experiments consolidated into canonical implementations; renderer caching
under written invariants.
**Done when:** a red check means a real problem, on every repo, every time.

## Phase 2 — Every failure carries its witness
Structured diagnostics across build, lint, and design; node-tree diffs for
design changes; false-positive rates measured against the public ecosystem
before any rule may gate work — and rules demoted in public when they cry
wolf.
**Done when:** an agent can act on any failure in the stack without asking
a human where it happened.

## Phase 3 — Close the design-to-production chain
One component model from canvas to production: templates edited visually,
behavior as typed capabilities, server logic in plain Elixir; the editor
preview and the production renderer become the same pipeline; design
artifacts rendered, diffed, and indexed server-side. And the end-to-end
demonstration: designed on canvas → compiled → deployed → used → session
recorded → a regression caught by replay → localized by the dependence
graph → repaired.
**Done when:** that demonstration is a public recording anyone can rerun.

## Phase 4 — Specs become oracles
Property-based testing and doctests in the standard gates; agent workflows
that propose properties before implementations; gradual set-theoretic types
by default, with type errors as precise counterexamples; concurrency
checking seeded from recovered process topology.
**Done when:** a feature ships spec-first end to end, and the type checker
is a routine source of counterexamples.

## Phase 5 — The ecosystem as a service
The Hex-wide structural code index available remotely over MCP to any
agent; "find the existing implementation" as default agent behavior; the
containerless deployment story composed and documented end to end.
**Done when:** a third-party agent with no stake in this stack searches
Elixir structurally — and a fresh project reaches a supervised production
deployment with no Docker and no Node.

## Phase 6 — The loop closes
Defects that slip the gates become candidate checks, validated against the
ecosystem corpus, admitted only by measured precision; a standing public
benchmark — the same application modified round after round, charting
defect escape rate, blast radius, and architectural drift; design-process
recording: the path, not the snapshot.
**Done when:** the check suite grows from its own escapes, and the drift
chart exists for anyone to inspect.
