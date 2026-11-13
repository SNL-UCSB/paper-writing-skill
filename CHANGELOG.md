# Changelog

All notable changes to the paper-writing skill.

## [v3.3.0] 2026-11-13: Story document template

### Added
- **`story_document.md`**: the template for the one-page story document that co-authors read, built in story mode steps 7 to 9. Front matter (key terms, the argument as a figure and numbered steps, a roadblock table, the novelty test), then the paper's sections in order. These are: a Kurose-question introduction; background built around the core abstraction; a problem specification table (unit, input, output, ground truth, how records are generated, each record's blind spot, rules for learning, assumptions); empirical motivation with anecdotes first and a table of trends second; design with the alternatives mapped; and an evaluation plan with numbered questions, a setup inventory, a per-question table of data available against data needed, and a stop rule. Includes readability rules (a figure for every main point, plain words over internal names, no decode-me labels, sources and scope on every number, separating overlapping events, naming the start and end of every lead time), a review checklist and failure modes. All examples are synthetic.

### Changed
- `story_mode.md` lists the story document among the outputs and points to the template in steps 7 and 8. Three failure modes were added: a cryptic page with no arc, a problem statement nobody can check, and an evaluation plan without setup.
- `setup`, `README.md` and `USAGE.md` include the new file.

## [v3.2.0] — 2026-11-04 — Story mode, usage guide, synthetic examples

### Added
- **Story mode** (`story_mode.md`): ten steps (plus a short path for workshop papers and short deadlines) from an existing record (meeting notes, drafts, code, reviews, related work) to a paper story, a Draft 0 introduction and outline, a calibration spec, and a filled `project_context.md` that hands off to Stage 2. Each step has a request template, a done-when test and the usual failure with its corrective request; includes intro structures by venue and a failure-mode table.
- **`USAGE.md`**: a student-facing guide covering setup, the first session, a situation-to-mode table, outputs, iteration, other skills, customizing, privacy and troubleshooting.
- **Private overlays**: the skill reads a private overlay skill when one is installed, so a group can keep real cases and project records out of this public repository.

### Changed
- Every example is now synthetic. The filled-in project context is a fictional system (`examples/example_project_context.md`); example sentences, headings, captions and numbers across the rules and guides were rewritten to teach the same lessons without referring to real projects.
- The skill description names the defaults as the SNL lab's house style (customizable) and no longer lists specific papers as triggers.
- `setup` installs `story_mode.md`.
- Story mode tightened after a blind test in which simulated students fell short on story arc, ties to the earlier paper, design derivation, running example and density, and over-ran review rounds: ask for missing authoritative sources before step 2; list the fate of every agreed element, with a precedence rule for the advisor's latest statement (step 2); an exemplar table and a simpler-method check (step 5); seed the identity paragraph from the advisor's latest restatement (step 6); a required ledger of earlier terms, a contribution-to-example table, a Figure 1 sketch and a claims-by-evidence grid (step 7); a required requirements derivation, and stated aims stay argued (step 8); a three-round cap for story documents and a story document gate profile (step 9); the Kurose questions named for systems venues.
- Checklists: `design_questions.md` asks to derive the artifact from its requirements; `evaluation_questions.md` asks for a pinned-agent baseline when an AI agent could do the task.

### Fixed
- `gate_mechanical.md` M2 grep: the "not" branch now uses `([[:alnum:]]|\\)` in single quotes, so it runs on both GNU and BSD (macOS) grep.

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
