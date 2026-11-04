# Project Context: Tessel → Systems Venue

<!-- FULLY SYNTHETIC EXAMPLE. Tessel, its traces, baselines, and every number below are invented -->
<!-- to show what a filled-in project context looks like. It follows the structure of -->
<!-- examples/project_context.md. Replace everything with your own project. -->

*Last updated: Week 6 of drafting. Incorporates decisions from the last three advisor meetings and the evaluation audit.*

## Identity

This paper shows that an edge cache network can raise its hit rate at equal capacity by placing objects from cross-site first-miss signals, by feeding those signals into a cost model that decides which sites should hold an object before it becomes popular, enabling operators to serve more requests from the edge without buying more cache.

- **Problem:** Each edge site decides what to cache from its own history. An object that is new to a site misses there even when other sites have already seen it, so every site pays the same cold misses independently.
- **60-second takeaway:** A first miss at one site is cheap evidence that nearby sites will soon see the same object; a cost model that weighs that evidence against capacity raises hit rate at little overhead.

## Venue

- **Target:** A top systems/networking conference
- **Deadline:** [fixed by the venue's call for papers]
- **Page limit:** 12 pages + references
- **Format:** Systems venue. Use `\smartparagraph{}` labels. Reviewers expect a trace-driven prototype, named baselines, and overhead numbers (CPU, memory, control-plane traffic).

## Contributions (as claims with evidence)

### Claim 1: Hit-rate gain at equal capacity (§5.2)
At equal cache capacity per site, Tessel raises the network-wide hit rate from N% to M% over the best per-site policy, and cuts p99 object fetch latency by K ms.
- **Evidence:** Hit rate vs. cache capacity per policy (Figure 5); p50 and p99 fetch latency per policy (Table 2).
- **Dataset:** Trace A (a public CDN request trace, 30 days) and Trace B (a synthetic trace generated from a published video-service workload model).
- **Status:** STRONG on Trace A. Trace B replay is running.

### Claim 2: Low placement overhead (§5.4)
The placement controller adds under N% CPU per edge node and M KB/s of control-plane traffic per site, and makes each placement decision in under K ms at p99.
- **Evidence:** Overhead table (Table 4); decision-latency CDF (Figure 7).
- **Dataset:** Trace A replayed on the prototype across N emulated sites.
- **Status:** MISSING. Must exist before submission; systems reviewers will ask.

## Narrative Spine

Two claims, in order: Tessel serves more requests from the edge at the same capacity (Claim 1), and it does so cheaply enough to deploy (Claim 2). §2 shows why per-site policies leave hits on the table; §3–§4 build the mechanism; §5 tests the two claims.

## Framing

- **Paper type:** Systems (the contribution is what we built), with a short measurement section that motivates it.
- **Closest competing works:**
  1. Per-site replacement policies (LRU, LFU, ARC): structurally different because each site learns only from its own misses.
  2. Cooperative caching (sites fetch from peers on a miss): structurally different because it shares cached copies after the miss, not placement decisions before it.
  3. Static replication by region: structurally different because it assigns objects once and does not react to which sites are seeing them.

## Section Architecture

| Section | Pages | Key Claim | Figures/Tables |
|---------|-------|-----------|----------------|
| §1 Introduction | 1.5 | Both claims, headline number in paragraph 4 | Figure 1 (per-site vs. cross-site placement) |
| §2 Background + Motivation | 1.5 | Repeated cold misses across sites | Figure 2, Table 1 |
| §3 Cost Model | 2 | Why first-miss signals justify placement | Figure 3 (cost model inputs) |
| §4 Tessel Design | 2 | Controller, signal exchange, placement | Figure 4 (architecture) |
| §5 Evaluation | 3.5 | Claims 1–2 | Figures 5–7, Tables 2–4 |
| §6 Related Work | 1 | Positioning (three categories above) | — |
| §7 Conclusion | 0.5 | Synthesis | — |

## Locked Decisions

1. **Baselines run at equal cache capacity.** Any result without matched capacity goes to the appendix or is cut. (Advisor, Week 3.)
2. **Label the synthetic trace as synthetic.** Trace B is called "synthetic" in every caption and paragraph where it appears; never imply it is production data.
3. **Report p50 and p99, not only means.** Every latency number in the paper comes with both percentiles.
4. **The cost model's weights are fixed before the evaluation runs.** No per-trace tuning; the values are stated once in §3.
5. **The prototype, not the simulator, produces the overhead numbers.** Simulator results may support Claim 1 only.

## Open Questions

1. **Capacity sweep range:** Do we report the full hit-rate-vs-capacity curve, or three operating points? (Student A to propose; advisor to decide.)
2. **Trace B fidelity:** Does a reviewer accept a synthetic trace as confirmation? A co-author suggests replacing it with a second public trace if one exists.
3. **Site count:** Is N emulated sites enough to argue the control-plane cost scales, or do we need a scaling experiment?

## Key Figures Needed

### Non-Data Figures (architecture, pipeline, concept diagrams)

1. **Per-site vs. cross-site placement** — §1
   - **Archetype:** comparison_schematic
   - **Shows:** under per-site policies every site pays its own cold miss; under Tessel one site's first miss places the object at its neighbours.
   - **Claim supported:** Claim 1
2. **Cost model inputs** — §3
   - **Archetype:** concept_illustration
   - **Shows:** first-miss signals, object size, and remaining capacity combine into a per-site placement score.
   - **Claim supported:** Claim 1
3. **Tessel architecture** — §4
   - **Archetype:** architecture_overview
   - **Shows:** edge sites → first-miss reports → placement controller → placement orders back to sites.
   - **Claim supported:** Claims 1 and 2

### Data Figures (plots from experimental results)

1. **Repeated cold misses across sites** — §2
   - **Shows:** most misses at a site are for objects another site has already requested.
   - **Data source:** Trace A, Trace B
2. **Hit rate vs. cache capacity** — §5.2
   - **Shows:** Tessel's hit rate exceeds every per-site policy at each capacity point.
   - **Data source:** trace-driven simulator on Trace A and Trace B
3. **Placement decision latency (CDF)** — §5.4
   - **Shows:** p50 and p99 time from a first-miss report to a placement order.
   - **Data source:** prototype on Trace A

## Timeline

| Date | Milestone | Status |
|------|-----------|--------|
| Week 6 | Capacity sweep done; Figure 5 drafted | IN PROGRESS |
| Week 7 | Trace B replay; Table 2 complete | PENDING |
| Week 7 | Prototype overhead runs; Table 4 | BLOCKED on Open Question 3 |
| Week 8 | Full draft; advisor read | PENDING |
| Week 9 | Red-team pass and closure gate | PENDING |
| Deadline | Submission | |

## Writing Constraints (DO NOT VIOLATE)

- Mean sentence length: ~21 words.
- Topic sentences assert claims. Never open a paragraph with background.
- Zero hedging except where `gate_semantic.md` S18 requires it.
- Headings are claims: "Cross-site placement raises hit rate at equal capacity," not "Evaluation Results."
- Name things precisely: "the Tessel placement controller," not "our system."
- Justify every design choice in the same paragraph that introduces it.
