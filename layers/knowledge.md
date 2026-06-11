# Knowledge — the ecosystem, searchable by shape

*Status: draft. Block: [Exograph](https://github.com/elixir-vibe/exograph),
with QuackDB.*

## The blindness

An agent hand-rolls a pagination module. It is mediocre, subtly buggy,
and the third such module in the codebase — and the ecosystem it was
generated into contains at least three published packages that solve the
problem properly. The agent didn't look, because it *can't* look: its
training data is a year-old, blurry photograph of the ecosystem, and the
only live search it has is grep, which finds strings, not meaning.

The tool author has the same blindness from the other side: "is this
pattern I want to warn about actually rare in real code — or do I just
not read enough code?" Every linter author answers that question by
intuition, and ships the false positives that intuition produces.

## The mechanism

Exograph is CodeQL-class indexing for Elixir, local-first. It parses
source into normalized tables — files, AST fragments, definitions,
references, call edges, comments, *package versions* — inside plain
DuckDB files, and answers structural queries verified by exact AST
matching:

```elixir
{:ok, index} = Exograph.index("lib", repo: MyApp.QuackDBRepo)
{:ok, hits}  = Exograph.search(index, "Repo.transaction(fn -> _ end)")
```

The query is a *shape*, not a string — it finds every transaction-taking
function call regardless of formatting, variable names, or line breaks,
and no regex can express it.

The same machinery scales to the whole ecosystem:

```text
mix exograph.index.hex --mode top --limit 5000
```

downloads and indexes packages directly from Hex — one version per
package, the top N, or every version ever published — into sharded local
DuckDB files, on one machine, resumable. The entire public history of
Elixir code becomes a database you can ask questions of: *which private
functions call `Repo.transaction/1`, ecosystem-wide? How do the five most
downloaded packages shape this kind of API? Has anyone ever actually
written this pattern?*

## What it proved

That ecosystem-scale structural indexing is a laptop problem, not a
cluster problem — local files, no server, no service dependency
([manifesto §6](../manifesto/README.md#6--the-commitments), local-first).

And it changes what is checkable. The [Quality](quality.md) layer's
precision discipline — every rule earns a measured false-positive rate
before it may gate — gets its corpus here: a candidate smell rule is
scanned against *all published Elixir code* before promotion. "Validated
against the entire public ecosystem" is a sentence no other linter
ecosystem can currently write, and it is the difference between shipping
a hunch and shipping a measurement.

## Composition

Three roles. For [Quality](quality.md) and [Causality](causality.md): the
false-positive corpus. For the [Agent](agent.md): the
anti-hand-rolling oracle — before implementing anything nontrivial,
search the ecosystem for existing implementations and API shapes; ground
truth beats model memory, and the lookup is one eval call. For the wider
world: the index is becoming a remotely available MCP service, so *any*
agent — not just this stack's — can search Elixir structurally. The
library stays local-first; the service is a deployment of it, not a
dependency of yours.

## Status and limits

Working at full-ecosystem scale locally; the public MCP service is a
committed direction, not a shipped product — this page will change when
it ships, and the [roadmap](../roadmap/README.md) Phase 5 carries the
"done when" (a third-party agent with no stake in this stack, searching
Elixir structurally). Honest scope: Elixir only, by design — depth over
language count is the moat, not a limitation being apologized for.
