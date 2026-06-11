# Contributing to the standard

This repository is a document. Contributions here are contributions to
prose and to claims — which makes the rules a little different from a code
repo.

## The two-track rule

**Facts are everyone's; voice and direction are the author's.**

- **Track 1 — Corrections.** Wrong claims, drifted numbers, broken links,
  code samples that don't run, sections that assume knowledge they never
  introduce. These are bugs. File an issue or send a PR directly; the bar
  is evidence, and good corrections are merged fast.
- **Track 2 — Proposals.** Changes to what the standard *says*: a new
  section, a reframed argument, a new uncertainty, a claim promoted from
  draft to settled, a layer page restructured. These start as issues using
  the proposal template, get discussed, and only then become PRs. A
  proposal PR without a discussed proposal issue will usually be converted
  back into one — not as a brush-off, but because in a document, *whether*
  to say something is the expensive decision; the wording is the cheap one.

What is not open for contribution: the document's voice. Edits that
"improve" tone, smooth digressions, add rhetorical questions, or de-hedge
the uncertainties will be declined regardless of quality. The hedges are
content.

## Status lines are the contract

Every page carries a status: `outline` → `draft` → `settled`.

- **outline** — structure only; corrections premature, proposals welcome.
- **draft** — prose exists; both tracks open.
- **settled** — stable enough to link to; the bar for changes rises.
  Promoting a page to settled is itself a Track-2 proposal.

## Style rules (enforced in review)

The document's register, mechanically checkable:

1. Concrete objects argue; adjectives don't.
2. No "it's not just X, it's Y". No "Let's dive in". No sentence starting
   "Most developers". No rhetorical questions as transitions.
3. Mechanism over slogan: every capability claim is followed by how it
   works.
4. Real numbers carry `[verify]` markers until checked against the repos
   at review time; merged prose has no unverified numbers.
5. Uncertainty is stated where it lives. Removing a stated limit requires
   evidence the limit no longer exists.
6. Layer pages keep the five-act structure (blindness / mechanism / proved
   / composition / status). Don't add acts; don't skip acts.

## Cross-references

- Pages reference each other by relative links and *boundary* names
  ("see [Causality](layers/causality.md)"), not by product names — products
  are the current answers; boundaries are the standard's stable vocabulary.
- Every layer page links back to the [map](layers/README.md) and to any
  uncertainty that qualifies its claims.

## What happens to merged history

There are no versions. The commit history is the changelog, so commit
messages on prose matter: say what claim changed and why, not "update
docs". Squash-merge with a real message is the norm.

## For maintainers and agents

PRs drafted with AI assistance are fine — this standard would be a strange
place to pretend otherwise — but the two-track rule and style rules apply
to generated prose with extra force, and the submitting human owns every
claim in the diff.
