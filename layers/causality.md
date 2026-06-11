# Causality — what depends on what, with proof

*Status: draft. Block: [Reach](https://github.com/elixir-vibe/reach).*

## The blindness

Before touching old code, every developer asks the same question: *what
breaks if I change this?* The tooling answer, in every mainstream stack,
is grep, hope, and a senior engineer's memory.

The deeper version of the blindness is architectural. Every team has a
diagram — layers, boundaries, "web never talks to the repo directly."
The diagram lives in a wiki. Six months later the diagram is fiction,
because nothing ever checked it: conformance was a code-review virtue,
and code review under time pressure approves one convenient import at a
time until the architecture is an archaeological claim about how the
system once was. For an AI generating code at machine speed, "review
will catch it" is not a plan.

## The mechanism

Reach builds a dependence graph of the program — control flow, data flow,
call graph, effects, and OTP process relationships — and answers
questions against it with evidence:

- **Impact:** "what depends on `Accounts.update_user/2`?" — callers,
  callees, and the transitive cone, before you edit.
- **Slicing:** "show me everything this value's history touches" — the
  classical backward slice, computed where it actually works (more below).
- **Taint:** "can user input reach a shell command, a raw query, a file
  write?" — flow from sources to sinks across module boundaries.
- **Architecture as code:** a policy file in the repo —

  ```elixir
  # .reach.exs
  layers: [
    web: ~r/^MyAppWeb\./,
    domain: ~r/^MyApp\.(Accounts|Billing)/,
    infra: ~r/^MyApp\.Repo/
  ],
  forbidden: [web: :infra]
  ```

  — checked in CI. The wiki diagram becomes a failing build.
- **Witnesses everywhere:** every finding carries its `--why` path — the
  chain of edges that proves it. Findings are advisory by default;
  gating is opt-in, and the philosophy is strict about the difference:
  analysis surfaces review leads; only checks that essentially never lie
  may block a merge.

The honest lineage paragraph: none of these algorithms are new. Program
slicing is the 1980s; dependence graphs and architecture conformance
models are the 1990s; the citations are in
[the lineage](../lineage/README.md#dependence-and-slicing) and in the
source. The new part is the substrate. In mainstream languages these
analyses drown in alias
analysis — *do these two references overlap?* — and forty years of
research made that question expensive and approximate. In a language with
immutable data, the question barely exists: value flow is term flow, and
a backward slice is close to the true cone of cause. On a runtime where
concurrency is explicit processes and supervision trees, the
architecture is *syntactically recoverable* — processes, messages, and
restart strategies are in the code, so "check the concurrency structure"
is parsing, not heroics. Same algorithms, better world, strictly better
output.

## What it proved

Reach gates its own ecosystem: every repository in this program runs its
architecture and smell checks in CI, which means the analyzer is
continuously tested against the codebases of the people most motivated to
notice when it lies. And its rules are held to the precision discipline
described in [Quality](quality.md): a new smell rule needs a measured
false-positive scan against real code before it may complain — including,
at the limit, the whole public ecosystem via [Knowledge](knowledge.md).

## Composition

Reach is the eyes of the stack. The [Agent](agent.md) layer queries it
before editing (impact sets), during review (boundary violations), and
after changes (what got riskier). [Quality](quality.md) consumes its
evidence layer. The graph extends across languages: every JavaScript↔BEAM
crossing in the [Runtime](runtime.md) layer is an explicit call edge, so
one dependence graph spans both worlds — a value can be traced from a
template expression through a JS computation back into a server function.
And the natural next partner is [Time](time.md): *what* happened at
t=840ms is the recording; *why* is a slice.

## Status and limits

The analyses are stable and in daily CI use. The honest limit is
incrementality: today the whole-program graph is rebuilt rather than
maintained, which caps the comfortable project size; sub-second
warm-cache impact analysis on large codebases is the open engineering
frontier, and it is named in the [roadmap](../roadmap/README.md) rather
than assumed.
