# The Story Document: One Page That Carries the Paper's Argument

Story mode (`story_mode.md`) produces working files: a digest, an outline, a spec. Co-authors and advisors need one more thing, a **story document**. It is a single page a co-author can read in twenty minutes and come away with the paper's argument, the problem stated precisely, the evidence, and what remains to be learned. It is written in the paper's section order, so it becomes the skeleton of the paper.

This file gives its template, the rules that make it readable, and the failures it guards against. It applies in step 7 (structure), step 8 (evaluation plan) and step 9 (review) of story mode. Build it after the digest and the novelty verdicts exist, and rebuild it, rather than patch it, when the framing changes.

Every example below is synthetic.

## Who it is for

Write for a reader with basic familiarity with the field (for a networking paper: IP links, routers, loss and latency) who has not followed the project. Assume nothing about the domain beyond that, and nothing about internal tools or names. If a co-author from an adjacent area cannot follow it, it is not done.

## The template

### Front matter (before Section 1)

1. **Before reading: key terms.** A table of at most fifteen terms, each defined in plain words. Every acronym, internal tool and coined term the page uses appears here or is defined at first use.
2. **The argument.** One figure that shows the whole argument (problem → why hard → approach → result needed, plus the catches that decide whether it holds). Then the argument as eight to ten numbered steps, from the need to the evidence. Each step is one or two sentences and points to the section that supports it.
   - Example step: *"Gap. Earlier schedulers either fix the batch size before the job starts or tune it per job by hand; none adapts it to queue depth while the job runs (§7)."*
3. **The contribution.** One core contribution in one sentence, the goal it serves, and a **roadblock table**:

   | Roadblock | Why it blocks the goal | How it is handled today | How we remove it | Evidence today |
   |---|---|---|---|---|

   The last column is honest: "measured (§4.1)", "anecdote only", "not yet tested".
4. **The novelty test.** One question a reviewer will ask, of the form *"What would the strongest simple baseline miss that we catch?"*, and the current answer, including "no case yet" when that is true.

### Section 1: Introduction, by the Kurose questions

Use one subheading per question, in this order:
1. Why it matters.
2. The problem.
3. Why it is hard.
4. Why existing solutions fall short.
5. Key idea.
6. Approach.
7. Contributions.
8. Results (placeholders until measured).
9. Limits.

Each answer is a short paragraph or a few bullets. The page's later sections expand these answers. They do not introduce new claims.

### Section 2: Background and motivation, with the core abstraction

- **2.1 The system as the reader needs it.** Explain the domain from the reader's level upward, with one figure. Example: *how a request travels through the cache tiers, drawn as a stack from the client down to the storage node.*
- **2.2 How the problem is handled today.** The current workflow as a figure, with where time or cost is lost marked on it.
- **2.3 The abstraction.** The object the paper reasons about, defined plainly first and formally second, with a figure. Example: *"a shared group: the set of tenants whose requests land on the same storage shard, together with the share of each tenant's traffic it serves."* Show the same data seen without and with the abstraction, so the reader sees what it reveals.
- **2.4 Requirements.** A table: each requirement, the difficulty it answers, and the evaluation question that tests it.

### Section 3: Problem specification

This is the section that is most often missing. Write it as a specification, not a description. For a learning or inference problem it must state:

| Item | What to write |
|---|---|
| The unit | What gets a verdict or a score (one request, one host, one group), and why that unit and not another |
| The questions | Each task the paper solves, as a plain question. Keep diagnosis (after the event), prediction (before it) and measurement (rates in the population) separate; they need different data |
| Input | Exactly which signals, over which window, at which granularity |
| Output | What the system returns, in operator terms |
| Ground truth | Where the correct answer comes from, for each task |
| How the data is generated | What causes a record to exist, drawn as a figure. Which cases leave no record at all |
| Blind spot of each record | What that source of truth can never contain |
| Rules for learning from it | What counts as negative, what counts as unknown, how accuracy is measured without trusting the biased source |
| Assumptions | Each assumption stated, with the measurement that tests it |

Put the tasks on a timeline figure: input window, the event, the look-ahead window, and where the ground truth comes from.

If the specification needs symbols, define each in words in the same sentence. A table of symbols with no prose is not a specification a co-author can check.

### Section 4: Empirical motivation, anecdotes first, then trends

- **4.1, 4.2 Anecdotes.** One or two exemplary incidents or cases, each told as a short story with a figure (a timeline, a trace). Each ends with a **takeaway** and a **what it does not show** line. The second line keeps the anecdote from claiming more than it can.
- **4.3 From anecdotes to trends.** A table of the general trends the paper must establish, so the anecdotes are shown to be common and costly:

  | Trend | Why the story needs it | Known today | Data needed | Status |
  |---|---|---|---|---|

  Example row: *"How often a hot shard serves more than half of one tenant's traffic | shows the anecdote is not a one-off | 3 of 40 tenants in one region | the same check across all regions | partial."*

### Section 5: Design

Components as a numbered list, then **why this way and not something else**: a figure that maps where each alternative works (for example, a 2×2 of fault type against unit judged), and a table of alternatives with what each does well and where it stops. The strongest simple baseline appears here by name.

### Section 6: Evaluation plan

The evaluation plan has four parts, and each one is required:
1. **Questions.** A numbered list (Q1 to Qn), each answerable and each tied to a contribution.
2. **Setup: what exists today.** A data inventory: each dataset or system, its detail, its history or scale, and its status (available, unknown, to be built). Mark unknowns in bold.
3. **Per question: available against needed.**

   | Q | Compared against | Available now | Additional data or insight needed | Would change the claim |
   |---|---|---|---|---|

   The fourth column is the work plan: it names the data and the analyses that must come in to answer the question. The fifth column is the refutation, with a pass bar set in advance. Mark a bar you proposed, rather than one the team agreed, as "proposed".
4. **Expected outcomes and the stop rule.** What the team expects, where the paper's weight rests, and what the paper becomes if the riskiest question fails.

### Sections 7 and 8: Related work and discussion

- **Related work** is a table: line of work, closest papers, difference. It is followed by the novelty verdicts from story mode step 5 in plain words: *not new on its own*, *method not new but the finding is*, *new if shown*, *a new measurement*.
- **Discussion** holds what the main sections should not: the different views among co-authors (a figure helps), how each view moved, and where each person's points landed.

### For the authors (end of the page)

- The plan for the next meeting: goal, the questions to ask in order, the table the student fills, and deliverables with dates.
- **What each claim rests on today:** claim, evidence, status. This keeps the page honest as it ages.

## Rules that make it readable

1. **A figure for every main point.** At least these: the argument (front matter), the system (2.1), today's workflow (2.2), the abstraction (2.3), the tasks on a timeline (3), how records are generated (3), each anecdote (4), and where alternatives work (5). Each figure has a one-sentence caption that states its point.
2. **Plain words over internal names.** Use "the automated alarm system", not its internal codename; "the flooded building", not its site code. Keep an internal name only when the reader must look it up, and define it once.
3. **No labels the reader must decode.** Do not write "T2", "R/B/O", "N3" or "§6.3 C2" in running text. Name the thing. Labels may appear only in a table's first column, next to the full name.
4. **Every number carries its source and its scope.** Write "one incident", "two neighborhoods", or "<person>, meeting of <date>". Write "measured" only for measurements.
5. **Separate overlapping events before writing about them.** When a source describes two events close in time, list each with its own timestamps before drawing a conclusion. Merging two faults into one is a classic, credibility-ending misreading.
6. **Every lead-time or speedup number names its start and end points.** Example: "22 minutes from the moment the rule fires to the outage".
7. **Proposed thresholds are labeled as proposed.**
8. **Internal pages stay private.** A page carrying unpublished numbers or partner names is published privately and says so in its header.

## Review checklist (step 9)

A fresh reviewer, given only the sources and the page, checks:
- [ ] A reader new to the project can restate the problem, the unit, and the core contribution after the front matter.
- [ ] Section 3 states unit, input, output, ground truth, how records are generated, and each record's blind spot.
- [ ] Each anecdote has a takeaway and a "does not show" line; the trend table exists.
- [ ] Each evaluation question has an "available now" and a "needed" entry and a refutation with a bar.
- [ ] Every number matches its source; overlapping events are kept apart.
- [ ] Every person's quoted position matches the record (no strawman of a co-author's view).
- [ ] No undefined acronym, codename or decode-me label in running text.
- [ ] Every figure renders and is read: open the page and look at each one.

## Failure modes

| Symptom | Cause | Fix |
|---|---|---|
| "Cryptic and hard to parse" | Working files pasted into a page; symbols and labels without prose | Rebuild from the template; define in words; add figures |
| "Missing a coherent story" | Sections follow the order the work happened, not the paper's order | Front matter, then Sections 1 to 8 in paper order |
| A problem statement that cannot be checked | No unit, no ground truth, no blind spots | Fill the Section 3 table |
| An anecdote treated as a finding | No trend table | Add 4.3 with the data each trend needs |
| An evaluation plan that is a list of metrics | No setup, no available-versus-needed | Fill the Section 6 tables |
| A wrong headline number | Two events merged, or a lead time with no named start | Rules 5 and 6; re-read the source timeline |
| Co-authors' views flattened | Paraphrase instead of quotes | Quote each view with date and source |
