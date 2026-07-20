# Design rationale

Why this skill is built the way it is, and the evidence behind its rules. The README covers what
the skill does and how to run it; this document covers *why*.

## The problem it targets

PhD students usually know what they found. Turning it into an accepted paper is where they stall —
not from weak writing ability, but from operational friction: structuring sections, holding
cross-section coherence, compressing to a page limit, matching voice conventions, synthesizing
figures, answering reviewers. That scaffolding costs weeks. An experienced advisor could sketch it
in an afternoon.

The skill offloads the scaffolding to the machine and leaves three competencies with the human:
**discrimination** (does the draft argue what it should?), **critique** (where does the argument
break?), and **framing** (what does the paper claim, and why should anyone care?). Everything else —
section templates, voice consistency, page-limit compression, figure scaffolding — is plumbing.

The worry that AI writing assistance weakens intellectual muscles gets the causality backwards. The
skill forces the writer to articulate their own argument at every stage: brainstorming demands they
name who suffers, what breaks structurally, and what evidence supports each claim *before any prose
is generated*; the introduction-twice ordering forces them to confront what the results actually
show; the checklists surface violations that would otherwise survive to peer review.

## The empirical basis

The rules are patterns, not prescriptions. They come from reconstructing how papers were actually
written and revised — full edit histories across 7,600+ Overleaf edits, 100+ tex versions, and 5
peer-review cycles — classifying every advisor intervention by type and impact, and keeping the
patterns that correlated with acceptance. See
[*The Paper Behind the Paper*](https://sites.cs.ucsb.edu/~arpitgupta/blog/the-paper-behind-the-paper.html).

This forensic approach surfaces things prescriptive guides miss. The **key abstraction** — the
coined term at a paper's intellectual core — appears only in final versions; it is discovered
through writing, not planned beforehand. So brainstorming sets a placeholder name and the skill
treats abstraction-naming as iterative.

## The load-bearing principles

**Introduction-twice.** The single strongest predictor of acceptance was whether the final
introduction was constrained by the evaluation's actual results. Draft 0 is a disposable framing
scaffold written *before* the evaluation, so the writer knows what they are trying to show. The final
introduction is rewritten from scratch *after* the evaluation, promising only what the evidence
supports. Papers with stable framing were accepted; papers where Draft 0's promises survived
unchecked drifted and were rejected; writers who skipped Draft 0 entirely produced incoherent
experiments. The resolution is to write it twice.

**Writing as thinking.** Draft 0 is not communication — it is thinking committed to prose. Starting
early forces concepts to be articulated precisely and reveals gaps that persist while ideas live
only as intuitions. Much early material never reaches the final paper; that is the point.

**The expansion–compression arc.** Every accepted paper followed the same shape: comprehensive first
draft → structural reorganization → aggressive compression. Student drafts averaged ~24 words per
sentence; final versions ~21. Background sections were cut 50–65%; the largest deletions were always
tutorial material the audience already knew. Compression above ~50% signals a framing shift, not
wordiness; below ~15% means the draft was already near-final. Normal is 30–50%.

**Structured externalization.** The brainstorming workflow targets a specific failure: ideas that
live as unstructured intuitions. Its 34 questions force each claim into a concrete, falsifiable
statement — "my system is faster" becomes "reduces RTT estimation error by 13× on bursty telemetry
by decomposing the signal into event-triggered and baseline components." The sequence prevents
premature commitment: problem discovery must complete before contribution; knowing what the evidence
shows constrains how the work is positioned.

**Scaffolding-first drafting.** Before full prose, write the topic sentences and read them in
sequence — they should form a coherent argument on their own. If they do not flow, the paragraphs
will not either. Fill in prose only after the topic-sentence sequence holds.

**Rhetorical move theory.** Each section type follows a sequence of functional moves (introduction:
6, evaluation: 6, design: 5, related work: 3), extracted by comparing accepted papers against
rejected versions of the same papers. The moves are functional requirements, not templates: later
moves depend on earlier ones. The most diagnostic finding — student evaluations used only the
head-to-head comparison move; the takeaway paragraph, absent from every early draft and present in
every final one, is what turned "here are numbers" into "here is what they mean."

**Advisor intervention taxonomy.** Revision histories revealed seven intervention types, ordered by
impact: framing, structural rewrite, background deletion, evaluation strengthening, terminology
tightening, contribution reframing, argument clarification. The skill applies them in that order
when simulating advisor feedback, and naming them makes the advising process legible — a writer who
sees that a rewrite is a *framing* intervention can learn to do the framing themselves.

## The style-enforcement architecture

The style layer is organized by *mechanism*, not by source, into three files:

- **`gate_mechanical.md`** — everything a regex can catch (em-dashes, antithesis, filler/pompous/
  fancy words, passive voice, wordiness, term-drift), in one grep script. Runs on every edit.
- **`gate_semantic.md`** — everything only a reader can catch (undefined terms, followability,
  coherence, consistency, grounding, honest positioning), as fresh-reader judgment. Runs in the
  red-team.
- **`craft_reference.md`** — the positive "what to write instead," in three registers (terse
  empirical, conceptual/position, warm narrative).

Two design commitments drive this:

1. **Enforcement, not just coverage.** A live-review audit found the failure mode was not too few
   rules but *self-certification* — the agent that wrote the prose graded its own audit and skipped
   the greps. So review is independent (a reviewer that did not write the text), evidence-gated
   (paste the grep output, not "audited"), and loopable to closure (`red_team_protocol.md`,
   `loop_mode.md`). More rules that never get run is worse than fewer rules that do.

2. **Rules stand on their own.** Checks are stated as self-justifying rules, not attributed to
   authorities or modeled on a named writer's voice. The registers describe what to *do* (short,
   declarative; hedge rationale but assert measured numbers; front-load limitations), not whom to
   imitate.

## Design principles

- **Empirical over theoretical.** Every rule traces to an observed revision pattern.
- **Shunt the plumbing, protect the thinking.** The machine handles scaffolding, voice, checklists,
  figures, compression; the human handles argument, positioning, and honest self-assessment.
- **Compression is a stage, not editing.** First drafts are comprehensive; compression is separate,
  with specific operations and quantitative targets. A short paper with the right content beats a
  padded one at the limit.
- **The student draft is material, not the paper.** It is raw material for restructuring — never
  discarded, always restructured to serve the argument.
- **Figures serve claims.** Every figure answers a specific question and is interpreted in prose,
  not just cited.

## Sources

Writing guidance that informed the rules, for further reading:

- [*The Paper Behind the Paper*](https://sites.cs.ucsb.edu/~arpitgupta/blog/the-paper-behind-the-paper.html) — the forensic revision-analysis methodology this skill is built on.
- [*Systems for Agents, Agents for Systems*](https://sites.cs.ucsb.edu/~arpitgupta/blog/systems-for-agents-agents-for-systems.html) — the argument for compressing the operational middle of the research pipeline.
- [*Storytelling 101*](https://practicespace.substack.com/p/storytelling-101-writing-tips-for) — write introductions twice; scaffolding-first; signposting.
- [*Writing the Introduction*](https://www.cs.columbia.edu/~hgs/etc/intro-style.html) — introduction structure conventions for CS papers.
- *The Elements of Style* — concision and sentence efficiency ("omit needless words"); the composition-craft rules in `craft_reference.md`.
- *On Writing Well* — writing as thinking; clarity through revision.
- *Research Power Tools* — paragraph-purpose comments as scaffolding; pre-submission mechanical checks.

## Contributing

Most useful additions: editorial principles from fields beyond systems/networking; rhetorical-move
examples from more venues (SOSP, OSDI, CHI, USENIX Security); real `project_context.md` examples;
domain-specific brainstorming questions; figure prompt templates for more archetypes.
