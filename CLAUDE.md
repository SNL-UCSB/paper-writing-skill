# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A Claude Code **skill** (invoked via `/paper-writing`) that encodes the writing methodology of the [Systems and Networking Lab (SNL)](https://github.com/SNL-UCSB) at UC Santa Barbara. Part of a [three-skill family](https://github.com/SNL-UCSB) (literature-survey, data-visualization, paper-writing) that compresses the operational middle of the research pipeline. It is not a traditional codebase — there is no build system, no tests, and no application code. The repo contains markdown files that Claude reads at runtime to enforce editorial rules, rhetorical move sequences, and a structured writing pipeline.

## Installation

```bash
./setup
```

This copies skill files into `~/.claude/skills/paper-writing/`. Always edit files in this repo, then re-run `./setup` to propagate changes.

## Architecture

**SKILL.md** is the main entry point Claude reads when `/paper-writing` is invoked. It defines:
- The three-layer model: fixed pipeline → customizable voice/editorial rules → per-paper project context
- The five-stage writing pipeline: Brainstorming → Architecture → Section Drafts → Integration → Compression
- The mandatory style audit gate (runs on every tex edit)
- Section checklist and rhetorical move dispatch tables

**author_profile/** contains the editorial rules (source of truth for all style enforcement):
- `editorial_principles.md` — 14 cross-paper principles with evidence from forensic revision analysis
- `voice_profile.md` — sentence-level style rules (~21-word mean, zero hedging, claim-first, banned words)
- `compression_patterns.md` — 7 compression operations with before/after examples
- `rhetorical_moves.md` — cross-section move sequences
- `intervention_types.md` — 7 advisor intervention types for draft review

**writing_checklists/** contains post-draft structural checklists (intro, evaluation, design, related work). These run automatically after every section draft.

**section_rhetorical_moves/** contains detailed per-section move sequences with concrete examples from accepted/rejected papers.

**brainstorming_guide.md** defines the 34-question, 6-phase brainstorming workflow that produces a `project_context.md`.

**examples/** contains the `project_context.md` template and a real completed example (NetBurst).

## Key Design Decisions

- **Introduction-twice** is the most impactful principle. The introduction is written twice: Draft 0 (framing scaffold) before the evaluation, then the final introduction rewritten from scratch after the evaluation. Section drafts follow: Draft 0 Introduction → Evaluation → Design → Background → Related Work → Final Introduction → Abstract.
- **`project_context.md`** is generated per-paper in the user's working directory (not in this repo). It should be gitignored.
- The `setup` script copies a specific subset of files (SKILL.md, brainstorming_guide.md, author_profile/, writing_checklists/, section_rhetorical_moves/, examples/) — it intentionally excludes README.md and setup itself.
- Voice rules ship as defaults from the SNL lab. Students customize by editing `author_profile/` files after installation.
- **Signposting through claims**: section preambles are encouraged when they state the section's conclusion, but banned when content-free ("In this section, we describe...").
- **Topic-sentence-first drafting**: before writing full prose for any section, write all topic sentences and verify they form a coherent argument. This intermediate scaffolding step sits between Architecture (section outline) and full drafting.
