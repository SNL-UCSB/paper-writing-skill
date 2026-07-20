# Loop Mode — Iterative Audit-and-Fix (for the `/loop` feature)

> Purpose: let a user run `/loop apply the paper-writing skill iteratively` (or "audit with loop") and have
> the skill run a RESUMABLE audit → red-team → fix cycle with NO further instructions. Each loop iteration is
> a self-contained unit that reads its state from disk, does the next chunk of work, and updates state — so
> progress survives context loss between iterations. The loop ends itself when the paper is clean.
> Added 2026-07 from a live-review audit (`notes/PAPER_WRITING_SKILL_GAPS.md`).

## What the user types (invocation)
- `/loop apply the paper-writing skill iteratively`   ← self-paced; recommended
- `/loop audit with the paper-writing skill until clean`
- Scoped: `/loop audit §2 and §3 with the paper-writing skill`
No other detail is needed — this file supplies the obvious parts (which checks, in what order, when to stop).

## The durable ledger (memory across iterations)
Maintain `notes/AUDIT_LEDGER.md` in the paper directory: one row per section, one column per gate.

| Section | Mechanical (gate_mechanical) | Semantic (gate_semantic S1–S31) | Independent red-team | Status |
|---------|---------------------------|---------------------------------|----------------------|--------|

Statuses: `PENDING` / `FINDINGS(n)` / `CLEAN`. Record, per section, the pasted grep counts and the last
red-team findings. This ledger IS the loop's memory — read it fresh at the start of every iteration.

## One iteration (the repeatable unit)
1. Read `notes/AUDIT_LEDGER.md` (create it from the section list if absent) and `project_context.md`
   (the paper's controlling idea).
2. Select the next unit of work: the highest-priority section not yet `CLEAN` through all three gates
   (respect any user scope).
3. Run the three gates on that section (see `red_team_protocol.md`):
   a. MECHANICAL — run ALL `author_profile/gate_mechanical.md` Part C greps; paste the counts.
   b. SEMANTIC — apply `author_profile/gate_semantic.md` (S1–S31): accessibility, coherence,
      consistency, rigor, honest positioning, figures.
   c. CRAFT — check the changed prose against `author_profile/craft_reference.md` (the register's
      positive rules).
   d. INDEPENDENT RED-TEAM — spawn a FRESH reviewer (subagent) that did NOT write the text; it returns findings.
4. FIX every finding in the section's text.
5. Re-run the gates on the fixed text. Zero surviving findings → mark the section `CLEAN`; else `FINDINGS(n)`.
6. Update `notes/AUDIT_LEDGER.md` (grep counts, findings, fixes, status). Rebuild the PDF if a section changed.
7. Emit a one-line iteration summary (section, findings fixed, status).

## Stop condition (the loop ends itself)
End when EVERY in-scope section is `CLEAN` through all three gates (ledger all green): report the final
ledger and stop. If two consecutive iterations make no progress (the same findings survive), STOP and
surface the blocker instead of spinning.

## Rules
- The independent red-team MUST be a separate reviewer (fresh subagent), never the author self-checking —
  self-audit is the exact failure mode this protocol fixes.
- Keep each iteration small (one section, or one gate on a large section) so it fits a single loop tick.
- For FIGURES, "clean" requires a rendered-image inspection, not a checklist pass (a checklist-green figure
  can still be an illegible mess — verify by looking).
- Default section order = the paper's order; override with the user's scope.
