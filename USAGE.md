# Using the paper-writing skill

A practical guide for students. `README.md` explains what the skill is; this file explains how to get the most out of it on a real paper.

## 1. Before you start

- **Install.** `git clone https://github.com/SNL-UCSB/paper-writing-skill.git && cd paper-writing-skill && ./setup`. Re-run `./setup` after every `git pull`; Claude reads the installed copy in `~/.claude/skills/paper-writing/`.
- **Pre-submission checks** need `pdfinfo` and `pdffonts` (from poppler: `brew install poppler` or your package manager).
- **Paper layout.** The style gate's grep commands assume sections live in `sections/*.tex`. If your paper is one file or uses another layout, say so at the start of the session ("my sections are in `paper.tex`").
- **Have ready:** the target venue and page limit; your draft (if any); results (tables, figure files); notes from meetings; the reviews (if revising).
- **Use git in your paper repository.** The skill edits files; commits let you see and undo every change.
- **The rules are a lab house style.** Zero passive voice, no hedging and the banned-word lists are one lab's defaults. Your advisor's style and the venue's norms win. Fork the repo and adjust `author_profile/` (section 7) before relying on the gate.

## 2. Your first session

```text
cd my-paper
claude
> /paper-writing
```

- With no `project_context.md`, Claude starts **Structured Brainstorming**: 34 questions in 6 phases (`brainstorming_guide.md`). Answer in your own words; "I don't know" is a useful answer because it shows a gap early. You can stop after a phase and continue in a later session.
- The result is `project_context.md` in your paper directory: the paper's identity, contributions with evidence, venue, and decisions. Every later session reads it. Read it carefully and correct it; `examples/example_project_context.md` shows a finished one.
- If you already have a draft, skip most questions: *"Build project_context.md from my draft in `sections/`; ask me only about the gaps."*
- If the ideas live in meeting notes, code, old drafts and earlier reviews rather than in your head, start with **Story mode** instead (section 3, first row). It ends by filling `project_context.md` for you.
- Working alone with one notebook and a short reading list: use Brainstorming and say *"read my notebook first and skip the questions it answers."*

## 3. Which mode for which situation

| You have | You want | Say or run |
|---|---|---|
| Ideas spread across notes, drafts, code, reviews and related work, or a draft whose framing is unsettled | A coherent story and an outline for a venue | *"Turn our notes into a <venue> outline"* or `/paper-writing story` (`story_mode.md`; short path for workshops) |
| An idea, no draft | A framed plan | `/paper-writing` (Structured Brainstorming) |
| A draft, no project context | To join the pipeline midway | *"Build project_context.md from my draft; ask only about gaps"* |
| Story-mode files, and co-authors who need one page to read | A story document: argument, problem specification, anecdotes then trends, evaluation plan with data available versus needed, a figure per main point | *"Build the story document"* (`story_document.md`) |
| A project context | Section plan and page budget | *"Plan the architecture for <venue>"* (Stage 2) |
| A plan | A section draft | *"Help me write the evaluation"* (Stage 3; order: Draft 0 intro, evaluation, design, background, related work, final intro, abstract) |
| All sections drafted | Consistency across sections | *"Run the integration pass"* (Stage 4) |
| A section over the page limit | A shorter section | *"Compress section 4"* (Stage 5; reports before and after) |
| A section | Feedback | *"Review this introduction"* |
| A full draft | Clean prose, repeatedly checked | `/loop audit with the paper-writing skill until clean` (`loop_mode.md`) |
| A system to explain | An architecture or pipeline figure | *"I need an architecture diagram"* (`figure_synthesis_guide.md`) |
| Data to plot | Plots | Use a data-visualization skill (for example `/viz`); this skill handles non-data figures only |
| A finished PDF near the deadline | Submission checks | *"Run the pre-submission checks"* |
| Reviews | A response | *"Help me respond to the reviewers"* and paste the reviews |

## 4. What the skill writes, and where

| File | Written by | Commit it? |
|---|---|---|
| `project_context.md` | Brainstorming or story mode | No by default: the skill adds it to `.gitignore`. Commit it only if your paper repository is private |
| Your `.tex` sections | Drafting, compression and review modes edit them in place | Yes; review each diff |
| `figure_spec.md` | Figure synthesis | Yes |
| `notes/AUDIT_LEDGER.md` | Loop mode (one entry per finding) | Yes, it is how a loop resumes |
| `story/digest.md`, `story/outline.md`, `story/spec.md`, `story/page.md` (the story document) | Story mode | Only if your paper repository is private; otherwise add `story/` to `.gitignore` |

## 5. Iterating well

- **Ask for findings, then decide.** A review returns numbered findings; you choose which to apply. Ask *"show before and after for each change"* when cutting or rewording.
- **Loop mode** processes one section per iteration, records findings in the ledger, and stops when every section in scope is clean. Scope it ("audit sections 2 and 3") to save time. To resume after a break, start the same `/loop` command; it reads the ledger. To stop, interrupt it.
- **Fresh eyes.** The red-team runs as a separate agent that did not write the text. If it keeps finding the same issue, rewrite the paragraph's structure or change the rule in your fork (section 7); rewording the same sentence again will not clear it.
- **Check facts yourself.** The skill improves structure and prose; it cannot know whether a number or a citation is right. Verify every result and reference against your data and the papers.

## 6. Using it with other skills

| Need | Skill | How it connects |
|---|---|---|
| Reading and positioning related work | A literature-survey skill (for example `/survey`) | Feed its synthesis into story mode step 4 or the related-work section |
| Exploring data, plotting results | Data-exploration and visualization skills (for example `/explore`, `/viz`) | Plots return as figure files plus a short description of the claim each supports |
| Prose quality beyond the gate | A general writing skill (for example `/writing`) | Run its checks after a drafting pass |
| Slides from the paper | A presentation skill (for example `/present`) | Uses the outline and claims from `project_context.md` |

"WALTER" in the figure guidance is a figure-narration checklist from the visualization skill: **W**hat hypothesis the figure tests, **A**xes (what x and y represent), **L**ook (where to focus), **T**rend (the dominant pattern), **E**xception (what breaks the pattern), **R**esult (the one-line takeaway).

## 7. Customizing

- `author_profile/gate_mechanical.md`: the grep checks (passive voice, banned words, dashes). Relax or remove checks your advisor does not want.
- `author_profile/gate_semantic.md`: the reader-judgment checks the red-team applies.
- `author_profile/craft_reference.md`: sentence length, tone, registers.
- `section_rhetorical_moves/` and `writing_checklists/`: add examples from your field.

Edit your fork, then run `./setup` again.

## 8. What not to use it for

- Generating claims, results or citations you do not have.
- Data plots (use a visualization skill).
- Accepting edits without reading the diff.
- A field with different conventions, before you customize the rules.

## 9. Keeping unpublished work private

This repository is public. Your `project_context.md`, story-mode notes, ledgers and drafts describe unpublished work: keep them in your own private paper repository. If you contribute an improvement back to this skill, replace project details with synthetic examples (a fictional system, invented numbers, roles instead of names), and never paste reviews, internal feedback or meeting notes into an issue or pull request.

## 10. Troubleshooting

- **`/paper-writing` is not recognized:** run `./setup`, then check `ls ~/.claude/skills/paper-writing/SKILL.md`.
- **The grep gate finds nothing:** your sections are not in `sections/*.tex`; tell Claude where they are.
- **Too many passive-voice hits on correct sentences:** relax the check in your fork (section 7).
- **The skill starts when you did not want it:** its description triggers on paper-writing requests; say "without the paper-writing skill" for a one-off.
- **Claude uses old rules after you edited the repo:** run `./setup` again.
