# Quality — machine-generated code, machine-checked

*Status: draft. Blocks: [ExAST](https://github.com/elixir-vibe/ex_ast),
[ExDNA](https://github.com/elixir-vibe/ex_dna),
[ExSlop](https://github.com/elixir-vibe/ex_slop),
[vibe_kit](https://github.com/elixir-vibe/vibe_kit).*

## The blindness

The AI-generated pull request passes every existing check. It compiles.
Tests are green. The formatter is content. It is still wrong: a blanket
`rescue` swallowing the one error that mattered, a third copy of an
existing helper with two variables renamed, a comment narrating what the
next line visibly does, an N+1 query wearing idiomatic clothes. Reviewing
it feels like grading middle-school math homework — at some point you
start doubting your own sanity and re-reading the basics.

Classical linters cannot see this, and the reason is structural: they
encode what *human experts reject*, and human experts were never tempted
to write narrator comments or rescue everything. The failure distribution
changed; the checks didn't.

## The mechanism

Four tools, one system.

**ExAST** — structural search and replace, where patterns are plain
Elixir: variables capture, `_` is a wildcard, pipes are normalized. The
difference between grep and structure in one line: `Enum.take(_, -_)`
finds the *bug class* (negative take, whatever the arguments look like),
not a string. Rewrites ship as previewed plans with conflict detection —
the repair half of the witness/repair loop.

**ExDNA** — clone detection on the AST, so `fn a, b -> a + b end` and
`fn x, y -> x + y end` are the same code. Type I/II/III taxonomy,
multi-clause awareness (duplicated pattern-matching heads are caught as a
unit), cross-file grouping. The distinctive part: the suggestion is
*computed*, not heuristic. The detector anti-unifies the clone family into its
[least general generalization](../lineage/README.md#clones-and-generalization)
— the algebraically canonical common shape — so the recommended
extraction (with a generated name like `build_changeset`) is the
mathematically right answer to "what should the shared function be."
Detection tools that stop at detection leave the hard half to you; here
the finding arrives with its repair.

**ExSlop** — a genuinely new category: a linter for what the *generator*
over-produces. Forty checks for patterns LLMs emit and experienced
Elixir developers don't — blanket rescues, narrator docs, try/rescue
around functions that don't raise, identity passthroughs, anti-idiomatic
Enum chains. Classical linting encodes the expert's taste; this encodes
the model's bias. It lints the distribution, not the language.

**vibe_kit** — one installer wiring all of it, plus Credo strict and
Dialyzer, into a single gate:

```text
mix igniter.new my_app --install vibe_kit   # or igniter.install into existing
mix ci   # compile --warnings-as-errors · format · test
         # credo --strict (+ ExSlop) · dialyzer
         # ex_dna --max-clones 0 · reach checks
```

Note the clone budget: zero. These repositories live under it, which
changes how code grows — extraction stops being a someday-refactor and
becomes the path of least resistance.

The system-level rule that makes the pile a system: **checks earn their
place** ([manifesto §6](../manifesto/README.md#6-the-commitments)). The
reason is specific to agents: a human shrugs off a false positive; an
agent *complies* with it, contorting correct code to satisfy a wrong
check — slop-avoidance becoming its own slop. So gates and advisories are
asymmetric by design: gates must essentially never lie; advisories must
earn >90% usefulness or be demoted — a regime with
[industrial prior art](../lineage/README.md#precision-discipline).
Precision is measured against real code at ecosystem scale — see
[Knowledge](knowledge.md) for how.

## What it proved

That a zero-clone budget is livable on real, fast-moving codebases. That
structural rewriting beats regex decisively for agent-driven refactors
(the agent states intent as a pattern, not a string-surgery script). And
that linting the generator's distribution finds real defects that every
classical linter, by construction, walks past.

## Composition

This layer is the immune system the [Agent](agent.md) loop runs inside:
generate → gates refute with located findings → repair → re-check.
[Causality](causality.md) supplies the architecture and effect evidence;
[Knowledge](knowledge.md) supplies the false-positive corpus; ExAST is
also the agent's everyday search/rewrite instrument, exposed directly as
tools. Design lint findings ([Design](design.md)) follow the same
witness discipline — one philosophy, two artifact kinds.

## Status and limits

Stable and in daily gating use across the ecosystem. The visible next
step is the public precision dashboard — per-check usefulness measured on
the ecosystem corpus, with demotions in the changelog. The honest limit:
these checks are syntactic-to-structural; semantic and concurrency
checking are the [roadmap](../roadmap/README.md)'s Phase 4, not today's
inventory.
