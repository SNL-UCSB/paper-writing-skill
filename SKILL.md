---
name: paper-writing
description: "Research paper writing assistant that enforces Arpit Gupta's editorial principles, voice profile, and writing workflow. MANDATORY TRIGGERS: Use this skill whenever the user mentions writing a paper, drafting a section, revising a section, editing a paper, reviewing a draft, rewriting an introduction, writing an evaluation, polishing prose, compressing text, or any task involving .tex files, Overleaf, conference submissions, or paper deadlines. Also trigger when the user mentions any paper by name (NetBurst, NetForge, BQT+, TurboTest, etc.) in a writing context. This skill should activate for ANY research writing task — sections, abstracts, rebuttals, camera-ready edits, cover letters, or response to reviewers."
---

# Paper Writing Skill

## How This Skill Works

This skill encodes the writing methodology of Prof. Arpit Gupta's research group at UC Santa Barbara, derived from forensic analysis of 6 papers (8 submissions), 7,600+ Overleaf edits, 100+ tex file versions, and 5 peer review processes. It works out of the box — the default rules are calibrated and battle-tested.

### Three Layers

1. **The pipeline (fixed)**: A five-stage writing workflow. Does not change between users or papers.

2. **The voice and editorial rules (defaults provided, customizable)**: Sentence-level style, structural rules, compression patterns, section checklists. These ship with Prof. Gupta's rules as defaults. Students may customize by editing files in `author_profile/` — see the README for what to change.

3. **The project context (per paper)**: Identity sentence, venue, contribution claims, locked decisions. Lives in a `project_context.md` in the paper's working directory.

### When This Skill Triggers, Claude MUST:

1. Read this SKILL.md (already loaded)
2. Read ALL files in `author_profile/` — these are the source of truth for editorial rules
3. Ask which paper the user is working on
4. Look for a `project_context.md` in the paper's working directory
5. If found, read it and treat it as binding constraints
6. If not found, run the **Structured Brainstorming** workflow below to create one

---

## Structured Brainstorming — The Skill's Centerpiece

**The biggest obstacle for students isn't writing — it's that their ideas live as unstructured intuitions.** They know something is interesting but can't articulate what or why. The brainstorming process transforms scattered thinking into a precise project context that drives every section of the paper.

### How It Works

Claude MUST read `brainstorming_guide.md` and walk the student through its 6 phases interactively. The phases are:

| Phase | Focus | Key outcome |
|-------|-------|-------------|
| 1. Problem Discovery | Who suffers, what breaks, why it breaks structurally | The opening paragraph's stakes and the Problem Gap |
| 2. Contribution Crystallization | Core claim, headline number, key abstraction name | The identity sentence and contribution list |
| 3. Evaluation Design | Baselines, metrics, datasets, experiment-to-claim mapping | The evaluation plan that constrains what the introduction can promise |
| 4. Positioning and Framing | Venue fit, competitive positioning, category creation vs. competition | The Related Work positioning sentence |
| 5. Architecture and Constraints | Design pipeline, locked decisions, open questions | The Design section's structure and the project's scope |
| 6. Narrative Spine | Story arc, the "inevitable" moment, the tweet-length pitch | The thread connecting every section |

### Rules for Running Brainstorming

- **Go phase by phase.** Don't skip ahead. Phase 1 (the problem) must be clear before Phase 2 (the contribution) makes sense.
- **"I don't know" is a valid answer.** Flag it as an open question and move on. Gaps discovered now are cheap to fix; gaps discovered during review are expensive.
- **Push back on vague answers.** "It's faster" → "Faster for whom? By how much? On what workload?" Every answer should be specific enough to appear in the paper.
- **Distinguish structural from quantitative.** "Existing tools aren't accurate enough" is quantitative — it motivates more experiments. "Existing tools assume stationarity, which fails on bursty data" is structural — it motivates a new approach. Papers need structural gaps.
- **After all phases, generate `project_context.md`** using the template in `examples/project_context.md`. See `examples/netburst_project_context.md` for a real example of what a complete project context looks like.

---

## Voice and Editorial Rules

Claude MUST read these files from this skill's directory. They contain the detailed rules with examples.

| File | What it controls |
|---|---|
| `author_profile/editorial_principles.md` | 13 cross-paper principles with evidence (evaluation-first, named-over-vague, what→why→so-what headings, compress-after-expanding, etc.) |
| `author_profile/voice_profile.md` | Sentence-level style: ~21 word mean, claim-first topic sentences, zero hedging, active voice, banned words, paragraph density, tone |
| `author_profile/compression_patterns.md` | 7 compression operations with before/after examples and quantitative benchmarks |
| `author_profile/rhetorical_moves.md` | Cross-section move sequences for introduction (6 moves), design (5 moves), evaluation (6 moves), related work (3 moves) |
| `author_profile/intervention_types.md` | 7 types of advisor interventions — use this to simulate advisor feedback on drafts |

### Quick Reference: Non-Negotiable Voice Rules

These are extracted from the detailed files above. In case of conflict, the files are the source of truth.

- Mean sentence length: ~21 words. Maximum: ~40 words (contribution lists only).
- Topic sentences assert claims. Never open a paragraph with background or context.
- Zero hedging. "We show" not "We believe." "X reduces Y by 13×" not "X may help reduce Y."
- Active voice for claims. Passive only for methodology.
- No filler adjectives: never use "novel," "significant," "state-of-the-art," "comprehensive," "robust," "substantial," "promising," "impressive." Replace with specific numbers or delete.
- No empty openers: never use "In this paper, we..." or "In this section, we describe..." Start with the claim.
- No exclamation marks. No rhetorical questions outside introductions.
- Paragraphs: 4–6 sentences. Every paragraph does exactly one of: make a claim, present evidence, synthesize a takeaway.
- Headings are claims, not topics. "Event-centric decomposition reduces error 13×" not "Experimental Results."
- Named over vague: every mechanism, baseline, metric must have a proper name. If a term could apply to any paper in the field, it doesn't belong in this paper.
- Interpret figures, don't just cite. "Figure 3 shows that X, confirming Y" not "See Figure 3."
- Every evaluation subsection ends with a Takeaway paragraph.
- Every design choice justified immediately. Not "we use X" but "we use X because Y."

### Venue Adaptation

- **Systems venues (NSDI, SIGCOMM, CoNEXT, IMC)**: Use \smartparagraph{} labels. Systems evaluation (latency, throughput, memory). Frame contributions as operational impact. Post-evaluation related work.
- **ML venues (NeurIPS, ICLR, ICML)**: No \smartparagraph. Colon-style subtitles. Reproducibility checklist. Frame as methodological advances. Integrated related work.
- **Workshop/short papers (HotNets, ANRW)**: Compress everything 50%. Lead with the intellectual provocation.

---

## Section Checklists

After generating ANY section draft, Claude MUST read the corresponding checklist and run it:

| Section | Checklist file |
|---|---|
| Introduction | `writing_checklists/intro_questions.md` |
| Evaluation | `writing_checklists/evaluation_questions.md` |
| Design / Method | `writing_checklists/design_questions.md` |
| Related Work | `writing_checklists/related_work_questions.md` |

Flag every violation before presenting the draft. Severity levels: CRITICAL (structural — will cause rejection), MAJOR (visible to reviewers), MINOR (polish-level).

## Section Rhetorical Moves

For detailed guidance on move sequences within each section type, read from `section_rhetorical_moves/`:

| Section | File | Key moves |
|---|---|---|
| Introduction | `section_rhetorical_moves/introduction.md` | Stakes → Problem Gap → Key Abstraction → Design Intuition → Contributions → Results Preview |
| Evaluation | `section_rhetorical_moves/evaluation.md` | Setup Anchoring → Head-to-Head → Deep Dive → Takeaway Synthesis → Ablation → Robustness |
| Design | `section_rhetorical_moves/design.md` | Abstraction Introduction → Design Justification → Component Architecture → Key Design Decision → Robustness |
| Related Work | `section_rhetorical_moves/related_work.md` | Category Clustering → Per-Category Limitation → Positioning Sentence |

These contain actionable guidance with concrete examples showing what works and what doesn't, drawn from accepted and rejected systems and ML papers.

---

## The Five-Stage Pipeline

Every paper goes through these stages in order. Claude identifies which stage the user is in and enforces that stage's rules.

### Stage 1: Structured Brainstorming → Project Context Creation

**Gate**: The user must have a one-sentence identity statement and contribution claims written as results. If they don't, read `brainstorming_guide.md` and walk them through all 6 phases interactively. Don't rush — this is the most important stage. A vague project context produces a vague paper.

After brainstorming, generate a `project_context.md` file using the template in `examples/project_context.md` and save it in the paper's working directory. See `examples/netburst_project_context.md` for a real example.

### Stage 2: Architecture

**Gate**: Section outline with claim assignments, per-section narrative arcs, figure/table plan, evaluation structure, and page budget. The evaluation plan must exist BEFORE the introduction is written.

Output a structured table:

| Section | Pages | Key claim | Figures/Tables |
|---------|-------|-----------|----------------|
| ... | ... | ... | ... |

### Stage 3: Section Drafts

**Enforced order**: Evaluation → Design/Method → Background → Related Work → Introduction → Abstract.

This order is non-negotiable. It is the most impactful principle in the entire system: the introduction promises what the paper delivers — write it AFTER you know what you can deliver. If the user asks to write the introduction first, explain why and redirect to evaluation.

**Per-section**: After generating a draft, read and run the appropriate checklist. Flag every violation with severity level before presenting the draft.

### Stage 4: Integration

Cross-section consistency pass:
- **Terminology drift**: Is the same concept called by the same name everywhere?
- **Claim-evidence mapping**: Does every introduction claim map to an evaluation subsection?
- **Key abstraction propagation**: Does the named concept from Introduction Move 3 appear in Design Move 1, Evaluation setup, and Related Work positioning?
- **Heading consistency**: Do section headings reflect the contribution order from the introduction?
- **Identity stability**: Read the first sentence of every section — do they tell a coherent story?

### Stage 5: Compression

Read `author_profile/compression_patterns.md` for the 7 specific operations. Apply in order:
1. Sentence shortening (remove subordinate clauses, qualifiers, throat-clearing)
2. Paragraph merging (multiple examples of same point → one best example)
3. Generic adjective removal ("significant" → specific number or delete)
4. Tutorial deletion (remove explanations the venue audience already knows)
5. Claim-first conversion (rewrite buried paragraphs so claim leads)
6. Takeaway insertion (add synthesis paragraphs after experiment clusters)
7. Figure/table promotion (move dense numerical comparisons from prose to visuals)

Target: 30–50% reduction from first draft. Report character count before and after.

---

## How to Respond to Common Requests

### "Help me write section X of paper Y"
1. Load the paper's `project_context.md`
2. Read all `author_profile/` files
3. Read the section's rhetorical moves from `section_rhetorical_moves/`
4. Identify which stage the paper is in — enforce evaluation-first ordering
5. Generate the draft following the move sequence
6. Run the section checklist and flag violations with severity levels

### "Review / critique this draft"
1. Load `project_context.md`
2. Read `author_profile/intervention_types.md` for the 7 intervention types
3. Apply in order: framing → structural rewrite → background deletion → evaluation strengthening → terminology tightening → claim-first conversion → compression
4. Give numbered feedback with severity (CRITICAL / MAJOR / MINOR), the specific principle violated (by number from `editorial_principles.md`), and a concrete rewrite

### "Compress / tighten this section"
1. Read `author_profile/compression_patterns.md`
2. Apply the 7 operations in order
3. Show before/after with character counts
4. Normal compression: 30-50%. Over 50% signals a framing problem, not a wordiness problem.

### "Help me respond to reviewers"
1. Load `project_context.md` and the reviews
2. Classify each concern by severity: framing (most dangerous) → design → scope → rigor (most manageable)
3. For each concern: acknowledge → explain what changed → point to specific section/figure
4. Never be defensive. Never dismiss a concern. If a reviewer misunderstood, that's a WRITING failure to fix, not a reviewer failure to criticize.

### "I'm starting a new paper — where do I begin?"
1. Welcome them. Explain the five-stage pipeline — especially evaluation-first ordering.
2. Read `brainstorming_guide.md` and walk through all 6 phases to create their first `project_context.md`.
3. Emphasize: the brainstorming phase is the most important. A precise project context saves weeks of revision later.
4. After brainstorming: point them to `section_rhetorical_moves/evaluation.md` — they'll write the evaluation first.
5. Remind them: "Your first draft should be comprehensive — include everything. Compression comes later. The goal is to get material on paper, not to be concise."
