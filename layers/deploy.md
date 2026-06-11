# Deploy — a supervised path to a Linux box

*Status: emerging — every piece below runs today; the composed story is
design, not shipped product. This page is detailed about what and why,
and explicit about what is unproven.*

Blocks: Xamal, xamal_proxy, Livery, systemdkit, unitctl. `[verify: orgs
and Hex status at publish time]`

## The blindness

The application that ran all month as `mix phx.server` now needs to reach
a server, and the standard path is a cliff: a Dockerfile, an image
registry, a compose file or an orchestrator, an ingress, and a CI
pipeline that builds an artifact whose insides nobody can introspect.

The container solved "works on my machine" by making *every* machine
opaque. That trade is often right — and for this stack it is a regression
at the last mile: a program whose entire thesis is inspectability ends
its life inside a box that can only be observed from outside. The
boundary between a working prototype and a production deployment is the
last place where the thesis currently dies — which is exactly the
[manifesto §4](../manifesto/README.md#4--the-method) criterion for what
gets attacked next.

## The mechanism

The thesis — call it **containerless**: a Phoenix application whose
toolchain already lives inside it ([Build](build.md) removed the Node
island) does not need a container around it. It needs a *supervised path
to a Linux box*. The primitives are the original ones — native releases,
systemd, an edge proxy — and the new fact is that all of them are now
programmable from Elixir:

- **Deploys, Kamal-style but Elixir-native (Xamal):** Mix tasks and
  `config/xamal.exs` instead of YAML; the artifact is the BEAM release
  itself — already self-contained, already the thing containers were
  wrapping; TLS and routing by Caddy rather than a proxy container.
- **Traffic switching as an OTP application (xamal_proxy):** blue-green
  cutover, health checks, and rollback as supervised operations that
  return structured results — not YAML rituals observed from outside.
- **systemd as an API, not an incantation (systemdkit, unitctl):** unit
  files written, validated, installed, started, and *queried* from
  Elixir over D-Bus — Docker-like process controls from pure systemd
  primitives. The process manager becomes one more thing an agent can
  ask.
- **One handler set, every HTTP (Livery):** a BEAM-native server
  framework serving HTTP/1.1, HTTP/2, and HTTP/3 from a single router
  and middleware stack — owning the server layer so that streaming and
  push semantics stay under the same observability as everything above.

Why no Docker, argued rather than asserted: the release is already the
deployment artifact (containers around BEAM releases mostly wrap a thing
that needed no wrapping); the isolation that matters for this workload —
process supervision, restart policy, resource limits — is what systemd
provides natively; and opacity is the one cost this program refuses on
principle. Where containers are genuinely required (someone else's
platform, someone else's policy), nothing here forbids them — this is
the *default* path being rebuilt, not a prohibition.

The composition this enables — stated as design, marked unproven: the
agent that wrote the code inspects the *production* service over the
same primitives it used in development. Deploy, watch the health checks,
read the witness when one fails, roll back — from the same eval session,
with the same discipline:
every operation returns *where, why, and what now*.

## What it proved

So far: each piece runs — releases deploy, traffic switches, units
install and report, all three HTTP versions serve from one handler set.
What is *not* yet proved is the composed one-command story and its
ergonomics under failure, which is precisely the
[roadmap](../roadmap/README.md) Phase 5 exit: a fresh project reaching a
supervised production deployment with no Docker and no Node.

## Composition

This layer completes [the chain](../architecture/chain.md): design →
compile → run → record → deploy, every link inspectable. It is also,
deliberately, the first customer of the program's own service ambitions —
the [Knowledge](knowledge.md) layer's public search service will deploy
on this stack, eating the dogfood at the layer where dogfood is scarcest.

## Status and limits

The youngest layer, and the page models the uncertainty discipline the
standard demands of itself: detailed about what and why, explicit that
the composition is unproven, and carrying its own falsifier — if the
composed path cannot match container workflows on rollback safety and
fleet ergonomics for its target case (one app, a few boxes), the thesis
retreats to "a good option for simple deployments," and this page will
say so.
