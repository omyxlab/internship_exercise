# CRESS-DNA Virus Genome Recovery — Intern Task

**Omyx Lab · Bioinformatics internship exercise**

Welcome. This repository contains a small, self-contained research task. Everything you
need to start is already here: three raw sequencing read sets and this brief. Your job is
to recover, annotate and characterise the genomes, then decide how they relate to viruses
that are already known. Read this whole README before you run anything.

---

## 1. Background / scenario

As part of a larger project, environmental/plant material was processed with a
**circular-DNA enrichment step (rolling-circle amplification, RCA)** and sequenced on an
Oxford Nanopore platform. The project was originally looking for *geminiviruses*.

During the first pass (host read removal → de novo assembly → taxonomic classification),
a subset of contigs did **not** classify as geminivirus. On closer inspection with a
profile/HMM search, they carried a **replication-associated protein (Rep)** signature
resembling *other* Rep-encoding viruses — the hallmark of a small **circular Rep-encoding
single-stranded DNA (CRESS-DNA) virus**. In other words: something circular and
Rep-encoding, but not what the project set out to find.

To recover these genomes properly, a pair of **abutting (outward-facing) primers** was
designed from the initial contigs and used to **PCR-amplify the full circular genome** of
three positive samples. Each amplicon was then sequenced on nanopore. **Those three read
sets are what you have been given** — one per isolate:

| File (in `01_Data/`) | Isolate |
|---|---|
| `isolate_A.fastq` | A |
| `isolate_B.fastq` | B |
| `isolate_C.fastq` | C |

The isolates have deliberately been given neutral names. You are **not** being told the
genus, the species, the host, or whether these are novel — working that out is part of the
task.

---

## 2. What you need to deliver

For **each** of the three isolates:

1. **A complete genome.** Recover the full, closed circular genome from the reads.
2. **An annotation.** Identify the open reading frames and label them (you should expect a
   Rep and a capsid/CP; look also for conserved replication-associated motifs). Report
   genome length, GC content, ORF coordinates and strand/orientation.
3. **Closest known relatives.** Find the most similar sequences in **GenBank** and report
   them with accession numbers and percent identity.

Then, **across all three isolates together and against the GenBank relatives**, compare at
**three levels**:

- **Whole genome** (nucleotide),
- **Rep** (the replication-associated protein / gene),
- **CP** (the capsid protein / gene).

Finally, **state a conclusion**: are these isolates **similar to things that already
exist, or are they divergent** (novel strains / a distinct lineage)? Back the conclusion
with your own evidence — identity values, alignments, and/or phylogenetic trees — not with
intuition.

---

## 3. Rules and constraints (please read carefully)

- **Everything is done from the command line.** All steps — QC, assembly, annotation,
  alignment, phylogenetics, **and the database searches you use to find candidate matches**
  — must be run from the terminal and be reproducible from your scripts. Do **not** use
  web-based BLAST, web submission forms, or GUI genome editors for the actual analysis.
  Query NCBI programmatically (e.g. BLAST+ with `-remote`, Entrez Direct / `efetch`,
  or the NCBI `datasets` CLI — your choice).
- **Use whatever tools you prefer** — but you must **justify every tool and key parameter
  choice in your report**. "I used X because Y" for each major step.
- **Reproducibility is graded.** Anyone should be able to re-run your scripts, in order,
  and regenerate every file in `04_results/`.
- **Do not edit the files in `01_Data/`.** Treat them as read-only inputs. Any
  filtered/trimmed/derived reads go in `03_analysis/`.
- **Log everything** (see `99_logs/` below). This is for auditing — we want to be able to
  see exactly what was run, with which versions, and when.

### One note on the data (please don't skip this)

These reads are **amplicon** reads: they come from a **single abutting-primer pair** on a
**circular** template. Before you reach for a general-purpose de novo assembler, **look at
your reads** — their length distribution and how (or whether) they overlap each other.
Think about what "assembly" even means for data shaped like this, and about how you will
represent a *circular* genome correctly (where does it start? are the ends handled?).
Choose your assembly/consensus strategy deliberately and **justify it in the report**.
There is more than one defensible route; a thoughtless one will mislead you.

---

## 4. Repository layout

You will organise all of your work into these folders. They are numbered in roughly the
order you will use them. Each folder has its own short `README.md` as a reminder.

| Folder | What lives here |
|---|---|
| `00_meta/` | Project metadata: the sample manifest, notes on your compute environment and **exact tool versions** (e.g. a `conda env export`), and any provenance notes. Descriptive info, **not** analysis outputs. |
| `01_Data/` | **Raw input reads — read-only.** The three FASTQ files. Never modify these. |
| `02_Scripts/` | **All of your code**, ideally numbered in execution order (e.g. `01_qc.sh`, `02_assembly.sh`, `03_annotation.sh`, …). Everything must be driven from scripts here. |
| `03_analysis/` | **Working / intermediate outputs**: QC reports, filtered reads, assembly/consensus attempts, alignments, BLAST hit tables, tree files, scratch work. This is your workbench. |
| `04_results/` | **Final, report-ready outputs only**: the three finished genome FASTAs, annotation tables, the whole-genome/Rep/CP identity matrices, final trees/figures, summary tables. These are the files your report points to. |
| `99_logs/` | **Logs for auditing.** Command logs, stdout/stderr from runs, timestamps, and a record of tool versions. Keep a running log so your whole process is traceable. |

A simple, auditable habit for `99_logs/`: wrap meaningful commands so their output is
captured, e.g.

```bash
# from the repo root
{ echo "### $(date -Is)  assembly run"; your_command --here; } 2>&1 | tee -a 99_logs/02_assembly.log
```

and record versions once per tool, e.g. `tool --version >> 99_logs/tool_versions.txt`.

---

## 5. Your report

Produce a report and place it at the **repository root as `REPORT.md`** (Markdown is
preferred so it renders directly on GitHub; if you strongly prefer another format, that is
acceptable, but Markdown is what we would like to read).

Your report should cover, at minimum:

- **Methods**, step by step, with the tools/versions used and a short **justification** for
  each major choice (especially your assembly/consensus strategy and how you handled
  circularity).
- **Per-isolate results**: genome length, GC%, ORF/annotation summary (Rep, CP, any
  conserved motifs), and sequencing/assembly quality evidence (e.g. coverage).
- **Closest GenBank matches** for each isolate: accession, description, % identity, and how
  you found them (from the command line).
- **Comparisons** at all three levels — **whole genome, Rep, CP** — as identity
  matrices and/or alignments, and phylogenetic trees if you build them.
- **Conclusion**: divergent vs. similar, with the evidence that supports it.
- A short **reproducibility** section: how to re-run everything from `02_Scripts/`.

---

## 6. How to work

1. **Clone** this repository and work **locally**.
2. Set up your environment, record it in `00_meta/`, and start capturing versions/logs.
3. Work through the stages, keeping scripts in `02_Scripts/`, intermediates in
   `03_analysis/`, and only polished outputs in `04_results/`. Log as you go.
4. Write `REPORT.md` at the repo root.
5. When finished, **push everything to your own GitHub repository** (public or shared with
   us) so we can review it, with `REPORT.md` in Markdown so it renders on GitHub.

Take your time, be rigorous, and make your reasoning visible. We are at least as interested
in *how* you work and justify your decisions as in the final numbers.

Good luck.
