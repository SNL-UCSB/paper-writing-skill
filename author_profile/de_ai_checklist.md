# De-AI Checklist and Shenker Voice Profile

## Why this file exists

The base `voice_profile.md` bans hedging, passive voice, and a few adjectives, but it is
SILENT on the failure modes that most make prose read as machine-written: em-dashes,
antithesis flourishes, editorializing closers, and rule-of-three decoration. This file closes
that gap. **It is a hard gate: run it on every `.tex` edit before claiming the edit is done,
and run the mechanical greps at the bottom — do not rely on a mental pass.** Over-claiming "the
style audit passed" after checking only the base 7 items is itself the bug this file fixes.

The target register is **Scott Shenker's modern-HotNets voice**: short, plain, declarative,
concrete, unpretentious. Say the thing. Then stop.

---

## Part A — Banned constructions (the De-AI checklist)

### A1. Em-dashes — BANNED, no exceptions
No `---` (em), no ` -- ` (en used as a dash), no `—`/`–` (unicode). They are the single
strongest machine-writing tell. Replace with a comma, a colon, parentheses, or a full stop.
- ✗ `One process on a laptop --- the Core --- parses the intent.`
- ✓ `One process on a laptop, the Core, parses the intent.`
- ✗ `\sys realizes it --- and nothing more.`
- ✓ `\sys realizes it.`
Parenthetical aside → parentheses; appositive → commas; dramatic reveal → new sentence.

### A2. Antithesis / mirror flourishes — BANNED as decoration
`X, not Y` · `not X but Y` · `X rather than Y` · `less X than Y` when used for rhythm.
- ✗ `a wide top, measured, not asserted.`  → ✓ `a wide top, measured on real studies.`
- ✗ `whatever it is called.`  → ✓ (delete)
- ✗ `Engineering, not research.` (as a heading)  → ✓ `Engineering obligations.`
**Exception:** keep the contrast ONLY when the two sides carry real, non-obvious content the
argument needs (e.g., "a limit of the design, not only the implementation" — the design/impl
distinction is the paper's thesis). Even then, prefer stating the positive claim first and
letting the contrast fall out. When in doubt, cut the negated half.

### A3. Editorializing closers / "the point is" — BANNED
Sentences whose job is to tell the reader how to feel about the previous sentence.
- ✗ `The saving is the point.` · `because a system that ignores them is not real.` ·
  `This is the empirical content of the claim.` · `That is the whole idea.`
- ✓ State the fact; trust it. `This closes the gap between an idea and the data behind it.`

### A4. Vacuous intensifiers and hype — BANNED
`in effect`, `in a sense`, `at its core`, `truly`, `simply`, `essentially`, `fundamentally`
(as filler), `exactly the kind of`, `means nothing until`, `armed with`, `powerful`,
`seamless(ly)`, `crucial`, plus the base list (`novel`, `significant`, `substantial`,
`comprehensive`, `robust`, `state-of-the-art`, `paradigm`, `leverage`, `utilize`).
- ✗ `\sys is in effect the next-generation Mininet.` → ✓ `\sys is the next-generation Mininet.`

### A5. Rule-of-three / triad decoration — USE SPARINGLY
Three-part parallel lists (`reliable, consistent, and verifiable`; `deploy, probe, and run`)
read as generated when the members are near-synonyms or padding. Keep a triad ONLY when all
three items are distinct and load-bearing; otherwise cut to the one that matters.

### A6. Throat-clearing sentence openers — BANNED
`Moreover`, `Furthermore`, `Additionally`, `Notably`, `Importantly`, `Indeed`, `Ultimately`,
`It is worth noting that`, `It is important to note`, `Crucially`. Start with the claim.

### A7. Rhetorical questions — introductions only, at most one
Elsewhere, convert to a declarative claim.

### A8. Exclamation marks — BANNED (inherited from base profile).

### A9. Grandiose empty setup sentences — BANNED
Sentences that announce importance and defer the payoff: `Two commitments no predecessor made set this
grammar apart.`, `What sets X apart is`, `The key insight is`. State the commitment or insight directly.
- ✗ `Two commitments no predecessor made set this grammar apart.`
- ✓ `The grammar specifies the bottleneck precisely and shares it across workflows; each route is approximate.`

### A10. Branded / evocative metaphor filler — BANNED
Impressive-sounding metaphors with no operational content: `the machine that runs any admissible spec`,
`draw their power from backends they do not build`, `at the heart of`, `under the hood`. Use the mechanism.
- ✗ `The architecture: the machine that runs any admissible spec.`
- ✓ `The architecture: how the abstraction becomes a running system.`

### A11. Vague-mechanism verbs — AUDIT (not auto-ban)
`keeps X from Y`, `stands in the way of`, `unlocks`, `powers`. They often hide an unstated mechanism.
- ✗ `intent bloat that keeps intents from transferring`
- ✓ `intent bloat that widens the ideation-to-data-generation gap`

### A12. Fancy, showy, or figurative verbs — prefer the plain word (AUDIT)
When a plain, common verb exists, a colorful or literary one reads as authorial performance
(↔ Elements of Style R14 "avoid fancy words", R21 "prefer the standard to the offbeat"). Replace
the fancy verb with the ordinary one. Keep the fancy verb ONLY when it is precise domain jargon
with no plain equivalent (e.g., `amortize` a cost, `traverse` a graph, `spawn` a process). This
is distinct from A4 (which bans hype adjectives) and Part B.2 (Latinate → Anglo-Saxon); A12
targets verbs chosen for flavor.
Common offenders → plain swap:
- `pit (X against Y)` → `run X against Y` · `dispatch` → `send` · `chip (away) at` → `narrow` /
  `reduce` · `bind` (as "constrain harder") → `tighten` · `anew` / `afresh` → `again`
- `marshal` → `gather` · `orchestrate` → `coordinate` / `run` · `wrangle` → `manage` ·
  `harness` → `use` · `forge` → `build` · `weave` → `combine` · `usher in` → `start` / `bring`
- `delve into` → `examine` · `grapple with` → `address` · `surface` (v.) → `reveal` / `show` ·
  `unpack` (figurative) → `explain` · `leverage` → `use` (also A4)
- ✗ `no single tool can pit a real Meet call against a bulk download`
- ✓ `no single tool can run a real Meet call against a bulk download`
Polysemous words (`bind`, `surface`, `run`, `chip`) are JUDGE-only — read for the flavor sense;
do not blanket-replace, since the plain sense is usually fine.

---

## Part B — The Shenker register (what to write INSTEAD)

1. **Short, declarative sentences.** One idea each. If a sentence has two clauses joined by a
   dash or semicolon plus a "so/and", it is probably two sentences.
2. **Plain words over Latinate.** `use` not `utilize`; `enough` not `sufficient`; `let` not
   `enable` where it fits; `shows` not `demonstrates`.
3. **Concrete over abstract.** Name the mechanism, the number, the system. Avoid abstract
   subjects ("the existence of X enables...") — put an agent in the subject slot.
4. **Claim-first topic sentences.** The first sentence of a paragraph states its conclusion.
5. **Verbs over nominalizations.** `we compile the intent` not `compilation of the intent is
   performed`.
6. **No pomposity.** The tone is a smart colleague explaining at a whiteboard, not a press
   release. Understate; let the result carry the weight.
7. **Cut the last sentence.** AI prose (and eager student prose) adds a summarizing/uplifting
   closer to most paragraphs. Delete it unless it states a new fact.

---

## Part C — Project-configurable conventions (confirm per project_context)

- **One-line titles.** Section and subsection titles fit on a single line in the target
  column width. Prefer 3–6 words. (This is a per-project rule; check `project_context.md`.)
- **`\smartparagraph` labels** are short and plain: `Engineering obligations.` not
  `Engineering, not research.`

---

## Part D — Mechanical gate (RUN THESE; do not eyeball)

Before claiming any `.tex` edit is clean, run:

```bash
# 1. Em-dashes (must return nothing)
grep -rn -- '---' sections/*.tex ; grep -rn $'—\|–' sections/*.tex
# 2. Antithesis / editorializing flourishes (inspect each hit)
#    NOTE: the "not" branch matches a digit, letter, OR a \macro after "not" — so
#    "not 168 ..." and "not \NumImplGap ..." are caught (the old [a-z] missed both).
grep -rnoE ",? not [0-9A-Za-z\\]|not only .* but|rather than|is the point|whatever it is|in effect|in a sense|means nothing|at its core|exactly the kind" sections/*.tex
# 2b. Grandiose empty setups, evocative metaphors, vague mechanisms (A9-A11; inspect each hit)
grep -rnoE "set(s)? .* apart|no (predecessor|one) .* (made|posed)|the key (insight|idea) is|the machine that|draw(s)? .* power from|at (its|the) (heart|core)|under the hood|keeps .* from|stands? in the way" sections/*.tex
# 3. Throat-clearing openers
grep -rnE "^(Moreover|Furthermore|Additionally|Notably|Importantly|Indeed|Ultimately|Crucially)" sections/*.tex
# 4. Banned adjectives
grep -rnoE "\bnovel\b|\bsignificant\b|\bsubstantial\b|\bseamless|\bpowerful\b|\brobust\b|\bcomprehensive\b|\bcrucial\b" sections/*.tex
# 5. Fancy / figurative verbs (A12; inspect each — keep only precise domain jargon like "amortize")
grep -rnoiE "\b(pit(s|ted|ting)?|dispatch(es|ed|ing)?|chip(s|ped|ping)? (away )?at|marshal(s|led|ling)?|orchestrat(e|es|ed|ing)|wrangl(e|es|ed|ing)|harness(es|ed|ing)?|forge[sd]?|weav(e|es|ed|ing)|delv(e|es|ed|ing) into|usher(s|ed)? in|grappl(e|es|ed|ing) with|anew|afresh)\b" sections/*.tex
# 6. Sentence length (flag > 40 words), and section titles on one line
```
Every hit is either fixed or explicitly justified (Part A2 exception) before reporting done.
Report the grep counts in the audit summary, not just "audited."

---

## How to apply

This file is part of the **Mandatory Style Audit** (see `SKILL.md`). Run Parts A + D on every
edit; run Part B when drafting or rewriting a paragraph; check Part C once per section title.
The base `voice_profile.md` still governs sentence length, claim-first structure, and tone;
this file adds the de-AI and Shenker layers on top.
