# Craft Reference — how to write (the positive layer)

> The "what to write INSTEAD" companion to the two gates. `gate_mechanical.md` and
> `gate_semantic.md` catch defects; this file is drafting guidance. It is a reference to read while
> writing or rewriting a paragraph, not a checklist to run.

The base rules below hold in every register. Three registers then flex a few of them for the
paper's job. All registers are distilled from sentence-level analysis of well-written published
papers; they are stated as self-standing rules, not attributed to any author.

---

## Part 1 — The base register (default: terse, empirical)

The default for a systems/ML paper that designs something and measures it.

- **Sentence length.** Mean ~21 words; ~40 max, reserved for contribution-list items. Student
  drafts run ~24; compress by ~12%.
- **Claim-first topic sentences.** Lead with the assertion; cite the table or figure after. Never
  open a paragraph with background or context.
- **Active voice, present tense, direct verbs.** No exceptions (see `gate_mechanical.md` M11).
- **Plain words over Latinate.** `use` not `utilize`; `enough` not `sufficient`; `show` not
  `demonstrate`; `let` not `enable` where it fits.
- **Verbs over nominalizations.** `we compile the intent`, not `compilation of the intent is
  performed`.
- **Concrete over abstract; named over vague.** Name the mechanism, the number, the system. Put an
  agent in the subject slot, not "the existence of X." Every mechanism, baseline, and metric has a
  proper name; if a term could apply to any paper in the field, it does not belong.
- **Numbers, not adjectives.** "2–4× higher savings," never "significant savings."
- **Positive form.** Assert what IS. `not honest` → `dishonest`; `did not remember` → `forgot`.
  Use `not` only for a real factual absence, never as a rhetorical move.
- **Omit needless words.** Every word tells (see `gate_mechanical.md` M12, `compression_patterns.md`).
- **Paragraph density.** 3–6 sentences (design paragraphs up to 8). Every paragraph does exactly
  one of: make a claim, present evidence, synthesize a takeaway. No empty connector paragraphs.
- **Signposting through claims, not placeholders.** A section may open with a preamble that states
  the section's conclusion ("This section shows that event-centric decomposition reduces error
  13×"). Banned: content-free preambles ("In this section, we describe our setup"). Test: does the
  opener tell a skim-reader what the section *concludes*?
- **Calibrated confidence.** Strongly assertive; claims match evidence scope. No "we solve the
  problem of" — instead "we address [specific aspect] of [bounded problem]." Zero hedging, with the
  one exception in `gate_semantic.md` S18 (hedge a claim that genuinely outruns its evidence).
- **Contribution framing.** Numbered, labeled list, each item a bold domain label + a claim (not a
  process description): "(1) Learning problem. We formulate… (2) Design. We implement… (3)
  Evaluation. We show…". Prefer "We show" over "We propose."
- **Headings.** Claim-first, one typeset line, consistent capitalization across the document, no
  terminal punctuation on section titles. "Event-centric decomposition reduces error 13×," not
  "Experimental Results."
- **Emphatic words at the end.** Put the new, load-bearing element last; the second-strongest
  position is the start.
- **Parallel construction** for coordinate ideas; **keep related words together** (modifiers next
  to what they modify; `only`/`not all` next to the word they qualify).

---

## Part 2 — Composition craft (general good-writing principles)

Positive form of the craft items the gates hunt for. Adapted from standard writing craft
(the "omit needless words" tradition).

- **Choose a suitable design and hold to it.** Foresee the shape before drafting (maps to the
  Architecture stage and topic-sentence-first drafting).
- **Make the paragraph the unit of composition.** One topic per paragraph; do not print single
  sentences as paragraphs except transitions.
- **Write with nouns and verbs, not adjectives and adverbs.** A weak noun cannot be rescued by an
  adjective.
- **Use definite, specific, concrete language.** "It rained every day for a week," not "A period of
  unfavorable weather set in."
- **Vary sentence structure.** Avoid a run of loose two-clause sentences joined by `and`/`but`/
  `which`; the sing-song monotone reads as generated. Recast some as simple, some semicolon-joined,
  some periodic.
- **Revise and rewrite.** Restructuring is normal, not failure (maps to introduction-twice and the
  compression stage).
- **Do not overwrite or overstate.** One carefree superlative destroys reader trust in the whole.
- **Be clear.** When a sentence gets muddy, start fresh and split it. "When you say something, make
  sure you have said it."
- **Avoid fancy words, foreign flourishes, and offbeat jargon.** Prefer the short, standard word.
- **Use figures of speech sparingly; never mix a metaphor.**

---

## Part 3 — The plain, declarative register (the default target)

Short, plain, concrete, unpretentious. Say the thing, then stop.

1. **Short, declarative sentences.** One idea each.
2. **Plain words over Latinate** (as Part 1).
3. **Concrete over abstract.** Name the mechanism, the number, the system.
4. **Claim-first topic sentences.**
5. **Verbs over nominalizations.**
6. **No pomposity.** The tone is a smart colleague at a whiteboard, not a press release.
   Understate; let the result carry the weight.
7. **Cut the last sentence.** Machine prose (and eager student prose) adds a summarizing or
   uplifting closer to most paragraphs. Delete it unless it states a new fact.

---

## Part 4 — The conceptual / position-paper register

Use when the paper **argues a position, proposes an architecture, or is a vision/provocation**
rather than measuring a system. This register RELAXES several base rules (noted); do not apply it
silently, and never to a terse empirical results paper.

**Relaxations from the base:** sentence mean ~28–32 with high variance is fine *iff* every clause
advances one argument; question-titled sections are allowed; the pronouns `one` (impersonal design
generality) and a rare, deliberate `I` (a contestable personal choice) join `we`; essayistic 6–10
sentence paragraphs are allowed; discourse markers (`Thus,` `However,` `Of course,` `Similarly,`)
are kept where they carry the argument's logic. The conceptual vocabulary `fundamental` /
`in essence` / `at the core of` is permitted ONLY when it marks a real reduction or invariant
(delete-test: remove it; if a claim about what is essential is lost, keep it).

**The moves:**
1. **Choose the register before drafting.** It sets hedging level, heading style, and close.
2. **Focus-narrowing disclaimer.** State early what the paper is *not* doing, then what it *is*.
   Pre-empts the "why didn't you do X" reviewer.
3. **One explicit purpose sentence.** "The purpose of this paper is to…".
4. **Hedge the rationale, assert the measurement.** Design *reasoning* is hedged; measured *results*
   are stated flat. **Never hedge a number you measured** — this carve-out holds in every register.
5. **Grade confidence to evidence** on a ladder: structural certainty ("Clearly,") → strong
   expectation ("we expect") → plausible ("it seems plausible") → open ("this is debatable") →
   conceded limit ("Of course,", "impossible to gauge precisely").
6. **Coin one organizing lever or crux and run the argument through it.** Restate later questions in
   its terms. Mark the crux plainly: "the key idea is…", "the hard part is…", "the bottom line is…".
7. **Define by naming.** Introduce terms with "we call / termed," plus a short parenthetical gloss.
8. **Preview the count, then deliver in prose.** "This causes two problems. First,… Second,…".
9. **Name both costs of a trade-off; refuse to over-decide when it is genuinely open.** Defer to
   future experience rather than settling a question the evidence cannot.
10. **Concede before the reader objects.** "Of course, these advantages come at the cost of…".
11. **Front-load an honest limitations enumeration** — in the overview, not buried in a final
    paragraph. A frank "here is what it cannot do" earns trust for what it can.
12. **Challenge the conventional wisdom explicitly.** Name the prevailing assumption, then position
    against it.
13. **Position statement (for a position paper):** one quotable sentence that names the gap and the
    proposed primitive, set off from the prose and reused as a refrain.
14. **Defend deliberate under-specification as a principle,** not a gap, when full specification is
    genuinely impossible in the setting.
15. **Justify a design choice by analogy to a proven precedent** where the analogy carries the
    argument.
16. **Confidence plus humility.** Full confidence in the argument; explicit about its limits.
17. **Intuition before the formal mechanism.** Give the reader the intuition first ("Intuitively,
    routing works by following the straight-line path…"), then the formal description.
18. **Name and reject the obvious alternative first.** Clear the path a reader would ask about
    before presenting yours, to earn trust ("One approach, which we do not take, is…").
19. **Criteria first, then the solution as a compromise against them.** Enumerate the goals the
    solution must satisfy, then present the proposal as meeting them — framed as a pragmatic
    compromise, not an optimum.
20. **Extract the lesson explicitly.** After a result or worked example, state the takeaway in
    plain words so the reader cannot miss it ("the point to take away is…"), then interpret the
    number rather than restating the arithmetic.
21. **Guard against the wrong inference.** Preempt a likely misreading of your own result ("One
    should not conclude from these tests that X, because…").

Cheap moves that also help: state a clean binary (name both options, then contrast in turn); give a
one-sentence approach summary ("In a nutshell, our strategy is…"); orient with backward references
("Recall that in Section 3 we…").

---

## Part 5 — The warm / narrative register

Use for **teaching prose, textbook-style writing, and some introductions** where a reader is being
brought along, not a committee addressed.

1. **Warm, reader-facing register.** Write as if a specific reader is following, not to a committee.
2. **Mixed sentence rhythm.** Intercut short declaratives with longer flowing sentences; avoid runs
   of 3+ sentences of similar length. Do not over-correct into a staccato chain of 10-word
   sentences — the fix for one long sentence is usually two medium ones, not five short ones.
3. **Concrete before abstract.** A vivid example or specific image comes before the named
   abstraction. Show first, name second.
4. **Sparing `I` / `you`** for author presence, in introductions only.
5. **Topic sentences that hook, not preview.** "A student trained on this framework does three
   things" hooks; "The framework's payoff is what students learn" previews.
6. **Discourse markers as narrative glue.** Keep an argument-marking signpost ("However,", "The
   catch is,", "Here,") when a real logical move follows it; strip it when nothing follows.
7. **Close on a complete sentence** carrying the claim, not a punchy slogan fragment.

---

## Part 6 — Choosing the register

| Paper job | Register | What flexes |
|---|---|---|
| Systems/ML design + measurement | Base (Part 1) + plain declarative (Part 3) | nothing — the terse default |
| Position / vision / architecture / provocation | Conceptual (Part 4) | longer sentences, hedged rationale, question headings, `one`/`I` |
| Teaching / textbook / some intros | Warm narrative (Part 5) | mixed rhythm, `I`/`you`, concrete-first |

**Held constant in EVERY register** (never relaxed): claim-first structure, active voice, the
em-dash ban, no filler adjectives, named-over-vague, no passive voice, and — above all — measured
results are asserted plainly, never hedged. Only sentence length, hedging of *rationale*, heading
style, and pronoun set flex between registers. Default to the base register unless the paper's job
clearly calls for another.
