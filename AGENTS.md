# Agent Notes — building-blocks

This repository is a **document** (a living standard), not a codebase.
Agents working here edit prose and claims, not code. These rules apply to
the maintainer's own agents and to contributors' agents equally — if an AI
helped produce your diff, these are your instructions too.

## The prime rules

1. **Every claim needs a source.** Before writing that a tool does X at
   speed Y, verify against the actual repo, README, or a command you ran.
   Numbers you could not verify get a `[verify]` marker and must not
   survive into a merged PR. Never copy a number from your own memory of a
   model training set.
2. **Voice is read-only.** Do not "improve" tone, smooth digressions,
   remove hedges, add rhetorical questions, or introduce constructions
   like "it's not just X, it's Y". The hedges and digressions are content.
   If a sentence seems off-topic, it is load-bearing — keep it.
3. **The two-track rule binds you** (see CONTRIBUTING.md): corrections
   (facts) may go straight to PR; anything that changes what the standard
   *says* starts as a proposal issue. When unsure which track you're on,
   you're on Track 2.
4. **Structure is fixed.** Layer pages keep the five acts (blindness /
   mechanism / proved / composition / status). The manifesto keeps §1–8.
   Do not add sections, merge sections, or reorder without a proposal.
5. **Status lines are law.** Do not promote `outline → draft → settled` as
   a side effect of editing. Promotion is its own proposal.

## Style rules (mechanical, check before committing)

- Concrete objects argue; adjectives don't.
- Banned: "Let's dive in", sentences starting "Most developers",
  rhetorical questions as transitions, "seamlessly", "powerful",
  "game-changing".
- Mechanism over slogan: every capability claim is followed by how it
  works.
- Cross-references use **boundary names** and relative links
  (`[Causality](layers/causality.md)`), never bare product names as link
  text.
- Code samples must run. If you cannot run them, mark them and say so in
  the PR.

## For the maintainer's agents specifically

- Source material for layer-page drafting lives in the private program
  repo (drafting workshop with per-page insight ledgers). Draft there,
  land here. **Anything landed here must be publishable as-is** — no
  internal phase numbers, no internal vocabulary, no strategy detail
  beyond what public READMEs already state.
- When drafting a layer page, every item in that page's insights ledger
  must survive into the final text. Missing ledger items are a review
  failure.
- Commit messages state what claim changed ("state Volt rebuild number,
  verified 38–45ms locally"), never "update docs".
- Owner-marked sections (manifesto §1, §6, §8) are not yours to draft.
  Leave the `(Owner prose pending)` markers in place.

## For contributors' agents

- Run your diff against the style rules above before submitting; reviews
  enforce them without exceptions for generated prose.
- Your human owns every claim in the diff. "The model said so" is not a
  source.
- Do not bulk-edit. One claim or one page per PR; the review unit is the
  claim.

## What agents may freely do

Fix typos and broken links (Track 1). Verify numbers and resolve
`[verify]` markers with evidence. Check cross-reference integrity. Propose
(as issues) new uncertainties, falsifiers, or composition links you can
substantiate. Keep `architecture/` diagrams in sync with the prose — see
`architecture/README.md` for the source-of-truth rules.
