# Story Mode: From Existing Ideas to a Paper Story and Outline

Use this mode when the ideas for a paper already exist, scattered across meeting notes, drafts, code, chat threads, reviews of earlier submissions and related work, and you need one coherent story and a section-level outline for a target venue. It ends by writing `project_context.md`, so the rest of the pipeline in `SKILL.md` (Stage 2 onward) starts from its output.

It is different from Structured Brainstorming (`brainstorming_guide.md`). Brainstorming draws ideas out of your head through questions. Story mode assembles a story from a record that already exists, and its first rule is to invent nothing. If your record is one notebook and a short reading list, Brainstorming is usually the better start: say *"read my notebook first and skip the questions it answers."*

**Trigger phrases:** "turn our notes into a paper outline", "what is the story for <venue>", "connect the dots for a <venue> submission", "draft the outline from what we have discussed", or `/paper-writing story`.

## What you get

Saved in your paper repository under `story/`:

1. `story/digest.md`: what each source says, with dates and citations, and where sources disagree.
2. `story/outline.md`: the problem, why it is hard, why existing work falls short, the key idea, one core contribution with supporting contributions, the evaluation questions with what would refute each claim, a Draft 0 introduction, and a skeleton of every section with a page budget.
3. `story/spec.md`: a one-page **calibration spec** (framing, facts to state carefully, terms) that every later document is checked against.
4. `project_context.md`, filled from the three files above (step 10).

These files describe unpublished work. Keep them in a private repository; if your paper repository is shared or public, add `story/` and `project_context.md` to `.gitignore` (the skill adds `project_context.md` by default).

## Before you start

- Put every source where Claude can read it: draft files, exported meeting notes, a local clone of each repository, reviews of earlier submissions, saved PDFs of the closest work. Claude cannot read what it cannot open; a missing source becomes a hole in the story.
- If a source the record treats as authoritative (an earlier outline, a calibration spec, a key paper) is unavailable, ask for it before step 2. Do not reconstruct it from summaries.
- Use git for the `story/` files and commit after every step, so you can compare versions and undo a bad rewrite.
- Decide who confirms the framing: your advisor, your co-authors, or you.

**Terms used below.** A **fresh reviewer** is a new Claude session or subagent that did not write the text, given only the sources and the output to check. Review findings are **critical** (a claim is false, unsupported or untestable), **major** (a reviewer at the venue would object), or **minor** (wording).

## Full path and short path

The full path (steps 1 to 10) suits a conference paper built from months of work by several people. For a workshop paper, a solo project, or a deadline under a month away, take the **short path**: steps 1, 2, 4 (read the three to five closest papers yourself, without reader agents), 5, 6, 7, 8, 9 (one review round) and 10; skip step 3.

## The steps

Each step gives its purpose, a request to adapt, what the output must contain, how you know it is done, and the usual failure with the request that corrects it. Replace `<...>` with your specifics.

### 1. Read the whole record before writing anything

**Purpose.** Know the project as the whole record shows it; one meeting or one memory gives a partial view.

**Request.**
> Before writing anything, read <draft>, the notes from <meetings since date>, <repo> including its commit history, the code it describes, and the reviews of <earlier submission>. Trace the ideas from <start of the project> to now. List each claim in the draft that the code, an earlier decision or a review contradicts, with file and line. Then summarize where the project stands in `story/digest.md`.

**Output.** A digest per source with dates and citations; how the main idea evolved; the conflicts between sources (definitions, counts, what the code does); what earlier reviewers objected to.

**Done when** every statement in the digest cites a source, and any premise that changes the method (how much data exists, what is already built) has been checked against the primary source.

**Failure.** A summary of one meeting with no wider context. *Say:* "This is superficial. Read <repo, earlier notes, code> first and tell me what contradicts it."

### 2. Connect the dots; invent nothing

**Purpose.** Assemble a candidate story only from ideas you (or your group) have already discussed, cited or claimed.

**Request.**
> Synthesize a possible outline for <venue> in `story/outline.md`. Do not come up with ideas that have not been discussed. Connect the dots of what has already been discussed, cited and thought about as contributions. Don't make things up. Put anything that goes beyond the record in a separate section for me to accept or reject.

**Output.** The outline, a section "Steps beyond the record", and a note "Where the record differs from your account". The note also lists every element agreed earlier (in meetings, in the earlier paper, in the advisor's last restatement of the story) and what the new outline does with each: keeps it, refines it, demotes it or drops it.

**Precedence.** The latest statement in the record from the advisor (or whoever decides the framing) outranks any reviewer's suggestion, including a fresh reviewer's. Demoting or dropping an agreed element needs their words. A proposal they heard without objecting is "on record, pending", and it does not go under "Steps beyond the record".

**Done when** a fresh reviewer has checked the outline against the sources, every proposal not in the record is labeled, and every agreed element has a stated fate.

**Failure.** Claude's own proposals presented as yours, or two projects merged. *Say:* "Mark what is yours. Keep <project A> and <project B> separate."

### 3. Place the idea in its intellectual history

**Purpose.** Show where the idea sits among earlier approaches in the field, and why it was not possible or not built before.

**Request.**
> Develop the intellectual history of <area> from first principles. For each era, give the constraint that held it in place and what it could not see. Place our idea in that history and say what change makes it possible now.

**Output.** A table of eras, each with its constraint and blind spot; one sentence placing the idea; the enabling change (often a change in cost or scale); where that change does not hold.

**Done when** every "no earlier approach did X" sentence has been checked against the papers themselves.

**Failure.** A sweeping claim that one counterexample breaks. *Say:* "Narrow this until the papers support it."

### 4. Read the closest work in depth, with one shared brief

**Purpose.** Compare against prior work on the same terms, from the papers and code rather than from abstracts.

**Request.**
> Write one brief for reader agents with fixed dimensions: <goal, what it builds, how it is built, behavior at run time, how it handles variation, how it is validated, how it is evaluated, stated limits>. Each reader reads the full paper or the code and cites a section or line for every cell. Don't inflate or shrink the overlap, and report where the work is ahead of ours. Launch one reader per family of related work.

**Output.** Comparable tables, then three sentences per work: the overlap to concede, the difference to claim, the objection a reviewer would raise.

**Done when** you have spot-checked the three claims the argument depends on most (a quote or a code line), and anything unverified is marked.

**Failure.** Verdicts drawn from abstracts or READMEs, or an over-reaction to a loose resemblance. *Say:* "Show the quote or code line," or "You over-reacted; compare the mechanisms in detail."

### 5. Decide what is new: one core contribution

**Purpose.** Settle the novelty claim before writing prose around it.

**Request.**
> Adjudicate these novelty claims: <list>. For each, give the strongest case that <prior work> already does it, then the defense. Count prior work as covering our claim if someone could use it as it stands, with little effort, to do what we do. Keep work that inspired us separate from the baselines we must beat. Iterate until the arguments converge.

The "usable as it stands" bar is a default suited to systems venues. Adjust it to your field: ML reviewers also count conceptual overlap against novelty, so name close conceptual work explicitly.

**Output.** One core contribution, with supporting contributions as bullets under it. Every other claim gets a verdict: survives, becomes a measurement, becomes a design choice, narrowed, or dropped. Positioning rests on two or three differences in mechanism; avoid feature-checklist tables.

Also required:
- An exemplar table with columns *case | the closest work's reading | our reading | how the paper uses it*. Include at least one case where the two readings agree, and name the case the story leads with.
- An answer to: "Could a simpler method (for example, trimming or relabeling data after the run) produce the same result?" If yes, the contribution must say why that method does not suffice.

**Done when** a reader can say in one sentence what the paper contributes, the exemplar table names the lead case, and the simpler-method question has an answer.

**Failure.** "I can't point at what this paper is about." Go to step 6.

### 6. Choose one framing

**Purpose.** Pick the one claim the paper defends, so every section serves it.

**Request.**
> The draft tries to be several papers. Give a table of candidate framings with columns: the claim in one plain sentence, what the paper must show, what it can drop. Recommend one and say why not the others.

Then: *"Rewrite around <choice> and cut everything that serves only the other framings."*

**Output.** An identity paragraph you can point at; the terms the paper will use from now on; what the artifact is and is not. When the record holds a spoken or written restatement of the story by the advisor, seed the identity paragraph from the latest one and say where it came from (meeting and date, or document).

**Done when** a fresh reviewer restates the paper in one sentence and gets it right.

### 7. Write a Draft 0 introduction, the motivation, and the skeleton

**Purpose.** Turn the story into the structure reviewers read, in the pipeline's drafting order.

**Request.**
> Draft a Draft 0 introduction following `section_rhetorical_moves/introduction.md` and the venue notes below. Outline the motivation section (Section 2 in systems papers; the problem setup with a motivating failure case in ML papers) around one simple, real example from our data or experience; it is written in full at the Background stage, after the evaluation. Then give a skeleton of design, implementation, evaluation and related work with a page budget. Use existing notes as source material; the structure comes from the story. For every design choice, name the alternative and defend the choice from first principles or with a measurement.

If a draft already exists, add: *"Rethink the structure rather than patching the old one."*

**Output.**
- The Draft 0 introduction and the motivation outline.
- A design section organized around the one question the paper answers.
- Required whenever an earlier paper or draft exists: a ledger of its terms marked kept, refined, added or dropped, with a reason for each.
- A running example detailed enough to exercise every mechanism.
- A table mapping each supporting contribution to the line or step of the running example that exercises it. When a contribution concerns coordination or interaction, the example has at least two interacting components.
- A sketch of Figure 1: what it shows, in words.
- A claims-by-evidence grid: one row per claim, one column per kind of evidence, with cells already measured kept apart from cells still to run.
- A page map: where each section and figure lands within the limit.

**Done when** every design choice has a defense, the running example breaks no mechanism, and the ledger (when an earlier paper or draft exists), the contribution-to-example table, the Figure 1 sketch, the claims-by-evidence grid and the page map are all present.

### 8. Tie every claim to an evaluation question that could refute it

**Purpose.** Make sure each claim in the introduction has an experiment that tests it.

**Request.**
> List the questions a skeptical reviewer would ask, in the order they would ask them. For each: the contribution it tests, the metric, the success criterion set in advance, the result that would refute the claim, and the baselines (the existing approach, ablations of our design, the strongest alternative given the same budget). Add a takeaway sentence per question.

Then: *"List every requirement between <goal> and <output>. For each, the property it forces, how many cases need it, and whether a known technique meets it. Confirm that our contribution covers what is fundamental and that the rest is engineering."*

**Output.** The question list with its columns, and the requirements derivation: every requirement, the property it forces, whether a known technique meets it, and a statement that the contribution covers what is fundamental. An aim the advisor stated (for example, robustness to future tools) is argued in the design. Moving it to "not claimed" needs the advisor's agreement; ask first.

**Done when** every claim in the Draft 0 introduction maps to an experiment, no comparison is won by construction, and the requirements derivation is present.

### 9. Converge: review rounds and a calibration spec

**Purpose.** Keep the story correct while it changes, and keep every document that depends on it consistent.

**Request.**
> Run a review round: a fresh reviewer checks that each fix holds everywhere the point appears, and finds contradictions, terms used before they are defined, and success criteria that cannot decide their question. Fix, commit, and repeat until a round finds nothing critical or major.

Then: *"Write `story/spec.md`, a one-page calibration spec: the core contribution and its supports, facts to state carefully ('say X, never Y'), the evaluation questions, and the terms. Check every document against it."* After each meeting or major new result: *"Calibrate the outline and spec against <notes or results>."*

**Story documents.** For an outline or other story document (as opposed to paper prose), cap review at three rounds. Count only critical findings and majors that change a claim; minor wording does not block. The closure reviewer also confirms that the ledger of earlier terms, the contribution-to-example table, the requirements derivation and the page map are present.

**Done when** a round finds nothing critical or major (for a story document, nothing critical and no major that changes a claim, within three rounds). Before submission, have the reviewer apply the venue's published review criteria.

#### Story document gate profile

A planning document is checked with a lighter profile than paper prose:
- Coin about ten new terms at most; explain each where it first appears.
- Keep citations out of running text: use bracket keys or a sources list.
- No process asides addressed to the reader (for example, "as I read your draft").
- The Draft 0 introduction may restate later sections. Phrases repeated between the Draft 0 and the outline do not count as repetition.
- Run the mechanical tools on Markdown through the conversion your group's tooling provides, or remove tables and bullets first.

### 10. Hand off to the pipeline

**Purpose.** Start Stage 2 from the story instead of starting over.

**Request.**
> Fill `project_context.md` from `examples/project_context.md` using the story files: identity from the step 6 paragraph; contributions from step 5, with step 8's experiments as evidence and status MISSING until results exist; section architecture from the step 7 page map; locked decisions and terms from `story/spec.md`.

Stage 2 (Architecture) then refines the step 7 page map, and Stage 3 starts from the step 7 introduction as Draft 0. Contributions written with result placeholders satisfy Stage 1 until the results exist; replace each placeholder when its experiment finishes.

## Introduction structure by venue

Every venue uses the six moves in `section_rhetorical_moves/introduction.md` (Stakes, Problem Gap, Key Abstraction, Design Intuition, Contributions, Results Preview). Story mode adds two: **why it is new** (two or three mechanism-level contrasts, one sentence each) after Design Intuition, and **limits, stated once** at the end.

- **Systems and networking conferences (e.g., SIGCOMM).** Answer the Kurose questions, from Jim Kurose's advice on writing introductions, in order: why the problem matters, what the problem is, why it is hard, why existing solutions fall short, the key idea and approach, the contributions, the results, and the limits. The story arc leads with the stakes and a measured need before any mechanism. The Problem Gap names why the problem is hard (the binding constraint, often a cost) and why existing solutions fall short (what each assumes or fixes in advance). Section 2 argues the pain point in depth and maps each difficulty to a requirement and the experiment that tests it.
- **Systems venues with a deployment focus (e.g., NSDI).** The Problem Gap includes a deployment constraint; the Results Preview leads with performance against strong baselines.
- **Measurement venues (e.g., IMC).** Stakes lead with what the data reveals. The Problem Gap covers how representative the vantage point is and how much ground truth exists. Contributions are findings plus the method; limits cover bias in the data.
- **Workshops (e.g., HotNets).** Lead with the question and the observation. Contributions are an insight and a sign it is feasible; open questions replace most of the Results Preview.
- **Machine learning conferences.** The problem is a task with metrics; prior work becomes baselines tested with equal budgets; ablations and robustness checks are part of the core claim. **ML workshops:** a clear claim, one convincing experiment with its strongest baseline, and the limits stated openly.
- **Other venues.** Read two or three recent accepted papers from the venue, list the moves their introductions make in order, and adapt the six moves to match.

## Failure modes

| Symptom | How to catch it | What to say |
|---|---|---|
| Story built from one source | The digest cites one document | "Read <repo, earlier notes, code>; what contradicts this?" |
| Claims taken from abstracts or READMEs | Ask where each claim came from | "Show the quote or the code line." |
| Over-reaction to a loose resemblance | "Already done" with no defense written | "Argue it both ways. Could they use it as it stands?" |
| Novelty as a checklist table | Rows of checkmarks | "Give two or three differences in mechanism." |
| Several papers in one | You cannot state the paper in a sentence | "Table of framings; pick one; cut the rest." |
| A hook the paper never pays off | The introduction promises what the evaluation never tests | "Tie it to an experiment or drop it." |
| An evaluation ours must win | Inputs or metrics favor our method | "Same inputs and budget for every method; make it able to fail." |
| Drift from the record | The outline overrides the code and the notes | "Rebuild from the record with a source for each claim." |
| Patching | New text beside old text; it reads worse | "Rethink the structure rather than patching it." |
| Generic AI prose or jargon | Slogans, repeated cadence, coined labels | "Plain words; explain each term where it first appears." |
| A late rename | A settled term changes | "Keep the settled terms; fix the explanation." |
| Lossy compression | Half a claim disappears | "Show before and after, sentence by sentence." |
| "Done" without checking | A figure does not render | "Render it and look at every figure." |

## Other skills that help

| Step | Helpful skill | Without it |
|---|---|---|
| 1 | Connectors for mail, chat and meeting notes; `git log` | Export notes to files; clone repositories locally |
| 2, 3, 5 | A brainstorming skill with independent lenses and a critic | Run three or four agents with different lenses, then one critic per question |
| 4 | A literature-survey skill (for example `/survey`) | The shared brief plus full-text PDFs |
| 6 to 8, 10 | This skill's checklists and `section_rhetorical_moves/` | The venue notes above and a claim-to-experiment table |
| Every step | A prose checker and repeated passes (`/loop`) | A fresh reviewer that lists every sentence it had to read twice |
| 9 | Git | A spec file and a commit per version |

## Keeping private work private

Story mode reads unpublished material, and its outputs describe it. Keep `story/` and `project_context.md` in your own private repository; this skill's repository and other public places must never hold them. When you share a request that worked, replace project specifics with `<placeholders>` as above.
