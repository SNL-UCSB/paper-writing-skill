# Project Context: NetBurst → NSDI '27

*Last updated: March 27, 2026 — incorporates March 27 §4 structural audit decisions plus all March 14–24 meeting decisions, Slack discussions, and meeting transcripts.*

## Identity

NetBurst learns event-centric representations of sparse-bursty network telemetry that enable accurate forecasting, interpretable characterization, and efficient similarity search — the three queries every network operator asks about their traffic: "What will happen next?", "What is this behavior?", and "When have we seen this before?"

## Venue
- **Target:** NSDI 2027 (Spring)
- **Deadline:** Mid-April 2026
- **Page limit:** 14 pages + references (USENIX format)
- **Format:** Systems venue. Use \smartparagraph{} labels. Expect systems evaluation (latency, throughput, memory). Reviewers will check for deployed-system baselines, not just ML baselines.

## Contributions (as claims with evidence)

### Claim 1: Forecasting (§4.2 — "What will happen next?")
Event-centric decomposition reduces forecasting error on sparse-bursty telemetry compared to all baselines. **Numbers updated March 27 from current master table:** On PINOT service-level, NetBurst MAPE=0.394 vs. Chronos 18.76 (47.6×), Chronos2 1.06 (2.7×), Lag-Llama 0.93 (2.4×), DeepAR 1.00 (2.5×). On MAWI service-level, NetBurst MAPE=0.038 vs. DeepAR 0.999 (26×). **The old "13–605×" claim is STALE — prose was from an earlier run.** Actual headline range: ~2.4–47× on PINOT service, ~26× on MAWI service. Wasserstein distance preserves distributional fidelity where point-wise metrics fail.
- **Evidence:** MAPE comparison table (telemetry datasets only — benchmarks removed), 3-panel PINOT scatter figure, per-entity CDF, Wasserstein distance.
- **Dataset:** PINOT IP dataset (Sanjay's collection) for pretraining and forecasting evaluation. MAWI, Stardust, PINOT-Ping for cross-dataset confirmation.
- **Status:** STRONG but STALE PROSE. All prose multiplier claims must be recalculated from current Table 3. Stardust IP-level results still missing (blocking). Needs reframing from "better forecaster" to "forecasting as downstream task enabled by representations."

### Claim 2: Characterization (§4.3 — "What is this behavior?") — PROMOTED TO CENTRAL PILLAR
NetBurst embeddings cluster into semantically meaningful behavioral regimes that statistical feature extraction (TSFresh) cannot distinguish. At K=500, NetBurst achieves H_norm=0.993, Calinski-Harabasz=335, zero degenerate clusters — compared to Chronos2 (H_norm=0.943, CH=193) and TSFresh (H_norm=0.853, CH=48).

The headline finding: time series pairs with >0.997 cosine similarity in TSFresh's 770-dimensional feature space — statistically near-identical by TSFresh — are correctly separated by NetBurst into distinct clusters because NetBurst captures subtler structural properties (absolute_sum_of_changes, Lempel-Ziv complexity, AR coefficients) that are washed out by TSFresh's high-magnitude features (mean, sum, quantiles).

- **Evidence (confirmed March 24):**
  - Three-way comparison table: NetBurst vs. Chronos2 vs. TSFresh on H_norm, CH, cluster size CDFs at K=100/500/1000
  - TSFresh cosine similarity trap examples: clusters 322 vs. 289 (0.997 cosine similarity but visually and structurally distinct)
  - Feature heatmap: dual-encoded (color=z-scored mean, opacity=within-cluster variance) showing multiple feature families jointly define cluster structure
  - Recurring discriminative features: quantiles (q_0.2, q_0.9), skewness, c3_lag_2 (nonlinear temporal dependence), Lempel-Ziv complexity, sparsity (value_count_0), local trend intercepts
- **Dataset:** NetReplica /32 IP data (~54K time series for twinhead).
- **Status:** STRONG. This is now the paper's most distinctive contribution. The 0.997 cosine similarity example is the §2 motivation AND the §4.3 headline result.

### Claim 3: Semantic Search (§4.4 — "When have we seen this before?")
NetBurst enables 8.2× faster embedding computation (3,042 vs. 369 series/sec), 2× faster search (3.2s vs. 6.3s for 10K × top-10 retrievals), and 44% lower Wasserstein distance (0.021 vs. 0.037) compared to Chronos2 in a Milvus-based vector similarity search pipeline.

**CRITICAL REFRAMING (March 24, Walter's insight):** §4.4 is an *efficiency* story, not a *quality* story. The quality of the clusters is established in §4.3. Once cluster quality is validated, search inherits that quality. §4.4 argues: NetBurst's balanced clusters (uniform sizes from high H_norm) → smaller per-cluster search spaces → faster retrieval. This is a systems argument, not an ML argument.

- **Evidence:** Embedding throughput, search latency, Wasserstein distance (normalized and unnormalized), cluster size CDFs.
- **Dataset:** NetReplica (~180K points; 170K indexed in Milvus, 10K evaluation set).
- **Open issue (March 24):** Walter flags that Wasserstein captures distributional similarity but misses temporal structure. DTW needed as complementary metric. Also: normalized vs. unnormalized Wasserstein conflates shape with scale. May not use any similarity metric in this section — pure efficiency argument.
- **Status:** IMPLEMENTED. Results in hand. Framing needs revision per Walter's efficiency-not-quality directive.

### Claim 4: Generality
NetBurst representations transfer across modalities (packet loss, latency) and datasets.
- **Evidence:** Cross-dataset MAPE comparison (PINOT, MAWI, Stardust, PINOT-Ping, PerfSonar). Fano-MAPE correlation quantifies the central claim that burstiness predicts difficulty.
- **Status:** PerfSonar kept as "mild regime baseline." Non-telemetry benchmarks (Electricity, Taxi, ETT, Exchange Rate) removed from §4 tables per March 27 decision — may appear in appendix only. Fano-MAPE figure in §2.

## Narrative Spine (Walter's Q1/Q2/Q3)
The paper is organized around three operational queries that self-driving networks must answer:
- **Q1 (Forecasting):** "What will happen next?" → §4.2
- **Q2 (Characterization):** "What is this behavior?" → §4.3
- **Q3 (Historical Search):** "When have we seen this before?" → §4.4

These three queries recur throughout the paper as a unifying thread. The thin-waist architecture metaphor: Input → Representation → Validation → Query.

## Section Architecture

| Section | Claim | Target Length |
|---------|-------|---------------|
| §1 Introduction | Self-driving networks need representations, not just predictions. NetBurst provides the telemetry representation layer answering Q1, Q2, Q3. Open with networking operator problem, not ML. | ~2 pages |
| §2 Background + Motivation | Sparse-bursty traffic defeats standard forecasters. Fano factor predicts difficulty (Fano-MAPE figure). TSFresh cosine similarity trap: 0.997 similarity masks structural differences. Existing foundation models ignore event structure. | ~2 pages |
| §3 Design | **RESTRUCTURED March 25 (Round 2).** Linear pipeline (eventization → tokenization → encoder) → shared representation → three parallel downstream tasks. Cross-cutting design requirements up front. See locked decisions #11–16. | ~3.5 pages |
| §4 Evaluation | **RESTRUCTURED March 27 (Round 3 — question-driven).** §4 is organized around six questions that test §3's specific claims. Preamble lists questions with backward references to §3. §4.1 Setup. §4.2 Representation Quality (Q1: geometric properties §3 requires). §4.3 Downstream Applications (Q2: forecasting, Q3: interpretability, Q4: search efficiency). §4.4 Ablation \& Generalizability (Q5: design choices, Q6: generalization). See locked decisions #24–29. | ~5 pages |
| §5 Related Work | 3 categories: time-series foundation models, network traffic analysis, learned representations for telemetry | ~1 page |
| §6 Conclusion | Restate three pillars. Future: agentic network management using NetBurst as perception layer. | ~0.5 pages |

## Locked Decisions (as of March 24)

1. **Twin-head is THE architecture.** Dual-model is the ablation.
2. **K=500 is the primary evaluation granularity.** K=10 is pipeline sanity only. K=100 and K=1000 shown for sweep context. Any number going in the paper must be at K≥100.
3. **Corrected §4.3 feature selection pipeline (LOCKED March 24):**
   - Start with ALL ~770 TSFresh features
   - Fisher ratio as global filter → top ~100 features
   - Local CKA per cluster on Fisher-filtered features
   - Top-k per cluster (k=5–10) → cluster's defining features
   - Visual validation: 2–3 cluster pairs
   - Key: global CKA is a separate §4.2 sanity check on the representation, NOT part of this per-cluster pipeline.
4. **Lead with Calinski-Harabasz, not Davies-Bouldin.** DB favors TSFresh because TSFresh creates a few very tight clusters on 2–3 dominant features while being blind to subtler structure. CH rewards the uniform separation NetBurst achieves. Frame DB with "curse of dominant dimensions" explanation.
5. **§4.4 is an efficiency argument, not a quality argument** (Walter, March 24). Cluster quality established in §4.3. Search inherits quality. §4.4 argues throughput, latency, uniform cluster sizes.
6. **Paper is framed as "representation system for network operators"** not "better forecaster" and not "LLM-for-networks." Opening paragraph: networking operations problem, not ML problem.
7. **Dataset disambiguation:** PINOT IP (Sanjay) → pretraining and forecasting (§4.1/§4.2). NetReplica /32 IP → clustering and search (§4.3/§4.4). Never conflate them.
8. **K-means++ initialization confirmed.** sklearn default. Multi-seed stability confirmed (10 seeds tested, results stable).
9. **Global CKA at /32 IP level is 0.22** (not the previously reported 0.45 from all aggregation levels). NetBurst 0.22 vs. Chronos2 0.12. The gap matters, not the absolute number. CKA increases at higher aggregation (subnets).
10. **Walter's NetFound framing (March 18):** NetBurst and NetFound are both targeting NSDI — they CANNOT share the same opening paragraph. NetBurst leads with the agentic AI systems angle (backed by concrete operational queries). NetFound leads with "understanding NFMs" / informing practitioners about when to use foundation models.
11. **§3 restructured as first-principles design (Arpit, March 25).** The section must extract cross-cutting design requirements up front and show how they cascade through the pipeline. Each stage is justified not by a single downstream task but by what the data's structure and all three capabilities jointly demand. The causal chain (eventization → isotropy → balanced clusters → efficient search) must be explicit.
12. **K selection is methodology, not a locked number (March 25).** K=500 is the empirical result on NetReplica /32 IP data. The design section describes the metric-driven selection procedure (convexity trends across CH, H_norm, degeneracy ratio). Different datasets may yield different K. The specific K does not belong in §3 — it belongs in §4.3 results.
13. **§3 describes methodology; §4 presents results (March 25).** The clustering methodology (algorithm choice, granularity selection procedure, Fisher-CKA attribution pipeline) lives in §3. The specific numbers (K=500, H_norm=0.993, cluster pair examples) live in §4.3. Do not mix them.
14. **§3 pipeline is linear-then-parallel, not six linear stages (March 25).** Eventization → tokenization → encoder are linear and produce the shared representation. Forecasting, characterization, and search are three parallel downstream tasks consuming that representation. The overview and section structure must reflect this fork.
15. **Every statistical tool must be defined for NSDI audience (March 25).** Fisher ratio, CKA, Calinski-Harabasz, H_norm, degeneracy ratio — each needs a one-sentence "what it measures" definition and a "why we need it" justification. Do not assume ML/statistics background.
16. **Technical claims require citations (March 25).** When §3 references content established with citations in §2, it must carry forward the relevant \cite{} commands. Uncited technical claims are fluff. This applies to overview paragraphs especially.
17. **§4 preamble motivates with Q1/Q2/Q3 questions (Arpit, March 27).** The opening of §4 must frame the evaluation around the three operational questions from §1/§3, not list subsections. Assert what §4 validates and how.
18. **§4.1 metrics block removed (March 27).** Metrics are defined in their respective subsections: MAPE and WD in §4.2 (forecasting), CH/H_norm/degeneracy ratio in §4.3 (characterization), efficiency metrics in §4.4 (search). §4.1 contains only datasets, baselines, and training protocol.
19. **Non-telemetry benchmarks removed from §4 tables (March 27).** Electricity, Taxi, ETT, Exchange Rate are not in Table 3 or any §4 figure. §4 focuses exclusively on telemetry datasets (PINOT, MAWI, Stardust, PINOT-Ping, PerfSonar). Non-telemetry benchmarks may appear in appendix only.
20. **Table 3 adds ×-improvement column (March 27).** Next to NetBurst's MAPE, report improvement factor over second-best baseline. This bridges the visual gap: reader sees the multiplier in the table, Takeaway cites it, no mental math required.
21. **Figure 3 = 3-panel PINOT scatter, placed BEFORE Table 3 (March 27).** Panels: (a) PINOT-Service, (b) PINOT-IP, (c) PINOT-Subnet. Each panel: x=MAPE, y=WD, one dot per model. Visual storytelling of NetBurst's leadership across granularities. The gap widening at finer granularity confirms the §2 Fano-difficulty hypothesis. MAWI/Stardust scatters go to appendix or follow-up figure.
22. **§4.1 baselines must cover all three capabilities (March 27).** Forecasting baselines (Chronos, Chronos2, Lag-Llama, DeepAR, N-BEATS, THP, Toto) for §4.2. TSFresh as the characterization baseline for §4.3. Chronos2 embeddings as the search efficiency comparison for §4.4. The baseline paragraph must acknowledge different subsections use different comparison targets.
23. **§4.1 dataset-to-subsection mapping must be explicit (March 27).** PINOT IP → §4.3 forecasting. NetReplica /32 IP → §4.2 representation quality, §4.3 characterization and search blocks. MAWI/Stardust/PINOT-Ping → §4.3 forecasting cross-dataset. PerfSonar → §4.4 generalizability.
24. **§4 has four subsections mirroring §3 (March 27, Round 2).** §4.1 Setup, §4.2 Representation Quality, §4.3 Downstream Applications (one subsection, three \smartparagraph-labeled blocks: forecasting, characterization, search), §4.4 Ablation & Generalizability. Representation quality comes FIRST — it validates the shared embedding before any downstream task. The three use cases are evidence that the representation works, not separate contributions. Generalizability folds into ablation since both answer "how robust is this design?"
25. **§4.2 Representation Quality is the new central pillar (March 27).** This subsection absorbs: CKA/isotropy table (corrected CKA=0.22), three-way CH/H_norm/degeneracy comparison at K=500, K selection sweep, 0.997 cosine similarity headline, cluster size CDFs. The 0.997 example is representation quality evidence (NetBurst separates what TSFresh can't); the feature decomposition explaining WHY lives in the characterization block of §4.3.
26. **Forecasting moves from §4.2 to §4.3 (March 27).** Forecasting is one of three downstream use cases, not the primary evaluation. It appears as the first \smartparagraph block within §4.3, after representation quality is established. This reorders the paper's emphasis: representation first, applications second.
27. **§4 is question-driven, with questions mapped to §3 claims (March 27, Round 3).** Each §4 subsection answers specific questions that test specific §3 arguments. The preamble lists all six questions with explicit backward references to the §3 claims they test. \smartparagraph{} labels are claim-bearing answers to these questions — a skim-reader sees the conclusions without reading prose. The six questions are:
    - **Q1** (§4.2, tests §3.1 isotropy claim + §3.3 K-means rationale): Does the representation produce the isotropic, well-separated geometry that §3 requires? Evidence: CKA/isotropy, CH/H_norm/degeneracy three-way at K=500, 0.997 cosine trap, cluster size CDFs.
    - **Q2** (§4.3 forecasting block, tests §3.2 + §3.2's caveat that "forecasting alone cannot validate representation quality"): Does the representation translate to forecasting accuracy? Evidence: MAPE/WD, Figure 3, Table 3. Also tests: do baselines forecast despite poor representations (DeepAR zero-prediction pathology)?
    - **Q3** (§4.3 characterization block, tests §3.3 Fisher-CKA pipeline + interpretability bridge): Are the behavioral clusters interpretable in operator-readable terms? Evidence: Fisher-CKA attribution, feature heatmap, taxonomy, feature diversity, 30-of-50 global/local divergence.
    - **Q4** (§4.3 search block, tests §3.4 causal chain: isotropy → balanced clusters → efficient search): Does geometric balance yield efficient retrieval? Evidence: throughput, latency, H_norm → N/K bound.
    - **Q5** (§4.4 ablation, tests §3.1 design choices): Does each architectural choice contribute? Evidence: quantile vs uniform, twin-head vs dual-model, oracle decomposition, threshold sensitivity.
    - **Q6** (§4.4 generalizability, tests §3 overview cross-cutting claims): Does the architecture generalize beyond wild-regime? Evidence: mild-regime (PerfSonar), cross-granularity, cross-scale transfer.
28. **§4 preamble is three sentences, not a table of contents (March 27).** Format: (1) State what §3 proposes. (2) State the six questions grouped as: representation geometry (Q1), downstream capabilities (Q2–Q4), robustness (Q5–Q6). (3) Each with backward reference to the §3 claim it tests. No content-free "In this section we evaluate..." openers.
29. **\smartparagraph{} labels in §4 are claim-bearing answers (March 27).** Not topics ("Head-to-head comparison") but conclusions ("NetBurst embeddings are isotropic where baselines collapse"). A skim-reader who reads only the labels should see the full argument.

## Resolved Questions (closed in past 10 days)

1. **Multi-seed stability (CLOSED March 22):** 10 seeds tested with K-means++, results stable across all metrics. No longer blocking.
2. **Local CKA vs Global CKA (CLOSED March 24):** Both are used, for different purposes. Global CKA = §4.2 sanity check on representation quality. Local CKA = §4.3 per-cluster feature identification after Fisher filtering. Per-feature CKA implementation (N×1) avoids the finite-sample bias from Murphy et al. (ICLR 2024).
3. **Feature selection methodology (CLOSED March 24):** Fisher-first, local-CKA-second pipeline replaces the old CKA-first approach. Fisher ratio prevents noise features from surfacing; local CKA ensures features explain why the embedding grouped these specific points.
4. **DB vs CH paradox (CLOSED March 24):** Lead with CH. TSFresh wins DB because it clusters tightly on 2–3 dominant features — this is a limitation, not a strength. Frame as "curse of dominant dimensions."
5. **Semantic search quality vs efficiency (CLOSED March 24, Walter):** §4.4 = efficiency story. Quality lives in §4.3.

## Open Questions (still require resolution)

1. **Wasserstein vs DTW for search evaluation:** Walter flags Wasserstein misses temporal structure. DTW is P1 action item for Satya. May drop similarity metric from §4.4 entirely and keep pure efficiency argument.
2. **"Background features" diagnostic (Walter, March 24):** Which global CKA top-50 features appear in ZERO local CKA top-50 lists across all clusters? If found, these are globally aligned but locally unexplanatory — needs investigation.
3. **Normalized vs unnormalized Wasserstein (March 24):** Forecasting eval used normalized (PDFs summing to 1); search uses unnormalized (raw values). These conflate shape with scale. Need to report both explicitly or drop one. **March 27 discovery:** Prose WD numbers are ~100× smaller than current Table 3 values — confirms normalization mismatch. Must settle convention before rewriting §4.2 WD paragraph.
4. **High-magnitude feature domination (March 24):** Need to show explicitly that TSFresh's cosine similarity is dominated by high-magnitude features (mean, sum, quantiles) that wash out subtler structure. This is the analytical backbone of the 0.997 cosine similarity argument.
5. **Systems evaluation (§4.5):** Training time, inference latency, memory footprint, preprocessing throughput still not benchmarked. NSDI desk-reject risk without this section.
6. **Stardust IP-level results missing (flagged March 27).** Stardust IP row in Table 3 is all placeholders. This is the only missing wild-regime row. Was due March 25–26 per timeline. BLOCKING Table 3 completion and the "Production WAN" paragraph in §4.2.
7. **Non-telemetry benchmarks disposition (March 27).** Electricity, Taxi, ETT, Exchange Rate removed from §4. Do they appear in appendix as a generalizability reference? Or dropped entirely? PerfSonar stays (it's telemetry).

## Timeline (as of March 27)

| Date | Milestone | Status |
|------|-----------|--------|
| March 25–26 | Satya delivers P0 items: CH decomposition, corrected feature selection pipeline, 2–3 cluster pair examples, background feature diagnostic | PARTIAL — Stardust IP still missing, §4.3 data status unclear |
| March 25–26 | Arpit starts writing §4.2 and §4.3 from committed notebook results | DELAYED — §4 audit revealed structural issues requiring restructuring before writing |
| March 27 | §4 structural audit complete. Locked decisions #17–23. Stale numbers identified. | DONE |
| March 27–28 | Rewrite §4.1 (compressed) + §4.2 (fix numbers, add ×-column, 3-panel figure). Satya: deliver Stardust IP numbers. | IN PROGRESS |
| March 28–29 | Rewrite §4.3 (K=500, Fisher-first, 0.997 headline) + §4.4 (efficiency reframe) | PENDING — partially blocked on Satya's §4.3 data |
| March 29–30 | Walter reviews. Iterate on framing. | |
| Mid-April | Submission deadline | |

## Key Figures Needed

1. **§2 Motivation:** TSFresh cosine similarity trap — two time series with 0.997 cosine similarity but visually distinct + the discriminating feature (absolute_sum_of_changes)
2. **§2 Motivation:** Fano-MAPE correlation scatterplot across datasets (burstiness predicts difficulty)
3. **§3 Design:** Eventization pipeline figure (raw → thresholded → IBG + BI) per Walter's March 16 request
4. **§4.2 (Figure 3, LOCKED March 27):** 3-panel PINOT scatter — (a) Service, (b) IP, (c) Subnet. x=MAPE, y=WD, one dot per model. Placed BEFORE Table 3. Individual PDFs exist (`mape_vs_wd_PINOT-service.pdf`, etc.) — need to compose into single `figure*`. Visual storytelling: NetBurst in bottom-left, gap widens at finer granularity.
5. **§4.2 (Table 3, LOCKED March 27):** MAPE table with ×-improvement column. Telemetry datasets only (PINOT 3 gran, MAWI 3 gran, Stardust 2 gran, PINOT-Ping, PerfSonar). No Electricity/Taxi/ETT/Exchange Rate.
6. **§4.3:** Three-way CKA bar chart (NetBurst vs Chronos2 vs TSFresh, global)
7. **§4.3:** H_norm and CH comparison across K values (three models)
8. **§4.3:** Cluster size CDFs (three models, K=500 and K=1000)
9. **§4.3:** Feature heatmap (dual-encoded: color=z-scored mean, opacity=within-cluster variance)
10. **§4.3:** Cluster pair examples (2–3 pairs: obvious distinction, subtle 0.997 cosine pair, cross-method comparison)
11. **§4.4:** Embedding throughput and search latency comparison (NetBurst vs Chronos2)

## Writing Constraints (DO NOT VIOLATE)
- Mean sentence length: ~21 words. Compress aggressively.
- Topic sentences assert claims. Never open a paragraph with background.
- Zero hedging. "We show" not "We believe." "X reduces Y by 13×" not "X may help reduce Y."
- No filler terms: never use "novel," "significant," "state-of-the-art," "comprehensive," "robust" as adjectives. Replace with specific numbers or delete.
- Active voice for claims, passive for methodology only.
- Every evaluation subsection ends with a **Takeaway** paragraph that synthesizes the result and connects it to the paper's thesis.
- Use \smartparagraph{} labels as structural anchors. Each label is a narrative commitment.
- Headings are claims, not topics. "Event-centric decomposition reduces error 13×" not "Experimental Results."
- When referencing figures/tables: interpret, don't just cite. "Figure 3 shows that X, confirming Y" not "See Figure 3."
- No exclamation marks. No rhetorical questions in technical sections.
- Paragraphs: 4–6 sentences. If longer, split. If shorter, merge.
- Name things precisely. "NetBurst encoder" not "our model." "Fano-weighted MAPE" not "our metric."
- When introducing a design choice, justify it immediately. Not "we use X" but "we use X because Y, which matters because Z."
- The opening paragraph frames this as a networking operations problem, not an ML problem. Lead with the domain, not the method.
