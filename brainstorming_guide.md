# Structured Brainstorming: From Unstructured Ideas to a Precise Project Context

## Why This Exists

The biggest obstacle for students isn't writing skill — it's that their ideas live as unstructured intuitions. They know *something* is interesting but can't articulate *what* or *why*. This guide walks through a series of pointed questions that force clarity. By the end, you'll have a `project_context.md` precise enough to drive every section of your paper.

**How to use this**: Claude will walk you through these questions interactively when you invoke `/paper-writing` on a new project. Answer honestly — "I don't know" is a valid and useful answer. The questions are designed to reveal gaps early, when they're cheap to fix.

---

## Phase 1: Problem Discovery (Before You Think About Your Solution)

Most students skip straight to describing their system. That's backwards. Reviewers care about the problem first. If the problem isn't compelling, nothing else matters.

### 1.1 The Domain Problem

1. **Who is suffering?** Name specific actors — not "users" but "network operators at Tier-1 ISPs" or "ML practitioners training models on heterogeneous clusters." If you can't name the actor, you don't know who your work helps.

2. **What breaks for them today?** Describe the concrete failure mode. Not "things are slow" but "operators must manually inspect 10,000+ time series daily to detect anomalies, and 60% are false positives." Ground it in observable behavior.

3. **Why does it break?** This is the structural limitation — the root cause that can't be fixed by just "doing more" of the current approach. Is it a fundamental assumption that doesn't hold? A missing abstraction? An architectural mismatch? If the answer is "they just need more data/compute/effort," you may not have a research contribution — you have an engineering task.

4. **What happens if nobody solves this?** What's the cost of the status quo? Dollar amounts, user impact, operational burden, scientific dead-ends — any concrete consequence. This becomes your opening paragraph's stakes.

### 1.2 The Intellectual Gap

5. **What do existing approaches assume that doesn't hold?** Name 2-3 specific systems or methods. For each, identify the assumption they make that fails in your setting. Example: "Chronos2 assumes stationarity, which fails on bursty telemetry where 90% of timestamps are zero." This becomes your Problem Gap (Introduction Move 2).

6. **Is the limitation STRUCTURAL or QUANTITATIVE?** Structural: "Linear models can't capture event-triggered dynamics." Quantitative: "Existing models only achieve 80% accuracy." Structural limitations motivate new approaches. Quantitative gaps motivate more experiments. If your gap is only quantitative, your paper will face "incremental" criticism.

7. **Can you draw a 2×2 matrix of existing approaches?** Put approach categories on one axis and capabilities on the other. Where is the empty quadrant? That's your paper's territory. If there's no empty quadrant, your contribution needs to be something other than a new approach (maybe a new understanding, a new benchmark, or a unifying framework).

---

## Phase 2: Contribution Crystallization

### 2.1 The Core Claim

8. **Complete this sentence: "This paper shows that ______."** Not "proposes" or "introduces" — *shows*. The verb forces you to state a finding, not an activity. If you can't complete it, you don't yet know your contribution.

9. **What is the ONE thing a reader should remember after skimming for 60 seconds?** If your answer is a list, you haven't found the core insight yet. Boil it down to one sentence. This becomes your key abstraction (Introduction Move 3).

10. **Can you name your core idea in 2-4 words?** A compound noun phrase that captures the intellectual contribution. Examples of the pattern: "event-centric decomposition," "progressive disaggregation," "intrinsic evaluation framework." If you can't name it, it isn't crisp enough yet. The name will evolve — but having even a placeholder forces precision.

### 2.2 Evidence Mapping

11. **For each contribution, what is the specific evidence?** Fill in this table:

| Claim | Evidence type (figure/table/experiment) | Status (have it / running / planned / missing) |
|-------|----------------------------------------|-----------------------------------------------|

If a claim has no evidence entry, it's an aspiration, not a contribution. Move it to "Open Questions."

12. **What is your headline number?** The single most impressive quantitative result. "13× error reduction," "44% lower latency," "2× faster search." This goes in the introduction's Results Preview and the abstract. If you don't have one yet, which experiment would produce it?

13. **What CAN'T your system do?** Every system has limitations. Name them now. Honest limitations in the paper build reviewer trust. Hidden limitations get discovered during review and destroy trust.

---

## Phase 3: Evaluation Design (Before Writing Anything)

This is the most important phase. The evaluation determines what claims you can make. Design it before writing a single section.

### 3.1 Experimental Setup

14. **What are your baselines, by name?** Not "prior work" or "state-of-the-art." Specific system names. For each baseline, explain WHY it's the right comparison — what does beating this baseline prove?

15. **What are your metrics, and why these specific ones?** For each metric: what aspect of your claim does it measure? Are there metrics that would make your system look BAD? If so, should you report them anyway? (Usually yes — it builds trust.)

16. **What datasets will you use?** For each dataset: what makes it appropriate for your claims? Is it publicly available? If not, how will reviewers verify your results? Are there dataset characteristics (size, distribution, domain) that affect generalizability?

### 3.2 Experiment Planning

17. **What experiments do you need, and what does each one prove?** Map experiments to claims:

| Experiment | Tests claim | Expected result | What if it fails? |
|-----------|-------------|-----------------|-------------------|

The "what if it fails" column is critical. If an experiment could fail and you have no backup, that claim is at risk.

18. **Do you have an ablation plan?** Which components of your system can be removed or varied? For each: what does the ablation show? If removing a component doesn't hurt performance, you need to either justify its inclusion or remove it.

19. **What's the "honest disaggregation"?** Where does your system work BEST and WORST? Breaking results down by meaningful dimensions (difficulty level, data characteristics, edge cases) shows you understand your system. Uniform improvement everywhere is suspicious; nuanced results are convincing.

20. **Do you have a robustness/generalization story?** Does your system work beyond the specific evaluation setup? New data? New domains? Different parameters? Temporal shift? If not, reviewers will ask "does this generalize?" — have an answer ready.

---

## Phase 4: Positioning and Framing

### 4.1 Venue Fit

21. **What venue are you targeting?** What is the page limit, deadline, and review process?

22. **Is this a SYSTEMS paper or a MEASUREMENT/ANALYSIS paper?** Systems papers: the contribution is what you built and what it enables. Measurement papers: the contribution is what you found and what it means. The framing is completely different. Systems papers need more "why this design" arguments. Measurement papers need more "so what" arguments.

23. **Read the last 3 accepted papers in your topic area at this venue. What do they have in common?** How long are their evaluations? How many baselines? Do they have systems benchmarks (latency, memory)? Do they have user studies? This calibrates your evaluation ambition.

### 4.2 Competitive Positioning

24. **Name the 2-3 closest competing works. For each, answer:** (a) What is the structural difference between their approach and yours? (b) Is there a scenario where their approach is better than yours? (c) If a reviewer says "how is this different from X?" — what is your one-sentence answer?

25. **Are you creating a new category or competing in an existing one?** If competing: you need to beat the best in that category convincingly. If creating: you need to define the category clearly and show why existing categories don't cover it. The positioning sentence in your Related Work section depends on this answer.

---

## Phase 5: Architecture and Constraints

### 5.1 Design Decisions

26. **What is the high-level architecture?** Describe it as a pipeline: "Input → [Stage 1] → [Stage 2] → ... → Output." Name each stage. These names become the vocabulary of your paper.

27. **For each major design choice, why this and not the obvious alternative?** The "obvious alternative" is whatever a reviewer would try first. If you can't articulate why the simple approach fails, reviewers will wonder why you didn't just do the simple thing.

28. **What is the key "knob" in your system?** The parameter or trade-off that the user controls. Its existence shows the system isn't one-size-fits-all. If there's no knob, is your system really general enough?

### 5.2 Scope and Constraints

29. **What are the locked decisions — things NOT up for debate?** Architecture choices, evaluation setup, dataset selection, venue — anything that's final. Write them down. These prevent revisiting settled questions during writing.

30. **What are the genuinely open questions?** Things you don't know yet. Experimental results pending, design alternatives still being explored, framing you're unsure about. Separating "locked" from "open" prevents wasted effort.

31. **What is your paper's section architecture?** Fill in:

| Section | Target pages | Key claim | Figures/Tables needed |
|---------|-------------|-----------|----------------------|

---

## Phase 6: Narrative Spine

### 6.1 The Story

32. **What is the narrative arc?** Every good paper tells a story: "The world has problem X → Existing approaches fail because Y → We observe insight Z → This leads to system W → W achieves [results]." Write yours in 3-4 sentences.

33. **What is the "inevitable" moment?** The point where your design feels like the only reasonable response to the problem. This comes from showing that the obvious approach fails (Design Move 2). What negative result or failed experiment motivates your approach?

34. **If your paper were a tweet, what would it say?** Force yourself into ~280 characters. This is your elevator pitch, your abstract's first sentence, and the thing reviewers will remember.

---

## After Brainstorming: What Comes Next

Once you've answered these questions, Claude will generate a `project_context.md` file that captures:

- Your identity sentence (from Q8-10)
- Your venue and constraints (from Q21-22, Q29-30)
- Your contributions as claims with evidence pointers (from Q11-12)
- Your evaluation plan (from Q14-20)
- Your competitive positioning (from Q24-25)
- Your section architecture (from Q31)
- Your locked decisions and open questions (from Q29-30)
- Your key figures needed (from Q31)

This `project_context.md` becomes the binding contract for all writing sessions. Every time you invoke `/paper-writing`, Claude reads it and holds you to it. If something changes (new results, revised framing, shifted venue), update the context file — don't just tell Claude in conversation.

See `examples/netburst_project_context.md` for what a complete, real project context looks like.
