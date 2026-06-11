# Architecture

*Status: draft. The visual half of the standard: how the blocks compose.*

Diagrams are [Mermaid](https://mermaid.js.org/) — they render on GitHub,
diff in PRs, and are corrected like prose (Track 1) when they drift from
reality. **The prose is the source of truth; a diagram that contradicts a
layer page is a bug in the diagram.**

- [The boundary map](map.md) — the one-screen picture: eleven boundaries, eleven
  blocks.
- [Package graph](packages.md) — every package, what depends on what, with
  links to the repositories.
- [The chain](chain.md) — design to production: one artifact changing
  representations.
- [Feedback loops](loops.md) — how failures travel back to the agent as
  witnesses.

## Conventions

- Nodes are named by **package name** (linking to the repo); clusters are
  named by **boundary** (linking to the layer page).
- Solid arrows = runtime/compile dependency ("uses"). Dashed arrows =
  produces/consumes an artifact ("feeds"). Thick arrows in `loops.md` =
  feedback (witness) paths.
- A diagram may show *less* than reality (omission for clarity is fine);
  it may never show *more* (no aspirational edges — unbuilt links belong
  in the [roadmap](../roadmap/README.md), or appear here dotted with an
  explicit "(planned)" label).
