# Runtime — JavaScript inside the observable VM

*Status: draft. Block:
[QuickBEAM](https://github.com/elixir-volt/quickbeam).*

## The blindness

Your Elixir application needs to render a Vue page server-side. The
standard answer is a Node sidecar — and now you operate two runtimes with
two failure modes, a serialization boundary between them, and a process
your supervisor cannot see. Everything about the arrangement works, and
nothing about it is observable: when the sidecar leaks, hangs, or dies,
your application learns about it the way you learn about a neighbor's
problems — through the wall.

The general form: JavaScript is unavoidable (SSR, reactivity, the entire
frontend ecosystem), and every mainstream way of running it next to
another runtime produces an opaque island with a network cable to it.

## The mechanism

QuickBEAM's answer: if you cannot get rid of JS, put it inside the
observable machine. JavaScript runtimes become OTP citizens:

```elixir
{:ok, rt} = QuickBEAM.start()                 # a supervised GenServer
{:ok, 3}  = QuickBEAM.eval(rt, "1 + 2")

{:ok, rt} = QuickBEAM.start(handlers: %{
  "db.query" => fn [sql] -> MyRepo.query!(sql).rows end
})
# JS calls Elixir through an explicit, named edge:
QuickBEAM.eval(rt, "const rows = await Beam.callSync('db.query', [sql])")
```

The properties that follow from *being* a BEAM process: supervision
(crash → restart, isolated), per-context heaps measured in tens of
kilobytes `[verify]` (one JS context per user/LiveView is affordable),
preemptive scheduling (a hot JS loop cannot starve the system), and Web
APIs backed by OTP machinery rather than reimplementation.

The trajectory is the more radical part. It began as an embedded
interpreter; it grew a pure-Elixir JavaScript frontend (lexer, parser,
bytecode compiler), with the official JavaScript conformance suite as the
referee; the current work compiles JS bytecode ahead-of-time *to BEAM
code* — JavaScript as, literally, another language on the VM. The
motivating benchmark was the surprise that justified the effort: on
numeric workloads, BEAM's JIT-compiled code beats the embedded
interpreter by integer factors `[verify numbers]`.

## What it proved

That server-side per-user JavaScript is sane: a reactive context per
LiveView process — the substrate the [One program](one-program.md) layer
stands on — costs kilobytes, restarts cleanly, and cannot take neighbors
down. That Vue's actual reactivity system runs on the BEAM unmodified.
And at 3,300+ commits `[verify]`, that the scope is a serious runtime
rather than a demo binding.

The fair comparison, stated both ways: polyglot-on-one-VM is a proven
thesis (GraalVM demonstrated the analyzability and tooling wins years
ago). The trade here is different — give up partial-evaluation JIT speed,
get supervision, microscopic isolated contexts, and preemptive
scheduling. For short, I/O-bound web workloads — SSR, reactivity graphs,
template expressions — that is the right trade. For compute kernels it is
the wrong one, and this page says so.

## Composition

The explicit `Beam.callSync` edge is the analyzability win: every
JS↔BEAM crossing is a visible call that [Causality](causality.md) can
trace — one dependence graph spanning both languages, against which
N-API-style opacity has no answer. [One program](one-program.md) runs
`<script setup>` reactivity and complex template expressions in QuickBEAM
contexts; [Build](build.md) prebundles its dependencies; the
[Agent](agent.md) layer inspects JS contexts the way it inspects any
process — because they are processes.

## Status and limits

Large, fast-moving, young-and-ambitious tier: conformance is climbing
slice by slice, the AOT compiler is in progress, and the honest scope
sentence is printed here: this is a web-workload runtime, not a Node
replacement — npm-ecosystem compatibility is bounded, and code that needs
V8-class throughput should run on V8. The committed conformance tier
follows the subset doctrine
([uncertainty 2](../uncertainties/README.md#2-the-compatibility-treadmill)).
