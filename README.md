# Building Blocks for the Future Web

*A living standard for a web stack that AI agents can verify, not just
generate.*

## What is this?

AI can generate a design, a frontend, an API, a schema — and each piece
looks plausible while the whole quietly disagrees with itself. The tools
that could catch the disagreement don't exist, because the web stack is
split by boundaries no analyzer can see across: design vs. code, client
vs. server, JavaScript vs. everything hosting it, the build vs. the app,
what happened vs. what you can inspect.

This document describes a stack built to remove those boundaries — so
that software written by AI (and by people) can be *checked*: every
failure carrying its location and cause, every session replayable, every
dependency traceable, every design reviewable as a diff. The thesis in
one line: **don't wait for smarter models — build the environment that
pushes back.**

It is not a vision deck. Most of the blocks are shipped, open-source
packages in daily use — a Figma-compatible design editor
([OpenPencil](https://github.com/open-pencil/open-pencil)), a LiveView
session recorder, a whole-program dependence analyzer, a no-Node.js
frontend toolchain, a JavaScript runtime inside the Erlang VM, a
Vue-to-LiveView compiler, AI-aware linters, an ecosystem-wide structural
code search, and a coding agent that lives inside the running
application. They are built on the Erlang VM, used on each other, and
useful one at a time. This repository is the document that explains how
they compose and where it's all going.

**Who is this for?**

- *Evaluating one tool?* Jump to its [layer page](#layers) — each
  explains what the tool sees that others can't, how it works, and its
  honest limits.
- *Wondering why these projects exist at all?* Read the
  [manifesto](manifesto/README.md) — fifteen minutes, no project names
  until the end.
- *Want the big picture fast?* [The map](layers/README.md) is one
  screen.
- *Deciding whether to bet on this?* Read the
  [roadmap](roadmap/README.md) and — especially — the
  [uncertainties](uncertainties/README.md).

## Why a "living standard"?

This repository is a document, not a codebase — maintained the way
living standards are: no versions, no release dates, continuously
corrected, with the commit history as the changelog. If a claim is wrong,
[open an issue](../../issues) against the prose; the prose is the product
here. The code lives in the
[Elixir Vibe](https://github.com/elixir-vibe),
[Elixir Volt](https://github.com/elixir-volt), and
[OpenPencil](https://github.com/open-pencil) organizations — this is the
plot they share.

## The document

Start with the manifesto, or jump to any layer — every page stands alone.

### [Manifesto](manifesto/README.md)

The declaration: AI generates the surface of software without its
structure; environments, not bigger models, are the answer; boundaries are
where verification dies; the method is to remove them.

### [The Map](layers/README.md)

One screen: ten boundaries, ten building blocks, how they compose.

### Layers

| Boundary | Block | Page |
|---|---|---|
| Design | a design is a node tree, not a picture | [layers/design.md](layers/design.md) |
| Time | what happened, replayable | [layers/time.md](layers/time.md) |
| Causality | what depends on what, with proof | [layers/causality.md](layers/causality.md) |
| Build | the toolchain lives inside the app | [layers/build.md](layers/build.md) |
| Runtime | JavaScript inside the observable VM | [layers/runtime.md](layers/runtime.md) |
| One program | no client/server agreement to break | [layers/one-program.md](layers/one-program.md) |
| Quality | machine-generated code, machine-checked | [layers/quality.md](layers/quality.md) |
| Knowledge | the ecosystem, searchable by shape | [layers/knowledge.md](layers/knowledge.md) |
| Agent | the worker is a supervised system | [layers/agent.md](layers/agent.md) |
| Deploy | a supervised path to a Linux box | [layers/deploy.md](layers/deploy.md) |

### [Architecture](architecture/README.md)

The visual half: the boundary map, the full package graph with repository
links, the design-to-production chain, and the feedback loops that carry
failures back as witnesses. Diagrams are Mermaid — they diff in PRs and
are corrected like prose.

### [Roadmap](roadmap/README.md)

Phases ordered by dependency, no dates. Each phase states what becomes true
and how you will know it is done.

### [Uncertainties](uncertainties/README.md)

What is unproven, what is a bet, and what would falsify each claim. A
standard that hides its doubts is marketing.

## Status of this document

Early. Sections marked *draft* are drafts; sections marked *settled* have
been stable long enough to link to. Every layer page carries its own status
line — the document applies its own honesty rules to itself.

## Contributing

Issues and PRs against the prose are welcome — especially "this claim is
wrong" with evidence, and "this section assumes knowledge it never
introduces". Two tracks: **corrections** (facts — everyone's) and
**proposals** (what the standard says — discussed first). The voice of the
document is maintained by its author. See [CONTRIBUTING.md](CONTRIBUTING.md)
for the rules and the issue templates for the entry points.

## License

CC BY 4.0 (prose). Code samples: MIT.
