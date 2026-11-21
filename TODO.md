# GenAU (CVPR 2027): supplementary checklist

Status, 2026-11-21: the supplementary (`suppl.tex` → `sec/7_appendix.tex`, compiled to
`appendix.pdf`) is ready for upload; `git log -p -- sec/7_appendix.tex` shows the changes.
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
- [ ] Confirm that the five qualitative figures (`imgs/tubes.jpg`, `capsules.jpg`, `pipe-fryum.jpg`,
      `mounts.jpg`, `rolled_strip_base.jpg`) come from the reported halfmix checkpoint. They date from
      the initial commit; re-render with `../scripts/figures/render_qual.py` if not.
- [ ] Upload `appendix.pdf` (10 pages, review format). Anonymity was checked: no author metadata or
      local paths; the only author name is in the MultiADS reference.
- [ ] Optional: add a language example figure built from real outputs of the reported checkpoint
      (e.g. VisA-D&R answers), never from `imgs/text.pdf`.

## Housekeeping

- [ ] `../scripts/data_prep/defect_mappings.py` is a stale taxonomy (the evaluation and the appendix
      tables use `defect_mappings_multiads.py`); rename or delete it so it is not reused.
- [ ] Bump the `GenAU_cvpr` submodule in the parent GenAU repo.
