# Time — what happened, replayable

*Status: draft. Block:
[PhoenixReplay](https://github.com/elixir-vibe/phoenix_replay).*

## The blindness

"It broke when I clicked the thing." The bug report arrives with a
screenshot of the aftermath and a timestamp; the logs show the symptom;
nobody can replay the cause. The developer's options are archaeology or
divination.

The industry's answer is client-side session recording — capture DOM
mutations in the browser, ship them somewhere, play them back. It works,
at a price: a recording bundle in every page, megabytes per session,
privacy review for everything the DOM contains. The price means sampling.
Sampling means the session you need is, with high probability, the
session you didn't record.

## The mechanism

One property of the substrate makes the whole problem cheap. LiveView
templates are pure functions: the same assigns produce the same HTML.
There is nothing to record on the client — no DOM, no video, no
JavaScript — because the rendered page is *derivable* from the server
state that produced it.

So the recorder attaches as a server-side hook:

```elixir
live_session :default, on_mount: [PhoenixReplay.Recorder] do
  live "/dashboard", DashboardLive
end
```

and captures the only thing that matters: the event stream and the
assigns deltas at each state transition. A recording is typed data —

```elixir
%PhoenixReplay.Recording{
  view: MyAppWeb.DashboardLive,
  events: [
    {0,    :mount,   %{params: %{...}}},
    {840,  :event,   %{name: "search", params: %{"q" => "ali"}}},
    {841,  :assigns, %{results: [...]}},   # delta only
    ...
  ]
}
```

— and replay is re-evaluation: feed the recorded assigns back through the
original view, frame by frame, in the shipped dashboard with a scrubber,
or programmatically. A thirty-second session with active form input is
roughly four hundred events and ~8 KB on disk `[verify]`. At kilobytes
per session, you record *everything* and discard the sessions with no
interaction; the unreproducible bug stops existing as a category, because
every session is a reproduction.

## What it proved

Omniscient debugging — replaying any moment of any execution — is a
twenty-year-old research dream that was always crushed by recording cost:
general time-travel debuggers record execution itself, and execution is
enormous. The delta here: record *inputs to a pure function* instead, and
the dream costs four hundred tuples. The substrate, not cleverness, made
an impossible thing nearly free.

It also proved that recordings-as-typed-data changes who can use them. A
human uses the scrubber; an agent walks the events list, finds the exact
event where an assign diverged from expectation, and re-renders that
moment to verify a fix — debugging becomes querying, with no browser in
the loop.

## Composition

Recordings are the [Agent](agent.md) layer's ground truth for interactive
bugs: instead of guessing from source, walk the timeline. Each recording
is also a latent regression test — re-render the same timeline against
*changed* code and diff the HTML; a divergence is a witness naming the
exact event and assign where rendering changed. (Golden replays; landing.)
When an agent drives the app through browser automation, the session it
generates becomes a recording it can then inspect structurally — the
[feedback loop diagram](../architecture/loops.md) shows this edge. The
[Causality](causality.md) layer is the natural partner for the next
question: not just *what* changed at t=840ms, but *why*.

## Status and limits

Young but shaped: the recorder, storage, sanitization, and dashboard
ship today. The replay-as-oracle workflows (golden replays, agent-facing
query helpers) are the active frontier. Honest scope: this works because
LiveView rendering is pure — it is a LiveView instrument, not a universal
one, and sessions dominated by client-side state live partly outside its
sight (see [One program](one-program.md) for how that boundary is being
narrowed).
