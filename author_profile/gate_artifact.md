# Artifact Gate — compile, render, and inspect the PDF

> The gates in `gate_mechanical.md` and `gate_semantic.md` audit the **source**. A paper is
> source **plus a rendered artifact**, and figures, tables, and page layout ship in the artifact,
> not the source. This gate catches what a text-only pass cannot see: a float that overflows the
> column, a table that dies in grayscale, a caption that runs six lines, a `figure*` reused from
> another template that now bleeds off the page. **Run it on every edit that touches a float,
> caption, or table, and once before every "done."** A source-only audit is half a gate; do not
> report a section "audited" until you have compiled it and *looked* at the page.

This gate has two parts: **Part A** is mechanical (grep the `.log`, deterministic). **Part B** is
judgment (render each changed page to an image and inspect it). Both must run. Report the Part A
counts and paste or describe the Part B rendered evidence; a mental pass is not a run.

---

## Part A — Log gate (grep the compile log, fix every hit)

Compile the paper, then grep `paper.log`. Each rule states the ban and the fix.

**A1. Compile must succeed with no fatal error.** `grep -nE '^!' paper.log` returns nothing.
A `Fatal error occurred, no output PDF` is a hard stop, not a warning.

**A2. Overfull boxes — a float or line runs past its box.** `grep -nE 'Overfull \\[hv]box' paper.log`.
A large `Overfull \hbox` (over ~5pt) on a figure/table page is the top signal that a float or a
tabular is wider than its column. Fix the float (A6/A7), do not ignore it.

**A3. Undefined references and citations.** `grep -nE 'Warning: (Citation|Reference).*undefined' paper.log`
returns 0. A `??` or `[?]` in the PDF is never acceptable in a reported-done section.

**A4. Missing or unloaded assets.** `grep -niE 'File .* not found|Missing character|Undefined color' paper.log`
returns nothing. A reused figure that calls a color or package the new preamble does not define
fails here (define it, do not delete the call blindly).

---

## Part B — Render gate (look at the page)

Render every changed page to an image (`pdftoppm -png -r 110 paper.pdf out`) and inspect it. These
are the checks a regex cannot make.

**A5. No clipped or overflowing float.** Every figure and table sits inside the column or, for a
full-width `figure*`/`table*`, inside the text block. Nothing runs off the right margin or past the
page edge. **This is the single most common miss.** If any cell, node, or curve is cut off, the
float fails, even if the log looks clean.

**A6. Reused float must be re-fit to this template.** A `figure*`, `tabular`, or TikZ block imported
from another venue or paper was sized for that column geometry. Re-check its width, TikZ `scale`,
and font size against the *current* template (a two-column ACM figure overflows a USENIX column, and
vice versa). Reuse is not free; a reused float is not "drafted" until it fits here.

**A7. Legible at print size, with units.** In-figure text (axis labels, tick labels, legends, node
text) is readable at the rendered column width, not only when zoomed. Every axis has a unit. A
checklist-green figure can still be an illegible mess; the render is the check.

**A8. No color-as-only-channel.** A table or figure must not carry meaning by color alone. If cells
are colored to mean "kept vs lost / good vs bad," add a redundant marker (bold, a symbol, or a
dedicated column) so the artifact survives grayscale printing and red-green color blindness. State
the encoding in the caption.

**A9. Caption terseness and float placement.** Caption is a bold takeaway plus at most one clause,
about three lines maximum (semantic G7). The float appears near its first `\ref` and on or before
the page that discusses it, not dumped at the end. No section heading is orphaned at a column bottom.

---

## Process

1. Compile (`latexmk -pdf` or two `pdflatex` passes + `bibtex`).
2. Part A: run the four log greps; fix or justify every hit. Report the counts.
3. Part B: render every changed page; inspect each float against A5–A9. Report what you saw, with
   the rendered image as evidence, not "looks fine."
4. Only source that passes `gate_mechanical` **and** `gate_semantic` **and** this gate is reported
   done. If the independent red-team (`red_team_protocol.md`) runs, hand it the **rendered page
   images**, not only the `.tex`, so it can catch layout and float defects a source read cannot.

**Data figures** (CDFs, bar charts, heatmaps, scatter plots) are authored in the
[data-visualization skill](https://github.com/SNL-UCSB/data-visualization-skill); this gate enforces
that whatever ships in the PDF still passes A5–A9 in the final template. Route a failing data figure
back to `/viz` rather than hand-patching it here.
