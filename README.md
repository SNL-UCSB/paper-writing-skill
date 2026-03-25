# Paper Writing Skill for Claude Code

A research paper writing assistant built from forensic analysis of 6 papers, 8 submissions, 7,600+ Overleaf edits, and 5 peer review processes from the Gupta research group at UC Santa Barbara. It encodes battle-tested editorial principles, section-specific rhetorical moves, and a structured writing pipeline that mirrors how accepted papers were actually written.

**This skill works out of the box.** You do not need to configure anything to start using it. Prof. Gupta's writing rules ship as defaults.

---

## Getting Started (Students)

### Prerequisites

You need **Claude Code** installed on your machine. If you don't have it yet:

1. Go to https://docs.claude.com and follow the installation instructions for your OS
2. Run `claude` in your terminal to verify it works — you should see the Claude Code prompt
3. Make sure you're signed in (Claude Code will prompt you on first launch)

### Installation (one time, ~30 seconds)

```bash
git clone <this-repo-url>
cd paper-writing-skill
./setup
```

That's it. The setup script copies the skill files into `~/.claude/skills/paper-writing/`, which is where Claude Code looks for skills. You'll see a confirmation message when it's done.

### Using the Skill

**Starting a new paper** — the most common starting point:

1. Open a terminal and `cd` into your paper's working directory (where your `.tex` files live, or where you want to start a new paper)
2. Run `claude` to launch Claude Code
3. Type `/paper-writing`
4. Claude will ask which paper you're working on. If it's new, Claude walks you through a structured brainstorming session — 34 pointed questions across 6 phases that turn your unstructured ideas into a precise `project_context.md`
5. Once your project context exists, tell Claude what you need: "write the evaluation section," "review my introduction draft," "compress section 3"

**Returning to an existing paper:**

1. `cd` into the paper's directory (where `project_context.md` already lives)
2. Run `claude`
3. Type `/paper-writing`
4. Claude finds your `project_context.md`, loads it, and picks up where you left off

**Example commands once the skill is active:**

- "Help me write the evaluation section" — Claude generates a draft following the 6-move evaluation sequence, then runs the evaluation checklist and flags violations
- "Review this introduction" — Claude reads your draft, applies the 7 intervention types in order, and gives numbered feedback with severity levels and concrete rewrites
- "Compress section 4" — Claude applies 7 compression operations and reports before/after character counts
- "Help me respond to these reviews" — paste in reviewer comments and Claude classifies each concern and drafts responses
- "I have a new paper idea about X" — Claude runs the full brainstorming workflow to create a fresh project context

### What Happens Behind the Scenes

When you type `/paper-writing`, Claude loads the skill and reads the editorial rules, checklists, and rhetorical move guides. Every draft it generates is checked against the same checklists used to review papers in the group. Every section follows a specific move sequence extracted from accepted papers. The voice rules (sentence length, hedging, filler words, claim-first paragraphs) are enforced automatically.

You don't need to know the details — just type `/paper-writing` and start describing what you need. Claude handles the rest.

---

## What You Get

When you invoke `/paper-writing`, Claude:

1. Loads 13 editorial principles derived from real paper revision histories, with evidence from accepted and rejected papers
2. Enforces sentence-level voice rules (21-word mean, zero hedging, claim-first topic sentences, banned filler words)
3. Runs section-specific checklists after every draft — the same checklists used to review drafts in the group
4. Follows a five-stage pipeline: Structured Brainstorming → Architecture → Section Drafts → Integration → Compression
5. Enforces evaluation-first writing order (the single most impactful principle: write the evaluation before the introduction)
6. Applies 7 specific compression operations with quantitative targets (30-50% reduction)

## Starting a New Paper — Structured Brainstorming

The skill's centerpiece is a structured brainstorming process that transforms unstructured ideas into a precise project context. Claude walks you through 34 questions across 6 phases:

- **Phase 1 — Problem Discovery**: Who suffers, what breaks, why it breaks structurally (not quantitatively). Forces you to articulate the domain problem before touching your solution.
- **Phase 2 — Contribution Crystallization**: Core claim as a finding ("shows" not "proposes"), headline number, key abstraction name, honest limitations.
- **Phase 3 — Evaluation Design**: Named baselines, specific metrics, dataset justification, experiment-to-claim mapping, ablation plan, robustness story. Designed BEFORE writing any section.
- **Phase 4 — Positioning and Framing**: Venue fit, systems vs. measurement, competitive positioning, category creation vs. competition.
- **Phase 5 — Architecture and Constraints**: Design pipeline with named stages, locked decisions, open questions, section architecture with page budgets.
- **Phase 6 — Narrative Spine**: Story arc, the "inevitable" moment where your design feels like the only response, tweet-length pitch.

This produces a `project_context.md` — the binding contract for all writing sessions on that paper. See `examples/netburst_project_context.md` for what a complete one looks like (real paper, real decisions, real timeline).

## The Writing Order

The skill enforces this section-writing order: Evaluation → Design → Background → Related Work → Introduction → Abstract.

This is non-negotiable. The introduction promises what the paper delivers. Writing it after the evaluation ensures you promise exactly what you can deliver. Among the group's papers, papers with stable framing (written evaluation-first) were accepted; papers whose introduction was written before the evaluation had framing problems that led to rejection.

## Customizing Your Voice (Optional)

The skill ships with Prof. Gupta's voice rules, which means your output will be consistent with advisor feedback. You do not need to customize anything — the defaults work. But if you want to develop your own voice while keeping the structural pipeline:

1. Navigate to `~/.claude/skills/paper-writing/author_profile/`
2. Edit these files:

| File | What to customize | What to keep |
|------|-------------------|--------------|
| `voice_profile.md` | Sentence length, hedging policy, tone preferences | The claim-first and no-filler rules (these are universal) |
| `compression_patterns.md` | Compression intensity, which operations to prioritize | The 7-operation framework |
| `editorial_principles.md` | Add your own principles or modify priorities | The evaluation-first and named-over-vague principles |

The structural rules (five-stage pipeline, rhetorical move sequences, section checklists) are in `SKILL.md` and should generally not be changed — they encode patterns that distinguish accepted from rejected papers.

## Troubleshooting

**"Claude doesn't recognize /paper-writing"** — Run `./setup` again. The skill files need to be in `~/.claude/skills/paper-writing/`. Check that the directory exists: `ls ~/.claude/skills/paper-writing/SKILL.md`

**"Claude isn't following the writing rules"** — Make sure you typed `/paper-writing` at the start of the conversation. The skill only activates when explicitly invoked. Also check that `author_profile/` files exist in the skill directory.

**"I want to start over with a paper's context"** — Delete the `project_context.md` in your paper's directory and invoke `/paper-writing` again. Claude will re-run the brainstorming workflow.

**"I updated the repo but Claude is using old rules"** — Run `./setup` again after pulling. The setup script copies files from this repo to the skill directory. Edits in this repo don't take effect until you re-run setup.

---

## Directory Structure

```
paper-writing-skill/
├── SKILL.md                              # Main skill file (Claude reads this)
├── brainstorming_guide.md                # 34 questions across 6 phases — the skill's centerpiece
├── setup                                 # One-command installer
├── README.md                             # This file
├── author_profile/                       # Editorial rules (real defaults, customizable)
│   ├── editorial_principles.md           # 13 principles with evidence from 6 papers
│   ├── voice_profile.md                  # Sentence-level style rules
│   ├── compression_patterns.md           # 7 compression operations with examples
│   ├── rhetorical_moves.md              # Cross-section move sequences
│   └── intervention_types.md             # 7 advisor intervention types
├── writing_checklists/                   # Post-draft self-diagnostics
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

## For Advisors: Sharing With Your Group

If you're sharing this with students, the recommended workflow:

1. Fork this repo for your group
2. Optionally customize `author_profile/` to add your own principles
3. Have students clone YOUR fork — they get your group's standards immediately
4. Students can further customize voice rules if they want, but the pipeline and structure stay consistent

The key insight: students don't need to configure anything. The skill produces output that's consistent with the feedback you'd give them anyway. Configuration is optional, for students who want to develop their own voice as they mature.

## Updating

```bash
cd paper-writing-skill
git pull
./setup
```

Always edit files in this repo directory, not directly in `~/.claude/skills/`. Running `./setup` copies changes to the right place.
