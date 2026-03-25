# Project Context: NetBurst → NSDI '27

*Last updated: March 24, 2026 — incorporates all decisions from March 14–24 meetings, Slack discussions (Arpit/Satya/Walter group DM, Arpit/Satya DMs), and meeting transcripts.*

## Identity

NetBurst learns event-centric representations of sparse-bursty network telemetry that enable accurate forecasting, interpretable characterization, and efficient similarity search — the three queries every network operator asks about their traffic: "What will happen next?", "What is this behavior?", and "When have we seen this before?"

## Venue
- **Target:** NSDI 2027 (Spring)
- **Deadline:** Mid-April 2026
- **Page limit:** 14 pages + references (USENIX format)
- **Format:** Systems venue. Use \smartparagraph{} labels. Expect systems evaluation (latency, throughput, memory). Reviewers will check for deployed-system baselines, not just ML baselines.

## Contributions (as claims with evidence)

### Claim 1: Forecasting (§4.2 — "What will happen next?")
Event-centric decomposition reduces forecasting error 13–605× on sparse-bursty telemetry compared to Chronos2, TimesFM, and ARIMA. Wasserstein distance preserves distributional fidelity where point-wise metrics fail.
- **Evidence:** MAPE comparison table, per-dataset breakdown, Wasserstein distance plots.
- **Dataset:** PINOT IP dataset (Sanjay's collection) for pretraining and forecasting evaluation.
- **Status:** STRONG. Results stable. Needs reframing from "better forecaster" to "forecasting as downstream task enabled by representations."

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
NetBurst representations transfer across modalities (packet loss, latency, non-network fire data) and datasets.
- **Evidence:** Cross-dataset evaluation table. Fano-MAPE correlation (Fires: Fano=3876→MAPE=81; Exchange-Rate: Fano=0.016→MAPE=0.42) quantifies the central claim that burstiness predicts difficulty.
- **Status:** Fano-MAPE correlation figure needs to be in §2 as quantitative motivation. Cross-domain datasets (Fires, PINOT-Ping) strengthen generality. PerfSonar kept as "mild regime baseline."

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
| §3 Design | Twin-head architecture with event-centric decomposition. Soft cross-entropy loss with learnable temperature. Shared encoder forces representations useful for both forecasting and characterization. Eventization process (IBG + BI) with threshold selection guidance. | ~3 pages |
| §4 Evaluation | §4.1 Setup, §4.2 Forecasting (Q1), §4.3 Characterization (Q2) — CENTRAL, §4.4 Semantic Search (Q3) — efficiency story, §4.5 Systems evaluation (timing, memory, throughput) | ~5 pages |
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

## Resolved Questions (closed in past 10 days)

1. **Multi-seed stability (CLOSED March 22):** 10 seeds tested with K-means++, results stable across all metrics. No longer blocking.
2. **Local CKA vs Global CKA (CLOSED March 24):** Both are used, for different purposes. Global CKA = §4.2 sanity check on representation quality. Local CKA = §4.3 per-cluster feature identification after Fisher filtering. Per-feature CKA implementation (N×1) avoids the finite-sample bias from Murphy et al. (ICLR 2024).
3. **Feature selection methodology (CLOSED March 24):** Fisher-first, local-CKA-second pipeline replaces the old CKA-first approach. Fisher ratio prevents noise features from surfacing; local CKA ensures features explain why the embedding grouped these specific points.
4. **DB vs CH paradox (CLOSED March 24):** Lead with CH. TSFresh wins DB because it clusters tightly on 2–3 dominant features — this is a limitation, not a strength. Frame as "curse of dominant dimensions."
5. **Semantic search quality vs efficiency (CLOSED March 24, Walter):** §4.4 = efficiency story. Quality lives in §4.3.

## Open Questions (still require resolution)

1. **Wasserstein vs DTW for search evaluation:** Walter flags Wasserstein misses temporal structure. DTW is P1 action item for Satya. May drop similarity metric from §4.4 entirely and keep pure efficiency argument.
2. **"Background features" diagnostic (Walter, March 24):** Which global CKA top-50 features appear in ZERO local CKA top-50 lists across all clusters? If found, these are globally aligned but locally unexplanatory — needs investigation.
3. **Normalized vs unnormalized Wasserstein (March 24):** Forecasting eval used normalized (PDFs summing to 1); search uses unnormalized (raw values). These conflate shape with scale. Need to report both explicitly or drop one.
4. **High-magnitude feature domination (March 24):** Need to show explicitly that TSFresh's cosine similarity is dominated by high-magnitude features (mean, sum, quantiles) that wash out subtler structure. This is the analytical backbone of the 0.997 cosine similarity argument.
5. **Systems evaluation (§4.5):** Training time, inference latency, memory footprint, preprocessing throughput still not benchmarked. NSDI desk-reject risk without this section.

## Timeline (as of March 24)

| Date | Milestone |
|------|-----------|
| March 25–26 | Satya delivers P0 items: CH decomposition, corrected feature selection pipeline, 2–3 cluster pair examples, background feature diagnostic |
| March 25–26 | Arpit starts writing §4.2 and §4.3 from committed notebook results |
| March 27 (Friday) | Paper in respectable shape — all eval sections drafted with real numbers and figures |
| March 28–30 | Walter reviews. Iterate on framing. |
| Mid-April | Submission deadline |

## Key Figures Needed

1. **§2 Motivation:** TSFresh cosine similarity trap — two time series with 0.997 cosine similarity but visually distinct + the discriminating feature (absolute_sum_of_changes)
2. **§2 Motivation:** Fano-MAPE correlation scatterplot across datasets (burstiness predicts difficulty)
3. **§3 Design:** Eventization pipeline figure (raw → thresholded → IBG + BI) per Walter's March 16 request
4. **§4.2:** Three-way CKA bar chart (NetBurst vs Chronos2 vs TSFresh, global)
5. **§4.3:** H_norm and CH comparison across K values (three models)
6. **§4.3:** Cluster size CDFs (three models, K=500 and K=1000)
7. **§4.3:** Feature heatmap (dual-encoded: color=z-scored mean, opacity=within-cluster variance)
8. **§4.3:** Cluster pair examples (2–3 pairs: obvious distinction, subtle 0.997 cosine pair, cross-method comparison)
9. **§4.4:** Embedding throughput and search latency comparison (NetBurst vs Chronos2)

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
