# paper-writing-skill

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that encodes a research-paper
writing methodology: a five-stage pipeline, section-by-section rhetorical moves, a style audit that
runs on every edit, an independent red-team, and a figure-synthesis workflow for non-data figures.
The rules are empirical — distilled from forensic analysis of real paper revisions (7,600+ Overleaf
edits, 100+ tex versions, 5 peer-review cycles across systems and ML venues). See
[DESIGN.md](DESIGN.md) for the rationale and evidence behind each rule.

It offloads the scaffolding — section structure, cross-section coherence, page-limit compression,
voice consistency, figure generation — so the writer keeps the hard parts: what the paper claims,
whether the evidence supports it, and how to position the work.

**Who it's for:** PhD students and researchers writing conference or journal papers. Tuned for
systems/networking (SIGCOMM, NSDI, CoNEXT, IMC) and ML (NeurIPS, ICLR, ICML); adaptable to any
field (see [Customize](#customize-for-your-field)).

## Quick Start

```bash
git clone https://github.com/SNL-UCSB/paper-writing-skill.git
cd paper-writing-skill
./setup            # installs to ~/.claude/skills/paper-writing/
```

Then, in your paper's directory: run `claude`, and type `/paper-writing`. If a `project_context.md`
exists, Claude loads it and continues; if not, it runs a structured brainstorming session (34
questions across 6 phases) to create one.

Requires Claude Code installed and signed in (https://docs.claude.com).

## How It Works

Three layers: a **fixed pipeline**, **customizable rules** (defaults provided), and a per-paper
**project context**.

**The pipeline** (five stages):

1. **Brainstorming** — 34 questions across 6 phases produce a `project_context.md` (the binding
   contract for every later session).
2. **Architecture** — section outline, claim assignments, figure/table plan, page budget.
3. **Section drafts** — enforced order: Draft 0 Introduction → Evaluation → Design → Background →
   Related Work → Final Introduction → Abstract. The introduction is written **twice**; topic
   sentences are written and checked for a coherent argument before full prose.
4. **Integration** — cross-section consistency (terminology, claims, headings, flow).
5. **Compression** — 7 operations, 30–50% target reduction.

**The style audit.** Every tex edit passes a mandatory gate. The style layer is three files, split
by mechanism:

| File | Role |
|---|---|
| `author_profile/gate_mechanical.md` | The single **grep gate** (M1–M18): em-dashes, antithesis, filler/pompous/fancy words, passive voice (zero tolerance), wordiness, term-drift. Runs on every edit. |
| `author_profile/gate_semantic.md` | The single **reader-judgment gate** (S1–S31): define-before-use, followability, coherence, consistency, rigor/grounding, honest positioning, figures, closure. Runs in the red-team/loop pass. |
| `author_profile/craft_reference.md` | **How to write** (the positive layer): sentence-level style and three registers (base/terse, conceptual/position, warm/narrative). |

**The red-team.** After the author's audit, an *independent* reviewer that did not write the text
re-runs the greps and applies the semantic gate with a fresh-reader lens, returning findings — not a
yes/no. Run it iteratively via `/loop` (`loop_mode.md`), which keeps a disk-backed ledger and stops
itself when every section is clean. See `red_team_protocol.md`.

**Figure synthesis** (`figure_synthesis_guide.md`). A three-mode workflow (spec → generate →
critique) for non-data figures — architecture diagrams, pipeline flows, concept illustrations — with
venue-specific styling. Data figures (plots) go through the separate `/viz` skill.

### Common workflows

- "Help me write the evaluation" — drafts the 6-move sequence, then runs the checklist.
- "Review this introduction" — applies the intervention types, returns numbered feedback.
- "Compress section 4" — applies the 7 compression operations, reports before/after counts.
- "Audit with the paper-writing skill until clean" — runs the `/loop` red-team cycle.
- "I need an architecture diagram" — walks the figure spec → generate → critique.

## What's in This Repo

```
paper-writing-skill/
├── SKILL.md                       # Main skill file (Claude reads this)
├── brainstorming_guide.md         # 34 questions across 6 phases
├── figure_synthesis_guide.md      # Non-data figure workflow
├── red_team_protocol.md           # Independent, evidence-gated review
├── loop_mode.md                   # Resumable /loop audit-and-fix
├── setup                          # One-command installer
├── DESIGN.md                      # Rationale + evidence behind the rules
├── author_profile/                # Editorial rules (defaults, customizable)
│   ├── editorial_principles.md    #   14 principles with evidence
│   ├── craft_reference.md         #   how to write (three registers)
│   ├── gate_mechanical.md         #   the grep gate (M1–M18)
│   ├── gate_semantic.md           #   the reader-judgment gate (S1–S31)
│   ├── compression_patterns.md    #   7 compression operations
│   ├── rhetorical_moves.md        #   cross-section move sequences
│   └── intervention_types.md      #   7 advisor intervention types
├── writing_checklists/            # Post-draft structural checklists
├── section_rhetorical_moves/      # Per-section move sequences + examples
├── figure_templates/              # Venue styles, prompts, TikZ skeletons
└── examples/                      # project_context template + a real example
```

The rules, checklists, and move guides also work without Claude Code — print them for a lab
workshop, or use the checklists for self-review before sending a draft to an advisor.

## Customize for Your Field

Defaults ship for systems/networking and ML venues. Edit files in `author_profile/`:

- **`craft_reference.md`** — sentence-length targets, tone, and the three registers.
- **`gate_mechanical.md` / `gate_semantic.md`** — add or relax specific checks (keep the term-drift
  list current per paper).
- **`editorial_principles.md`** — add or reprioritize principles (keep introduction-twice and
  named-over-vague; they are venue-agnostic).
- **`compression_patterns.md`**, **`section_rhetorical_moves/`**, **`figure_templates/`** — tune
  compression intensity, add move examples from your field, adjust figure styling per venue.

**For advisors:** fork the repo, encode your standards in `author_profile/`, and have students clone
your fork — they get your group's conventions immediately. Students can further customize; the
pipeline stays consistent.

## Troubleshooting

- **`/paper-writing` not recognized** — re-run `./setup`; check `ls ~/.claude/skills/paper-writing/SKILL.md`.
- **Rules not being followed** — invoke `/paper-writing` explicitly at the start; the skill only
  activates when invoked.
- **Start a paper's context over** — delete `project_context.md` and invoke `/paper-writing` again.
- **Updated the repo but Claude uses old rules** — run `./setup` again after `git pull`.

Always edit files in this repo, not in `~/.claude/skills/`; `./setup` copies them into place.

## License

MIT. If you use this in your research workflow, cite:

```
@misc{paper-writing-skill,
  author = {Gupta, Arpit},
  title  = {paper-writing-skill: A Claude Code skill for research paper writing},
  year   = {2026},
  publisher = {GitHub},
  url    = {https://github.com/SNL-UCSB/paper-writing-skill}
}
```
