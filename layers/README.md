# The Map

*Status: draft.*

Ten boundaries where tools — and therefore agents — go blind today. Ten
building blocks, each removing one. Every block is useful alone; the
composition is the point.

*(The designed map figure lands here; the
[Mermaid source](../architecture/map.md) is the diffable original.)*

**[Design](design.md)** — a design is a node tree, not a picture. An
editor where the structure is the artifact: agents design through the
same small operations a designer uses, a linter makes the design system
executable, and changes become diffs. *OpenPencil.*

**[Time](time.md)** — what happened, replayable. Render is a pure
function of server state, so recording a session costs kilobytes and
replay is re-evaluation. Every session recorded; the unreproducible bug
retired as a category. *PhoenixReplay.*

**[Causality](causality.md)** — what depends on what, with proof. One
dependence graph — calls, data, effects, processes — answering "what
breaks if I change this?" with the path that proves it, and turning the
architecture diagram into a failing build. *Reach.*

**[Build](build.md)** — the toolchain lives inside the app. Bundler,
Tailwind, and package management as supervised parts of the application;
~40ms rebuilds; the browser's console flowing into the server's logs.
*Volt.*

**[Runtime](runtime.md)** — JavaScript inside the observable VM. JS
contexts as supervised processes: isolated, preemptively scheduled,
crash-safe, every JS↔BEAM crossing an explicit, traceable call.
*QuickBEAM.*

**[One program](one-program.md)** — no client/server agreement to break.
Vue components compiled to native server rendering, the client/server
split derived by a compiler from what the code does — not maintained by
hand across a network boundary. *Phoenix Vapor.*

**[Quality](quality.md)** — machine-generated code, machine-checked.
Structural search and rewrite, clone detection whose suggested fix is
computed rather than guessed, a linter for what AI over-produces, one
command wiring it all into a gate. *ExAST · ExDNA · ExSlop · vibe_kit.*

**[Knowledge](knowledge.md)** — the ecosystem, searchable by shape. Every
published package indexed into local files, queryable by AST structure:
find existing implementations instead of hand-rolling; measure a lint
rule against all public code before shipping it. *Exograph.*

**[Agent](agent.md)** — the worker is a supervised system. A tiny tool
surface over a stateful eval session inside the live runtime; the agent
composes APIs instead of accumulating tools, and its memory lives in
your project, not someone's cloud. *pi-elixir · Vibe.*

**[Deploy](deploy.md)** — a supervised path to a Linux box. Native
releases, programmable systemd, blue-green switching as OTP operations:
production without the container ceremony, inspectable to the last mile.
*systemdkit · unitctl · Xamal · Livery (emerging).*
