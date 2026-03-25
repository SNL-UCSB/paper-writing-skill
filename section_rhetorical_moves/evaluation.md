# Writing an Evaluation: The 6-Move Sequence

## Overview

The evaluation is where the paper earns its claims. Student drafts typically use only Move 2 (head-to-head comparison). Adding Moves 3–6 is what transforms a lab notebook into a conference paper.

---

## Move 1: Setup Anchoring

**Function**: Establish dataset, baselines, and metrics in compact form.

**How to do it**: Use labeled paragraphs (Datasets / Baselines / Metrics) — one paragraph each. Compress ruthlessly: just enough for reproducibility, not a technical report.

**Common mistake**: The setup section is bloated with hardware specs and software versions. In final versions, this is a few labeled blocks, not a multi-page section.

---

## Move 2: Head-to-Head Comparison

**Function**: Compare the proposed system directly against named baselines.

**How to do it**: Baselines must be NAMED (not "prior work" or "state-of-the-art"). Tables and figures are dense here. The comparison uses the exact metrics defined in setup.

**This is the core evidence.** If your head-to-head doesn't support a claim from the introduction, either the claim or the experiment needs to change.

---

## Move 3: Deep Dive / Disaggregation

**Function**: Break down results by meaningful dimensions to show WHO benefits and WHEN.

**How to do it**: Disaggregate by relevant conditions (e.g., by difficulty level, data characteristics, geographic region, user type). Show where the system helps MOST and LEAST.

**Why it matters**: Honest disaggregation actually STRENGTHENS the paper by showing nuanced understanding. Reviewers trust authors who acknowledge their system doesn't uniformly excel.

---

## Move 4: Takeaway Synthesis

**Function**: After each experiment cluster, state what was learned.

**How to do it**: An explicit "Takeaway." paragraph that states the IMPLICATION, not just the result. The Takeaway ties results back to a specific design claim or contribution.

**This is the most important advisor convention.** It is absent from all student drafts and present in all accepted papers. The Takeaway is where the author controls the reader's interpretation.

**Test**: Is the Takeaway interpretable without reading the detailed results? Does it tie back to a contribution?

---

## Move 5: Ablation / Sensitivity

**Function**: Show which design choices matter and which don't.

**How to do it**: Test the system with components removed or varied. Show each component contributes. This answers "is your design over-engineered?"

**Warning**: If an ablation variant outperforms the full system, you MUST acknowledge and explain it. Hiding this will be caught by reviewers.

---

## Move 6: Robustness / Generalization

**Function**: Show the system works beyond the specific evaluation setup.

**How to do it**: Test on conditions not in the training set. Common sub-moves: temporal generalization (does it work on new data?), spatial generalization (does it work in new contexts?), computational overhead (is it practical?).

---

## The Evaluation Evolution Pattern

Every evaluation goes through three stages:

| Stage | Character | What it looks like |
|-------|-----------|-------------------|
| v1: Lab notebook | Exploratory, question-based headings | "Can we explain?" / "What happens if...?" |
| v2: Technical report | Comprehensive, infrastructure-heavy | 30+ subsections, everything included |
| v3: Conference paper | Compressed, narrative-driven | Labeled paragraphs + Takeaways, claims-first headings |

Target: Stage v3. Get there by writing comprehensively first (v2), then compressing with Takeaways (v3).

---

## The Monolithic-to-Split Decision

**When to split your evaluation**: If a single evaluation section answers two fundamentally different questions (e.g., "Does the system work correctly?" and "Does it improve downstream performance?"), split it. The questions require different baselines and metrics — combining them confuses the narrative.

**Decision rule**: If your evaluation subsections don't share baselines or metrics, they should probably be separate sections.

---

## The Evaluation Maturity Test

Rate your evaluation:

| Level | Description | Missing element |
|-------|-------------|----------------|
| 1 | Lab notebook | Results without interpretation |
| 2 | Technical report | Comprehensive but unfocused |
| 3 | Conference paper | Results organized by claims |
| 4 | Systems narrative | Labeled paragraphs + Takeaways + claim-first headings |

Target: Level 4.
