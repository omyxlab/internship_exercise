# CRESS-DNA Virus Genome Recovery — Report

<!--
This is a skeleton for you to fill in. Replace every _(…)_ prompt and <!-- … --> note
with your own content, and delete the guidance comments as you go. Keep it in Markdown so
it renders on GitHub. Add, remove, or reorder sections if your work calls for it — this is
a starting point, not a cage.
-->

**Author:** _(your name)_
**Date:** _(YYYY-MM-DD)_
**Repository:** _(link to your GitHub repo)_

---

## Summary

_(3–5 sentences: what you set out to do, what you did, and your main finding — are the
three isolates similar to known viruses or divergent? Write this last, once you know the
answer.)_

---

## 1. Environment & tools

_(List everything you used so the work can be reproduced. Record versions — not "latest".)_

| Step | Tool | Version | Why you chose it |
|---|---|---|---|
| Quality control | _(…)_ | _(…)_ | _(…)_ |
| Read filtering | _(…)_ | _(…)_ | _(…)_ |
| Assembly / consensus | _(…)_ | _(…)_ | _(…)_ |
| Polishing | _(…)_ | _(…)_ | _(…)_ |
| Annotation | _(…)_ | _(…)_ | _(…)_ |
| Database search | _(…)_ | _(…)_ | _(…)_ |
| Alignment | _(…)_ | _(…)_ | _(…)_ |
| Phylogenetics | _(…)_ | _(…)_ | _(…)_ |

_(How to reproduce your environment — e.g. the `conda env export` file in `00_meta/`.)_

---

## 2. Methods

_(Walk through what you did, step by step, in enough detail that someone could follow it.
For each major step, give a one-line reason for your choice. Point to the script in
`02_scripts/` that runs each step.)_

### 2.1 Quality control and read inspection
_(What do the reads look like — numbers, length distribution, quality? What did you notice
about them, and what did that tell you?)_

### 2.2 Assembly / consensus
_(How did you turn reads into a genome? **Explain and justify your strategy here** —
especially given the kind of data this is — and say how you made sure the final genome is
circular and starts/ends sensibly. If your first approach didn't work, say so and what you
changed.)_

### 2.3 Annotation
_(How did you find and label the ORFs (Rep, CP) and any conserved motifs?)_

### 2.4 Finding relatives in GenBank
_(How did you search GenBank from the command line, and how did you pick the closest
matches?)_

### 2.5 Comparison and phylogenetics
_(How did you compare sequences — alignments, identity matrices, trees — at the
whole-genome, Rep, and CP levels?)_

---

## 3. Results

### 3.1 Per-isolate summary

| Isolate | Genome length (bp) | GC % | ORFs found | Mean coverage | Closest GenBank hit (accession) | % identity |
|---|---|---|---|---|---|---|
| isolate_A | _(…)_ | _(…)_ | _(…)_ | _(…)_ | _(…)_ | _(…)_ |
| isolate_B | _(…)_ | _(…)_ | _(…)_ | _(…)_ | _(…)_ | _(…)_ |
| isolate_C | _(…)_ | _(…)_ | _(…)_ | _(…)_ | _(…)_ | _(…)_ |

### 3.2 Annotation details
_(For each isolate: the Rep and CP ORFs — coordinates, strand/orientation, lengths — and
any conserved motifs you detected. A figure of each genome map is welcome.)_

### 3.3 Closest GenBank matches
_(A short table or list per isolate: accession, description, % identity, and query
coverage. Note how you found them.)_

### 3.4 Comparisons

**Whole-genome identity**
_(Identity matrix across the three isolates and relevant GenBank sequences.)_

**Rep**
_(Identity matrix and/or tree for the Rep gene/protein.)_

**CP**
_(Identity matrix and/or tree for the CP gene/protein.)_

_(Embed or link to your final figures/trees in `04_results/`.)_

---

## 4. Conclusion

_(State it plainly: are the isolates similar to existing viruses, or divergent? Are the
three isolates alike or different from each other? Back every claim with the evidence
above — identity values, alignments, tree placement. It's fine to note what remains
uncertain.)_

---

## 5. How to reproduce this

_(The order to run the scripts in `02_scripts/`, and anything a reader needs to know to get
from the raw reads in `01_data/` to the files in `04_results/`.)_

```bash
# example
# bash 02_scripts/01_qc.sh
# bash 02_scripts/02_assembly.sh
# ...
```

---

## 6. Use of AI tools

_(A short, honest note: did you use any AI assistants, and for what — understanding a
concept, debugging, checking syntax? See the README for how we'd like AI used on this
task.)_

---

## 7. References

_(Papers, tools, and documentation you relied on.)_
