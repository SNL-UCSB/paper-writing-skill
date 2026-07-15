# Independent Adversarial Red-Team Protocol

> Why this exists: the de-AI mechanical gate and the style audit are run by the SAME agent that wrote the
> prose, which then self-reports "passed." Self-audit rationalizes its own phrasing, and the mechanical grep
> is skipped or its output never shown. Result: AI-sounding, inaccessible, and illogical text ships anyway.
> This protocol makes review INDEPENDENT and EVIDENCE-GATED. Added 2026-07 from a live-review audit
> (see the paper's `notes/PAPER_WRITING_SKILL_GAPS.md`).

## When
On every section draft or non-trivial rewrite: AFTER the author runs the de-AI mechanical gate + style
audit, and BEFORE the text is shown to the user or committed.

## Three gates — text must survive all three
1. **WRITE** — the author agent produces the draft.
2. **MECHANICAL GATE** — run ALL of `de_ai_checklist.md` Part D (expanded). PASTE the raw output (hit counts
   + lines). Zero unjustified hits to proceed. "Audited" without pasted output is INVALID.
3. **INDEPENDENT RED-TEAM** — a SEPARATE reviewer that did NOT write the text. It:
   - re-runs the Part D greps itself (does not trust the author's report);
   - applies `author_profile/accessibility_checklist.md` (G1–G6) with a FRESH-READER lens: "a venue reader
     who has never seen this paper — flag every undefined term, every claim not tied to the thesis, every
     illogical jump";
   - applies the `[JUDGE]` and `[MECH]` items of `author_profile/elements_of_style_checklist.md` (the
     Strunk & White craft layer): passive voice (zero tolerance), needless words, weak/positive form,
     parallelism, emphatic-end, pompous/vague usage, and White's 21 reminders;
   - hunts the four failure classes explicitly: AI-sounding, inaccessible, incoherent, illogical;
   - REFUSES to pass until clean, returning a findings list (category, line, concrete fix) — not a yes/no.

## Evidence rule
The grep output + the red-team findings ARE the audit record. A section is "clean" only when the red-team
returns zero surviving findings WITH the pasted grep evidence. Never report clean on a mental pass.

## How to run the independent reviewer
Spawn a subagent (or a clearly separated reviewer persona) whose ONLY input is the drafted text + these
checklists + the paper's controlling idea. It must NOT see the author's self-assessment. In a multi-agent
workflow this is a distinct verify stage; interactively it is a fresh Agent that reviews and returns findings.
The author then fixes every finding and the cycle repeats until the reviewer returns clean.
