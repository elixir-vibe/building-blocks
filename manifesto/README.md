# Building Blocks for the Future Web

*Status: draft — full prose; §1, §6, §8 pending the author's voice pass.*

---

## 1. The observation

Open the layers panel of an AI-generated design. The render above it is
plausible — a hero, a headline, cards in a grid, nothing obviously wrong.
Underneath: no components, no tokens, a layer named `Frame 427` containing
a layer named `Frame 428`, frames placed like furniture after an
earthquake. A designer opening that file does the only reasonable thing
and throws it away. The artifact looks finished and is, structurally,
nothing. A screenshot pretending to be a design.

I am a designer, so I saw it there first. But it is not a design problem.

A vibe-coded application is the same artifact at full scale. A React
component, an API route, an ORM schema, a migration, some client state —
each layer plausible in isolation, each one the kind of code a reviewer
skims and approves. The application is not in the layers. It is in the
*agreement between* the layers: that the shape the component fetches is
the shape the route returns, that the route's query matches the schema,
that the schema matches the migration that actually ran. No tool on earth
checks that agreement. Type systems stop at the network boundary. Linters
see one file. Static analysis sees one language.

So when the layers disagree — and generated layers disagree the way
strangers disagree — the machine patches by trial and error, because
nothing can tell it *where* the disagreement is. The result is the wall
every prompt-to-app platform hits at the same project size: the code is
not exactly wrong; it is unmanaged. Surface without structure, everywhere
you look.

## 2. The wrong answer

The standard response is: models will get smarter. They will. It is still
the wrong answer, and the reason is older than software.

Human intelligence has never scaled by enlarging heads. It scales by
attaching instruments. Writing is an instrument; long division is an
instrument; the calculator did not replace arithmetic intelligence — it
became part of how that intelligence is expressed. A person with a
calculator is not a person who outsourced thinking; they are a larger
cognitive system. Philosophers have a name for this — the extended mind —
and the decades-old argument settles a question people treat as new:
asking when models will stop needing tools is like asking when
mathematicians will stop needing notation.

The empirical case is sitting in plain sight. Coding agents became useful
before any other kind of agent — before browsing agents, before computer-use
agents, before everything — for exactly one reason: code already had
instruments attached. Compilers complain. Tests complain. Diffs are
readable. Version control undoes damage. A model working on code can be
wrong many times per minute and still converge, because the environment
pushes back. A model working on anything else gets one shot and a shrug.

The question is not when models will stop needing feedback. Nothing that
learns stops needing feedback. The question is why the feedback is so
rich at the compiler and so thin everywhere else — and what the rest of
the stack would look like if it pushed back too.

## 3. The environment thesis

An agent is exactly as good as the refutations its environment can
produce.

An environment that can only say "looks wrong" produces guessing. An
environment that can say "this path, this line, this value, at this
moment" produces repair. Between those two sentences lies most of the
practical difference between an agent that flails and an agent that
converges — at a fixed model, with fixed prompts. The model is the same;
the environment decided the outcome.

Two terms, used throughout this document:

A **witness** is a failure that carries its own evidence. A failing test
with a minimal counterexample is a witness. A type error with the exact
incompatible types is a witness. "The page looks broken" is not. The
practical law: a rejection without a witness teaches nothing and costs a
guess; a rejection with a witness is an instruction.

A **repair** is what a witness enables: a mechanical or near-mechanical
fix derived from the evidence. The loop *generate → refute with witness →
repair → re-check* is how verification substitutes for intelligence — the
checker does not need to be smart, and neither, within limits, does the
generator. The loop does the thinking.

Now the diagnosis. The modern web stack is structurally hostile to
witnesses. Its boundaries — between a design and the code it implies,
between the client and server halves of one behavior, between JavaScript
and everything that hosts it, between the build toolchain and the
application, between what the system did last Tuesday and what you can
inspect now — are precisely where witnesses die. A cross-boundary failure
has no single tool responsible for it, therefore no tool can produce its
witness, therefore everyone — human or machine — guesses. Nobody chose
this hostility. Each layer simply grew its own world, its own runtime,
its own truth.

## 4. The method

> Don't build smarter analysis for a hostile world. Reshape the world
> until simple analysis suffices.

There is a precedent, and it is the most successful systems-language story
of the century so far. Rust did not solve alias analysis. Forty years of
research had produced heroic, partial, expensive approximations of the
question "do these two pointers overlap?" — and Rust made the question
illegal to need. Ownership rules reshaped the programs themselves, and an
intractable analysis became typechecking. The lesson is not that
restrictions are virtuous. The lesson is that **the substrate decides what
is checkable** — and substrates can be chosen, and where necessary,
rebuilt.

Applied to the web, the method produces a list. Each entry is a boundary
where tools currently go blind; each is removable; removing it turns an
unverifiable guess into a checkable artifact:

- the boundary between a **design** and the code it implies — removed when
  the design *is* a structured tree that compiles, not a picture that gets
  reinterpreted;
- the boundary between the **client and server** halves of one behavior —
  removed when both halves are compiled from one source, so there is no
  agreement left to break;
- the boundary between **JavaScript** and the runtime hosting everything
  else — removed when JS runs inside the same observable machine, every
  crossing an explicit, traceable call;
- the boundary between the **build toolchain** and the application —
  removed when the bundler is a supervised part of the app rather than a
  fleet of foreign binaries;
- the boundary between **what the system did** and what you can inspect —
  removed when every session is recorded cheaply enough to record all of
  them, and replay is exact;
- the boundary between **your code** and the ecosystem's accumulated
  knowledge — removed when all published code is searchable by structure,
  not by string;
- the boundary between the **worker** — the agent itself — and the system
  it works on — removed when the agent runs inside the same supervised
  runtime, inspectable like everything else;
- the boundary between a **working prototype** and a production
  deployment — removed when deployment is the same supervised, scriptable
  machinery, not a container ritual.

That is the whole program: a list of boundaries, and a method for removing
them. Everything else is execution.

## 5. The substrate

These collapses are possible in many ecosystems and cheap in exactly one,
and the difference is economics, not taste.

Static analysis lives or dies on three properties of its substrate. The
first is **traceable value flow**: in a language with immutable data,
where a value cannot be mutated behind your back through an alias, the
history of a value is close to the visible history of the program — a
dependency slice computed by a forty-year-old algorithm is *tight* instead
of drowning in maybes. The second is **explicit structure**: on a runtime
where concurrency is processes and supervision trees rather than threads
and shared memory, the architecture is written in the code itself —
"check the architecture" can be a command instead of a meeting. The third
is **a live, askable system**: static analysis refutes universally ("no
such path exists"), a live runtime refutes existentially ("this value, at
this moment, in production") — and a feedback loop armed with both kinds
of refutation dominates a loop with either alone.

One mainstream virtual machine has all three properties, and has had them
in production for thirty years. The language's own creator now argues its
fitness for AI-assisted development; this document only argues the step
further: agents should not merely *write* this language — they should
live inside its runtime, instrumented by it, verified by it.

You do not have to care about this VM. You have to care whether *any*
part of the web stack can be made this inspectable. This is where it is
cheapest, so this is where it is being built first. Someone should do it
where it is expensive, too.

## 6. The commitments

A manifesto is distinguished from an essay by promises that can be
checked. These are the standing commitments of this program; each is
falsifiable, and the [uncertainties](../uncertainties/README.md) register
names what would count as breaking them.

**Open source, all of it.** Not as generosity — as physics. Anything that
runs client-side is open source already to anyone with a decent agent and
an afternoon; reverse engineering has become approximately free, and
moats built on client-side secrecy are dead men walking. The durable
assets are elsewhere: in composition, velocity, and conventions. So every
block ships as an open package, independently useful, adoptable one at a
time.

**Local-first memory.** Sessions, analytics, code intelligence, recorded
replays — plain local database files inside your project. Your project's
memory belongs to your project. Anything hosted is opt-in, layered above,
and never load-bearing: the core loop must work on a laptop with the
network cable pulled.

**Checks earn their place.** An agent cannot shrug off a false positive
the way a human does — it will contort code to satisfy a wrong check, and
the contortion becomes its own pathology. Therefore: every automated
check in this stack has a measured false-positive rate against real
code — the entire public ecosystem of it — before it is allowed to gate
anyone's work, and checks that cry wolf are demoted or retired, in
public. A check suite the agent cannot trust trains the agent to ignore
it.

**Failures carry witnesses.** Every tool in this stack reports *where,
why, and when possible how to fix* — a location, an evidence path, a
suggested repair. A bare "failed" is treated as a bug in the tool. The
consumer of diagnostics is no longer only human, and machines cannot read
between lines.

**Uncertainty is stated, in writing, next to the claim it qualifies.**
Every page of this standard carries its own status and its own doubts.
A standard that hides its doubts is marketing.

## 7. The blocks

The boundaries of §4 are not a wishlist. Each has a working attack; most
are shipped and in daily use — not least on each other, since every block
is built and gated using the rest. They are catalogued, with their
mechanisms, their receipts, and their own stated limits, in
[the map](../layers/README.md); the
[architecture section](../architecture/README.md) shows how they compose,
at boundary, package, and feedback-loop granularity.

They are useful alone. That is deliberate: the composition is the point,
but the door is any single piece.

## 8. Coda

There is a longer-horizon reason to build environments rather than wait
for models, and it should be stated carefully, because it sounds
grandiose.

Today's models learned from artifacts: the code that survived, the design
that shipped, the cleaned-up explanation. The false starts, the failed
hypotheses, the check that caught the bug, the human correction at
exactly the right moment — the actual *path* of problem-solving — was
never in the training data, because nothing recorded it. Agent
environments can record it. Environments with strong feedback will not
just produce better software; they will produce the record of verified
problem-solving that the next generation of models learns from. A passing
test is a better training signal than a thumbs-up. A replay divergence is
better still. A correction attached to the exact operation that caused it
may be the best there is.

That is a bet, not a promise — and it is explicitly the kind of bet where
every piece pays rent today even if the horizon never arrives.

The nearer-term test is simpler, and it is the standard this whole
document should be judged by:

Fewer screenshots pretending to be designs. Fewer layers agreeing by
accident. Fewer bugs that cannot be reproduced. Fewer sessions that
vanish into chat logs.

**Fewer places to guess.**
