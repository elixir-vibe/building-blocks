# Design — a design is a node tree, not a picture

*Status: draft. Block: [OpenPencil](https://github.com/open-pencil/open-pencil),
with [figma-use](https://github.com/dannote/figma-use), iconify, and a
BEAM-side parsing/rendering layer.*

## The blindness

Every "AI designer" demo works the same way: prompt in, screen out. For a
landing page it even looks plausible — hero, headline, CTA, cards. There is
a narrow distribution to imitate, and the model imitates it.

Product design does not work like that. A designer doesn't transcribe a
finished picture out of their head; they make a small move and look at it.
Duplicate, nudge, compare two versions, move an icon a pixel, check mobile,
realize mobile breaks the layout, undo, branch. Real design happens at the
scale of small operations with feedback after each one — and a thing can be
geometrically centered and visually wrong; equal padding can feel unequal.
The eye is not a ruler.

One-shot generation skips that loop entirely, and the skipped loop is
visible in the artifact: open the layers panel and find no components, no
tokens, `Frame 427` inside `Frame 428`. The render is a screenshot
pretending to be a design, and the designer who receives it throws it away
and starts over. Tools cannot help, because to a tool, a picture is opaque.

## The mechanism

OpenPencil is a design editor built so that the structure *is* the
artifact. It opens real `.fig` files and its own `.pen` documents; the
node tree — frames, auto-layout, components, variants, tokens — is the
single source of truth, and everything operates on it:

- **Agents get designer operations, not a prompt box.** Through MCP, a
  CLI, and a headless SDK, an agent does what a designer does: query the
  tree (XPath over nodes), mutate it, render the result, compare, and run
  the linter. The loop, mechanized.
- **The design linter** ships in the core — rules for hardcoded colors
  versus tokens, WCAG contrast, touch-target sizes, auto-layout usage,
  default layer names, detached instances, deep nesting. This is the
  design system made executable: tokens and styles are not documentation,
  they are a spec the linter enforces, with each finding carrying the node
  id and, where possible, the suggested binding ("this `#1A1A1A` has a
  matching token — bind it").
- **Design changes become diffs.** A node-tree diff format is landing —

      @@ /Card/Title #1:23
      -opacity: 0.8
      +opacity: 1

  — so an AI design edit is reviewable the way a code change is
  reviewable: by reading what changed, not by squinting at two renders.
- **Review by vision models is choreographed, not trusted.** A lesson
  learned the hard way: a model reviewing its own fifth iteration goes
  blind — the eye glazes. So review uses fresh eyes by rotation, pairwise
  "what got better, what got worse" comparisons between renders instead
  of absolute scores, and independent reviewers at stages. The
  methodology generalizes to any subjective check; it was discovered
  here.
- **Server-side design machinery exists below the editor:** the Figma
  binary format parsed natively on the BEAM, layout computed by the same
  engine family the editor uses, rasterization through a native graphics
  pipeline — so a backend or an agent can parse, render, and diff design
  artifacts with no browser and no editor open. `[verify: package names
  public at publish time]`

## What it proved

OpenPencil is a real product with a community measured in thousands of
stars, external contributors, and users who open actual production `.fig`
files in it. It proved that "AI-native" and "ecosystem-compatible" are not
opposites: compatibility with the dominant format is the adoption surface;
the node-tree model is the contract.

It also proved the strategic lesson that shaped this whole program's
open-source commitment. Its predecessor, figma-use, drove Figma through
its plugin API well enough that a patch shipped whose visible purpose was
to break it. At the client layer, secrecy is not a moat; the conclusion is
catalogued in the [manifesto's commitments](../manifesto/README.md#6--the-commitments).

## Composition

The node tree this editor edits is the head of [the chain](../architecture/chain.md):
the same structured component that, serialized, becomes a single-file
component, which the [One program](one-program.md) layer compiles to a
server-rendered module. Design lint findings are witnesses in the same
sense as code lint findings ([Quality](quality.md)); the icon pipeline
(compile-time discovery, only used icons shipped) rides the
[Build](build.md) layer; and BEAM-side parsing/rendering means the
[Agent](agent.md) layer can inspect and render design artifacts from a
live session.

## Status and limits

The editor is stable and in active use; collaboration and font handling
are under active hardening. Full Figma parity is explicitly a *demo*, not
a promise — the committed contract is the node-tree model and the
supported node classes. The open frontier is the component model shared
with the [One program](one-program.md) layer: how a visual editor edits a
component whose behavior a compiler splits across server and client is
the program's named open problem — see
[uncertainty 3](../uncertainties/README.md#3-the-component-model-is-unsettled).
