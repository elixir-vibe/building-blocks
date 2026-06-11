# About the author

*Status: draft.*

This standard is written by [Danila Poyarkov](https://github.com/dannote).
The work itself is the argument — the repositories are public, the gates
run in their CI, and every claim here can be checked against them. This
page only adds the context a reader might want: how the program is made,
and where its convictions come from.

## How this is made

I make tools, then use them to build more complex things. Each block here
exists because the previous one hit a wall, and the wall turned out to be
a missing instrument.

I don't write detailed plans or maintain long TODO lists, and I do almost
no prompt engineering. I use agents in every part of the work — most in
moments of uncertainty: reading unfamiliar domains, weighing options,
exploring solutions. I don't delegate decisions to them; we disagree
productively. Even before agents, most of my time went to thinking
rather than typing; agents accelerated that part too.

Much of the code in these blocks is written by agents — using the stack
on itself, under its own gates: duplication budgets at zero, structural
checks, generated-code checks, architecture rules. Whether that regime is
strict enough is not a claim for this page to settle; the gates run in
public.

## Where the convictions come from

This program started in design. Building agent tooling for design work, I
kept getting *pictures of designs* — plausible renders over structureless
node trees that any working designer throws away. A better prompt never
fixed it; making the structure the artifact did. That move — find the
boundary where tools go blind, reshape the artifact until they can see —
is the method of this standard. Design just happened to be where I
noticed it first.

The boundary list in [the manifesto §4](README.md#4-the-method) is not a
survey of the literature. It is a list of places I have lost time
guessing. The same goes for the substrate argument of §5: I tried these
kinds of analyses on other runtimes first; the cost difference is the
argument.

Everything ships in the open — see
[the commitments](README.md#6-the-commitments) for the reasoning.

## The document and me

The voice and direction here are mine; the facts belong to everyone — see
[Contributing](../CONTRIBUTING.md). Where my judgment is the only
evidence for a claim, it is marked as a bet, and the
[uncertainties](../uncertainties/README.md) page says what would change
my mind.
