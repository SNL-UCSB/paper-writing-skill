# Mechanical Gate — the single greppable style audit

> The ONE place for every mechanically detectable style rule. Merges what used to live in three
> separate grep scripts (de-AI, Elements-of-Style, hardening) plus the base voice profile's
> banned-word lists. Run it on every `.tex` edit in the base gate, and again in the red-team/loop
> pass. **Report the grep counts, not "audited."** Every hit is either fixed or explicitly
> justified before the edit is reported done. A mental pass is not a run.

This gate holds the checks a regex can catch. Reader-judgment checks (define-before-use,
followability, coherence, grounding) live in `gate_semantic.md`. Positive "what to write instead"
guidance lives in `craft_reference.md`.

---

## Part A — Banned constructions (fix every hit)

Each rule states the ban, then a wrong→right example.

**M1. Em-dashes — BANNED, no exceptions.** No `---` (em), no ` -- ` (en used as a dash), no
`—`/`–` (Unicode). The single strongest machine-writing tell. Replace with a comma, colon,
parentheses, or full stop. Parenthetical aside → parentheses; appositive → commas; dramatic
reveal → new sentence.
- ✗ `One process, the Core --- parses the intent.` → ✓ `One process, the Core, parses the intent.`
- Only exception: an em-dash inside a verbatim quotation from an external source.

**M2. Antithesis / negation-as-rhetoric — BANNED as decoration.** `X, not Y` · `not X but Y` ·
`X rather than Y` · `less X than Y` · `not only X but` · `X is what Y does not satisfy` ·
`more than just X` · `not in competition with X` · `treat X as Y rather than as Z` ·
`on one hand … on the other hand` (when only one side is made).
- **Keep-vs-cut test:** would the sentence lose factual content if the negation were removed and
  rewritten positively? If yes, the negation is doing factual work — keep it. If no, it was
  rhetorical — cut it and assert the positive.
- ✗ `measured, not asserted` → ✓ `measured on real studies`
- ✗ `Cross-layer coupling, not any single constraint, makes it hard.` → ✓ `Cross-layer coupling makes it hard.`
- **Factual negations to KEEP:** "students cannot derive…" (a real capability gap), "the system
  fails when…" (a real failure mode), "39 of whom had taken…" (a quantitative fact).

**M3. Editorializing / promotional closers — BANNED.** Sentences whose only job is to tell the
reader how to feel about the previous sentence: `the saving is the point` · `is not real` ·
`this is the empirical content of the claim` · `a testament to X` · `the kind of Y that…` ·
`a concrete demonstration of`. State the fact and trust it.

**M4. Vacuous intensifiers and hype — BANNED.** `in effect` · `in a sense` · `at its core` ·
`at its heart` · `in essence` · `truly` · `genuinely` · `simply` · `essentially` ·
`fundamentally` (as filler) · `real(ly)` · `in fact` · `indeed` · `actually` · `exactly` ·
`precisely because` · `armed with` · `seamless(ly)`. Delete, or replace with the precise referent.

**M5. Banned adjectives — BANNED (replace with a number or delete).** `novel` · `significant` ·
`substantial` · `impressive` · `promising` · `comprehensive` · `robust` · `powerful` ·
`state-of-the-art` (except when naming a specific prior system) · `paradigm` · `leverage` (verb) ·
`utilize`. They assert quality without stating it.

**M6. Throat-clearing openers — BANNED.** A sentence starting with `Moreover` · `Furthermore` ·
`Additionally` · `Notably` · `Importantly` · `Indeed` · `Ultimately` · `Crucially` · `In turn` ·
`That said` · `It is worth noting that` · `It should be noted that`. Start with the claim.
(Register exception, see `craft_reference.md`: in the narrative/position register a discourse
marker is kept ONLY when a real logical move follows it.)

**M7. Rule-of-three / triadic decoration — USE SPARINGLY.** Three-part parallel lists read as
generated when the members are near-synonyms or padding, or when `X does A, B, and C` repeats for
three consecutive sentences. Keep a triad only when all three items are distinct and load-bearing.

**M8. Grandiose empty setup sentences — BANNED.** Sentences that announce importance and defer the
payoff: `What sets X apart is…` · `The key insight is…` · `Two commitments no predecessor made…`.
State the insight directly.

**M9. Branded / evocative metaphor filler — BANNED.** Impressive-sounding metaphors with no
operational content: `the machine that runs any spec` · `draw their power from` · `under the hood`
· `X meets Y` (`where structure meets scale`). Use the mechanism.

**M10. Vague-mechanism / futurist hype verbs — AUDIT (replace with a stated mechanism).**
`keeps X from Y` · `stands in the way of` · `unlocks` · `powers` · `promises to` · `stands to` ·
`is poised to` · `opens the door to` · `is set to` · `has the potential to`. Each hides an unstated
mechanism. Replace with the mechanism, a named question, or a confidence-graded claim.

**M11. Passive voice — BANNED, zero exceptions, including methods and evaluation.** `X was
achieved by Y` → `Y achieves X`; `experiments were conducted` → `we evaluate`; `data was
collected` → `we collect`. Never generate passive technical prose. Every passive hit is fixed or
explicitly justified.

**M12. Needless words / wordiness — CUT.** `the fact that` · `the question as to whether` →
`whether` · `in order to` → `to` · `there is no doubt but that` → `no doubt` · `he/she is a X who`
→ `X` · `the reason … is because` → `because` · `owing to the fact that` → `since` ·
`in a <adj> manner` → `<adv>ly` · `this is a subject that` → `this subject` · `in terms of` ·
`one of the most` · `in the last analysis`.

**M13. Weak qualifiers — DELETE.** `rather` · `very` · `pretty` · `little` (as intensifier) ·
`quite` · `somewhat` · `fairly` · `certainly` (as emphasis). Use an inherently strong word instead.

**M14. Coined adverbs and false ordinals — FIX.** No `thusly` · `muchly` · `overly` ·
`firstly/secondly/thirdly` (use `first/second/third`) · `-wise` pseudo-suffix (`taxwise`).

**M15. Exclamation marks and stray rhetorical questions — BANNED.** No `!` in technical prose.
Rhetorical questions only in an introduction, at most one.

**M16. Pompous / vague single words — AUDIT (see also M5).** `finalize` · `-oriented` · `factor` ·
`feature` · `meaningful` · `insightful` · `prestigious` · `possess` (→ `have`) · `impact` (verb;
prefer `affect`) · `contact` (verb) · `currently` (redundant with present tense).

**M17. Fancy / showy / figurative verbs — AUDIT (prefer the plain word).** When a plain verb
exists, a colorful one reads as authorial performance. Keep the fancy verb ONLY when it is precise
domain jargon with no plain equivalent (`amortize` a cost, `traverse` a graph, `spawn` a process).
- `pit (X against Y)` → `run` · `dispatch` → `send` · `chip away at` → `narrow`/`reduce` ·
  `marshal` → `gather` · `orchestrate` → `coordinate`/`run` · `wrangle` → `manage` · `harness` →
  `use` · `forge` → `build` · `weave` → `combine` · `usher in` → `start` · `delve into` →
  `examine` · `grapple with` → `address` · `surface` (v.) → `reveal` · `unpack` (fig.) → `explain`
  · `anew`/`afresh` → `again`.
- Polysemous words (`bind`, `surface`, `run`, `chip`) are JUDGE-only: read for the flavor sense; do
  not blanket-replace.

**M18. Content-free openers — BANNED.** `In this paper, we…` and `In this section, we describe…`
Delete and lead with the claim (see `craft_reference.md` on signposting through claims).

---

## Part B — Precision word pairs (inspect each hit for the wrong member)

Not banned, but frequently misused. Flag and check: `affect`/`effect` · `imply`/`infer` ·
`comprise` (avoid "is comprised of") · `data is` (data is plural) · `fewer`/`less` ·
`farther`/`further` · `that`/`which` (restrictive vs. nonrestrictive) · `unique` (no "very
unique") · `different than` (→ `different from`) · `due to` (→ `because of` in adverbial use).

---

## Part C — The grep script (RUN THIS; do not eyeball)

```bash
# ── M1: Em-dashes (must return nothing) ──
grep -rn -- '---' sections/*.tex ; grep -rn $'—\|–' sections/*.tex

# ── M11: PASSIVE VOICE (also runs in the base per-edit gate). Over-catches on purpose. ──
grep -rnoE "\b(is|are|was|were|be|been|being)\s+([a-z]+ed|done|made|shown|given|taken|held|built|drawn|chosen|written|known|found|seen|set|put|sent|kept|met|run|used|based)\b" sections/*.tex

# ── M2: Antithesis / negation-as-rhetoric (inspect each; "not" branch matches digit/letter/\macro) ──
grep -rnoE ",? not [0-9A-Za-z\\]|not only .* but|rather than|less .* than|is the point|whatever it is|means nothing|more than just|not in competition with|on one hand|on the other hand" sections/*.tex

# ── M3/M4/M8/M9: editorializing, intensifiers, grandiose setups, metaphor filler ──
grep -rnoE "in effect|in a sense|at (its|the) (heart|core)|in essence|\btruly\b|\bgenuinely\b|\bindeed\b|\bin fact\b|precisely because|a testament to|the kind of .* that|exactly the kind|is the point|set(s)? .* apart|no (predecessor|one) .* (made|posed)|the key (insight|idea) is|the machine that|draw(s)? .* power from|under the hood|where .* meets" sections/*.tex

# ── M6: throat-clearing openers ──
grep -rnE "^(Moreover|Furthermore|Additionally|Notably|Importantly|Indeed|Ultimately|Crucially|In turn|That said)" sections/*.tex

# ── M5/M16: banned + pompous words ──
grep -rnoiE "\bnovel\b|\bsignificant\b|\bsubstantial\b|\bimpressive\b|\bpromising\b|\bcomprehensive\b|\brobust\b|\bpowerful\b|\bseamless|\bcrucial\b|\bparadigm\b|\bleverag|\butiliz|\bfinaliz|[a-z]+-oriented\b|\bfactor\b|\bfeature[ds]?\b|\bmeaningful\b|\binsightful\b|\bprestigious\b|\bpossess|\bcontact(s|ed|ing)?\b|\bcurrently\b|\bimpact(s|ed|ing)?\b" sections/*.tex

# ── M10: vague-mechanism / futurist hype verbs ──
grep -rnoiE "promises to|stands? to|is poised to|opens the door to|is set to|has the potential to|keeps .* from|stands? in the way|\bunlocks?\b" sections/*.tex

# ── M17: fancy / figurative verbs (inspect each; keep only precise domain jargon like "amortize") ──
grep -rnoiE "\b(pit(s|ted|ting)?|dispatch(es|ed|ing)?|chip(s|ped|ping)? (away )?at|marshal(s|led|ling)?|orchestrat(e|es|ed|ing)|wrangl(e|es|ed|ing)|harness(es|ed|ing)?|forge[sd]?|weav(e|es|ed|ing)|delv(e|es|ed|ing) into|usher(s|ed)? in|grappl(e|es|ed|ing) with|anew|afresh)\b" sections/*.tex

# ── M18: content-free openers ──
grep -rnE "In this (paper|section), we" sections/*.tex

# ── M12: needless words / wordiness ──
grep -rnoE "the fact that|the question (as to |of )?whether|as to whether|in order to|there is no doubt but|the reason .* is because|owing to the fact that|in a [a-z]+ manner|is a (subject|man|woman) (that|who)|in the last analysis|along these lines|in terms of|one of the most" sections/*.tex

# ── M13: weak qualifiers ──
grep -rnoiE "\b(rather|very|pretty|little|quite|somewhat|fairly|certainly)\b" sections/*.tex

# ── M14: coined adverbs / false ordinals ──
grep -rnoiE "\b(thusly|muchly|overly|firstly|secondly|thirdly)\b|[a-z]+wise\b" sections/*.tex

# ── M15: exclamation marks ──
grep -rn "!" sections/*.tex

# ── Part B: precision pairs (inspect for wrong member) ──
grep -rnoiE "\bcomprised of\b|\bdata is\b|different than|\bvery unique\b|\bdue to\b|\bless (than )?[0-9]" sections/*.tex

# ── Term/decomposition drift (see gate_semantic S8/S9): one name per concept ──
#   (a) For each canonical term in the project_context term-map, show every surface form + location,
#       then converge. Replace the list with the paper's real synonym clusters.
for t in "application workflow" "application behavior" "bottleneck conditions" "network conditions"; do
  echo "== $t =="; grep -rn "$t" sections/*.tex | grep -v '^\s*%'; done
#   (b) Decomposition cardinality: do all the "N <noun>" counts agree across sections?
grep -rnoE "\b(three|four|five|six|seven)\b (stages|concerns|axes|requirements|dimensions|systems)" sections/*.tex
```

Report the per-category hit counts in the audit summary. "Clean" means every hit fixed or
justified, with the counts shown.
