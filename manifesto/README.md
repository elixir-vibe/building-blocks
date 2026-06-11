# Building Blocks for the Future Web

*Status: draft — structure settled, prose in progress.*

---

## 1. The observation

*(Draft.)* AI generates the surface of artifacts without their structure.
The layers panel of an AI-generated design: the render is plausible;
underneath — no components, no tokens, frames placed like furniture after
an earthquake. The artifact looks finished and is, structurally, nothing.

A vibe-coded application is the same artifact at full scale. A component, a
route, a schema, a migration — each plausible alone. The application is not
in the layers; it is in the *agreement between* the layers, and no tool can
check that agreement. Type systems stop at the network boundary. Linters
see one file. Static analysis sees one language. When the layers disagree,
the machine patches by trial and error — because nothing can tell it where
the disagreement is.

## 2. The wrong answer

*(Draft.)* "Models will get smarter." Perhaps. But human intelligence never
scaled by enlarging heads; it scaled by attaching instruments. The
calculator did not replace arithmetic intelligence — it became part of how
that intelligence is expressed.

Coding agents became useful before any other kind of agent for exactly one
reason: code already had instruments attached. Compilers complain. Tests
complain. Diffs are readable. Version control undoes damage. The model can
be wrong many times and still converge, because the environment pushes
back. The question is not when models will stop needing feedback — nothing
that learns stops needing feedback. The question is why the feedback is so
thin everywhere except compilation.

## 3. The environment thesis

*(Draft.)* An agent is exactly as good as the refutations its environment
can produce. An environment that can only say "looks wrong" produces
guesses. An environment that can say "this path, this line, this value"
produces repairs.

Call a failure that carries its evidence a **witness**. A failing test with
a minimal counterexample is a witness; a vague bug report is not. A witness
paired with a mechanical fix turns rejection into iteration.

The modern web stack is structurally hostile to witnesses. Its boundaries —
design and code, client and server, JavaScript and everything else, the
build and the app, the past and the present — are exactly where witnesses
die. Not because anyone chose hostility; because each layer grew its own
world.

## 4. The method

*(Draft.)*

> Don't build smarter analysis for a hostile world. Reshape the world until
> simple analysis suffices.

Rust did not solve alias analysis — forty years of heroic, partial,
expensive approximation. It made aliasing illegal, and the intractable
became typechecking. The lesson is not that restrictions are good. The
lesson is that **the substrate decides what is checkable** — and substrates
can be chosen, and built.

The boundaries to remove:

- between a design and the code it implies;
- between the client and server halves of one behavior;
- between JavaScript and the runtime hosting everything else;
- between the build toolchain and the application;
- between what the system did and what you can inspect now;
- between your code and the ecosystem's accumulated knowledge;
- between the worker — the agent itself — and the system it works on;
- between a working prototype and a production deployment.

Each one, collapsed, turns an unverifiable guess into a checkable artifact.
That is the whole program: a list of boundaries and a method for removing
them.

## 5. The substrate

*(Draft.)* Why these collapses are cheap on exactly one virtual machine —
economics, not evangelism. Immutable data makes value flow traceable.
Explicit processes and supervision put the architecture in the code, not in
a wiki. A live, introspectable runtime means the system can be asked
instead of guessed at: static analysis refutes universally ("no such path
exists"), the runtime refutes existentially ("this value, at this moment"),
and a loop with both dominates a loop with either.

You don't have to care about this VM. You have to care whether any part of
the web stack can be made this inspectable. This is where it is cheapest.
Someone should do it where it is expensive, too.

## 6. The commitments

*(Owner prose pending; commitments settled.)*

1. **Open source, all of it.**
2. **Local-first memory.** Your project's memory belongs to your project.
3. **Checks earn their place.** Measured false-positive rates before
   anything may gate work.
4. **Failures carry witnesses.**
5. **Uncertainty is stated, in writing, next to the claim it qualifies.**

## 7. The blocks

*(Draft.)* The boundaries above are not a wishlist. Each has a working
attack; most are shipped, all are public, and each is useful alone. They
are catalogued — mechanisms, receipts, and their own stated limits — in
[the map](../layers/README.md).

## 8. Coda

*(Owner prose pending.)* Fewer screenshots pretending to be designs. Fewer
layers agreeing by accident. Fewer bugs that cannot be reproduced. Fewer
sessions that vanish.

**Fewer places to guess.**
