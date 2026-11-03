# CRESS-DNA Virus Genome Recovery — Intern Task

**Omyx Lab · Bioinformatics internship exercise**

Welcome! This is a small, self-contained research project designed for you to learn by
doing. Everything you need to begin is already in this repository: three sets of raw
sequencing reads and this guide. Your job is to turn those reads into finished virus
genomes, describe them, and work out how they relate to viruses that scientists already
know about.

Please read this whole page slowly before you run anything. It explains the story behind
the data, what we're asking you to produce, and the few rules we'd like you to follow. It
does **not** tell you exactly how to do each step — figuring that out (by reading,
experimenting, and thinking) is the point. But it does explain enough that you won't feel
lost.

Take your time. We care more about *how* you think and justify your decisions than about
how fast you finish.

---

## 1. The story behind the data

A bit of vocabulary first, so the rest makes sense:

- **DNA sequencing** reads the order of letters (A, C, G, T) in genetic material. The
  machine doesn't hand you a whole genome — it gives you lots of short-to-medium fragments
  called **reads**. Piecing those reads back into the full genome is a step called
  **assembly**.
- A **virus genome** is just the virus's own set of genetic instructions. The viruses in
  this task have **circular** genomes (a loop, not a straight line) made of DNA, and they
  are small — roughly 2,000–2,400 letters long.
- These particular viruses belong to a big, loosely-related group often called
  **CRESS-DNA viruses** (short for *circular Rep-encoding single-stranded DNA* viruses).
  The name tells you three useful things: the genome is **circular**, it is
  **single-stranded DNA**, and it always carries a gene for a protein called **Rep**
  (short for *replication-associated protein* — the protein the virus uses to copy
  itself). Most of these viruses also carry a second gene for a **CP** (*capsid protein* —
  the protein shell that packages the genome). Rep and CP are the two genes you should
  expect to find.

Here's what happened in the lab:

A sequencing project was set up to look for a well-known group of plant viruses (the
*geminiviruses*). To boost the amount of circular viral DNA before sequencing, the samples
were treated with a method called **rolling-circle amplification (RCA)** — a trick that
makes many copies of any circular DNA present. The samples were then sequenced on an
**Oxford Nanopore** machine.

When the team did a first automated pass over the data, a handful of sequences did **not**
look like geminiviruses. A more sensitive search (using *profile/HMM* models, which detect
protein families even when the sequences are quite different) showed these mystery
sequences carried a **Rep** signature — the fingerprint of a CRESS-DNA virus. So: something
circular and Rep-encoding had turned up, but it was *not* what the project went looking for.

To capture these genomes properly, the team designed a special pair of PCR primers that
point **outward**, away from each other (these are called **abutting primers**). Because
the genome is a circle, two outward-facing primers will travel around the loop and meet on
the far side — amplifying the **entire circular genome** as one piece. That amplified piece
is called an **amplicon**. Each amplicon was then sequenced on nanopore.

**The reads from three of these samples are what you've been given** — one file per isolate:

| File (in `01_data/`) | Isolate |
|---|---|
| `isolate_A.fastq` | A |
| `isolate_B.fastq` | B |
| `isolate_C.fastq` | C |

We've deliberately given them plain names. We are **not** telling you the exact virus, its
host, or whether it's new to science — discovering that is part of your task.

---

## 2. What we'd like you to produce

For **each** of the three isolates, work towards these three things:

1. **A complete genome.** Turn the reads into one finished, closed **circular** genome.
2. **An annotation.** *Annotation* means marking where the genes are and what they are. Find
   the **open reading frames** (stretches of DNA that code for a protein — usually
   abbreviated **ORFs**), and label them — you should expect to find **Rep** and **CP**.
   Also report each genome's **length**, its **GC content** (the percentage of G and C
   letters), and look for the short conserved **motifs** (recognisable little sequence
   patterns) that CRESS-DNA Rep proteins are known to carry.
3. **Closest known relatives.** Search public sequence databases (**GenBank**, run by NCBI)
   to find the sequences most similar to yours, and note them down with their **accession
   numbers** (GenBank's unique IDs) and how similar they are (**percent identity**).

Then step back and **compare** — both the three isolates against *each other*, and against
the GenBank relatives you found — at **three levels**:

- the **whole genome** (all the DNA),
- the **Rep** gene/protein,
- the **CP** gene/protein.

Two common, beginner-friendly ways to show these comparisons are:

- an **identity matrix** — a simple table showing "sequence X is N% identical to sequence
  Y" for every pair, and
- a **phylogenetic tree** — a branching diagram (like a family tree) that shows which
  sequences are most closely related.

Finally, **reach a conclusion and say it clearly**: are your isolates basically the
**same as things that already exist**, or are they **divergent** (different enough to be
new strains or a distinct lineage)? Whatever you decide, back it up with your own
evidence — the identity numbers, the alignments, the tree — not with a hunch.

---

## 3. A few rules (please read these carefully)

1. **Do everything from the command line.** Every step — quality-checking the reads,
   assembly, annotation, comparing sequences, building trees, **and even the database
   searches you use to find relatives** — should be run in the terminal and saved as a
   script. Please don't use the point-and-click website version of BLAST, or any
   drag-and-drop genome program, for the real work. You can query NCBI from the command
   line in several ways (for example BLAST+ run with the `-remote` option, the Entrez
   Direct tools, or the NCBI `datasets` program) — which one you use is up to you.
   *Why we ask this:* command-line work is repeatable, auditable, and is the core skill
   we want you to build.

2. **Use whatever tools you like — but explain why.** There is no fixed toolkit. For each
   major step, write one line in your report saying what you used and why you chose it
   ("I used X for this step because Y"). Choosing tools and defending the choice is a real
   scientific skill.

3. **Make your work reproducible.** Someone else should be able to run your scripts, in
   order, and get the exact same final files. That's the standard to aim for.

4. **Never change the files in `01_data/`.** Treat them as read-only originals. If you
   filter, trim, or otherwise alter the reads, save those new versions in `03_analysis/`.

5. **Keep logs as you go** (see `99_logs/` below). We want to be able to look back and see
   exactly what you ran, with which tool versions, and when. This is normal good practice,
   not a sign we distrust you — real labs audit their own work the same way.

### One important note about the data — please don't skip this

The reads you've been given are **amplicon** reads: they all come from that **single pair
of abutting primers** on a **circular** template. That makes them a bit unusual compared to
ordinary sequencing data, and it affects how you should think about assembly.

So before you grab the first assembly program you find and press go, **look at your reads
first**. How long are they? Do they overlap each other the way you'd expect? What does it
even mean to "assemble" a circle, and where should a circular genome start and end? Let what
you observe guide which assembly or consensus strategy you choose — and then **explain that
choice in your report**. There is more than one sensible route to a good answer, and a
little thinking here will save you a lot of confusion later. (If your first attempt gives
something that looks wrong, that's normal — treat it as information, not failure.)

---

## 4. How this repository is organised

You'll keep all of your work inside these folders. They're numbered in roughly the order
you'll use them, and each one has its own short `README.md` as a reminder of what belongs
there.

| Folder | What goes in it |
|---|---|
| `00_meta/` | Background info *about* the project — the sample list, notes on your computer setup, and the **exact versions** of every tool you used (for example, the output of `conda env export`). Information, not analysis results. |
| `01_data/` | **The raw reads — read-only.** The three FASTQ files. Never edit these. |
| `02_scripts/` | **All your code**, ideally numbered in the order it should run (e.g. `01_qc.sh`, `02_assembly.sh`, `03_annotation.sh`, …). Everything you do should be driven by a script here. |
| `03_analysis/` | **Your workbench** — all the in-between files: quality reports, filtered reads, assembly attempts (including ones that didn't work — keep them!), alignments, search results, and so on. |
| `04_results/` | **Only the finished, report-ready outputs**: the three final genomes, annotation tables, the whole-genome/Rep/CP comparison tables, final trees or figures. These are the files your report will point to. |
| `99_logs/` | **Logs, for auditing.** A record of the commands you ran, their screen output, timestamps, and tool versions — so your whole process can be traced. |

A simple habit for keeping logs: run important commands like this (from the repository
root), which both shows the output on screen *and* saves it to a log file:

```bash
{ echo "### $(date -Is)  what this step does"; your_command --here; } 2>&1 | tee -a 99_logs/02_assembly.log
```

and save each tool's version once, like:

```bash
some_tool --version >> 99_logs/tool_versions.txt
```

---

## 5. Using AI tools (please read)

You're welcome to use AI assistants (ChatGPT, Claude, Copilot, and so on) during this task.
We use them too. But we'd like you to use them **as a companion, not as the driver** — and
here that's not just a style preference, it's part of what's being assessed.

What that means in practice:

- **The ideas and decisions must be yours.** You should be the one deciding *what* to do and
  *why*. Use AI to help you understand a concept, debug an error message, remember a command's
  syntax, or sanity-check your reasoning — not to hand you a ready-made pipeline you then run
  without understanding.
- **Steer the ship.** If you ever can't explain, in your own words, *why* a step is in your
  workflow, stop and go learn it before moving on. In your report and in any follow-up chat,
  we'll ask you to walk us through your choices — "because the AI said so" is not an answer
  that will hold up.
- **Never paste in something you don't understand.** AI tools confidently give wrong
  answers, out-of-date commands, and tools that don't exist. Treat every suggestion as a
  lead to check, not a fact. Verify it, test it, and make sure it actually fits *this* data.
- **Be honest about it.** In your report, add a short line or two on where AI helped you
  (e.g. "used an assistant to understand what a phylogenetic tree shows, and to debug a
  samtools error"). This is completely fine and we'd rather you were open about it.

Think of it this way: the goal of this exercise is to grow *your* skills and judgement. An
AI that does the thinking for you robs you of exactly the thing you're here to build. Let it
tutor you, not replace you.

---

## 6. Your report

When you're done, write up what you did as a report and save it at the **top level of the
repository as `REPORT.md`**. We'd really like it in **Markdown** (the `.md` format) because
it displays nicely on GitHub and is easy to read, it also helps you easily showcase your work.  But if you much prefer another format,
that's okay.

Your report should include, at least:

- **What you did, step by step**, with the tools and versions you used and a one-line
  **reason** for each major choice (especially how you assembled the genomes and how you
  handled the fact that they're circular).
- **For each isolate**: genome length, GC%, the ORFs/annotation (Rep, CP, any motifs you
  found), and some evidence that the genome is well-supported (for example, how deeply the
  reads cover it).
- **The closest GenBank matches** for each isolate: accession number, what it is, how
  similar it is, and how you found it from the command line.
- **Your comparisons** at all three levels, **whole genome, Rep, and CP**, shown as
  identity tables and/or trees.
- **Your conclusion**: similar or divergent, and the evidence behind it.
- A short **"how to reproduce this"** section explaining how to re-run everything from
  `02_scripts/`.

---

## 7. How to get going

1. **Clone** this repository to your own computer and work there. `git clone <this repo's URL>` and `cd` into it. We want to see your own work.
2. Set up the software you'll need, write down your setup and tool versions in `00_meta/`,
   and start keeping logs from the very first command.
3. Work through the stages: keep your code in `02_scripts/`, your in-between files in
   `03_analysis/`, and only the polished outputs in `04_results/`. Log as you go.
4. Write `REPORT.md` at the top level.
5. When you're finished, **push everything to your own GitHub repository** and share the
   link with us, with `REPORT.md` in Markdown so it reads nicely on GitHub.

If you get properly stuck for a while, that's normal — note down what you tried and what
confused you, and bring it to your check-in. Being able to describe a problem clearly is
itself a skill we're looking for.

Good luck — enjoy it!

---

## What you'll learn working on this project

By the time you finish, you'll have hands-on experience with every one of these:

**Working at the command line (Bash)**
- Navigating a Unix/Linux system and managing files from the terminal
- Writing Bash scripts, using pipes, redirection, loops, and chaining tools together
- Capturing output and keeping audit logs (`tee`, timestamps, version capture)

**Project organisation & reproducibility**
- Structuring a bioinformatics project cleanly (raw data vs. scripts vs. results)
- Keeping raw data read-only and making every result reproducible from scripts
- Recording your software environment and exact tool versions (e.g. conda)

**Version control & collaboration (Git & GitHub)**
- Cloning, committing, branching, and pushing with Git
- Publishing and sharing work on GitHub
- Writing clear documentation and a report in Markdown

**Sequencing data literacy**
- Understanding FASTQ files, long-read (Oxford Nanopore) data, and quality scores
- Quality-checking reads and inspecting read-length and coverage/depth

**Genome assembly**
- What assembly actually is, and how to think critically about read structure
- Long-read assembly and consensus-building, and why amplicon/circular data needs care
- Handling circular genomes correctly and polishing a draft to a finished sequence

**Genome annotation & characterisation**
- Finding open reading frames and identifying genes (Rep, CP)
- Detecting conserved sequence motifs and computing genome stats (length, GC%)

**Sequence databases & comparison**
- Querying NCBI/GenBank programmatically from the command line (BLAST+, Entrez Direct, `datasets`)
- Interpreting BLAST hits, accession numbers, and percent identity

**Alignment & phylogenetics**
- Building multiple sequence alignments and pairwise identity matrices
- Constructing and interpreting phylogenetic trees (model choice, support values, rooting)

**Scientific thinking & communication**
- Forming a question, justifying every tool and parameter choice, and reaching an evidence-based conclusion (divergent vs. similar)
- Writing up methods and results so others can follow and reproduce them

**Responsible AI use**
- Using AI as a tutor and companion rather than a crutch, verifying its output, and documenting where you used it

**Independence & problem-solving**
- Reading documentation, troubleshooting errors, and working through ambiguity on your own
