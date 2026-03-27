# paper-writing-skill

PhD students know what they found. Turning that into a paper that gets accepted is where they stall. The problem isn't writing ability — it's operational friction. Structuring sections, maintaining cross-section coherence, compressing to page limits, matching voice conventions, responding to reviewers: students spend weeks on scaffolding that an experienced advisor could sketch in an afternoon.

This skill offloads that scaffolding to the machine. It encodes 14 editorial principles, section-specific rhetorical move sequences, and a five-stage writing pipeline. All were extracted from [forensic analysis](https://sites.cs.ucsb.edu/~arpitgupta/blog/the-paper-behind-the-paper.html) of 6 papers, 8 submissions, 7,600+ Overleaf edits, 100+ tex file versions, and 5 peer review processes. These are the patterns that empirically distinguished accepted papers from rejected ones in the same research group. The student retains the hard parts: deciding what the paper claims, whether the evidence supports it, and how to position the work. The machine handles drafting mechanics, structural compliance, and revision logistics.

**Who this is for:** PhD students and researchers writing conference or journal papers. Tuned for systems and networking (SIGCOMM, NSDI, CoNEXT, IMC) and ML venues (NeurIPS, ICLR, ICML). Adaptable to any field — see [Customize for Your Field](#customize-for-your-field).

Built on [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

## Why This Skill Exists

Academic research faces a structural squeeze: stagnant funding, rising costs, researchers who must produce more with less. Agentic AI offers a way through — not by replacing researchers, but by compressing the operational middle of the research pipeline. This is the argument laid out in [*Systems for Agents, Agents for Systems*](https://sites.cs.ucsb.edu/~arpitgupta/blog/systems-for-agents-agents-for-systems.html).

Three competencies remain structurally human: **discrimination** (evaluating whether a draft argues what it should), **critique** (recognizing where the argument breaks), and **framing** (deciding what the paper claims and why anyone should care). Everything else — section templates, voice consistency, cross-section coherence, page-limit compression — is plumbing that machines can handle.

The concern that AI writing assistance weakens intellectual muscles gets the causality backwards. This skill forces students to articulate their own arguments at every stage. The brainstorming workflow demands that students name who suffers, what breaks structurally, and what evidence supports each claim *before any prose is generated*. The introduction-twice ordering forces students to confront what their results actually show. The section checklists surface violations that would otherwise survive until peer review. The machine provides scaffolding and enforcement. The student provides discrimination, critique, and framing.

## Part of a Series

This skill is one of a family of Claude Code skills developed at the [Systems and Networking Lab (SNL)](https://github.com/SNL-UCSB) at UC Santa Barbara by [Arpit Gupta](https://sites.cs.ucsb.edu/~arpitgupta/). Each targets a phase of the research pipeline where plumbing buries ingenuity:

| Skill | What it compresses | Student retains |
|-------|-------------------|-----------------|
| **[literature-survey-skill](https://github.com/SNL-UCSB/literature-survey-skill)** | Paper ingestion, claim extraction, cross-referencing, landscape mapping | Hypothesis formation, assumption identification, gap recognition, narrative construction |
| **[data-visualization-skill](https://github.com/SNL-UCSB/data-visualization-skill)** | Plot formatting, code generation, style compliance | Deciding what to show, why it matters, and whether the figure is honest |
| **[paper-writing-skill](https://github.com/SNL-UCSB/paper-writing-skill)** | Drafting mechanics, structural scaffolding, revision logistics | Argument construction, positioning, voice |

The skills are independent — use any one alone — but they form a closed loop, and the connections between them are the mechanism by which research thinking develops.

**Literature survey → Paper writing.** The survey skill's gap analysis produces the structural limitations that motivate the paper (Brainstorming Phase 1). Its competitive positioning feeds directly into how the paper positions itself against prior work (Phase 4). The survey's Pass 3+ **writing craft extraction** teaches students *how* the best papers in their area communicate: introduction anatomy, evaluation architecture, design section craft. Those extractions become reference material during the Architecture stage and inform what a strong rhetorical move looks like at a specific venue.

**Data visualization → Paper writing.** The viz skill's exploration phase forces students to look at their data from multiple angles *before* forming hypotheses. Surprises and failed predictions reshape what claims are defensible, feeding into Brainstorming Phase 3 (Evaluation Design). The viz skill's `braindump.md` articulates what hypothesis each figure tests — the empirical foundation for the paper's claim-evidence mapping. Its WALTER narrations (Hypothesis → Axes → Look here → Trend → Exception → Result) are first drafts of the Takeaway paragraphs that the evaluation needs. The iteration in the viz skill — exploring, predicting, confronting surprises — *is* the process of figuring out what the paper can honestly claim.

**Paper writing → Literature survey → Data visualization.** The loop closes. A student who has written an evaluation using the six-move sequence reads the next paper's evaluation with sharper eyes. A student who has written a Deep Dive with honest disaggregation approaches their next dataset knowing to look for subgroup differences. Writing sharpens reading, and reading sharpens visualization.

## Quick Start

```bash
git clone https://github.com/SNL-UCSB/paper-writing-skill.git
cd paper-writing-skill
./setup
```

This installs the skill to `~/.claude/skills/paper-writing/`. Then:

1. `cd` into your paper's working directory
2. Run `claude` to launch Claude Code
3. Type `/paper-writing`

If a `project_context.md` exists, Claude loads it and picks up where you left off. If not, Claude walks you through a structured brainstorming session — 34 pointed questions across 6 phases — to create one.

### Prerequisites

You need **Claude Code** installed on your machine. If you don't have it yet:

1. Go to https://docs.claude.com and follow the installation instructions for your OS
2. Run `claude` in your terminal to verify it works — you should see the Claude Code prompt
3. Make sure you're signed in (Claude Code will prompt you on first launch)

### Manual Install

Copy `SKILL.md` to `~/.claude/skills/paper-writing/SKILL.md`. Copy `brainstorming_guide.md`, `author_profile/`, `writing_checklists/`, `section_rhetorical_moves/`, and `examples/` alongside it.

## How the Skill Works

When you type `/paper-writing`, Claude loads the editorial rules, checklists, and rhetorical move guides. Every draft is checked against the same checklists used to review papers in the SNL lab. Every section follows a move sequence extracted from accepted papers. Voice rules — sentence length, hedging, filler words, claim-first paragraphs — are enforced automatically.

### Common Workflows

- "Help me write the evaluation section" — Claude generates a draft following the 6-move evaluation sequence, then runs the evaluation checklist and flags violations
- "Review this introduction" — Claude reads your draft, applies the 7 intervention types in order, and gives numbered feedback with severity levels and concrete rewrites
- "Compress section 4" — Claude applies 7 compression operations and reports before/after character counts
- "Help me respond to these reviews" — paste in reviewer comments and Claude classifies each concern and drafts responses
- "I have a new paper idea about X" — Claude runs the full brainstorming workflow to create a fresh project context

## The Five-Stage Pipeline

Every paper goes through these stages in order:

**Stage 1 — Structured Brainstorming.** The skill's centerpiece. Claude walks you through 34 questions across 6 phases: Problem Discovery, Contribution Crystallization, Evaluation Design, Positioning and Framing, Architecture and Constraints, and Narrative Spine. Each phase forces externalization — vague intuitions become concrete, falsifiable statements. The output is a `project_context.md` that serves as the binding contract for all subsequent writing sessions.

**Stage 2 — Architecture.** Section outline with claim assignments, per-section narrative arcs, figure/table plan, and page budget. The evaluation plan must exist before the introduction is outlined.

**Stage 3 — Section Drafts.** Enforced order: Draft 0 Introduction → Evaluation → Design → Background → Related Work → Final Introduction → Abstract. The introduction is written *twice* ([see below](#the-writing-order--introduction-twice)). Topic sentences are written first and verified as a coherent argument before full prose. After each draft, the section checklist runs and flags violations by severity (CRITICAL / MAJOR / MINOR).

**Stage 4 — Integration.** Cross-section consistency: terminology drift, claim-evidence mapping, key abstraction propagation, heading consistency, identity stability. Flow audit, signposting check, and visual balance (figure distribution, whitespace, no walls of text).

**Stage 5 — Compression.** Seven operations applied in order: sentence shortening, paragraph merging, generic adjective removal, tutorial deletion, claim-first conversion, takeaway insertion, figure/table promotion. Target: 30–50% reduction. Character counts reported before and after.

## The Writing Order — Introduction-Twice

The skill enforces: **Draft 0 Introduction → Evaluation → Design → Background → Related Work → Final Introduction → Abstract.**

The introduction is written **twice**. This is the single most impactful principle in the entire system.

**Draft 0** is a disposable framing scaffold — stakes, problem gap, rough contribution claims — written before the evaluation to set guardrails. Without it, the student writes experiments without knowing what they're trying to show. Writing is a thinking tool, not just a communication tool: Draft 0 forces the student to externalize their framing before designing experiments.

**The final introduction** is rewritten from scratch after the evaluation is complete. It promises exactly what the evidence supports. Draft 0 is reference material for the rewrite, not a starting point for editing.

In our corpus, papers with stable framing — where the introduction was rewritten to match the evaluation — were accepted. Papers where Draft 0's promises survived unchecked to submission had framing problems that led to rejection.

## Intellectual Foundations

### Forensic Revision Analysis

The skill's principles are empirical patterns, not theoretical prescriptions. They come from systematic analysis of how papers were actually written and revised — 7,600+ Overleaf edits, 100+ tex file versions, 5 complete peer review cycles across systems and ML venues. The methodology: reconstruct each paper's full edit history, classify every advisor intervention by type and impact, identify which revision patterns correlated with acceptance, and distill the recurring patterns into teachable rules.

This forensic approach reveals things that writing guides miss. The Key Abstraction (a coined term capturing the paper's intellectual core) appears only in final versions — it is *discovered through writing*, not planned before it. The advisor coins named abstractions during late-stage revision, not the student in the first draft. The implication: brainstorming sets a placeholder name, but the real name emerges during drafting. The skill accounts for this by treating abstraction naming as iterative.

The single strongest predictor of acceptance was whether the final introduction was constrained by the evaluation's actual results. Papers where Draft 0 survived unchecked suffered from framing drift — they promised capabilities the evaluation couldn't deliver. Papers rewritten from scratch after the evaluation had stable framing. But students who skipped Draft 0 entirely produced narratively incoherent experiments. The resolution: write the introduction twice.

### Writing as Thinking

Writing clarifies thinking. Start writing early — much of the early material will not reach the final paper, but writing forces you to articulate concepts precisely. It reveals gaps in understanding that persist as long as ideas live only as intuitions. This principle, emphasized by Feamster in [*Storytelling 101*](https://practicespace.substack.com/p/storytelling-101-writing-tips-for) and by Zinsser in *On Writing Well*, is why the skill requires Draft 0 before the evaluation. Draft 0 is not communication — it is thinking committed to prose. The brainstorming workflow captures this at the question-and-answer level; Draft 0 captures it at the prose level, where different gaps become visible.

### The Expansion-Compression Arc

Every successful paper in the corpus followed the same arc: comprehensive first draft → structural reorganization → aggressive compression. Student drafts averaged ~24 words per sentence; final versions averaged ~21. Background sections were cut 50–65%. The largest single deletions were always tutorial material the venue audience already knew.

Compression above 50% signals a framing problem — the paper's identity shifted, and content was cut because it no longer fit the argument. Compression below 15% suggests the draft was already near publication-ready. Normal compression (30–50%) removes redundancy while preserving the argument.

### Structured Brainstorming as Externalization

The brainstorming workflow addresses a specific failure mode: students whose ideas live as unstructured intuitions. They know something is interesting but can't articulate what or why. The 34 questions force externalization — committing vague intuitions to concrete, falsifiable statements. "My system is faster" becomes "Our system reduces RTT estimation error by 13× on bursty telemetry because it decomposes the signal into event-triggered and baseline components."

The question sequence prevents premature commitment. Problem Discovery (Phase 1) must complete before Contribution Crystallization (Phase 2) — a student who starts with "I built X" frames the problem to fit their solution. Evaluation Design (Phase 3) comes before Positioning (Phase 4) — knowing what your evidence shows constrains how you position the work.

### Scaffolding-First Drafting

Before writing full prose for any section, write the topic sentences first. Read them in sequence — they should form a coherent argument on their own. If the topic sentences don't flow, the paragraphs won't either. Fill in full paragraphs only after the topic-sentence sequence holds together. This principle, articulated by Feamster as "build scaffolding first," catches structural problems before they're buried in prose.

### Rhetorical Move Theory

Each section type follows a characteristic sequence of rhetorical moves — functional units that accomplish specific communicative goals. We extracted the skill's move sequences (Introduction: 6, Evaluation: 6, Design: 5, Related Work: 3) by comparing accepted papers against rejected versions of the same papers.

The moves are not templates to fill. They are *functional requirements* — each move must accomplish something specific, and the sequence matters because later moves depend on earlier ones. The Introduction's Move 3 (Key Abstraction) only works after Move 2 (Problem Gap) has established *why* a new abstraction is needed. The Evaluation's Move 4 (Takeaway Synthesis) only works after Move 3 (Deep Dive) has provided the disaggregated results that the takeaway interprets.

The most diagnostic finding: student drafts used only Move 2 (head-to-head comparison) in evaluations. The advisor added Moves 3–6. The Takeaway paragraph — absent from all early drafts, present in all final versions — transformed "here are numbers" into "here is what the numbers mean."

### Advisor Intervention Taxonomy

The revision histories revealed seven types of editorial interventions, ordered by impact: framing intervention (changing what the paper claims to be about), structural rewrite (reorganizing sections), background deletion (removing tutorial material), evaluation strengthening (adding takeaways and baselines), terminology tightening (replacing vague terms with named alternatives), contribution reframing (converting process descriptions to claims), and argument clarification (connecting evidence to claims).

The taxonomy serves two purposes. First, it structures the skill's draft review mode — when simulating advisor feedback, the skill applies interventions in the empirically observed order (framing first, polish last). Second, it makes the advising process legible to students. A student who understands that their advisor's rewrites are *framing interventions* (not "arbitrary changes") can learn to do the framing work themselves.

## Design Principles

**Empirical over theoretical.** Every principle comes from observed revision patterns, not prescriptive writing advice. The evidence is in the edit histories.

**Shunt the plumbing, protect the thinking.** The machine handles section scaffolding, voice enforcement, checklist compliance, and compression mechanics. The student handles argument construction, competitive positioning, and honest self-assessment.

**Introduction-twice, always.** Draft 0 sets framing guardrails before the evaluation; the final version is rewritten from scratch afterward, constrained by what the evidence supports.

**Structured externalization.** Vague intuitions produce vague papers. The brainstorming workflow forces every claim into a concrete, falsifiable statement before any prose is generated.

**Compression is not editing.** First drafts should be comprehensive — include everything. Compression is a separate stage with specific operations and quantitative targets. If the paper is under the page limit after compression, that is fine — a short paper with appropriate content beats a padded paper that reaches the limit.

**The student draft is material, not the paper.** The student's comprehensive first draft is raw material for restructuring and compression — never discarded, always restructured to serve the argument.

## What's in This Repo

```
paper-writing-skill/
├── SKILL.md                              # Main skill file (Claude reads this)
├── brainstorming_guide.md                # 34 questions across 6 phases
├── setup                                 # One-command installer
├── README.md                             # You are here
├── author_profile/                       # Editorial rules (defaults, customizable)
│   ├── editorial_principles.md           # 14 principles with evidence from 6 papers
│   ├── voice_profile.md                  # Sentence-level style rules
│   ├── compression_patterns.md           # 7 compression operations with examples
│   ├── rhetorical_moves.md              # Cross-section move sequences
│   └── intervention_types.md             # 7 advisor intervention types
├── writing_checklists/                   # Post-draft structural diagnostics
│   ├── intro_questions.md                # 6-move introduction checklist
│   ├── evaluation_questions.md           # Claim-evidence + takeaway checklist
│   ├── design_questions.md               # What→Why→So-What structure check
│   └── related_work_questions.md         # Positioning + anti-pattern check
├── section_rhetorical_moves/             # Detailed section structure guides
│   ├── introduction.md                   # 6-move sequence + accepted vs rejected comparison
│   ├── evaluation.md                     # 6-move sequence + evolution stages
│   ├── design.md                         # 5-move sequence + anti-patterns
│   └── related_work.md                   # 3-move sequence + placement strategy
└── examples/
    ├── project_context.md                # Template with guided prompts
    └── netburst_project_context.md       # Real example (NetBurst NSDI '27)
```

### Reference Files

`editorial_principles.md` contains 14 cross-paper principles ordered by observed impact on acceptance outcomes. Each principle includes evidence from at least two paper revision cycles and a concrete test for violation.

`voice_profile.md` encodes sentence-level style rules: ~21-word mean sentence length, claim-first topic sentences, zero hedging, active voice for claims, banned filler adjectives, paragraph density targets. We derived these from sentence-level analysis comparing student drafts against advisor-edited final versions.

`compression_patterns.md` defines 7 compression operations with before/after examples and quantitative benchmarks. `rhetorical_moves.md` summarizes the per-section move sequences. `intervention_types.md` classifies 7 types of advisor interventions for simulating feedback on drafts.

### Writing Checklists

Four post-draft checklists (introduction, evaluation, design, related work) run automatically after every section draft. Each operationalizes the rhetorical move sequence for its section type, flagging structural violations by severity.

### Section Rhetorical Moves

Per-section guides with concrete examples from accepted and rejected papers. Introduction (6 moves: Stakes → Problem Gap → Key Abstraction → Design Intuition → Contributions → Results Preview), Evaluation (6 moves: Setup Anchoring → Head-to-Head → Deep Dive → Takeaway Synthesis → Ablation → Robustness), Design (5 moves), Related Work (3 moves).

### Use These Materials Without Claude Code

The editorial principles, rhetorical move guides, and checklists work independently of the skill runtime. Print `editorial_principles.md` for a lab writing workshop. Use the section checklists as self-review guides before submitting a draft to your advisor. Use `brainstorming_guide.md` to structure a whiteboard session for a new paper idea.

## Customize for Your Field

The skill defaults to systems and networking (SIGCOMM, NSDI, CoNEXT, IMC) and ML venues (NeurIPS, ICLR, ICML). Five adaptation points:

**Voice rules.** Edit `author_profile/voice_profile.md` to adjust sentence length targets, hedging policy, and tone. Keep the claim-first and no-filler rules — these are venue-agnostic.

**Editorial principles.** Edit `author_profile/editorial_principles.md` to add your own principles or modify priorities. Keep introduction-twice and named-over-vague — these are universal.

**Compression intensity.** Edit `author_profile/compression_patterns.md` to adjust which operations to prioritize and the target reduction range.

**Venue conventions.** The skill distinguishes systems venues (labeled paragraphs, post-evaluation related work) from ML venues (colon subtitles, integrated related work). Add conventions for your target venues.

**Rhetorical moves.** The move sequences in `section_rhetorical_moves/` come from CS papers. Add examples from your field's best-written work. The functional structure (moves and their dependencies) is domain-agnostic; the examples should be domain-specific.

## For Advisors: Sharing With Your Group

1. Fork this repo for your group
2. Customize `author_profile/` to encode your editorial principles
3. Have students clone YOUR fork — they get your group's standards immediately
4. Students can further customize voice rules, but the pipeline and structure stay consistent

Students don't need to configure anything. The skill produces output consistent with the feedback you would give them. Configuration is optional, for students developing their own voice.

## Troubleshooting

**"Claude doesn't recognize /paper-writing"** — Run `./setup` again. The skill files need to be in `~/.claude/skills/paper-writing/`. Check: `ls ~/.claude/skills/paper-writing/SKILL.md`

**"Claude isn't following the writing rules"** — Make sure you typed `/paper-writing` at the start of the conversation. The skill only activates when explicitly invoked. Check that `author_profile/` files exist in the skill directory.

**"I want to start over with a paper's context"** — Delete `project_context.md` in your paper's directory and invoke `/paper-writing` again.

**"I updated the repo but Claude is using old rules"** — Run `./setup` again after pulling. The setup script copies files to the skill directory.

## Updating

```bash
cd paper-writing-skill
git pull
./setup
```

Always edit files in this repo directory, not directly in `~/.claude/skills/`. Running `./setup` copies changes to the right place.

## Intellectual Lineage

| Source | Contribution | Where it appears |
|--------|-------------|------------------|
| Feamster, [*Storytelling 101*](https://practicespace.substack.com/p/storytelling-101-writing-tips-for) | Write introductions twice; scaffolding-first; composition as the most important aspect; signposting; don't pad | Introduction-twice principle, topic-sentence substep, flow audit, signposting, landscaping |
| Gupta, [*The Paper Behind the Paper*](https://sites.cs.ucsb.edu/~arpitgupta/blog/the-paper-behind-the-paper.html) | Forensic revision analysis methodology; identity stability tracks acceptance; naming as legibility; rejection forces abstraction | Every editorial principle, intervention type, and rhetorical move sequence |
| Gupta, [*Systems for Agents, Agents for Systems*](https://sites.cs.ucsb.edu/~arpitgupta/blog/systems-for-agents-agents-for-systems.html) (2025) | Compress the operational middle, protect judgment | Design philosophy, skill series rationale |
| Gupta, [*A First-Principles Approach to Networked Systems*](https://sites.cs.ucsb.edu/~arpitgupta/first-principles-networking/) | Structural analysis, invariant questions | Brainstorming Phase 1 (structural vs. quantitative gaps) |
| Winter, [*Research Power Tools*](https://research-power-tools.com/) (2023) | LaTeX paragraph-purpose comments as scaffolding; chained feedback; pre-submission mechanical checks; paper type taxonomy | Stage 3 scaffolding technique, feedback management, pre-submission checklist, brainstorming Phase 4 |
| Kurose, [*Writing the Introduction*](https://www.cs.columbia.edu/~hgs/etc/intro-style.html) | Introduction structure conventions for CS papers | Six-move introduction sequence |
| Strunk & White, *The Elements of Style* | "Omit needless words"; sentence efficiency | Voice profile, compression patterns |
| Zinsser, *On Writing Well* | Writing as thinking; clarity through revision | Writing-as-thinking principle, Draft 0 rationale |
| [literature-survey-skill](https://github.com/SNL-UCSB/literature-survey-skill) | Writing craft extraction, six-move introduction formula | Section rhetorical moves, cross-skill reinforcement |
| [data-visualization-skill](https://github.com/SNL-UCSB/data-visualization-skill) | WALTER principle, figure narration, visual argument analysis | Evaluation figure planning, figure-claim linkage |

## Contributing

Contributions welcome. The areas where additions would help most: editorial principles from fields beyond systems and networking, rhetorical move examples from additional venues (SOSP, OSDI, CHI, USENIX Security), example `project_context.md` files from real papers, and domain-specific brainstorming questions for fields where the 34-question sequence needs adaptation.

## License

MIT

## Citation

If you use this skill in your research workflow:

```
@misc{paper-writing-skill,
  author = {Gupta, Arpit},
  title = {paper-writing-skill: A Claude Code skill for research paper writing},
  year = {2026},
  publisher = {GitHub},
  url = {https://github.com/SNL-UCSB/paper-writing-skill}
}
```
