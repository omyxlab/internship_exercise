# 02_scripts

All of your code lives here. Number scripts in execution order so the pipeline is obvious
and re-runnable, for example:

```
01_qc.sh
02_filter.sh
03_assembly.sh
04_annotation.sh
05_blast_genbank.sh
06_align_and_compare.sh
07_phylogenetics.sh
```

Rules:
- Every result in `04_results/` must be reproducible by running these scripts in order.
- Read inputs from `01_data/`, write intermediates to `03_analysis/`, write final outputs
  to `04_results/`, and tee logs to `99_logs/`.
- Any language is fine (bash, Python, R, …); justify tool choices in `REPORT.md`.
