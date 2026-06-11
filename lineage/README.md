# Lineage — the algorithms and their sources

*Status: draft. This page is the technical bibliography of the standard.
Every layer page that says "this is the 1980s" or "the suggestion is
computed, not heuristic" is making a checkable claim about prior work;
this is where those claims are checked. Each entry states the exact
result, where it is used in this stack, and what is honestly new versus
honestly inherited.*

None of the core algorithms in this stack are original, and the document
says so wherever they appear. That is deliberate: a forty-year-old
algorithm with a thousand citations is a better foundation than a clever
one invented last month. What this program claims as its own is the
*substrate* — the observation that these analyses, expensive and
approximate on mainstream languages, become cheap and tight on a runtime
with immutable data and explicit process structure — and the *discipline*
of wiring them into one gate where every failure carries its witness.
The lineage below is the inheritance; the composition is the work.

Links prefer the publisher's DOI plus a freely readable copy where one
exists.

---

## Dependence and slicing

Used in: [Causality](../layers/causality.md) (Reach). This is the
"slicing is the 1980s; dependence graphs are the 1990s" paragraph,
expanded.

**Weiser, *Program Slicing*.** ICSE 1981, pp. 439–449; journal version in
IEEE Transactions on Software Engineering SE-10(4), 1984.
[1981 PDF](https://www.cse.msu.edu/~cse870/Public/Homework/SS2003/HW5/p439-weiser.pdf) ·
[1984 PDF](https://cseweb.ucsd.edu/classes/fa03/cse231/weiser.pdf)
The origin of the question "show me everything this value's history
touches." Weiser defined the slicing criterion, proved statement-minimal
slices undecidable, and gave the dataflow approximation — and observed
that experienced programmers already slice mentally while debugging. The
backward slice in Causality is this idea, computed on a dependence graph
rather than by dataflow equations.

**Ferrante, Ottenstein & Warren, *The Program Dependence Graph and Its
Use in Optimization*.** ACM TOPLAS 9(3), 1987, pp. 319–349.
[doi:10.1145/24039.24041](https://doi.org/10.1145/24039.24041) ·
[PDF](https://web.eecs.umich.edu/~mahlke/courses/583f23/reading/ferrante_toplas_87.pdf)
The program dependence graph: control dependence derived from
post-dominators, data dependence from definition–use chains, both in one
structure that drops the accidental sequencing of a control-flow graph.
Once a program is a PDG, slicing is graph reachability — linear time.
Reach's per-function graphs are PDGs in this exact sense.

**Horwitz, Reps & Binkley, *Interprocedural Slicing Using Dependence
Graphs*.** ACM TOPLAS 12(1), 1990, pp. 26–60.
[doi:10.1145/77606.77608](https://doi.org/10.1145/77606.77608) ·
[PDF](https://research.cs.wisc.edu/wpis/papers/toplas90.pdf)
The system dependence graph: PDGs stitched across procedure calls, with
summary edges so that slices respect calling context instead of bleeding
through every call site. Reach's whole-program graph and its two-phase
slicing follow this construction. The substrate claim in Causality is
about precisely this algorithm: its precision is bounded by alias
analysis, and on immutable data the aliasing question barely exists —
same 1990 algorithm, tighter cone.

**Murphy, Notkin & Sullivan, *Software Reflexion Models: Bridging the
Gap Between Source and High-Level Models*.** FSE 1995, pp. 18–28.
[doi:10.1145/222124.222136](https://doi.org/10.1145/222124.222136) ·
[author page](https://www.cs.ubc.ca/~murphy/papers/rm/fse95.html)
An engineer declares a high-level model and a mapping to source; a tool
computes where the code agrees with and diverges from the model. The
`.reach.exs` policy file — layers, mappings by module pattern, forbidden
dependencies, checked in CI — is a reflexion model in all but name. The
form was arrived at independently and the lineage claimed afterward,
which is itself evidence the design is a natural one.

## Clones and generalization

Used in: [Quality](../layers/quality.md) (ExDNA). This is the source of
"the suggestion is *computed*, not heuristic."

**Plotkin, *A Note on Inductive Generalization*.** Machine Intelligence
5, 1970.
[PDF](https://homepages.inf.ed.ac.uk/gdp/publications/MI5_note_ind_gen.pdf)
**Reynolds, *Transformational Systems and the Algebraic Structure of
Atomic Formulas*.** Machine Intelligence 5, 1970.
[PDF](https://www.cs.cmu.edu/afs/cs/user/jcr/ftp/transysalg.pdf)
The same result, discovered independently and published side by side:
anti-unification, the dual of unification, computes the *least general
generalization* of a set of terms — the most specific term of which all
of them are instances, unique up to renaming. When ExDNA anti-unifies a
clone family, the lgg *is* the recommended shared function: the
algebraically canonical answer to "what should the extraction be," not a
heuristic. That single property is why the finding can arrive with its
repair.

**Komondoor & Horwitz, *Using Slicing to Identify Duplication in Source
Code*.** SAS 2001, LNCS 2126, pp. 40–56.
[doi:10.1007/3-540-47764-0_3](https://doi.org/10.1007/3-540-47764-0_3) ·
[PDF](https://research.cs.wisc.edu/wpis/papers/sas01.pdf)
Clone detection on the PDG instead of the text: isomorphic dependence
subgraphs found by lock-step backward slicing, catching non-contiguous
and reordered clones that no textual or AST comparison can see. Not yet
shipped here — ExDNA today is structural (AST-level) — but this paper is
the known next step where Causality's graphs and Quality's clone
detection meet, and the honest marker of where the current detector's
semantic limit lies.

## The verification loop

Used in: [Manifesto §3](../manifesto/README.md#3-the-environment-thesis)
and the [Agent](../layers/agent.md) layer. The loop *generate → refute
with witness → repair → re-check* has a name and a literature.

**Solar-Lezama, Tancau, Bodík, Seshia & Saraswat, *Combinatorial
Sketching for Finite Programs*.** ASPLOS 2006, pp. 404–415.
[doi:10.1145/1168857.1168907](https://doi.org/10.1145/1168857.1168907) ·
[PDF](https://people.csail.mit.edu/asolar/papers/asplos06-final.pdf)
Counterexample-guided inductive synthesis (CEGIS): a generator proposes a
candidate program, a verifier refutes it with a concrete counterexample,
the counterexample constrains the next proposal, repeat until no
refutation exists. The generator there was a SAT solver and the verifier
a model checker; replace the generator with a language model and the
verifier with a gate suite, and the loop in the manifesto is the same
loop. The transferable insight is theirs: the checker does not need to
be smart, and neither — within limits — does the generator, *if and only
if* the refutation carries enough information to constrain the next
attempt. That is the formal ancestor of "a rejection without a witness
teaches nothing."

**Claessen & Hughes, *QuickCheck: A Lightweight Tool for Random Testing
of Haskell Programs*.** ICFP 2000, pp. 268–279.
[doi:10.1145/351240.351266](https://doi.org/10.1145/351240.351266)
Properties as executable specifications, checked against generated
inputs, failures shrunk to minimal counterexamples. The manifesto's
definition of a witness — "a failing test with a minimal counterexample"
— is describing a shrunk QuickCheck failure. Property-based testing is
the cheapest machine-checkable specification known, the BEAM has
first-class descendants of this tool, and its absence from the standard
gates is a named gap:
[roadmap Phase 4](../roadmap/README.md#phase-4--specs-become-oracles).

## Precision discipline

Used in: [Manifesto §6](../manifesto/README.md#6-the-commitments)
("checks earn their place") and [Quality](../layers/quality.md). The
gate/advisory asymmetry is not a taste; both halves have prior art.

**Lindahl & Sagonas, *Practical Type Inference Based on Success
Typings*.** PPDP 2006.
[doi:10.1145/1140335.1140356](https://doi.org/10.1145/1140335.1140356) ·
[PDF](https://user.it.uu.se/~kostis/Papers/succ_types.pdf)
The theory under Dialyzer, and the gold standard for what may gate: a
success typing never rejects a program that cannot crash — it reports
only what is *provably* wrong, at the cost of missing much. That is the
correct trade for a blocking check, stated and proven two decades ago.
"Only checks that essentially never lie may block a merge" is this
paper's discipline, generalized.

**Sadowski, van Gogh, Jaspan, Söderberg & Winter, *Tricorder: Building a
Program Analysis Ecosystem*.** ICSE 2015.
[doi:10.1109/ICSE.2015.76](https://doi.org/10.1109/ICSE.2015.76) ·
[PDF](https://research.google.com/pubs/archive/43322.pdf)
Google's account of why every static analyzer before Tricorder died:
false positives, defined from the developer's seat ("any report the user
chooses not to act on" — their *effective false positive*), and the cure:
per-check usefulness measured in production, feedback as a first-class
signal, checks demoted when they cry wolf. The commitment in §6 —
measured false-positive rates against real code before a rule may gate,
demotions in public — is Tricorder's regime applied to a consumer that
is even less forgiving than a busy engineer: an agent that *complies*
with wrong checks instead of ignoring them.

## Mind and modularity

Used in: [Manifesto §2](../manifesto/README.md#2-the-wrong-answer) and
the maintainability argument throughout.

**Clark & Chalmers, *The Extended Mind*.** Analysis 58(1), 1998,
pp. 7–19.
[doi:10.1093/analys/58.1.7](https://doi.org/10.1093/analys/58.1.7) ·
[free text](https://web.archive.org/web/20170521132351/consc.net/papers/extended.html)
The "philosophers have a name for this" in §2. Active externalism: when
an external process is reliably coupled into a cognitive task and plays
the role an internal process would, it is part of the cognitive system —
the person *with* the notebook is the mind. Substitute "agent with a
gate suite" and §2's argument is an application, not an analogy: asking
when models will stop needing tools is asking when cognition will stop
being extended.

**Baldwin & Clark, *Design Rules, Volume 1: The Power of Modularity*.**
MIT Press, 2000.
[publisher](https://mitpress.mit.edu/9780262538206/design-rules-volume-1/)
The economics under the architecture checking. Modularity is valuable
because each independent module is a *real option* — it can be improved,
replaced, or discarded without repricing the whole system — and design
rules (the interfaces and forbidden dependencies) are what keep the
options independent. An architecture policy checked in CI is the
mechanical enforcement of design rules in exactly their sense; the drift
it prevents is, in their terms, the silent expiry of the system's
options. This is the rigorous version of "maintainability," for when the
word needs to cash out in something other than taste.

---

## What is *not* inherited

Honesty cuts both ways. Three things in this stack do not have a
literature entry above because they are, as far as the author can
determine, new — stated here so the claim is falsifiable:

- **Linting the generator's distribution** (ExSlop): classical linters
  encode what human experts reject; a checker for what the *model*
  over-produces has no direct ancestor we know of. Closest prior art is
  style enforcement generally; the inversion — lint the bias, not the
  language — appears to be new.
- **The substrate observation as an organizing principle**: each entry
  above is "same algorithm, better world." Individually old; choosing a
  runtime *because* it makes the 1981–2001 canon cheap and tight, and
  building the whole gate on that arbitrage, is the program's actual
  bet — see [uncertainties](../uncertainties/README.md) for what would
  falsify it.
- **Replay as a first-class oracle** ([Time](../layers/time.md)):
  session recording is old; recording cheaply enough to keep *all*
  sessions and treating exact replay divergence as a gate-grade witness
  is, at minimum, uncommon.

If a reader knows prior art for any of these, that is exactly the kind
of correction this document wants —
[open an issue](https://github.com/elixir-vibe/building-blocks/issues).
