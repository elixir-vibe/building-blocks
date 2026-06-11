# Surface — the operational state, one queryable plane

*Status: emerging — the metadata core, LiveView adapter, and playground
run today; the analytics, replay, and agent verticals are designed and
partially built. This page is explicit about which is which. The
repository ([dannote/incant](https://github.com/dannote/incant), moving
to the Elixir Vibe organization) is being opened.*

## The blindness

Every layer below this one produces observable data and none of them
owns a screen. Session recordings accumulate, web analytics fill a
database, LLM calls leave traces and costs, the dependence graph knows
what calls what, the agent's sessions and decisions are on disk — and
the moment someone asks "why did conversions drop on Tuesday?", the
answer is spread across five inspection surfaces that don't know about
each other.

The standard answer is to build an admin. And the admin is where
verification quietly dies a second death: it is a *second application*
that re-describes the first one — its schemas re-declared in a dashboard
builder, its metrics re-implemented in a BI tool, its operations
re-exposed through hand-written forms. Every re-description drifts. The
admin shows what someone remembered to wire up, weeks ago, when the
schema looked different. No analyzer can see across the boundary between
an application and the tools built to operate it, because the tools
re-state the application instead of deriving from it.

## The mechanism

[Incant](https://github.com/dannote/incant) is an Elixir/Phoenix-native
control plane built on one rule: **the admin is derived from the
application, so it cannot drift from it.** Compile-time DSLs over the
resources the application already has — Ecto schemas, queries, datasets,
telemetry — emit inspectable metadata:

```elixir
defmodule MyApp.Admin.Resources.Product do
  use Incant.Resource,
    schema: MyApp.Catalog.Product,
    repo: MyApp.Repo

  table do
    column :name, link: true
    column :total, format: :money
    filter :status, :select
  end
end
```

The macro does not generate an opaque UI; it produces *metadata you can
query* — which a default LiveView adapter renders as tables, forms,
filters, dashboards, and drilldowns. Ecto remains the language of data
truth: the DSL describes admin intent, never a parallel schema. Beyond
records, `dataset` blocks describe analytical workloads — dimensions,
metrics, computed expressions — for the data the other layers produce:
analytics events, replay sessions, LLM usage, agent telemetry.

The consequence that earns this layer its place in the standard: **the
same metadata that renders a table for a human is a tool surface an
agent can call.** One definition, two consumers. A chat panel inside the
plane gives the agent the operator's exact vocabulary — resources,
datasets, charts, selected rows — and its mutations go through the same
declared operations as a human's clicks, witnessed the same way, gated
by the same approvals. The agent is not bolted onto the admin; the
admin's semantic model *is* the agent's API.

## What it proved

So far: the metadata core compiles real resources, the LiveView adapter
renders them, an Igniter installer (`mix incant.install`) wires a
Phoenix app, and a playground application exercises realistic catalog
and LLM-domain resources, an operations dashboard, and a theme contract.

What is *not* yet proved is the composition this page exists to claim:
several layers' data on one screen — application records next to web
analytics next to replayable sessions, drilling from a metric to the
sessions behind it. That demo is the layer's exit criterion, and until
it exists this page's central claim is design, not receipt.

## Composition

Surface is where the other layers become visible to a person:
[Time](time.md)'s recorded sessions appear as resources you filter and
replay; [Knowledge](knowledge.md)'s code facts become queryable
datasets; the [Agent](agent.md) layer meets the plane twice — its own
sessions and trajectories as data to inspect, and the chat panel as a
place to act. The drilldown chain this composes — funnel metric →
sessions that fell out → trace that served them → code that emitted the
event — is the standard's "every failure carries its location and
cause" promise, walked backward through one UI.

## Status and limits

Young and honest about it: pre-release, the DSL surface is still
moving, and the analytics/replay/agent verticals are the roadmap, not
the changelog. The falsifier for this layer: if deriving admin surfaces
at compile time cannot match the flexibility operators actually need —
if real operations work keeps escaping to hand-built LiveViews — then
the thesis retreats to "a very good Elixir admin generator," and the
unified-plane claim fails. The escape hatches (full LiveView islands
inside the plane) are deliberate: the bet is that derivation covers the
long tail, not that it forbids exceptions.
