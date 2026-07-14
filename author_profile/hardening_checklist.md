# Hardening Checklist — under-specification fixes from live use

> Why this file exists: in a long, real revision loop (HotNets 2026, "PRISM"), the advisor
> surfaced failure modes the base skill under-specifies. The mechanical de-AI grep gate can be
> CLEAN while the prose is still bombastic, inconsistent, over-glossed, un-grounded, or
> dishonestly positioned. These are the semantic + rigor + figure + process rules that close
> those gaps. Run them as part of the Mandatory Style Audit and the red-team loop.
> Added 2026-07 from advisor feedback during the PRISM revision.

---

## Part A — Voice consistency (what the grep gate misses)

### A1. Lexical consistency: ONE word per concept
Using different synonyms for the same concept reads bombastic and un-academic. This is the
single most common drift after many revision rounds. Pick ONE canonical word per concept and
use it everywhere; the base skill's "terminology drift" rule (Stage 4) covers *named concepts*
only — this extends it to ordinary **verbs, adjectives, adverbs, and nouns**.
- Build a synonym-cluster map across the whole paper before finalizing. Typical clusters:
  the core verb ("keeps" vs preserve/carry/retain/remember/capture/hold), the measured noun
  ("condition" vs facet/factor/context), model-axis adjectives (pre-registered "deep/wide" vs
  stray coarse/fine/whole-flow/broad/narrow), "tell apart" (discriminate/separate/distinguish),
  strong-example adjectives (sharp/pointed/stark/pronounced), "reads/scores/measures",
  failure words (near-chance/floor/collapse), "clean/controlled/shortcut-free".
- For each cluster: choose the canonical term, then replace every deviation. Leave genuine
  distinctions alone (flag any two "synonyms" that actually differ, e.g. a metric-name noun
  like "preservation" vs the ordinary verb; the paper *title* word may differ by design).
- Grep-assist: for each candidate cluster, list every surface form with counts, then converge.

### A2. Appositive / gloss pile-ups (the #1 post-refinement AI tell)
Accessibility edits accrete parenthetical glosses and appositive chains until a sentence reads
as machine-written: "X (a Y that does Z), a W, which V." Flag and split:
- Stacked parentheticals (2+ glosses in one sentence), appositive chains ("A, a B, that C"),
  and rule-of-N lists where each item carries its own mini-gloss.
- Fix: one idea per sentence; move a gloss to its term's first-use home; cut the trailing
  summarizing appositive. Prefer a short second sentence over a parenthetical.

### A3. De-AI creep is cumulative: re-hunt after EVERY refinement batch
The grep gate passing once does not hold. Each accessibility/coherence round adds creep. After
any substantive batch, re-run an **independent** de-AI hunter (not the author) for A2 pile-ups,
decorative antithesis, editorializing closers, and repeated stock phrases. Never report the
de-AI pass "done" from a single early grep.

---

## Part B — Accessibility, calibrated (and its tension with de-AI)

### B1. Gloss venue-foreign terms in plain language, once, at first use
For a cross-disciplinary audience (e.g. a networking venue, readers weak on ML), every foreign
term (embedding, probe, anisotropy, CKA, effective rank, decodability) gets a plain gloss the
target reader understands, at or before its first load-bearing use. A definition that is itself
jargon ("decodability, a probe's score above a shuffled-label control") fails — gloss the gloss.

### B2. Balance B1 against A2 — gloss ONCE, plainly, then stop
Accessibility and de-AI pull against each other: glosses aid the reader but pile up into AI
prose. Resolve by glossing each term exactly once (at first use), in the plainest words, and
never re-glossing the same term in later sections. Moving numbers into a table (Part D) lets the
prose drop inline glosses.

### B3. Recursive followability check
A fresh reader asks one question, applied **recursively** at every layer — section → subsection
(\smartparagraph) → paragraph → sentence → keyword: *"Have I been given enough information, by
this point, to follow the current argument?"* Flag any layer that leans on a term, number, or
claim before the reader was given what they need. This catches forward-references to
not-yet-seen figures, terms used before their gloss, and "that number / this gap / the third
metric" with an unresolvable antecedent. Run it as a red-team lens (see Part F).

---

## Part C — Rigor: mappability and grounding

### C1. Mappability: no orphan numbers, no orphan code
Every number, table cell, and empirical claim in the paper must map to: a committed analysis
script → a committed raw result file → and (for reproduced prior-work metrics) the prior-work
source it reproduces. Maintain a PROVENANCE note (claim → value → script → raw output). New
analysis code MUST be committed to the correct analysis repository, not left in a scratch dir.
If a number cannot be mapped, cut it or generate the mapping.

### C2. Ground critiques in numbers, not assertions
"Prior work X is not enough" is weak as an assertion. Make it a head-to-head: run the prior
method's OWN metric on the cases it omitted (e.g. run IEF's anisotropy/CKA on the two models it
never evaluated) and show what it does and does not reveal, beside your method's reading. A
grounded, reproducible contrast beats a rhetorical one and is honest.

### C3. Data-integrity guard
Before citing a computed number, verify the input is not a broken/degenerate extraction or an
uncontrolled artifact (check unique-row counts, variance, provenance caveats). Never cite a
number a collaborator has flagged as not-yet-validated. Prefer the dataset where all compared
items are validated.

---

## Part D — Structure and framing

### D1. Positives-first; move your own caveats to Discussion
Abstract, Introduction, and Conclusion carry the positive contribution, not the limitations.
If a weakness is an artifact of *your own* data collection (e.g. a capture-time shortcut),
do NOT foreground it upfront — assume the clean/correct dataset and move the lesson to a
Discussion paragraph (framed as a community best-practice, not a confession). Frame the
not-yet-done as an opportunity (community effort), not a deficiency.

### D2. Honest, building-upon positioning — especially of your OWN prior work
When positioning against close prior work (and above all your own), be transparent:
- Acknowledge what the prior work DID attempt; do not claim it "ignored" something it treated
  superficially. State the real delta.
- Frame your contribution as building upon / operationalizing it ("the operationally-meaningful
  version of X"), not as trashing it. Avoid "cannot / useless / backward" as blanket verdicts.
- Any critique must be scoped and evidence-backed (Part C2). No intellectual inconsistency
  between how you describe the prior work in Related Work vs Background vs Intro.

### D3. Describe an enabling system by its capability, and why you are non-trivial without it
When a system enables your work (a generator, a substrate), do not describe it abstractly.
State concretely what it does (from one intent it produces X, enforces Y, at scale Z), and make
explicit why your contribution is infeasible without a system of that kind.

### D4. Use a table to break a wall of text
When a section (often Background) accumulates findings as dense prose, move the findings into a
compact table and let the prose interpret it. Tables carry per-item numbers more legibly than
sentences and cut the wall of text. Give the table a self-contained caption and a legend for
any color/format code at first use.

### D5. Terse, assertive \smartparagraph titles
Paragraph-labels are claims, not topics, AND they are short (3–6 words, one line). "Decodability
splits wide from deep," not "Decodability: wide models keep the conditions while deep models keep
only the application." Audit every label for verbosity.

---

## Part E — Figures

### E1. No text panels inside a figure
All prose belongs in the body or the caption, never in an in-figure text box/reading-strip. A
figure holds structure, nodes, axes, and short data labels only. Interpretation goes in the
caption.

### E2. Put the numbers ON the structure
For a data-on-structure figure (a tree, a graph), the values belong on the nodes/edges
themselves, not in a side table beside the figure.

### E3. Right figure for the right section
The Section-1 anchor figure is usually a block diagram: input, output, how it differs from the
closest prior work, and the role of each enabling tool. The detailed conceptual figure (the
multi-view abstraction) belongs in the method section, not the intro.

### E4. Apply the viz + venue TikZ conventions, and verify by looking
Schematic figures follow the data-viz skill's checklist (golden-ratio aspect, data-ink, no
chartjunk) and the venue's TikZ conventions (rounded rects, pastel fills only, orthogonal
single-headed arrows, no diamonds, no style-key/reserved-word collisions). "Clean" requires a
rendered-image inspection, not a checklist pass.

---

## Part F — Process: red-team lenses and the closure gate

### F1. Distinct red-team lenses, run independently
Beyond the base accessibility red-team, run these as separate fresh-reviewer passes: (a) de-AI /
Shenker voice (Part A), (b) ML-accessibility for the target audience (Part B), (c) recursive
followability (B3), (d) lexical consistency (A1). Each reviewer must NOT have written the text.

### F2. Calibrate the intro/abstract to the venue's actual review process
Find how the venue reviews (e.g. HotNets 2026: the abstract + introduction alone decide read-on
vs desk-reject). Beef up the section that gate depends on so it carries the contribution and the
provocation in the first read; nothing else matters if the paper is rejected there.

### F3. The closure gate — iterate to closure, never defer
The loop is not done until a final independent **closure reviewer** reads the whole paper and
returns zero CRITICAL/MAJOR findings across de-AI, accessibility, and coherence. Applying most
findings and deferring "a few lower-priority items" is NOT closure. After every substantive
content change, re-run the relevant lens; do not hand off mid-iteration. Record per-section
status in the audit ledger; "CLEAN" requires the closure reviewer's sign-off, not the author's.
