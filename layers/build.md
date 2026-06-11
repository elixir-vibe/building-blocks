# Build — the toolchain lives inside the app

*Status: draft. Block: [Volt](https://github.com/elixir-volt/volt), with
oxc, oxide, npm, iconify.*

## The blindness

A standard Phoenix project carries three foreign bodies: an esbuild
binary downloaded at compile time, a Tailwind CLI binary downloaded at
compile time, and — the moment you want anything beyond vanilla
JavaScript — a Node.js installation with its own package manager, its own
lockfile, and its own supply chain. None of them supervised. None of them
visible in your logs. They cannot coordinate (template edit → who rebuilds
what, in which order?), and when something inside them fails, the failure
surfaces as silence or as a stack trace from a process your application
doesn't know exists.

The deeper cost is the one this standard cares about: the build layer is
an island. Nothing on it produces witnesses your tools can consume.

## The mechanism

The observation that makes the island unnecessary: modern frontend
toolchains are already thin JavaScript wrappers around Rust cores. Volt
skips the wrapper. The Rust cores — OXC for JS/TS parsing, transforming,
and linting (650+ rules `[verify]`); LightningCSS for CSS — run as native
functions *inside* the BEAM, supervised like everything else. One
dependency; `mix phx.server` boots the whole frontend toolchain.

What that buys, concretely:

- **Speed where it changes behavior:** Tailwind rebuilds in ~40ms on
  template edits `[verify]`; production builds in well under a second
  `[verify]`. At that speed, rebuilds stop being events you wait for.
- **The Vite feature card, server-side:** HMR with state preservation,
  code splitting, CSS modules, JSON imports, web workers, `import.meta`
  conventions, env vars, compile errors as a browser overlay.
- **npm without Node:** dependency resolution with real semver, a
  lockfile, a global cache — as Mix tasks. Fewer executable foreign
  bodies in the project is also a supply-chain posture.
- **Lint rules as host-language modules:** project-specific JS lint logic
  written in Elixir, because the linter runs in the same VM as the app.
- **Icons as compile-time facts:** the companion icon pipeline scans
  templates at compile time, fetches exactly the icons used (from a
  catalog of 200,000+ across 150+ sets `[verify]`), and ships them as
  inline SVG — no client icon runtime, and "does `lucide:gear` exist?"
  is a question an agent can ask *before* using it.

And the mechanism this page exists for — small to implement, large in
consequence: **the browser's console flows into the server's logs.** The
dev server injects a forwarder; `console.error` in the page becomes a
tagged `Logger` line in `mix phx.server` output:

    [Volt][browser] TypeError: Cannot read properties of undefined ...

The bundler, the application, and the logger are one supervised system,
so the page's runtime errors are simply *more log lines*.

## What it proved

That the no-Node Phoenix frontend is real and pleasant rather than a
compromise — TypeScript, Vue/React/Svelte, Tailwind v4, HMR, all of it,
with the project's only toolchain being `mix`.

And it proved the agent corollary of console-in-logs, which no README
states loudly enough: an agent that just edited frontend code can read
the browser's runtime errors *from inside the server* — no screenshot, no
browser automation, no devtools protocol. Edit, hot-swap, read the
witness, repair. The frontend feedback loop closes in one process, and it
is the only stack where that sentence is architecturally true.

## Composition

Volt is the build half of the [One program](one-program.md) story — the
SFC compiler uses it for dev serving and HMR. Its JS machinery is shared
with [Runtime](runtime.md) (the embedded runtime prebundles dependencies
through it). Browser-console forwarding feeds the
[Agent](agent.md) layer's log capture — the
[feedback loops diagram](../architecture/loops.md) shows the edge. Lint
results and build errors are witnesses in the
[Quality](quality.md) sense: file, line, message, as data.

## Status and limits

Stable for the main path and in production use across this ecosystem's
own sites and demos. The deliberate limit: Vite *plugin ecosystem* parity
is not chased — the committed surface is the feature card above, and the
escape hatch for exotica is honest ("use the island you actually need").
The subset doctrine is [uncertainty 2](../uncertainties/README.md#2-the-compatibility-treadmill)'s
answer, applied here.
