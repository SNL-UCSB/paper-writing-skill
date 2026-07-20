# Semantic Gate — the reader-judgment style audit

> The failures a grep cannot catch: undefined terms, thesis drift, unparseable sentences,
> ungrounded numbers, inconsistent naming, dishonest positioning. These are what reviewers
> actually reject on. Run this in the red-team and `/loop` passes, with a FRESH-READER lens — a
> reviewer who has not seen the paper and did not write the text. Each item returns findings
> (category, line, concrete fix), not a yes/no. Severity: MAJOR = reviewer-visible confusion;
> CRITICAL = structural or logical incoherence.

Greppable checks live in `gate_mechanical.md`; positive drafting guidance in `craft_reference.md`.

---

## Group 1 — Accessibility and followability

**S1. Define before use (MAJOR).** Every domain term, acronym, artifact, or coined construct is
defined in plain language at first use, before it appears in an argument. A first-time reader in
the venue must be able to follow. For a cross-disciplinary audience, a gloss that is itself jargon
fails — gloss the gloss.
- Check: for each CamelCase / ×-joined / capitalized construct, is there an upstream "is / means /
  defined as" before its first argumentative use?
- Grep aid: `grep -rnoE "[A-Z][a-zA-Z]+×[A-Z][a-zA-Z]+|\b[A-Z][a-z]+[A-Z][a-zA-Z]+\b" sections/*.tex`
  — for each hit, confirm a definition precedes its first argumentative use.

**S2. Term-introduction order (MAJOR).** In a concept chain (abstraction → grammar → DSL), each
term is introduced and connected before it is leaned on. List coined terms by first appearance;
each must be defined at or before that point.

**S3. Spell out acronyms on first use (MAJOR).** Every initialism is expanded at first use, then
abbreviated. Not every reader knows the short form, and new readers meet it fresh.

**S4. Recursive followability (MAJOR).** Apply one question at every layer — section → subsection →
paragraph → sentence → keyword: *"Given only what precedes this point, can the reader follow the
current argument?"* Flag any layer that leans on a term, number, or claim before the reader was
given what they need: forward references to unseen figures, terms used before their gloss, and
"that number / this gap / the third metric" with an unresolvable antecedent.

**S5. Parse-accessibility (MAJOR).** A sentence can be all common words and still unparseable.
Glossing every term is not enough. Hunt and fix:
- **Reduced relative clauses / dropped who-that**: "an operator told an embedding is anisotropic
  still cannot say…" forces the reader to reconstruct "an operator *who is* told *that*…". Restore
  the omitted words or front the participle.
- **Opaque idioms a literal reader cannot decode** ("it ties the two", "earns its keep", "reads at
  two altitudes"). Replace with the literal meaning.
- **Overloaded coined words**: reserve one meaning per word. If "read" names an input strategy, a
  recovered value, and the act of measuring, pick one and rename the others.
- **Compressed multi-clause sentences** packing a definition + a claim + a hedge into one breath.

**S6. Gloss once, plainly, then stop.** Accessibility and de-AI pull against each other: glosses
aid the reader but pile up into machine prose. Gloss each term exactly once, at first use, in the
plainest words; never re-gloss it in later sections. Moving numbers into a table lets the prose
drop inline glosses (see S16).

---

## Group 2 — Coherence and consistency

**S7. Thesis-tie, no new threads (CRITICAL).** Every paragraph's claim ladders to the paper's ONE
controlling idea, stated in FIXED vocabulary. Name the controlling idea (from `project_context.md`);
for each claim, draw the line to it. Flag any paragraph that opens a NEW standalone thread, and any
place the thesis is re-phrased instead of reinforced in the same words.

**S8. Lexical consistency: one word per concept (CRITICAL).** Different synonyms for the same
concept read bombastic and un-academic; this is the most common drift after many revision rounds.
Extends the base "terminology drift" rule from named concepts to ordinary verbs, adjectives,
adverbs, and nouns. For each synonym cluster (e.g. keep/preserve/retain/hold; condition/facet/
factor), choose the canonical term and replace every deviation. Leave genuine distinctions alone.

**S9. Cross-section term-map + decomposition invariance (CRITICAL).** The same load-bearing entity,
named abstraction, or fixed set of axes/stages/requirements gets renamed or re-counted as it
travels abstract → intro → background → design, because sections are drafted in isolation. This
creeps in at synthesis time and survives a per-section audit. Two artifacts, three checks:
1. **Term-map** (a glossary in `project_context.md`): every load-bearing concept and its ONE
   canonical name, plus key numbers as macros. Pull names FROM the term-map when drafting, never
   from memory or a half-recalled sibling section.
2. **Locked source of truth**: one section (usually the finalized introduction) or the term-map is
   canonical; every other section conforms TO it, never the reverse.
3. **Decomposition invariance**: whenever the paper breaks something into parts (N stages, N
   concerns, N axes), every section listing them uses the same members, count, and names. A "four
   stages" list here and "three concerns" for the same decomposition there is a defect. Check
   member-set equality and cardinality across sections. (Greps in `gate_mechanical.md` Part C.)

**S10. Cross-section non-duplication (CRITICAL).** A result, number, or argument appears ONCE, in
its home section. For each quantitative result, is it claimed in more than one section? Keep it
home; cross-reference elsewhere.

**S11. Referent and conflation scan (MAJOR).** Paper-wide sweep: flag every vague "it / this /
these / that <noun>" and vague subject ("a subset / some / you / a second") and name the antecedent
or actor. Disambiguate thing-vs-process ("generating the data", not "the data") and
abstract-vs-concrete ("cost = student time").

---

## Group 3 — Directness

**S12. Deliver the noun (MAJOR).** A paragraph whose topic sentence promises to define or explain X
must STATE X, not orbit it. For each "what is X" paragraph, underline the one sentence that states
X; if none, rewrite. ("The abstraction does A and does B" without ever saying what it IS = fail.)

**S13. No appositive / gloss pile-ups (MAJOR).** Accessibility edits accrete parenthetical glosses
and appositive chains until a sentence reads as machine-written: "X (a Y that does Z), a W, which
V." Flag stacked parentheticals (2+ glosses in one sentence), appositive chains, and rule-of-N
lists where each item carries its own mini-gloss. Fix: one idea per sentence; move a gloss to its
term's first-use home; prefer a short second sentence over a parenthetical.

**S14. Dangling modifiers (MAJOR).** A sentence-initial participial phrase must refer to the
grammatical subject. "Using this dataset, accuracy improved" dangles → "Using this dataset, we
improved accuracy." Check every leading `-ing`/`-ed` phrase points at the subject that follows.

---

## Group 4 — Rigor and grounding

**S15. Mappability: no orphan numbers (CRITICAL).** Every number, table cell, and empirical claim
maps to a committed analysis script → a committed raw result file → (for reproduced prior-work
metrics) the source it reproduces. Maintain a provenance note (claim → value → script → raw
output). New analysis code is committed to the correct repository, not a scratch dir. If a number
cannot be mapped, cut it or generate the mapping.

**S16. Ground critiques in numbers, not assertions (MAJOR).** "Prior work X is not enough" is weak
as an assertion. Make it a head-to-head: run the prior method's OWN metric on the cases it omitted
and show what it does and does not reveal, beside your reading. A reproducible contrast beats a
rhetorical one and is honest.

**S17. Data-integrity guard (CRITICAL).** Before citing a computed number, verify the input is not
a broken/degenerate extraction or an uncontrolled artifact (unique-row counts, variance, provenance
caveats). Never cite a number flagged as not-yet-validated.

**S18. Hedge scan (MAJOR).** Paper-wide: flag every empirical or causal universal ("X takes / means
/ is Y") and ask "always, or typically/often?" Add the calibrated hedge where the claim outruns the
evidence. (This is the one place hedging is ADDED — everywhere else the base rule is zero hedging;
see `craft_reference.md` on calibrated confidence.)

---

## Group 5 — Structure and framing

**S19. Requirement ↔ closure (CRITICAL).** Every requirement stated in motivation/background has an
explicit closure (which design choice satisfies it) in the design section. Build the requirement →
design-choice table; every requirement row has a design answer.

**S20. Positives-first (MAJOR).** Abstract, Introduction, and Conclusion carry the positive
contribution, not the limitations. If a weakness is an artifact of your own data collection, do not
foreground it upfront; assume the clean dataset and move the lesson to a Discussion paragraph,
framed as a community best-practice, not a confession.

**S21. Honest, building-upon positioning — especially of your OWN prior work (CRITICAL).**
Acknowledge what prior work attempted; do not claim it "ignored" something it treated superficially
— state the real delta. Frame your contribution as building upon / operationalizing it, not
trashing it; avoid "cannot / useless / backward" as blanket verdicts. Any critique is scoped and
evidence-backed (S16). No inconsistency between how Related Work, Background, and Intro describe the
same prior work.

**S22. Describe an enabling system by its capability (MAJOR).** When a system enables your work (a
generator, a substrate), state concretely what it does (from one intent it produces X, enforces Y,
at scale Z) and why your contribution is infeasible without a system of that kind — do not describe
it abstractly.

**S23. Claim-first, one-line headings and paragraph labels (MAJOR).** Headings and `\smartparagraph`
labels are claims, not topics, and short (3–6 words, one line). "Decodability splits wide from
deep," not "Decodability: wide models keep the conditions while deep models keep only the
application." Audit every label for verbosity and every heading for wrapping.

**S24. Break a wall of text with a table (MAJOR).** When a section (often Background) accumulates
findings as dense prose, move the per-item findings into a compact table and let the prose interpret
it. Tables carry numbers more legibly than sentences and cut the wall of text. Give the table a
self-contained caption and a legend for any color/format code at first use.

---

## Group 6 — Figures (verify by looking, not by checklist)

**S25. No text panels inside a figure.** All prose belongs in the body or caption. A figure holds
structure, nodes, axes, and short data labels only.

**S26. Put the numbers ON the structure.** For a data-on-structure figure (a tree, a graph), values
belong on the nodes/edges, not in a side table.

**S27. Right figure for the right section.** The section-1 anchor is usually a block diagram
(input, output, how it differs from the closest prior work, role of each enabling tool). The
detailed conceptual figure belongs in the method section.

**S28. Legibility by inspection.** Schematic figures follow the venue TikZ conventions and the
data-viz checklist (aspect ratio, data-ink, no chartjunk). "Clean" requires a rendered-image
inspection, not a checklist pass — a checklist-green figure can still be an illegible mess. See
`figure_synthesis_guide.md`.

---

## Group 7 — Process: red-team lenses and the closure gate

**S29. Distinct lenses, each an independent fresh reviewer.** Run as separate passes, none by the
author: (a) de-AI / mechanical drift (re-run `gate_mechanical.md`), (b) accessibility for the
target audience (Group 1), (c) recursive followability (S4/S5), (d) lexical + decomposition
consistency (S8/S9). Cumulative creep is real: the mechanical gate passing once does not hold —
re-hunt after every substantive batch, never report "done" from a single early grep.

**S30. Calibrate the intro/abstract to the venue's review process.** Find how the venue actually
reviews (e.g. some venues decide read-on vs. desk-reject from the abstract + introduction alone).
Strengthen the section that gate depends on so it carries the contribution and the provocation in
the first read.

**S31. The closure gate — iterate to closure, never defer.** The loop is not done until a final
independent closure reviewer reads the whole paper and returns zero CRITICAL/MAJOR findings across
de-AI, accessibility, and coherence. Applying most findings and deferring "a few lower-priority
items" is NOT closure. After every substantive change, re-run the relevant lens; do not hand off
mid-iteration. Record per-section status in the audit ledger; "CLEAN" requires the closure
reviewer's sign-off, not the author's.
