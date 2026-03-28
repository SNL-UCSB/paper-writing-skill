---
name: paper-writing
description: "Research paper writing assistant that enforces Arpit Gupta's editorial principles, voice profile, and writing workflow. MANDATORY TRIGGERS: Use this skill whenever the user mentions writing a paper, drafting a section, revising a section, editing a paper, reviewing a draft, rewriting an introduction, writing an evaluation, polishing prose, compressing text, or any task involving .tex files, Overleaf, conference submissions, or paper deadlines. Also trigger when the user mentions any paper by name (NetBurst, NetForge, BQT+, TurboTest, etc.) in a writing context. This skill should activate for ANY research writing task — sections, abstracts, rebuttals, camera-ready edits, cover letters, or response to reviewers."
---

# Paper Writing Skill

## How This Skill Works

This skill encodes the writing methodology of the [Systems and Networking Lab (SNL)](https://github.com/SNL-UCSB) at UC Santa Barbara, derived from forensic analysis of 6 papers (8 submissions), 7,600+ Overleaf edits, 100+ tex file versions, and 5 peer review processes. See [*The Paper Behind the Paper*](https://sites.cs.ucsb.edu/~arpitgupta/blog/the-paper-behind-the-paper.html) for the full analysis. It works out of the box — the default rules are calibrated and battle-tested.

### Three Layers

1. **The pipeline (fixed)**: A five-stage writing workflow. Does not change between users or papers.

2. **The voice and editorial rules (defaults provided, customizable)**: Sentence-level style, structural rules, compression patterns, section checklists. These ship with the SNL lab's rules as defaults. Students may customize by editing files in `author_profile/` — see the README for what to change.

3. **The project context (per paper)**: Identity sentence, venue, contribution claims, locked decisions. Lives in a `project_context.md` in the paper's working directory.

### How This Skill Connects to the Research Pipeline

This skill does not operate in isolation. It is part of a three-skill family, and the artifacts from the other two skills are direct inputs to the writing process:

**From the [literature-survey-skill](https://github.com/SNL-UCSB/literature-survey-skill):**
- **Gap analysis** → feeds Brainstorming Phase 1 (Problem Discovery). The gaps the survey identified — missing quadrants, shared assumptions that break, unexplored combinations — are the structural limitations that motivate your paper.
- **Writing craft extractions** (Pass 3+) → feed the Architecture stage and section drafting. The introduction anatomy, evaluation architecture, and design craft you extracted from the best papers in your area are the models for your own paper's structure.
- **Competitive positioning** → feeds Brainstorming Phase 4. The invariant matrix and dependency graph from synthesis show exactly where your paper sits relative to existing work.

**From the [data-visualization-skill](https://github.com/SNL-UCSB/data-visualization-skill):**
- **Exploration** (`exploration_log.md`) → feeds Brainstorming Phase 3 (Evaluation Design). The exploration forced you to look at your data from multiple angles before forming hypotheses. The surprises you found — distributions you didn't expect, subgroups that behaved differently — shape what claims are defensible and where the real contribution lives.
- **Brainstorm** (`braindump.md`) → feeds the figure/table plan. Each braindump articulates what question a figure answers, what you expected to see, and what would surprise you. These are the hypotheses your evaluation must validate.
- **Plan + Execute** (`plot_context.md`) → feeds the Architecture stage's figure/table plan. Each plot_context records intent, variable mappings, plot type rationale, and design decisions — ready-made entries for the paper's figure plan.
- **Analyze** (WALTER narrations) → feeds Evaluation Move 4 (Takeaway Synthesis). The WALTER Result — "what is the takeaway? does it connect back to the hypothesis?" — is a first draft of the Takeaway paragraph for that experiment cluster.

The three skills create a closed loop: the literature survey reveals the gap and teaches you how accepted papers communicate; data visualization forces you to understand what your evidence actually shows and what hypotheses it validates; paper writing turns both into a publishable argument. **If the student has artifacts from the other skills, Claude MUST load them.**

### When This Skill Triggers, Claude MUST:

1. Read this SKILL.md (already loaded)
2. Read ALL files in `author_profile/` — these are the source of truth for editorial rules
3. Ask which paper the user is working on
4. Look for a `project_context.md` in the paper's working directory
5. If found, read it and treat it as binding constraints
6. If not found, run the **Structured Brainstorming** workflow below to create one
7. Check for artifacts from sibling skills — survey paper notes with craft extractions, `exploration_log.md`, `braindump.md`, `plot_context.md`, WALTER narrations. If found, load them as reference material for the relevant pipeline stages

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
| `author_profile/editorial_principles.md` | 14 cross-paper principles with evidence (introduction-twice, named-over-vague, what→why→so-what headings, compress-after-expanding, etc.) |
| `author_profile/voice_profile.md` | Sentence-level style: ~21 word mean, claim-first topic sentences, zero hedging, active voice, banned words, paragraph density, tone |
| `author_profile/compression_patterns.md` | 7 compression operations with before/after examples and quantitative benchmarks |
| `author_profile/rhetorical_moves.md` | Cross-section move sequences for introduction (6 moves), design (5 moves), evaluation (6 moves), related work (3 moves) |
| `author_profile/intervention_types.md` | 7 types of advisor interventions — use this to simulate advisor feedback on drafts |

### Quick Reference: Non-Negotiable Voice Rules

These are extracted from the detailed files above. In case of conflict, the files are the source of truth.

- Mean sentence length: ~21 words. Maximum: ~40 words (contribution lists only).
- Topic sentences assert claims. Never open a paragraph with background or context.
- Zero hedging. "We show" not "We believe." "X reduces Y by 13×" not "X may help reduce Y."
- Active voice everywhere — no exceptions. Passive voice obscures agency and weakens prose.
- No filler adjectives: never use "novel," "significant," "state-of-the-art," "comprehensive," "robust," "substantial," "promising," "impressive." Replace with specific numbers or delete.
- Signpost through claims: section openers may state the section's conclusion ("This section shows that X reduces Y by 13×") but never use content-free placeholders ("In this section, we describe..."). The test: does the opener tell a skim-reader what the section *concludes*?
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

## Mandatory Style Audit (GATE — applies to ALL tex edits)

**Before presenting or committing ANY new or modified tex content, Claude MUST run a sentence-level style audit.** This is not optional, not triggered by the user, and not limited to full section drafts — it applies to every edit, including paragraph-level changes, subsection additions, and overview rewrites.

The audit checks every changed sentence against `author_profile/voice_profile.md` and `author_profile/compression_patterns.md`. Specifically, scan for and fix:

1. **Negation-first constructions**: "not X" or "rather than X" where the sentence should assert what something IS. Reframe positively.
2. **Throat-clearing**: "We address this problem by", "To address this issue", "In order to", "It should be noted that", "Note that". Delete and lead with the action.
3. **Hedging**: "can potentially", "can be expected to", "may help reduce", "it is possible that". Replace with assertive voice ("produces", "reduces", "achieves").
4. **Generic adjectives**: "significant", "substantial", "highly desirable", "novel", "robust", "comprehensive". Replace with specific numbers or delete.
5. **Sentence length**: Flag any sentence exceeding 40 words. Split or compress.
6. **Passive voice**: "accuracy was achieved by X" → "X achieves". "Experiments were conducted on X" → "We evaluate on X". Active voice everywhere — no exceptions.
7. **Missing citations**: Technical claims restated from other sections must carry forward their citations (Principle 14).

**Process**: After writing, read the changed text line by line. Fix all violations. Report a summary table of violations found and fixed (category, count). Only then present the draft or commit.

This gate is SEPARATE from and IN ADDITION TO the structural section checklists below.

---

## Section Checklists

After generating ANY section draft, Claude MUST also read the corresponding structural checklist and run it:

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

**Important:** After creating `project_context.md`, add it to the project's `.gitignore` (create the file if it doesn't exist). This file contains strategic framing notes and advisor commentary that should not be committed to shared repositories by default.

### Stage 2: Architecture

**Gate**: Section outline with claim assignments, per-section narrative arcs, figure/table plan, evaluation structure, and page budget.

**Craft reference**: If the student has run a literature survey (using the [literature-survey-skill](https://github.com/SNL-UCSB/literature-survey-skill) or manually), check for Pass 3+ paper notes with writing craft extractions — introduction anatomy, evaluation architecture, design section structure, and figure design choices from the strongest papers in their area. Load these as reference material for the architecture. The section structure of the best paper at your target venue is a better starting point for your outline than a generic template. Reading and writing develop together: craft patterns extracted during deep reading feed directly into the architecture of your own paper.

**Figure/table plan from visualization artifacts**: If the student has been working with the [data-visualization-skill](https://github.com/SNL-UCSB/data-visualization-skill), check for `plot_context.md` files and WALTER narrations. Each `plot_context.md` records the intent, variable mappings, plot type rationale, and design decisions for a figure — these are ready-made entries for the figure/table plan below. Each WALTER narration (Hypothesis → Axes → Look here → Trend → Exception → Result) maps directly to the evaluation prose that will accompany the figure. The iteration the student did in the viz skill — exploring what the data shows, forming predictions, confronting surprises — has already determined which figures carry the argument. The architecture should reflect that.

Output a structured table:

| Section | Pages | Key claim | Figures/Tables |
|---------|-------|-----------|----------------|
| ... | ... | ... | ... |

**Non-data figure specs**: For each figure in the plan that is NOT a data figure (architecture diagrams, pipeline illustrations, concept diagrams, comparison schematics), read `figure_synthesis_guide.md` and run spec mode to produce a `figure_spec.md`. Data figures (CDFs, bar charts, heatmaps, scatter plots) should be routed to the [data-visualization-skill](https://github.com/SNL-UCSB/data-visualization-skill). The boundary is clear: if the figure requires experimental data to render, it goes through `/viz`; if it illustrates structure, flow, or concepts, it goes through figure synthesis.

### Stage 3: Section Drafts

**Enforced order**: Draft 0 Introduction → Evaluation → Design/Method → Background → Related Work → Final Introduction → Abstract.

The introduction is written **twice**. This is the most impactful principle in the entire system (Principle 1).

**Draft 0 Introduction** comes first. It is a framing scaffold — stakes, problem gap, rough contribution claims — that sets guardrails for the evaluation. Draft 0 clarifies what the paper is *trying* to show. It is explicitly disposable: it probably will not survive to the final version. Writing is a thinking tool, not just a communication tool — Draft 0 forces the student to externalize their framing before designing experiments.

**The evaluation** comes next, constrained by Draft 0's guardrails. Then Design, Background, and Related Work.

**The final introduction** is rewritten from scratch after the evaluation is complete. It promises exactly what the evidence supports — no more, no less. Draft 0 is reference material, not a starting point for editing. If the user asks to skip Draft 0 and go straight to evaluation, explain that evaluation without framing guardrails produces experiments that don't build toward a unified argument.

**Per-section scaffolding**: Before writing any section's full prose, write the topic sentences first. Read them in sequence — they should form a coherent argument on their own. If the topic sentences don't flow, the paragraphs won't either. Fill in the full paragraphs only after the topic-sentence sequence holds together. In LaTeX, a practical technique is to annotate each paragraph with a purpose comment before writing prose:

```latex
\section{Introduction}
% Stakes: who suffers and why the domain matters.
...
% Problem gap: structural limitation of current approaches.
...
% Key abstraction: named concept that captures our insight.
...
% Contributions: numbered, claim-first list.
...
```

Each comment is a contract: the paragraph that follows must deliver on it. If a paragraph doesn't fit any comment, either the paragraph doesn't belong or a comment is missing.

**Per-section audit**: After generating a draft, read and run the appropriate checklist. Flag every violation with severity level before presenting the draft.

### Stage 4: Integration

Cross-section consistency pass:
- **Terminology drift**: Is the same concept called by the same name everywhere?
- **Claim-evidence mapping**: Does every introduction claim map to an evaluation subsection?
- **Key abstraction propagation**: Does the named concept from Introduction Move 3 appear in Design Move 1, Evaluation setup, and Related Work positioning?
- **Heading consistency**: Do section headings reflect the contribution order from the introduction?
- **Identity stability**: Read the first sentence of every section — do they tell a coherent story?
- **Flow audit**: Read the last sentence of paragraph N and the first sentence of paragraph N+1, throughout the paper. Does each transition work? Composition — the logical flow a reader can follow — is the most important aspect of technical writing. Readers forgive imperfect grammar but cannot follow broken logical progression. Structure produces flow, but structure alone doesn't guarantee it — the transitions between structural units matter.
- **Signposting check**: Does each section open with a claim-bearing sentence that tells a skim-reader what the section concludes? Is there an outline paragraph at the end of the introduction? Are figures distributed throughout pages rather than clustered?
- **Visual balance (landscaping)**: Check figure distribution across pages, paragraph length variation, and whitespace. Break up walls of text with signposts, figures, and paragraph headings. Avoid orphaned section headings at page bottoms.

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

**Do not pad to fill page limits.** If the paper is under the page limit after compression, that is fine. A short paper with appropriate content is better than a padded paper that reaches the limit. Padding introduces filler that weakens the argument.

### Pre-Submission Mechanical Checklist (Automated)

After compression and before submission, Claude MUST automatically run these checks using shell commands on the paper's `.tex` and compiled `.pdf` files. Do not ask the student to run them manually — execute them and report results.

**1. Page count.** Extract page count from the compiled PDF and compare against the venue's limit (from `project_context.md`). Flag whether references/appendices count — this varies by venue.
```bash
pdfinfo paper.pdf | grep Pages
```

**2. Broken references.** Search `.tex` source files for unresolved references that will render as `[?]` or `??` in the PDF. Also check the `.log` file for LaTeX warnings about undefined references and citations.
```bash
grep -n "LaTeX Warning.*undefined" paper.log
grep -rn '\\cite{' *.tex | grep -v '%' # list all citations for cross-check
```

**3. Embedded fonts.** Verify all fonts are embedded. Non-embedded fonts cause rendering differences across machines and are rejected by some submission systems.
```bash
pdffonts paper.pdf | grep -v "yes"
```
If any font shows `no` in the `emb` column, flag it and suggest adding `\usepackage[T1]{fontenc}` or compiling with `GS_OPTIONS=-dPDFSETTINGS=/prepress`.

**4. Figure quality.** Check that all included figure files are vector format (PDF/EPS) or high-resolution raster. List all figures referenced in the source and verify they exist.
```bash
grep -rn '\\includegraphics' *.tex  # list all figure references
file figures/*.pdf figures/*.png 2>/dev/null  # check file types
```
Flag any PNG/JPG figures — these should be vector unless they are photographs or screenshots.

**5. Anonymization (if double-blind).** Search all `.tex` files for author names, institution names, grant numbers, acknowledgment sections, and self-citations that could reveal identity.
```bash
grep -rni 'AUTHOR_NAMES_HERE\|INSTITUTION_HERE\|\\thanks\|acknowledgment' *.tex
```
Replace `AUTHOR_NAMES_HERE` and `INSTITUTION_HERE` with actual names from `project_context.md` before running.

**6. Column balancing.** Check whether the `balance` package is loaded (for two-column formats). If not, suggest adding `\usepackage{balance}` and `\balance` before `\bibliography`.
```bash
grep -rn 'balance' *.tex
```

**7. Common LaTeX issues.** Scan for frequently missed mechanical problems:
```bash
grep -rn '\\cite{.*}' *.tex | grep -v '~\\cite'  # missing ~ before \cite (dangling references)
grep -rn 'et al\.' *.tex | grep -v '~'  # missing ~ after "et al."
grep -rn '\\label{' *.tex | sort | uniq -d  # duplicate labels
```

**Report format**: Run all checks, then present a single summary table:

| Check | Status | Details |
|-------|--------|---------|
| Page count | ✓ or ✗ | N pages (limit: M) |
| Broken refs | ✓ or ✗ | List of undefined refs |
| Embedded fonts | ✓ or ✗ | List of non-embedded fonts |
| Figure quality | ✓ or ✗ | List of raster figures |
| Anonymization | ✓ or ✗ | List of leaks found |
| Column balance | ✓ or ✗ | Package present/missing |
| LaTeX issues | ✓ or ✗ | Count of dangling refs, duplicate labels |

Fix what can be fixed automatically (e.g., adding `~` before `\cite`). Flag what requires student decision (e.g., replacing a raster figure with vector).

---

## How to Respond to Common Requests

### "Help me write section X of paper Y"
1. Load the paper's `project_context.md`
2. Read all `author_profile/` files
3. Read the section's rhetorical moves from `section_rhetorical_moves/`
4. Identify which stage the paper is in — enforce introduction-twice ordering (Draft 0 intro → Evaluation → Design → Background → Related Work → Final intro → Abstract)
5. Write topic sentences first; verify they form a coherent argument before filling paragraphs
6. Generate the draft following the move sequence
7. Run the section checklist and flag violations with severity levels

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

### "I want to get feedback on this draft"
1. **First readings are precious.** Someone can read your work for the first time only once. Don't send your first draft to everyone simultaneously — chain your feedback. Send to one reader, incorporate their feedback, then send the revised version to the next reader. Each iteration strengthens the draft before it reaches the next pair of fresh eyes.
2. Help the student plan their feedback chain: who reads first (someone outside the subfield for framing clarity), who reads second (a domain expert for technical correctness), who reads last (the advisor, who sees the strongest version).
3. When asking a reader for feedback, tell them what to focus on: "Is the narrative in the introduction clear?" is more useful than "any thoughts?" Unfocused feedback wastes a first reading.
4. Use Claude's review mode (`author_profile/intervention_types.md`) to simulate a round of feedback *before* spending a human reader's first reading on an early draft.

### "Help me respond to reviewers"
1. Load `project_context.md` and the reviews
2. Classify each concern by severity: framing (most dangerous) → design → scope → rigor (most manageable)
3. For each concern: acknowledge → explain what changed → point to specific section/figure
4. Never be defensive. Never dismiss a concern. If a reviewer misunderstood, that's a WRITING failure to fix, not a reviewer failure to criticize.

### "I'm starting a new paper — where do I begin?"
1. Welcome them. Explain the five-stage pipeline — especially the introduction-twice principle.
2. Read `brainstorming_guide.md` and walk through all 6 phases to create their first `project_context.md`.
3. Emphasize: the brainstorming phase is the most important. A precise project context saves weeks of revision later.
4. After brainstorming: write a **Draft 0 introduction** — a disposable framing scaffold (stakes, problem gap, rough contributions) that sets guardrails for the evaluation. Then point them to `section_rhetorical_moves/evaluation.md` — they'll write the evaluation next, constrained by Draft 0.
5. Remind them: "Your first draft should be comprehensive — include everything. Compression comes later. The goal is to get material on paper, not to be concise. Draft 0 of the introduction will probably not survive — that's the point. It clarifies your thinking."

### "I need a non-data figure (architecture diagram, pipeline illustration, etc.)"
1. Load the paper's `project_context.md`
2. Read `figure_synthesis_guide.md` and the relevant files in `figure_templates/`
3. Classify the figure by archetype (architecture overview, pipeline flow, component detail, concept illustration, comparison schematic, taxonomy, deployment diagram)
4. Select the generation backend: AI image generation (for visually rich figures like architecture overviews and concept illustrations) or TikZ (for precise structural figures like pipeline flows and taxonomy matrices). The guide has defaults per archetype, but the student can override.
5. Run spec mode — walk through archetype-specific questions to produce a `figure_spec.md`
6. Run generate mode — assemble an AI prompt or TikZ code, produce the figure
7. Run critique mode — check against claims, venue formatting, and design principles
8. **NOTE**: Data figures (CDFs, scatter plots, bar charts, heatmaps) should go through `/viz`, not through figure synthesis
