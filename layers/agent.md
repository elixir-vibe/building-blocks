# Agent — the worker is a supervised system

*Status: draft. Blocks:
[pi-elixir](https://github.com/elixir-vibe/pi-elixir) (the product),
[Vibe](https://github.com/elixir-vibe/vibe) (the lab).*

## The blindness

The typical agent session is a chat log that vanishes: no supervision, no
inspection, no memory. The agent that fixed your bug yesterday
re-discovers your project's shape this morning. Its work happens through
a keyhole — run a shell command, read the text that came back — and when
it spawns parallel work, the parallel work is invisible until it returns
or doesn't.

There is a second, subtler failure: tool sprawl. The industry's answer to
agent capability is more tools — forty bespoke functions, each flat, none
composable, all of them burning context window just by existing. The
agent that needs to *combine* two capabilities discovers they don't
combine.

## The mechanism

Two artifacts, one philosophy, an honest division of labor.

**pi-elixir** (the product) bridges a mature coding agent into the BEAM.
The model-facing surface is three tools — evaluate Elixir, search by AST
pattern, rewrite by AST pattern — and the smallness is the design. The
eval is a *door*, not a function: a stateful session inside the running
application, where bindings persist between calls like a notebook, and
where every other capability is a composable Elixir API rather than a
tool —

```elixir
# one eval session, composing freely:
Repo.all(from u in User, where: ..., limit: 5)   # query the live DB
CodeMap.context("MyApp.Accounts.update_user/2")  # dependence graph
Pi.logs(grep: "browser", tail: 50)               # captured logs (incl. browser console)
Pi.Docs.module(Ecto.Changeset) |> Pi.Docs.search("validate")
```

— with results rendered as tables and trees, not strings. The model sees
three tools; the agent wields a hundred APIs. Tool sprawl is a context
tax; APIs compose for free.

**Vibe** (the lab) is the fully BEAM-native agent: sessions are
supervised processes, subagents have lifecycles you can monitor and
cancel, storage and telemetry are inspectable, and the agent's own
infrastructure can be examined and patched — by the agent — under the
strictest gate regime in the ecosystem. It exists to answer "what does an
agent look like when the harness itself is an OTP application?", and its
proven ideas flow into the product. Declaring which is which — product
and lab, in public — is itself part of the method: it is how one person
runs a stable surface and a moving experiment without pretending they are
the same thing.

Stateful eval changes how agents behave, not just how fast: compute once,
inspect, refine — instead of re-deriving state through repeated shell
commands. The agent works like a Livebook user, not a command cannon.

## What it proved

Eval-as-control-plane works, and the proof is unusually thorough: **this
entire ecosystem was built by agents using it** — under its own gates,
with its own analyzers, on its own runtime. Yes, agents wrote much of
this code; under the strictest verification regime its author knows how
to build. That regime is the product. The ecosystem is its own
longest-running integration test, and the dogfood loop is load-bearing:
the analyzer gates the analyzer's repository; the agent that extends the
agent is checked by the checks it extends.

## Composition

The agent layer is the consumer of everything else — the hub of the
[feedback loops diagram](../architecture/loops.md). It queries
[Causality](causality.md) before edits, runs under
[Quality](quality.md) gates, reads [Build](build.md)'s browser-console
witnesses, walks [Time](time.md)'s recordings, searches
[Knowledge](knowledge.md) instead of hand-rolling, and inspects
[Runtime](runtime.md) JS contexts as ordinary processes. Memory is
local-first by commitment: sessions and analytics live in plain DuckDB
files inside the project — the agent's history is queryable by the agent
(and by you), hosted by no one.

## Status and limits

pi-elixir is released, versioned, and stable; Vibe is experimental by
declaration and stays that way until it earns otherwise. The protocol
between agent and runtime is stabilizing toward a documented contract.
Honest limit: the agent layer inherits every blind spot of the layers it
stands on — where a boundary below is still open (see
[One program](one-program.md), [Deploy](deploy.md)), the agent guesses
there like everyone else. That is precisely why the
[roadmap](../roadmap/README.md) is ordered the way it is.
