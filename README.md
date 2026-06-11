# Building Blocks for the Future Web

*A living standard for a verifiable web stack.*

This repository is a document, not a codebase. It states the thesis behind
the [Elixir Vibe](https://github.com/elixir-vibe),
[Elixir Volt](https://github.com/elixir-volt), and
[OpenPencil](https://github.com/open-pencil) ecosystems: what is being
built, why, what is already proven, and what remains uncertain — maintained
continuously, in the open, like a living standard.

There are no versions and no release dates. The document evolves with the
work; the commit history is the changelog. If a section is wrong, [open an
issue](../../issues) against the prose — the prose is the product here.

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
introduces". The voice of the document is maintained by its author; factual
corrections are maintained by everyone.

## License

CC BY 4.0 (prose). Code samples: MIT.
