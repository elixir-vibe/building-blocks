# One program — no client/server agreement to break

*Status: draft. Block:
[Phoenix Vapor](https://github.com/elixir-volt/phoenix_vapor), with vize.*

## The blindness

Somewhere in a component there is a `fetch("/api/users")`. Somewhere
else — another directory, another language, sometimes another team —
there is a route handler. Between them is a contract that exists nowhere:
the shape one expects is the shape the other returns, until one of them
changes. The industry's best answer is to generate types from schemas —
that is, to *document the boundary harder*. The boundary itself remains,
and with it the entire failure class: mismatched shapes, divergent error
handling, auth that one side assumes and the other forgets.

For generated code the failure class is the norm, not the edge:
generate the five layers independently and they agree the way strangers
agree.

## The mechanism

Phoenix Vapor removes the agreement by removing the second program. A
Vue single-file component compiles to a native LiveView module — one
source, both halves derived from it.

The interesting part is *how the split is decided*. You write ordinary
Vue:

```vue
<script setup>
const props = defineProps(["contacts"])      // ← server data
const search = ref("")                       // ← client state
const filtered = computed(() =>              // ← reads both
  props.contacts.filter(c => c.name.includes(search.value)))
function deleteContact(id) { "use server" }  // ← server action
</script>
```

and the compiler **classifies every binding by what the code does**:
`defineProps` → LiveView assigns (server-owned); `ref()` → client
reactivity (instant, zero network); a `computed` reading both → runs on
the client, reacts to either source; a function writing to a prop → a
server action, auto-detected; `"use server"` → the explicit escape hatch
for the ambiguous rest. Roughly 90% of components need zero annotations.
Nobody decides "this is the client half" — the compiler derives it. The
classifier is the design.

Four modes form a gradient, and **mode is an inference, not a
commitment**: a sigil for Vue-syntax templates with zero client JS;
server-side reactivity (the `<script setup>` runs in a
[Runtime](runtime.md) context, still zero client JS); hybrid (server owns
data, client owns UI state, ~50KB of real Vue on the client `[verify]`);
and a full Vue runtime server-side for third-party component libraries.
A component migrates between modes as its bindings change, without
rewrites.

Underneath, templates compile to the same statics/dynamics split LiveView
diffs on — Vue's Vapor-mode compilation maps onto it precisely because
both reduce rendering to fine-grained effects on stable structure (this,
not tribal preference, is why Vue rather than React). Server logic stays
plain Elixir: the LiveView module is yours, the `.vue` file only
generates `render/1` and event-name stubs.

## What it proved

So far: the pipeline is real end-to-end — Vue templates render as native
`%Rendered{}` structs with the wire efficiency of hand-written LiveView;
Vue's reactivity runs server-side per-process; the classifier splits real
components correctly. What it has *not* yet proved is adoption of the
hybrid mode by people with no stake in this program — and this page says
so plainly, because the graveyard is real: one-program-across-tiers has
been tried (Links, Ur/Web, Opa, Meteor — brilliant, dead), and the
uniform cause of death was demanding developers abandon their language
and ecosystem. The bet here is the inversion: the surface is the most
adopted component format alive, and the multitier machinery is
underneath. A bet, stated as one —
[uncertainty 1](../uncertainties/README.md#1-the-multitier-graveyard).

## Composition

This is the keystone block. Upstream, the [Design](design.md) layer's
node tree serializes to exactly the SFC subset this compiles — which is
what makes "the editor preview *is* the production renderer" possible,
and what makes the shared component model the program's hardest open
design question
([uncertainty 3](../uncertainties/README.md#3-the-component-model-is-unsettled)).
The `<script setup>` halves run in [Runtime](runtime.md) contexts;
[Build](build.md) serves and hot-reloads it; and because the rendering
stays a pure function of assigns, [Time](time.md) records hybrid sessions
the same way — narrowing the client-state blind spot rather than widening
it.

## Status and limits

The youngest core block and the most ambitious claim in the program.
Tiers are explicit per the subset doctrine: the sigil and the
`<script setup>` reactive subset are commitments; the full-Vue-runtime
mode is a demo. Two of the six
[uncertainties](../uncertainties/README.md) qualify this page — by
design, the most-hedged page in the standard.
