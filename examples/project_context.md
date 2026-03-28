# Project Context: [Paper Name]

<!-- This template maps to the Brain Dump questions in the skill. -->
<!-- Fill this in by running /paper-writing and answering the Brain Dump prompts, -->
<!-- or copy this file into your paper directory and fill it in manually. -->
<!-- See examples/netburst_project_context.md for a real, complete example. -->

## Identity

<!-- Round 1, Question 1: One sentence. Use the template: -->
<!-- "This paper shows that [X] by [Y], enabling [Z]." -->

[Your identity sentence here]

<!-- Round 1, Question 2: What PROBLEM does this solve? Who is affected? -->

<!-- Round 1, Question 3: What should a reader remember after 60 seconds? -->

## Venue

- **Target:** [e.g., SIGCOMM 2027]
- **Deadline:** [e.g., January 31, 2027]
- **Page limit:** [e.g., 12 pages + references]
- **Format:** [Systems venue / ML venue / Workshop. Note venue conventions.]

## Contributions (as claims with evidence)

<!-- Round 2: Write each as a RESULT, not an action. -->
<!-- "We show that X" not "We propose X." -->

### Claim 1: [Section reference — e.g., §4.2]
[State the claim with a specific number or concrete finding]
- **Evidence:** [Figure/table/experiment that proves it]
- **Dataset:** [What data supports this]
- **Status:** [STRONG / NEEDS WORK / MISSING]

### Claim 2: [Section reference]
[...]
- **Evidence:** [...]
- **Dataset:** [...]
- **Status:** [...]

### Claim 3: [Section reference]
[...]

## Framing

<!-- Round 3: Venue and competitive positioning -->

- **Paper type:** [Systems (contribution = what you built) / Measurement (contribution = what you found)]
- **Closest competing works:**
  1. [Work A] — structurally different because [...]
  2. [Work B] — structurally different because [...]
  3. [Work C] — structurally different because [...]

## Section Architecture

<!-- Round 4, Question 10: Evaluation plan first, then the rest. -->

| Section | Pages | Key Claim | Figures/Tables |
|---------|-------|-----------|----------------|
| §1 Introduction | | All contributions | |
| §2 Background | | Problem definition | |
| §3 Design | | Contribution 1-2 | |
| §4 Evaluation | | Contribution 1-3 | |
| §5 Related Work | | Positioning | |
| §6 Conclusion | | Synthesis | |

## Locked Decisions

<!-- Round 4, Question 11: Things NOT up for debate. -->

1. [...]
2. [...]
3. [...]

## Open Questions

<!-- Round 4, Question 12: Things you genuinely don't know yet. -->

1. [...]
2. [...]

## Key Figures Needed

<!-- Classify each figure as DATA or NON-DATA. -->
<!-- DATA figures (CDFs, scatter plots, bar charts, heatmaps) → /viz skill. -->
<!-- NON-DATA figures (architecture, pipeline, concept diagrams) → /paper-writing figure synthesis. -->
<!-- The boundary: if it requires experimental data to render, it's DATA. -->
<!-- If it illustrates structure, flow, or concepts, it's NON-DATA. -->

### Non-Data Figures (architecture, pipeline, concept diagrams)

<!-- For each: name, section, archetype, what it shows, which claim it supports. -->
<!-- Archetypes: architecture_overview | pipeline_flow | component_detail | -->
<!-- concept_illustration | comparison_schematic | taxonomy | deployment_diagram -->

1. **[Figure name]** — §[section]
   - **Archetype:** [archetype]
   - **Shows:** [what the reader should understand]
   - **Claim supported:** [which contribution claim]

### Data Figures (plots from experimental results)

<!-- For each: name, section, data source, what it shows. -->
<!-- These go through /viz — brainstorm → plan → execute → analyze. -->

1. **[Figure name]** — §[section]
   - **Shows:** [what the reader should understand]
   - **Data source:** [which dataset/experiment]

## Timeline

| Date | Milestone |
|------|-----------|
| [Date] | [What should be done] |
| [Date] | [What should be done] |
| [Deadline] | Submission |
