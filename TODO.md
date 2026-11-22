# GenAU (CVPR 2027): supplementary checklist

Status, 2026-11-22: the supplementary sources (`suppl.tex` → `sec/7_appendix.tex`) are final and
`appendix.pdf` is recompiled from them; `git log -p -- sec/7_appendix.tex` shows the changes.
Paths starting with `../` are in the parent GenAU repo.

## Before uploading the supplementary

- [ ] Review the four removals in `sec/7_appendix.tex`:
  - Fig. A6 (dialogue). Its transcripts came from an earlier chain-of-thought model (header of
    `imgs/GenAU_Dialogue.tex`: copied from `imgs/text.pdf`), with a prompt, output format and
    taxonomy that the reported pipeline no longer uses.
  - The 0.5B full-reasoning-mix SBERT pair (0.565/0.656, above the default's 0.554/0.635). The
    full-mix discussion now covers 7B only.
  - The Real-IAD taxonomy tables (Real-IAD is not used for type recognition).
  - The old single/multi-defect image counts in Table A1 (VisA 923/277 and MVTec-AD 1,200/58 could
    not be reproduced from `../anomaly_dataset/`).
- [x] Re-rendered the five qualitative figures (`imgs/tubes.jpg`, `capsules.jpg`, `pipe-fryum.jpg`,
      `mounts.jpg`, `rolled_strip_base.jpg`) from the reported 7B halfmix checkpoint
      (`../scripts/figures/suppl_qual_maps.bsub`, `../scripts/figures/select_suppl_qual.py`, `../scripts/figures/render_qual.py`);
      samples spread over defect types and AP quantiles, not best-case. Qualitative Results text and captions rewritten.
- [x] Recompiled `appendix.pdf` from `suppl.tex` locally (no TeX install works on the cluster: tectonic's
      bundle is blocked by the proxy, TeX Live 2018 lacks libpoppler): 11 pages (with Tables A7 and A10), no undefined references.
- [ ] Upload `appendix.pdf` (review format, 11 pages). Anonymity was checked: no author metadata or
      local paths; the only author name is in the MultiADS reference.
- [x] Added the language example (Fig. `fig:text`, Qualitative Results): three VisA-D&R conversations of the
      reported 7B run, verbatim, at the class-median GPT-Score; maps from a re-run of the first turn
      (`../scripts/figures/render_dialogue.py`).

## Housekeeping

- [x] Renamed the stale taxonomy to `../scripts/data_prep/defect_mappings_legacy.py` (still imported by the
      old `merge.py`, `convert_real_iad.py`, `convert_mvtec_to_multi.py`); evaluation uses `defect_mappings_multiads.py`.
- [x] Bump the `GenAU_cvpr` submodule in the parent GenAU repo.
