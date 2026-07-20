# Changelog

All notable changes to the paper-writing skill.

## [v3.0.1] — 2026-07-20 — Fix-level provenance in the audit ledger

- The independent red-team's findings and the `/loop` audit ledger now **require citing the exact
  rule ID** (`M#` from `gate_mechanical.md`, `S#` from `gate_semantic.md`) for every fix — e.g.
  `§3.2: fixed 2× M11 passive, 1× M12 wordiness`. This restores full fix-level traceability (which
  rule each iteration addressed) that had been implicit after the v3.0.0 consolidation. Provenance
  keys to the self-contained gate IDs; no external-source citation is reintroduced.

## [v3.0.0] — 2026-07-20 — Style-enforcement pipeline + three-gate architecture

The skill moves from a style *reference* to an evidence-gated *enforcement pipeline*, and the style
layer is consolidated from seven overlapping files into three clean gates.

### Added
- **Mandatory style audit** on every tex edit — not user-triggered, applies to every paragraph-level change.
- **Independent, evidence-gated red-team** (`red_team_protocol.md`): a reviewer that did *not* write
  the text re-runs the greps and applies the semantic gate with a fresh-reader lens, returning
  findings rather than self-certifying "audited."
- **`/loop` mode** (`loop_mode.md`): a resumable, disk-backed audit → red-team → fix cycle that
  processes one section per iteration and stops itself when every section is clean.
- **Three-gate style architecture:**
  - `author_profile/gate_mechanical.md` — the single greppable gate (M1–M18): em-dashes, antithesis,
    filler/pompous/fancy words, passive voice, wordiness, term-drift — one grep script.
  - `author_profile/gate_semantic.md` — the single reader-judgment gate (S1–S31): define-before-use,
    followability, coherence, consistency, rigor/grounding, honest positioning, figures, closure gate.
  - `author_profile/craft_reference.md` — the positive "how to write" layer, in three registers
    (base/terse, conceptual/position, warm/narrative).
- **Zero-tolerance active voice**, enforced in the base gate — no exceptions, including methods and
  evaluation.
- **`DESIGN.md`** — the rationale and evidence behind each rule.

### Changed
- **Consolidated 7 style files into 3** (1,395 → 578 lines, 59% smaller) and resolved an internal
  contradiction where one file relaxed rules another hard-banned.
- **De-named throughout** — registers are described by what they do, not modeled on any named
  researcher's voice; sources are credited factually in `DESIGN.md`, not framed as authorities to
  imitate.
- **Lean README** (332 → ~150 lines); the rationale essay moved to `DESIGN.md`.
- Capability preservation for the consolidation was verified by an independent code-model red-team
  (two passes, closure clean).

### Removed
- **BREAKING:** `author_profile/de_ai_checklist.md`, `accessibility_checklist.md`,
  `hardening_checklist.md`, `elements_of_style_checklist.md`, `voice_profile.md`,
  `shenker_voice_profile.md`, `kurose_voice_profile.md` are removed, replaced by
  `gate_mechanical.md`, `gate_semantic.md`, and `craft_reference.md`. Forks that reference the old
  filenames must update their pointers.

## [v2.0] — Figure synthesis

- Three-mode figure-synthesis workflow (spec → generate → critique) for non-data figures.
- Seven figure archetypes; hybrid AI-image-prompt / TikZ generation; venue-specific styling.
- Active-voice rule tightened (no passive for methodology).

## [v1.0.0] — Writing pipeline

- Five-stage pipeline, 14 editorial principles, brainstorming workflow, rhetorical move sequences,
  section checklists, compression patterns.
