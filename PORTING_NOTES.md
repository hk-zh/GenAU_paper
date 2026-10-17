# GenAU → CVPR 2027 template port

Ported 2026-09-19. This directory *was* `GenAU_iclr` (renamed, not copied), so it carries the
ICLR-era content corrections; `../GenAU` (WACV 2027) is untouched and is where the two-column
scaffolding came from.

Build: `pdflatex main && bibtex main && pdflatex main && pdflatex main && pdflatex main`
(the third pass after bibtex is needed: with only two, the bibliography back-reference lists are
incomplete on pages 9-16)

Current state: **compiles clean — 0 errors, 0 unresolved references, 0 unresolved citations.**
19 pages = 10 (paper, body ends p.8 + references p.8–10) + 9 (supplementary, page counter reset).

---

## ⚠️ There is no CVPR 2027 author kit yet

`cvpr.sty` and `ieeenat_fullname.bst` here are **byte-identical to the official kit**
(github.com/cvpr-org/author-kit, `main` @ 2026-05-28), whose newest release is
`CVPR2026-v1(latex)`. Nothing in it is year-stamped except a cosmetic `\ProvidesPackage` date —
every user-visible year comes from `\confYear{2027}` in `main.tex`. This is the same situation
the ICLR port handled (its 2027 sty differed from 2026 only in two `\lhead` strings).

- [ ] **Re-check when the CVPR 2027 kit is released** and drop the new `cvpr.sty` in.

`ieeenat_fullname.bst` is also byte-identical to the one already in `../GenAU` — verified.

---

## Page budget: fits

| | |
|---|---|
| CVPR limit | **8 pages** body, references excluded and unlimited; supplementary is a **separate PDF** |
| Ours | body ends on **page 8**; references run p.8–10 |

Going back to two columns reversed the ICLR overflow by itself. The ICLR single-column build was
11 pages of body against a 9-page limit; the same content in two columns is 8. No cuts were needed
and **none were made** — no prose, table or figure was dropped in this port.

## Supplementary: one .tex, split the PDF

Per your call and the official kit (`sec/X_suppl.tex`), the appendix stays in `main.tex` after the
bibliography, behind `\clearpage \setcounter{page}{1} \maketitlesupplementary`. One compile, all
cross-references resolve (the main text cites `Appendix~\ref{app:impl}`, `app:related`, `app:tasks`,
`app:taxonomy` — all resolve to the `\appendix` letters A–J).

- [ ] 👤 **Split `main.pdf` before submitting: pages 1–10 → the paper, 11–19 → the supplementary.**
      **Do not upload it unsplit.** (`pdfseparate`/`pdfunite`, `qpdf --pages`, or Preview/Acrobat.)

Appendix floats keep the `A` prefix (Table A1, Figure A1) so they can never be confused with the
paper's Table 1 / Figure 1.

### Known wrinkle: `pagebackref` and the supplementary
`hyperref` is loaded with `pagebackref` (kept from the WACV version — it lists, per bibliography
entry, the pages that cite it). 11 of 91 backreferences come from the appendix, and because the
page counter resets at `\maketitlesupplementary` they report supplementary pages 1–9, which in the
split paper read as main-paper pages. Cosmetic but mildly misleading. Two one-line remedies, both
left to you because each gives something up:
- drop `pagebackref` from the `hyperref` options in `main.tex` (loses the feature for all entries), or
- delete the `\setcounter{page}{1}` so the supplementary numbers 11–19 continuously (backrefs become
  unambiguous; the split supplementary then starts at page 11 instead of 1).

---

## What changed relative to the ICLR version in this directory

| Item | ICLR 2027 | CVPR 2027 |
|---|---|---|
| Class | `\documentclass{article}` + `\usepackage{iclr2027_conference,times}` | `\documentclass[10pt,twocolumn,letterpaper]{article}` + `\usepackage[review]{cvpr}` |
| Columns | one | **two** |
| Bib style | `iclr2027_conference` (author–year) | `ieeenat_fullname` (**numeric**) |
| Citations | `\citep{}` ×98 | **`\cite{}` ×98** (converted back; identical rendering either way under `natbib[numbers]`, but `\cite` is the template idiom and matches `../GenAU`) |
| Fig. 1 (motivation radar) | `figure`, radar + capability-key table side by side | **`figure`, single column, table removed** (author's call, 2026-09-20) — back to the WACV form: radar at `scale=1.0` with the four-swatch legend below it. See the note below. |
| Fig. 2 (architecture) | `figure` | **`figure*`** (restores the WACV geometry; `width=1.0\textwidth`) |
| Appendix | inline after the references, same PDF, submitted as one file | same `.tex`, but behind `\maketitlesupplementary` and **split out** before upload |
| AI use / Ethics / Reproducibility statements | required by ICLR, `\input` in `main.tex` | **not inputted** — CVPR requires none |
| Anonymity | sty prints it automatically while `\iclrfinalcopy` is commented | `[review]` option prints "Anonymous CVPR submission / Paper ID" + line numbers |
| `preamble.tex` | had to declare graphicx/amsmath/amssymb/booktabs/caption/subcaption/enumitem/xcolor/cleveref by hand | back to **4 lines** (`multirow`, `colortbl`, `tikz`, `microtype`) — `cvpr.sty` supplies the rest |

### Fig. 1: the capability table was removed (2026-09-20)
The ICLR port had put a "Can it answer? / Is there · Where · What type · Why" table beside the
radar. It was removed on request and the figure is single-column again, which is **exactly the WACV
form** — `sec/1_intro.tex` now differs from `../GenAU/sec/1_intro.tex` only in comments and the
caption. Two things the table had been doing were carried over rather than dropped:

- **It was the legend.** Its coloured rules were the only colour→model mapping. The WACV inline
  legend (four swatches, each with its model name) is restored inside the `tikzpicture`, so series
  identity is still never colour-alone. The palette was re-validated at the same time: all checks
  pass, worst adjacent pair MultiADS blue ↔ Anomaly-OV purple at **CVD ΔE 8.3 (deutan)**, i.e. at
  the ≥8 target rather than in the 6–8 band.
- **It made the capability gaps explicit.** With it gone, "cannot perform this task" is once more
  encoded as a vertex at the radar's centre, where it is indistinguishable from a genuinely low
  score. The caption now says so in words ("a vertex at the centre marks a task the model cannot
  perform…"), which is how the WACV version handled it. **This is a real loss of clarity relative
  to the table** — flagging it, not re-litigating it.

Fits the column: at `scale=1.0` the build reports no overfull box for this figure, and that silence
was verified to be meaningful (forcing `scale=1.6` does produce `Overfull \hbox 74.8pt` for it).

### Gotchas handled
- **`cvpr.sty` loads `cleveref` itself**, via `\AtEndPreamble` (i.e. after `hyperref`), with CVPR's
  own `\crefname` abbreviations. Loading it in `preamble.tex` would clash — don't.
- **No `\PassOptionsToPackage{dvipsnames}{xcolor}` needed.** That was an ICLR-only workaround
  (`eso-pic` grabbed `xcolor` with no options first); `cvpr.sty` loads `xcolor[dvipsnames]` directly.
- `silence.sty` and `cleveref.sty` are vendored here because they are not in this machine's TeX Live;
  `natbib`/`fancyhdr` are, so the ICLR copies of those were removed.

### Removed (ICLR-only)
`iclr2027_conference.{sty,bst,bib,tex}`, `math_commands.tex`, `natbib.sty`, `fancyhdr.sty`.

### Kept but unused
`sec/8_statements.tex` — the drafted AI use / Ethics / Reproducibility statements. CVPR asks for
none of them, so `main.tex` does not `\input` it. Left on disk in case it is wanted for arXiv or a
venue that asks again. (Note it still carries `\TODO{}` markers and is **unverified** — it was never
author-reviewed.)

---

## Still to do

- [ ] 👤 **Pick the title.** Unchanged from the ICLR draft and still says *"Language-Grounded"*,
      which the `lqa` learned-query control refutes (random anchors tie or beat text anchors on both
      datasets). `../GenAU/ICLR_TODO.md` LOCKED DECISION 1 says it must change and lists candidates.
      Flagged in a comment in `main.tex`. **Nothing else is blocked on this.**
- [ ] 👤 Fill in `\def\paperID{*****}` once CVPR assigns one.
- [ ] 👤 Split the PDF before submission (above).
- [ ] Visual pass on the two-column tables and figures — **nothing has been eyeballed**; there is no
      PDF rasteriser on this machine, so only the `.log`/`.aux` were checked. The build has exactly
      **1 overfull hbox (12.05pt)**, which is byte-for-byte the same one the WACV build has
      (pre-existing, not introduced here).
- [ ] Decide the `pagebackref` wrinkle (above).

## Pre-existing, untouched
`sec/1_intro.tex.bak`, `sec/4_method.tex.bak`, `sec/5_experiments.tex.bak` (stale ICLR-era backups),
and ~46 lines of commented-out old drafts in `sec/4_method.tex`. Note `sec/4_method.tex:16-23` holds
the OLD, longer language-grounding motivation — do not un-comment it.
