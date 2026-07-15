# Elements of Style Checklist (Strunk & White)

> A faithful codification of *The Elements of Style* (Strunk & White, 4th ed.) into a checkable
> audit layer. This is the **slow, thorough tier**: it runs in the independent red-team and
> `/loop` passes (see `red_team_protocol.md`, `loop_mode.md`), NOT on every keystroke. The one
> exception is **active voice**, which is cheap to detect and runs in the base per-edit gate.
> Added 2026-07 at the author's request to give the de-AI process a richer, fuller specification.

## Why this file exists

The `de_ai_checklist.md` targets the *machine-writing fingerprint* (em-dashes, antithesis
flourishes, grandiose setups). This file targets *general craft* — the older, deeper failures of
weak, vague, wordy, or evasive prose that Strunk & White catalogued. The two layers reinforce
each other; where they overlap, this file cross-references rather than restates. It does not
replace `voice_profile.md` (sentence length, claim-first) or `accessibility_checklist.md`
(G1–G6 semantics); it sits alongside them.

**This book has ~22 rules + ~110 usage entries + 21 reminders, not 1000.** Padding it to a
bigger number would fabricate rules and defeat the purpose. The value is in *tagging* each real
rule so it is actually run, not in inflating the count. Runnability × precision, not volume.

## How to read the tags

Every item carries a severity + an enforcement tag:

- **`[MECH]`** — mechanically detectable. Has a grep or a concrete textual trigger. Goes in the
  grep gate at the bottom (§Mechanical gate).
- **`[JUDGE]`** — semantic judgment. A reviewer must read for it; no grep suffices. Runs in the
  red-team / loop pass only.
- **`[SKIP-SYS]`** — a real rule in the book, but **contested, dated, or handled elsewhere for
  systems/CS prose**. Listed for completeness and explicitly NOT enforced, so this file never
  quietly degrades a methods section or fights LaTeX. Each says why.

Overlap markers point to the sibling file that already owns the rule (e.g. `↔ de_ai A4`).

---

## I. Elementary Rules of Usage (Rules 1–11)

Mostly grammar and punctuation. Several are LaTeX/venue concerns for us, hence `[SKIP-SYS]`.

- **E1. Form the possessive singular of nouns by adding `'s`.** `[SKIP-SYS]` — orthographic; rare
  in technical prose and copy-editor territory. (Strunk: *Charles's friend*, *the witch's malice*;
  pronominal possessives *its, hers, theirs* take no apostrophe; do not confuse `it's`/`its`.)
- **E2. In a series of three or more terms with a single conjunction, use a comma after each term
  except the last (the serial/Oxford comma).** `[MECH]` — pick one convention per paper and hold
  it. Default to the serial comma unless `project_context.md` / venue says otherwise.
  Grep for risk: lists of three joined by `and`/`or` with no comma before the conjunction.
- **E3. Enclose parenthetic expressions between commas.** `[JUDGE]` — the load-bearing half:
  *never open a parenthetic comma and fail to close it*. Nonrestrictive clauses take commas;
  restrictive clauses take none (`that` vs `which`, see W-entry). Check both commas are present.
- **E4. Place a comma before a conjunction introducing an independent clause.** `[JUDGE]` — comma
  before `and/but/for/or/nor` joining two independent clauses; omit when the subject is shared and
  the relation is close.
- **E5. Do not join independent clauses with a comma (no comma splice).** `[MECH]` — use a
  semicolon or period. A comma splice is a hard error. `[JUDGE]` to confirm each flagged comma
  actually joins two independent clauses.
- **E6. Do not break sentences in two (no period for a comma; no sentence fragments).** `[MECH]` —
  flag fragments: a "sentence" with no finite main verb, or one opening with a subordinator
  (`Because…`, `Which…`, `Coming home…`) and ending in a period. Fragments are almost never right
  in technical prose.
- **E7. Use a colon after an independent clause to introduce a list, appositive, amplification, or
  quotation.** `[JUDGE]` — the colon must follow a complete clause; do NOT split a verb from its
  object (`requires: a knife` → `requires three props: a knife`). Also joins two clauses when the
  second amplifies the first.
- **E8. Use a dash to set off an abrupt break or a long appositive — sparingly.** `[SKIP-SYS,
  OVERRIDDEN]` — **the book permits the dash; our house style BANS it.** See `de_ai A1` (em-dashes
  are the single strongest machine-writing tell). Convert every dash to a comma, colon,
  parentheses, or full stop. This is a deliberate departure from Strunk & White.
- **E9. The number of the subject determines the number of the verb.** `[JUDGE]` — subject–verb
  agreement across intervening phrases. Watch `one of those who <plural verb>`; `each/either/
  everyone/nobody` take singular; a compound subject with `and` takes plural; `with / as well as /
  together with` do NOT make a singular subject plural. `data` is plural (see W-entry).
- **E10. Use the proper case of pronoun (who/whom, I/me, nominative in comparisons).** `[SKIP-SYS]`
  — rarely the failure mode in systems papers; low yield. Reviewer may still fix egregious `whom`
  errors by ear.
- **E11. A participial phrase at the beginning of a sentence must refer to the grammatical
  subject (no dangling modifiers).** `[JUDGE]` — high value. `Using this dataset, accuracy
  improved` dangles (accuracy did not use the dataset) → `Using this dataset, we improved
  accuracy`. Check every sentence-initial `-ing`/`-ed` phrase points at the subject that follows.

---

## II. Elementary Principles of Composition (Rules 12–22)

The heart of the book. This is where the craft lives — mostly `[JUDGE]`, and mostly reinforcing
`voice_profile.md` and `de_ai_checklist.md`.

- **E12. Choose a suitable design and hold to it.** `[JUDGE]` — the piece has a deliberate shape
  the writer commits to before drafting. ↔ maps to the skill's Architecture stage and
  topic-sentence-first drafting.
- **E13. Make the paragraph the unit of composition.** `[JUDGE]` — one topic per paragraph;
  begin with a sentence that states the topic or bridges from the last (↔ `voice_profile` claim-
  first topic sentences). Do NOT print single sentences as paragraphs (except transitions). Avoid
  both formidable blocks and a spray of tiny paragraphs.
- **E14. USE THE ACTIVE VOICE.** `[MECH]+[JUDGE]` — **HARD BAN on passive, no exceptions, enforced
  in the BASE per-edit gate.** The book itself hedges ("the passive is frequently convenient");
  **we override that hedge.** Active voice everywhere — including methods and evaluation. Replace
  `X was achieved by Y` → `Y achieves X`; `there is / could be heard` → a transitive active verb;
  `dead leaves were lying on the ground` → `dead leaves covered the ground`. Brevity is a
  by-product of vigor: the active version is almost always shorter. ↔ `voice_profile` item 6,
  `SKILL.md` audit item 6. See the passive-detection grep in §Mechanical gate. **Every passive
  hit is fixed or explicitly justified; never generate passive technical prose.**
- **E15. Put statements in positive form.** `[JUDGE]` — assert what IS, not what is not. `not
  honest` → `dishonest`; `did not remember` → `forgot`; `was not often on time` → `usually came
  late`. Use `not` only for genuine denial/antithesis, never for evasion. Save `would/should/
  could/may/might/can` for real uncertainty; needless conditionals read as irresolute. ↔ `de_ai
  A2` (antithesis) and `voice_profile` zero-hedging.
- **E16. Use definite, specific, concrete language.** `[JUDGE]` — prefer the specific to the
  general, concrete to abstract. `A period of unfavorable weather set in` → `It rained every day
  for a week`. Name the mechanism, the number, the system. ↔ `de_ai` Shenker register "concrete
  over abstract"; ↔ Principle "named-over-vague."
- **E17. OMIT NEEDLESS WORDS.** `[MECH]+[JUDGE]` — every word must tell. The core wordiness
  phrases are greppable (see §Mechanical gate): `the fact that`, `the question as to whether` →
  `whether`, `in order to` → `to`, `there is no doubt but that` → `no doubt`, `he is a man who`
  → `he`, `the reason … is because` → `because`, `owing to the fact that` → `since`, `who is /
  which was` (often deletable), `in a <adj> manner` → `<adv>ly`, `this is a subject that` → `this
  subject`. ↔ `compression_patterns.md` (this rule IS the intellectual root of that file).
- **E18. Avoid a succession of loose sentences.** `[JUDGE]` — do not string many two-clause
  sentences joined by `and`/`but`/`which`/`while`. Symptom: paragraph with sing-song mechanical
  symmetry. Remedy: recast some as simple, some semicolon-joined, some periodic. Detect with the
  "three or more consecutive sentences containing a mid-sentence `, and`/`, but`/`, which`" heuristic.
- **E19. Express coordinate ideas in similar form (parallel construction).** `[JUDGE]` — likeness
  of form signals likeness of content. Correlatives (`both…and`, `not only…but also`, `either…or`,
  `first…second`) must be followed by the same grammatical construction. An article/preposition
  before a series is used once before all, or repeated before each. ↔ `accessibility G` structural
  coherence.
- **E20. Keep related words together.** `[JUDGE]` — position shows relationship. Keep subject and
  main verb close; put modifiers next to what they modify; `only`/`not all` go next to the word
  they qualify (`She only found two` → `She found only two`; `All the members were not present` →
  `Not all the members were present`). The relative pronoun sits right after its antecedent.
- **E21. In summaries, keep to one tense.** `[JUDGE]` — for us: describe prior work / a system's
  behavior in a single consistent tense; do not drift between present and past. Do not overwork
  `he said / the authors state / we also note` when summarizing — signal once that a passage is
  summary, then stop repeating the attributive.
- **E22. Place the emphatic words of a sentence at the end.** `[JUDGE]` — the new, load-bearing
  element (the logical predicate) goes last; the second-strongest position is the start. `Humanity
  has hardly advanced in fortitude, though it has advanced in many other ways` →
  `…humanity has advanced in many ways, but it has hardly advanced in fortitude`. Applies to words
  in a sentence, sentences in a paragraph, paragraphs in a section.

---

## III. A Few Matters of Form

Almost all of these are handled by LaTeX, the venue class file, or `de_ai`/`voice_profile`, so most
are `[SKIP-SYS]`. Two are live.

- **Exclamations.** `[MECH]` — no exclamation marks in technical prose. ↔ `de_ai A8`.
- **Colloquialisms.** `[JUDGE]` — if a colloquial term is used, use it plainly; do not flag it
  with scare quotes ("putting on airs"). Low frequency; reviewer judgment.
- **Headings / Margins / Syllabication / Titles / Numerals / Parentheses / Quotations /
  References / Hyphen.** `[SKIP-SYS]` — LaTeX + `bibtex` + the venue style own all of these
  (manuscript margins, hyphenation, citation punctuation, italic titles, numeral spelling). Do not
  hand-audit. The one carry-over worth noting: **numerals** — follow the venue's number style
  (usually spell out < 10 in prose, figures for measurements/data); check `project_context.md`.

---

## IV. Words and Expressions Commonly Misused

The full list, faithfully preserved and tagged by relevance to technical prose. Grep patterns for
the enforceable subset are in §Mechanical gate.

### IV-A. Enforce in technical prose (`[MECH]` — pompous, vague, or wordy; they appear constantly)

- **Utilize → use.** The signature pompous verb. ↔ already in `de_ai A4` / `voice_profile` banned.
- **-ize coinages.** Do not coin verbs with `-ize` (`prioritize` is tolerated; `operationalize`,
  `incentivize`, `architect-ize` are not). A plain verb usually exists.
- **Finalize.** Pompous, ambiguous. Use `finish`, `complete`, `settle`.
- **-oriented.** Clumsy and pretentious (`performance-oriented`) — name the actual relation.
- **Factor.** Hackneyed; usually replace with something direct (`the great factor in` → say what
  caused it).
- **Feature (n. and v.).** Usually adds nothing; as a verb ("offers as a special attraction"),
  avoid.
- **Meaningful.** A bankrupt adjective (`a meaningful improvement`) — choose a real one or rephrase.
- **Insightful.** Suspicious overstatement for `perceptive`; often inflates the commonplace.
- **Prestigious / Relate / Personalize / Possess / -wise / Thrust.** Words of last resort or
  pretension; `possess` → `have`; `-wise` pseudo-suffix banned; `thrust` (n.) showy.
- **Currently.** Redundant with a present-tense verb; delete or give a precise time.
- **In terms of.** Padding; usually deletable (`unattractive in terms of salary` → `the salary
  made it unattractive`).
- **The fact that / the truth is… / the fact is… / in the last analysis / along these lines.**
  Debilitating filler; cut or recast. ↔ E17, `compression_patterns`.
- **Case / Character / Nature.** Often pure wordiness (`in many cases` → often; `acts of a
  hostile nature` → `hostile acts`).
- **He is a man who / one of the most.** Redundant / threadbare formulas — trim.
- **Contact (v.).** Vague and self-important; prefer a specific verb.
- **Noun used as verb.** Suspect by default (`the meeting was chaired` is fine and established;
  `she headquarters in Newark`, `to gift`, `to debut` — audit). Common CS offenders to watch:
  `to architect`, `to leverage` (↔ `de_ai` banned), `to action`, `to impact` (prefer `affect`).
- **Importantly.** Prefer `more important` to `more importantly`. ↔ `de_ai A6` throat-clearing.
- **Interesting / Funny.** Do not announce it; make it so.
- **Very / So / Certainly / Rather / Pretty / Little (as intensifier).** Weak intensifiers; delete
  and use inherently strong words. ↔ `de_ai A4`, White Reminder 8.

### IV-B. Precision distinctions worth a `[JUDGE]` read (correctness, not style)

- **Affect / Effect** (influence vs. result/bring about) · **Imply / Infer** (suggest vs. deduce) ·
  **Comprise** (the whole comprises the parts; parts do not comprise the whole; avoid "is comprised
  of") · **Data** (plural: "these data are") · **Fewer / Less** (number vs. quantity) ·
  **Farther / Further** (distance vs. degree) · **That / Which** (restrictive `that`, no comma;
  nonrestrictive `which`, comma) · **Unique** (admits no degree; no "very unique") ·
  **Disinterested / Uninterested** (impartial vs. bored) · **Enormity** (monstrous wickedness, not
  bigness) · **Literal/Literally** (not for emphasis or metaphor) · **Nauseous / Nauseated** ·
  **Compare to / compare with** (resemblance vs. examine differences) · **Different from** (not
  "different than") · **Due to** ("attributable to"; not for "because of" in adverbial use) ·
  **Alternate / Alternative** · **Among / Between** · **Presently** (soon, not currently) ·
  **Fortuitous** (by chance, not fortunate) · **Regretful / regrettable** · **Tortuous /
  torturous** · **Transpire** (become known, not happen) · **Principal / principle** (implied) ·
  **Allude / elude; allusion / illusion** · **Verbal / oral** · **Partially / partly** ·
  **Respective(ly)** (usually deletable) · **Than** (check no words missing from the comparison).

### IV-C. Listed for completeness, `[SKIP-SYS]` (archaic, dialogue-only, or non-issues for us)

`All right` (two words) · `And/or` (avoid in prose — mildly relevant) · `Anticipate/expect` ·
`Anybody/any body`, `Anyone/any one` · `As to whether → whether` (mildly relevant, ↔ E17) ·
`As yet → yet` · `Being` · `But` (redundant after doubt/help) · `Can/may` · `Care less` · `Claim`
(v.) · `Clever` · `Consider` (no "as") · `Cope` · `Divided into / composed of` · `Each and every
one` · `Enthuse` · `Etc.` (a misfit in formal writing; name the item) · `Facility` (don't dodge
the concrete noun — mildly relevant) · `Fix` · `Flammable/inflammable` · `Folk(s)` · `Get / have
got / gotten` · `Gratuitous` · `Hopefully` (not for "I hope" — mildly relevant) · `However`
(don't start a sentence with it meaning "nevertheless" — ↔ `de_ai A6`) · `In regard to` (not "in
regards to") · `Inside of` · `Irregardless → regardless` · `Kind of / sort of` · `Lay / lie` ·
`Leave / let` · `Like / as` (conjunction) · `Line / along these lines` · `Loan / lend` · `Memento`
· `Most / almost` · `Nice` · `Nor / or` · `Offputting / Ongoing` · `One` (+ his/her) · `Participle
for verbal noun` (prefer possessive with gerund) · `People / persons` · `Personally` · `Secondly,
thirdly (→ second, third)` · `Shall / will` · `Split infinitive` (do NOT enforce — the book itself
allows it for stress; splitting is often clearer) · `State` (not for "say") · `Student body` ·
`Thanking you in advance` · `The foreseeable future` · `They` with singular antecedent (use
singular or recast; modern usage diverges — reviewer judgment) · `This` (vague reference to a whole
clause — ↔ `accessibility G4` directness) · `Try to` (not "try and") · `Type` · `While` (not for
"although/and" indiscriminately) · `Worth while / worthwhile` · `Would` (habitual action).

> Note: a handful in IV-C are genuinely useful (`however`, `hopefully`, `this`-reference,
> `as to whether`, `and/or`, `facility`) and are cross-referenced where a sibling rule already
> enforces them. The rest are dialogue-, register-, or dictionary-level and stay unenforced.

---

## V. An Approach to Style — White's 21 Reminders

White's chapter is the closest thing in the book to a de-AI manifesto. Almost all `[JUDGE]`; each
is a lens the red-team reads with. Several map directly onto existing house rules.

- **R1. Place yourself in the background.** `[JUDGE]` — attention on the substance, not the author.
  ↔ zero editorializing (`de_ai A3`).
- **R2. Write in a way that comes naturally.** `[JUDGE]` — plain, ready words; still revise.
- **R3. Work from a suitable design.** `[JUDGE]` — ↔ E12, Architecture stage.
- **R4. Write with nouns and verbs, not adjectives and adverbs.** `[JUDGE]` — "the adjective hasn't
  been built that can pull a weak noun out of a tight place." ↔ Shenker "verbs over
  nominalizations," E16.
- **R5. Revise and rewrite.** `[JUDGE]` — restructuring is normal, not failure. ↔ the whole
  pipeline (introduction-twice, compression).
- **R6. Do not overwrite.** `[JUDGE]` — ornate prose is unwholesome; reread and cut. ↔ `de_ai A4`,
  E17.
- **R7. Do not overstate.** `[JUDGE]` — one carefree superlative destroys reader trust in the
  whole. ↔ banned hype adjectives (`de_ai A4`: novel, significant, powerful…).
- **R8. Avoid the use of qualifiers.** `[MECH]` — `rather, very, little, pretty` are "leeches that
  infest the pond of prose." Grep and delete. ↔ `de_ai A4`, `voice_profile` hedging.
- **R9. Do not affect a breezy manner.** `[JUDGE]` — no windy, showing-off, self-amused prose.
- **R10. Use orthodox spelling.** `[SKIP-SYS]` — spell-checker + venue own this.
- **R11. Do not explain too much.** `[JUDGE]` — sparing with explanatory adverbs on attributives;
  trust the content. ↔ E21 (don't overwork "the authors state").
- **R12. Do not construct awkward adverbs.** `[MECH]` — no coined `-ly` monsters (`thusly, muchly,
  overly, tangledly`). Grep `thusly|muchly|overly|firstly|secondly|thirdly`.
- **R13. Make sure the reader knows who is speaking.** `[JUDGE]` — for us: attribute claims and
  contributions clearly; never leave the agent of a statement ambiguous. ↔ `accessibility` clarity.
- **R14. Avoid fancy words.** `[MECH]+[JUDGE]` — prefer the short Anglo-Saxon word to the
  twenty-dollar Latinate one, judged by ear. ↔ Shenker "plain words over Latinate": `use` not
  `utilize`, `enough` not `sufficient`, `show` not `demonstrate`, `let` not `enable` where it fits.
- **R15. Do not use dialect unless your ear is good.** `[SKIP-SYS]` — not applicable to technical
  prose.
- **R16. Be clear.** `[JUDGE]` — the cardinal virtue. When a sentence gets muddy, start fresh and
  break it into shorter sentences. "When you say something, make sure you have said it." ↔
  `accessibility G4` directness, Shenker short declaratives.
- **R17. Do not inject opinion.** `[JUDGE]` — no gratuitous or ill-timed opinion; ↔ `de_ai A3`
  editorializing closers.
- **R18. Use figures of speech sparingly.** `[JUDGE]` — no rapid-fire similes; never mix a
  metaphor. ↔ `de_ai A10` (evocative metaphor filler).
- **R19. Do not take shortcuts at the cost of clarity.** `[MECH-assist]+[JUDGE]` — spell out every
  acronym / initialism at first use, then abbreviate. ↔ `accessibility G1` (define before use),
  G3 (term-introduction order). Grep for capitalized initialisms lacking a preceding expansion.
- **R20. Avoid foreign languages.** `[JUDGE]` — no gratuitous Latin/French flourish (`inter alia`,
  `a priori` when a plain phrase works). "Write in English."
- **R21. Prefer the standard to the offbeat.** `[JUDGE]` — no slang, ad-speak, or business jargon
  (`finalize`, `leverage`, `impactful`, `learnings`). ↔ `de_ai` banned-word spirit.

---

## Mechanical gate (the `[MECH]` subset — run in red-team; active-voice also in base gate)

```bash
# ── ACTIVE VOICE (E14) — also runs in the BASE per-edit gate. Zero tolerance. ──
# "be"-verb + past participle. Over-catches on purpose; fix or justify every hit.
grep -rnoE "\b(is|are|was|were|be|been|being)\s+([a-z]+ed|done|made|shown|given|taken|held|built|drawn|chosen|written|known|found|seen|set|put|sent|kept|met|run|used|based)\b" sections/*.tex

# ── OMIT NEEDLESS WORDS (E17) — wordiness phrases ──
grep -rnoE "the fact that|the question (as to |of )?whether|in order to|there is no doubt but|the reason .* is because|owing to the fact that|in a [a-z]+ manner|is a (subject|man|woman) (that|who)|in the last analysis|along these lines|in terms of|one of the most" sections/*.tex

# ── POMPOUS / VAGUE WORDS (IV-A) ──
grep -rnoiE "\butiliz|\bfinaliz|[a-z]+-oriented\b|\bfactor\b|\bfeature[ds]?\b|\bmeaningful\b|\binsightful\b|\bprestigious\b|\bcurrently\b|\bpossess(es|ed)?\b|[a-z]+wise\b|\bimpact(s|ed|ing)?\b|\bleverag" sections/*.tex

# ── QUALIFIERS / WEAK INTENSIFIERS (R8) ──
grep -rnoiE "\b(rather|very|pretty|little|certainly|quite|somewhat|fairly)\b" sections/*.tex

# ── COINED -ly ADVERBS & FALSE ORDINALS (R12) ──
grep -rnoiE "\b(thusly|muchly|overly|firstly|secondly|thirdly|importantly)\b" sections/*.tex

# ── COMMA SPLICE / FRAGMENT RISK (E5/E6) — inspect each ──
# (heuristic; requires human confirmation that the flag is a real independent-clause splice)

# ── PRECISION PAIRS (IV-B) — inspect each hit for the wrong member ──
grep -rnoiE "\bcomprised of\b|\bdata is\b|\bless (than )?[0-9]|different than|\bvery unique\b|\bdue to\b|as to whether" sections/*.tex

# ── EXCLAMATION MARKS (III) ──
grep -rn "!" sections/*.tex
```

Report the counts, not "audited." Every hit is fixed or explicitly justified before a section is
marked clean. The active-voice grep is the one that also fires in the fast base gate.

---

## Deliberate departures from Strunk & White (documented)

So this file's provenance is honest:

1. **Dashes (E8): the book allows them; we BAN them.** ↔ `de_ai A1`.
2. **Passive voice (E14): the book tolerates it "when convenient"; we forbid it outright,** in
   every section including methods and evaluation, enforced in the base gate. This is a standing
   author preference, not a style suggestion.
3. **Split infinitives, `shall`/`will`, `he or she` vs singular `they`, serial-comma-by-firm, and
   the dialogue/spelling rules** are `[SKIP-SYS]` — dated or venue-owned.

---

## How to apply

- **Red-team / loop only** (`red_team_protocol.md`, `loop_mode.md`): run the `[JUDGE]` items as
  reading lenses and the full `[MECH]` grep gate above, one section per iteration.
- **Base per-edit gate:** only the **active-voice** grep runs here (E14) — it is cheap and the
  author's hard rule.
- Where an item says `↔`, the sibling file already owns it; do not double-report. This file adds
  the craft layer (Rules 12–22, the usage distinctions, White's reminders) that the de-AI and
  accessibility files do not cover.
