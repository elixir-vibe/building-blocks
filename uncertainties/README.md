# Uncertainties

*Status: settled (the doubts are stable; resolutions are not). A standard
that hides its doubts is marketing. Each entry names what would change the
author's mind.*

## 1. The multitier graveyard
"One program across client and server" has been tried: brilliant systems,
all dead, all of the same cause — they demanded developers abandon their
language and ecosystem. The bet here is that putting the most adopted
component format alive on the surface, with multitier compilation
underneath, escapes the curse.
**Falsifier:** developers refusing to write `.vue`-surfaced hybrid
components even with zero new syntax. Watch real external usage, not stars.

## 2. The compatibility treadmill
Vue, Figma, and JavaScript semantics are moving targets owned by others.
Committed subsets are declared (and the rest explicitly not chased), but
living on someone else's treadmill is a permanent tax.
**Falsifier:** the committed subsets requiring breaking chases more than
rarely — then the subset doctrine fails its purpose.

## 3. The component model is unsettled
How a visual editor edits a component whose behavior a compiler splits
between server and client is an open design problem.
**Resolution criteria:** visual edits, capability edits, and Elixir edits
round-trip without stepping on each other, and the editor preview equals
production output. Until then, "unsettled" stays printed here.

## 4. Checks can be gamed
Any fixed set of checks becomes a target: an agent converges to code that
satisfies the checkers, which is not the same as good code.
**Falsifier:** in-loop quality metrics improving while held-out judgment
degrades. The audit that detects this is part of the design; if it fires,
the check suite gets rebuilt, not defended.

## 5. One person, many fronts
The honest version of "look how much shipped" is "consolidation is a
permanent debt." The ecosystem's enemy is not a competitor; it is entropy
across its own repositories.
**Falsifier:** consolidation phases slipping repeatedly. The reader is
invited to count active repos versus archived experiments over time.

## 6. The training-data bet
The long-horizon claim — that environments with strong feedback will
produce the verified problem-solving traces future models learn from — is
a bet, and explicitly not falsifiable on any near horizon.
**Which is why nothing load-bearing depends on it:** every block pays rent
today, or it gets cut.
