---
marp: true
html: true
title: Approaches for Orthology, Annotation, and Cross-Species Comparisons
paginate: true
style: |
  section {
    --ink: #20281f;
    --canopy: #234d35;
    --canopy-light: #5d8a6c;
    --loam: #8c5a35;
    --loam-light: #c99a6c;
    --mist: #eef3ec;
    --paper: #fbfbf8;
    --rule: #d7e1d6;

    font-family: "Liberation Sans", Arial, sans-serif;
    color: var(--ink);
    padding: 56px 68px;
  }

  h1, h2, h3 {
    font-family: "Bitstream Charter", Georgia, serif;
    font-weight: normal;
    color: var(--canopy);
  }

  h1 {
    font-size: 1.55em;
    line-height: 1.25;
    border-bottom: 3px solid var(--rule);
    padding-bottom: 0.3em;
  }

  h2 {
    font-size: 1.3em;
    border-bottom: 2px solid var(--rule);
    padding-bottom: 0.2em;
  }

  ul, ol { line-height: 1.5; }
  ul > li::marker { color: var(--canopy); }
  strong { color: var(--canopy); }

  code {
    font-family: "Liberation Mono", monospace;
    background: var(--mist);
    color: var(--canopy);
    border-radius: 4px;
    padding: 0.1em 0.4em;
  }

  pre {
    background: var(--mist);
    border-left: 4px solid var(--canopy);
    padding: 0.8em 1em;
    border-radius: 4px;
    font-size: 0.78em;
  }

  pre code { background: none; padding: 0; }

  a {
    color: var(--canopy);
    text-decoration: none;
    border-bottom: 1px solid var(--canopy-light);
  }

  blockquote {
    background: var(--mist);
    border-left: 5px solid var(--canopy);
    margin: 0.6em 0;
    padding: 0.5em 1em;
    color: var(--ink);
  }

  blockquote > p { margin: 0; }

  table { border-collapse: collapse; font-size: 0.85em; width: 100%; }

  table th {
    background: var(--canopy);
    color: var(--paper);
    font-family: "Bitstream Charter", Georgia, serif;
    font-weight: normal;
    padding: 0.4em 0.8em;
    text-align: left;
  }

  table td { border-bottom: 1px solid var(--rule); padding: 0.4em 0.8em; }
  table tr:nth-child(even) td { background: var(--mist); }

  footer {
    font-family: "Bitstream Charter", Georgia, serif;
    font-style: italic;
    color: var(--canopy-light);
    font-size: 0.55em;
  }

  section::after {
    font-family: "Liberation Sans", Arial, sans-serif;
    color: var(--canopy-light);
  }

  section.lead { background: var(--mist); }

  section.lead h1 {
    font-size: 2em;
    border-bottom: none;
    margin-bottom: 0.6em;
  }

  section.lead h1::after {
    content: "";
    display: block;
    width: 90px;
    height: 4px;
    background: var(--canopy);
    margin-top: 0.5em;
  }

  section.section-break {
    background: var(--canopy);
    color: var(--paper);
  }

  section.section-break h1 {
    color: var(--paper);
    border-bottom-color: var(--canopy-light);
    font-size: 2em;
  }

  section.section-break p { color: #b7e4c7; }

  .columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.2rem;
    align-items: start;
  }

  .note {
    background: var(--mist);
    border-left: 5px solid var(--canopy);
    border-radius: 4px;
    padding: 0.5em 1em;
    margin: 0.6em 0;
    font-size: 0.9em;
  }
  table.noborder,
    table.noborder th,
    table.noborder td {
      border: none !important;
      background: none !important;
    }
  table.noborder tr:nth-child(even) td { background: none !important; }
---

<!-- _class: lead -->

# Approaches for Orthology, Annotation, and Cross-Species Comparisons

**Resources for Plant Sciences: Data Integration and Interpretation Tools**

Tutorial Series · Systems biology, Multi-omics Integration and Modelling 
GitHub: `NIB-SI/ECCB2026_T12` · 2026

---

## Agenda

1. **Introduction**
2. **Plants & polyploidy**
3. **Why this?**
4. **Data sources**
5. **Tools**
6. **Integration & annotation**
7. **Workflow & summary**

<!-- > Scripts: `scripts/00_protocol.Rmd` · Repository: `skm-translate` -->

---

<!-- _class: section-break -->

# Introduction

Orthology and multiple methods

---

## What is Orthology?

**Homology** = evolutionary relatedness between sequences.

| Type | Definition | Evolutionary event |
|------|-----------|-------------------|
| **Orthologs** | Same gene in different species | Speciation |
| **Paralogs** | Duplicated genes within a genome | Gene/genome duplication |
| **Homeologs** | Orthologs from polyploidisation | Allopolyploidy |
| **Xenologs** | Horizontally transferred genes | HGT |


> **Key principle:** Orthologs tend to retain function across species — this is the basis for transferring annotations from well-studied references (e.g. *Arabidopsis thaliana*) to less-studied crops

---

## Why Multiple Methods?

No single tool is best in all situations. Each captures a different signal:

| Signal | Method |
|--------|--------|
| Sequence similarity | RBH, OrthoFinder, OrthoDB |
| Gene tree reconciliation | Ensembl Compara, PLAZA, FastOMA |
| Genome synteny / collinearity | JCVI / MCScanX |
| Functional classification | MapMan4 / Mercator7 |

**Strategy:** run all methods independently, then **integrate and filter** by priority and consensus — giving the most reliable set of ortholog pairs.

> `Synteny → Phylo-trees from MSAs → pairwise sequence similarity`

---

<!-- _class: section-break -->

# Plants & Polyploidy

Complexity

---

## Plant Genomes

Plants have some of the most **complex and dynamic genomes** of any organism:

- **Massive size range:** 135 Mbp (*Arabidopsis*) → 16 Gbp (wheat)
- **High repeat content:** up to 80–90% transposable elements in large genomes
- **Frequent polyploidy:** whole-genome duplications (WGD) throughout plant evolution
- **Ongoing gene family expansion:** NBS-LRR resistance genes, cytochrome P450s, ERFs and other TFs
- **Structural variation:** extensive inversions and translocations between closely related species

---

## Plant Genomes

<table class="noborder" style="width:100%; table-layout:fixed;"><tr>
<td style="width:50%; vertical-align:top; padding-right:1em; padding-top:0;">
<strong>Consequences for bioinformatics:</strong>
<ul>
<li>Many-to-many ortholog relationships</li>
<li>High risk of confusing paralogs with orthologs</li>
<li>Synteny breaks down across large evolutionary distances</li>
</ul>
</td>
<td style="width:50%; vertical-align:top; padding-top:0;">
<strong>Why it matters here:</strong>
<ul>
<li>Most of our study species are polyploids</li>
<li><em>Arabidopsis</em> is misleadingly "simple" — it has also experienced ancient WGDs</li>
</ul>
</td>
</tr></table>

---

## Polyploidy

**Autopolyploidy** — duplication of the same genome
```
  A A  →  A A A A   (same species, chromosome doubling)
```

**Allopolyploidy** — hybridisation of two species + chromosome doubling
```
  A A + B B  →  A A B B   (hybrid, then doubling)
```

**Ancient / Paleopolyploidy** — old WGD followed by diploidisation
```
  All flowering plants share an ancient WGD, core eudicots share an additional γ WGD
```
> ~70% of all flowering plant species have a polyploid history. Arabidopsis has undergone at least 3 WGDs since the origin of land plants — it just diploidised very efficiently.

---

## Polyploidy

| Species | Ploidy | 2n | Notes |
|---------|--------|----|-------|
| *Arabidopsis thaliana* | Diploid | 10 | 3 ancient WGDs detectable |
| *Prunus* spp. | Diploid | 16 | Relatively simple; no recent WGD |
| *Pyrus communis* | Diploid | 34 | Rosaceae WGD; complex synteny with Malus |
| *Malus domestica* | Paleopolyploid | 34 | Recent WGD — most genes duplicated |
| *Vitis vinifera* | Diploid | 38 | γ WGD signal; slowly evolving genome |
| *Solanum lycopersicum* | Diploid | 24 | Solanaceae WGD (T) |
| *Solanum tuberosum* | Autotetraploid | 48 | up to 4 homeologous copies of every gene — major challenge |

---

## Consequences for Orthology

**Gene fates after WGD:**
- **Subfunctionalisation** — each copy retains part of the ancestral function
- **Neofunctionalisation** — one copy evolves a new function
- **Gene loss (fractionation)** — one copy silenced and eventually deleted
- **Dosage balance** — transcription factors and kinases often retained in pairs

---

<!-- _class: section-break -->

# Why ?

Context and motivation

---

## **FruitDiv** — *Exploiting the untapped potential of fruit tree wild diversity for sustainable agriculture*

<small>Horizon Europe · fruitdiv.eu</small>

**The problem:** Current fruit production relies on a handful of elite **cultivars** — high yield and shelf life, but declining resistance to pests and disease and vulnerable to climate change.

**Crop Wild Relatives (CWR)** of pome (*Malus*, *Pyrus*) and stone (*Prunus*) fruits hold untapped genetic diversity:
- Pest and disease resistance
- Drought and salinity tolerance
- Adaptability to marginal lands and fluctuating climates

---

## **FruitDiv** — *Exploiting the untapped potential of fruit tree wild diversity for sustainable agriculture*

> **Cross-species orthology** — to unlock stress tolerance traits from wild relatives and transfer functional knowledge into breeding programmes, we must first know which genes are equivalent across species


---

## **ADAPT** — *Accelerated Development of multiple-stress tolerant Potato*

<small>Horizon Europe · adapt.univie.ac.at </small>

**The problem:** Potato is a staple crop and a non-model polyploid extremely sensitive to environmental stress:
- Heat + drought → yield reduction, quality losses
- Flooding → entire harvest lost within 1–2 days


> Cross-species orthology links conserved signalling pathways from model species to potato

---

## The Knowledge Gap: Model to Crop

**The challenge:** Most molecular knowledge comes from model organisms — not from crops

```
  Arabidopsis thaliana          Cultivars
  ────────────────────          ──────────────────────
  Fully annotated               Partially annotated
  40+ years of research         Limited functional data
  Thousands of mutants          Few or no mutants
  Rich ontology annotations     Many genes = "unknown"
  Deep stress response data     Stress mechanisms not fully understood
  High-quality genome           T2T genomes are more an exception than a rule
```

---

## The Knowledge Gap: Model to Crop

**Orthology-based annotation transfer bridges this gap:**

1. Identify ortholog of Arabidopsis stress gene in potato / apple / peach
2. Transfer ontology terms, pathway membership, knowledge graph node assignment
3. Prioritise candidates for experimental validation
4. Feed into multi-omics research and pre-breeding programmes

> Without reliable orthologs, functional genomics in crops starts from zero every time

---

<!-- _class: section-break -->

# Data Sources

The genomes, databases, and reference resources



---

## Databases & Data Sources

<table class="noborder" style="width:100%; table-layout:fixed;"><tr>
<td style="width:65%; vertical-align:top; padding-right:1em;">
<table style="width:100%; table-layout:fixed; font-size:0.85em;">
<tr><th style="width:50%; background:var(--canopy); color:var(--paper);">Database</th><th style="background:var(--canopy); color:var(--paper);">Species / Focus</th></tr>
<tr><td><strong>ARAPORT11</strong></td><td><em>A. thaliana</em> reference</td></tr>
<tr><td><strong>GDR</strong></td><td>Rosaceae genomes</td></tr>
<tr><td><strong>SpudDB / UniTato</strong></td><td>Potato</td></tr>
<tr><td><strong>Sol Genomics</strong></td><td>Solanaceae</td></tr>
<tr><td><strong>Grapedia</strong></td><td>Grapevine T2T v5</td></tr>
<tr><td><strong>Phytozome</strong></td><td>Multiple genomes</td></tr>
<tr><td><strong>Ensembl Plants</strong></td><td>Compara homologies</td></tr>
<tr><td><strong>PLAZA</strong></td><td>Plant orthogroups</td></tr>
<tr><td><strong>OrthoDB</strong></td><td>Hierarchical OGs</td></tr>
</table>
</td>
<td style="width:35%; vertical-align:top;">
<blockquote>Mostly primary/representative transcripts<br>
(longest isoform) used throughout</blockquote>
</td>
</tr></table>

---

<!-- _class: section-break -->

# Orthology Tools

Complementary methods

---

## OrthoFinder

<table class="noborder" style="width:100%; table-layout:fixed;"><tr>
<td style="width:55%; vertical-align:top; padding-right:1em;">
Emms & Kelly (2019) <em>Genome Biology</em><br><br>
<strong>Approach:</strong> <br>
DIAMOND ultra-sensitive + <br>
MCL clustering + <br>
IQ-TREE gene trees<br><br>
<pre><code>orthofinder -f proteomes/ -M msa \
  -S diamond_ultra_sens -T iqtree \
  -t 28 -n orthofinder -o results/</code></pre>
<strong>Two modes:</strong> within-genome (paralogs) · between-taxa (orthologs)<br><br>
<strong>Post-processing:</strong> sources crossed within <em>ath</em>: Compara · OrthoDB · PLAZA
</td>
<td style="width:45%; vertical-align:middle; text-align:center;">
<img src="figs/OrthoFinder.png" width="320">
</td>
</tr></table>


---

## Ensembl Compara

<table class="noborder" style="width:100%; table-layout:fixed;"><tr>
<td style="width:50%; vertical-align:top; padding-right:1em;">
Herrero et al. (2016) <em>Genome Biology</em><br><br>
<strong>Approach:</strong> HMM (TreeFam) →<br>
MSA (M-Coffee/MAFFT) →<br>
TreeBeST →<br>
gene tree reconciliation vs NCBI taxonomy<br><br>
<strong>High-confidence filters:</strong><br>
· GOC ≥ 50<br>
· WGA ≥ 50<br>
· % identity ≥ 25<br>
<code style="word-break:break-all;">ftp.ebi.ac.uk/ensemblgenomes/pub/plants/release-X/tsv/ensembl-compara/homologies/</code>
</td>
<td style="width:45%; vertical-align:middle; text-align:center;">
<img src="figs/Compara.jpeg" width="350">
</td>
</tr></table>



---

## PLAZA

<table class="noborder" style="width:100%; table-layout:fixed;"><tr>
<td style="width:45%; vertical-align:top; padding-right:1em;">
Van Bel et al. (2022) <em>Nucleic Acids Research</em><br>
<strong>Approach:</strong> DIAMOND + Tribe-MCL → <br>
OrthoFinder → <br>
<strong>MAFFT</strong> → <br>
<strong>FastTree</strong> → <br>
<strong>Notung</strong> reconciliation
<table style="width:100%; font-size:0.85em;">
<tr><th>Code</th><th>Description</th></tr>
<tr><td><strong>TROG</strong></td><td>Tree-based ortholog — used here</td></tr>
<tr><td><strong>ORTHO</strong></td><td>Sequence cluster</td></tr>
<tr><td><strong>anchor_point</strong></td><td><code>i-ADHoRe</code> syntenic anchors</td></tr>
<tr><td><strong>BHIF</strong></td><td>Best hits + inparalogs</td></tr>
</table>
<strong>Genome evolution:</strong> <code>i-ADHoRe</code> collinearity · <code>PAML</code>+<code>CLUSTALW</code> Ks dating
</td>
<td style="width:45%; vertical-align:middle; text-align:center;">
<img src="figs/PLAZA.png" width="320">
</td>
</tr></table>

---

## FastOMA

<table class="noborder" style="width:100%; table-layout:fixed;"><tr>
<td style="width:45%; vertical-align:top; padding-right:1em;">
Majidian et al. (2025) <em>Nature Methods</em><br><br>
<strong>Approach:</strong> OMAmer k-mer placement → <br>
Linclust for novel families → <br>
bottom-up HOG merging (MAFFT + FastTree2)<br>
<pre><code>omamer mkdb --db Pentapetalae.h5 \
  --oma_path ./oma_path/ \
  --root_taxon "Pentapetalae" --nthreads 28
nextflow run FastOMA.nf -profile standard \
  --input_folder ./input \
  --omamer_db ./input/Pentapetalae.h5 \
  --max_cpus 64 -c ./input/my_custom.config</code></pre>
Ortholog pairs ath → species<br>
Custom config: <code>filter_gap_ratio = 0.3</code><br>
<code>nr_repr_per_hog = 12</code><br>
<code>force_pairwise_ortholog_generation = true</code>
</td>
<td style="width:45%; vertical-align:middle; text-align:center;">
<img src="figs/FastOMA.png" width="600">
</td>
</tr></table>



---

## OrthoDB / OrthoLoger

<table class="noborder" style="width:100%; table-layout:fixed;"><tr>
<td style="width:45%; vertical-align:top; padding-right:1em;">
Kuznetsov et al. (2023) · Tegenfeldt et al. (2025) <br>
<em>Nucleic Acids Research</em><br><br>
<strong>Approach:</strong> MMseqs2 → <br>
best-reciprocal-hits → <br>
BRHCLUS clustering, hierarchical HOGs<br><br>
<strong>Pipeline:</strong> <br>
OrthoFinder within-genome × OrthoDB pair table → <br>
merge on OrthoDB protein IDs → <br>
Cartesian join → <br>
gene-level pairs<br>

<blockquote>Used at <strong>eudicotyledons</strong> level — balances sensitivity and specificity</blockquote>
</td>
<td style="width:45%; vertical-align:middle; text-align:center;">
<img src="figs/OrthoDB_2.jpeg" width="320">
</td>
</tr></table>



---

## RBH (Reciprocal Best Hits)

<table class="noborder" style="width:100%; table-layout:fixed;"><tr>
<td style="width:45%; vertical-align:top; padding-right:1em;">
<strong>Approach:</strong> Bidirectional BLAST → <br>
keep only pairs that are each other's top hit<br>
1. Download BLAST from https://ftp.ncbi.nlm.nih.gov/blast/ <br>
and install locally<br>
2. Add path to blast /bin<br>
3. Install dependencies<br>
<pre><code>BiocManager::install(c("Biostrings", "GenomicFeatures", 
"GenomicRanges", "Rsamtools", "IRanges", "rtracklayer", 
"biomaRt"))</code></pre>
4. Install <code>metablastr</code> and <code>orthologr</code> packages in R<br>
<pre><code>devtools::install_github("HajkD/metablastr")
devtools::install_github("HajkD/orthologr")</code></pre>
5. Run in R Console to see if it is working<br>
<pre><code>system("blastp -help")</code></pre>

</td>
<td style="width:45%; vertical-align:middle; text-align:center;">
<img src="figs/RBH.jpg" width="280"><br>
<blockquote>RBH is not a logically sufficient condition for orthology — supporting vote only.</blockquote>
</td>
</tr>
</table>



---

## Synteny Analysis — JCVI / MCScanX

<table class="noborder" style="width:100%; table-layout:fixed;"><tr>
<td style="width:50%; vertical-align:top; padding-right:1em;">
Tang et al. (2024) <em>iMeta</em><br><br>
<strong>Approach:</strong> LAST adaptive seeds →<br>
C-score filtering →<br>
proximal duplicate removal →<br>
<code>.anchors</code> synteny blocks<br>

<pre><code>python -m jcvi.formats.gff bed \
  --type=mRNA --key=Name X.gff3.gz -o X.bed
python -m jcvi.formats.fasta format X.cds.fa.gz X.cds
python -m jcvi.compara.catalog ortholog ath species \
  --no_strip_names
python -m jcvi.graphics.dotplot ath.species.anchors
python -m jcvi.graphics.karyotype seqids layout</code></pre>
process <code>.lifted.anchors</code>
</td>
<td style="width:50%; vertical-align:middle; text-align:center;">
<img src="figs/JCVI.jpg" width="500">
</td>
</tr></table>

---

<!-- _class: section-break -->

# Integration & Annotation

Combining six methods into a final ortholog set

---

## Integration Strategy — Filter Logic

Results from all six tools combined with a **priority-based filter**:

```
Priority order:
  1. MCScanX        ← synteny (highest confidence)
  2. Ensembl Compara
  3. PLAZA
  4. OrthoDB  ┐
  5. RBH      ├── require ≥ 2/3 agreement
  6. FastOMA  ┘    OR MapMan4 corroboration

Rule: once a from_geneID is assigned, it cannot be reassigned
      (covered_genes set — prevents double-counting)
```


---

## Annotation — MapMan4 / Mercator7

**Mercator v4.x** (web tool) — assigns **MapMan4 BINs**: hierarchical functional classification for plants

**Input:** per-species protein FASTA → `Y_Mercator4vX_results.txt`
**Output columns:** `IDENTIFIER` · `BINCODE` · `NAME` · `DESCRIPTION`

Normalise IDs + collapse multiple BINs per gene


Each species required custom ID regex normalisation (case fixes, isoform stripping, DMT→DMG for stu)

> MapMan4 brings **independent biological context** — a single method's ortholog call becomes credible when both genes share the same functional BIN

---

## From Ortholog Table to Biological Insight


The final ortholog table feeds directly into downstream biology:

- Orthologs from Arabidopsis carry established pathway membership
- Each gene in potato / fruit trees mapped to knowledge graph nodes
- Enables cross-species network comparison
- Candidate stress/resistance genes identified in wild relatives
- Supports GWAS and QTL candidate gene annotation



---

<!-- _class: section-break -->

# Workflow & Summary

---

## Full Pipeline Overview

<!-- <div style="overflow:hidden; height:50px;">
<img src="figs/pipeline.png" style="height:50px; width:auto;">
</div> -->

![w:750](figs/pipeline.svg)

---

## Summary

**Six tools, one integrated result:**

| Tool | Type | Priority |
|------|------|----------|
| JCVI / MCScanX | Synteny | ★★★★★ |
| Ensembl Compara | Tree-based | ★★★★☆ |
| PLAZA 5.0 | Tree-based, plant-specific | ★★★★☆ |
| OrthoDB / OrthoLoger | Hierarchical OGs | ★★★☆☆ |
| RBH | Sequence similarity | ★★★☆☆ |
| FastOMA | HOG-based, scalable | ★★★☆☆ |

---

## Summary

**Why it matters:**
- FruitDiv needs reliable orthologs to exploit CWR diversity in *Prunus* / *Malus* / *Pyrus*
- ADAPT needs cross-species SKM connections to transfer potato stress knowledge
- Plant polyploidy makes this hard — synteny + consensus approach handles it robustly

> The pipeline is fully scripted in R (`scripts/skm-tools`). 
All steps are reproducible.


---

## Summary


![w:500](figs/e.g..png)

---

## Resources & References

<table class="noborder" style="width:100%; table-layout:fixed;"><tr>
<td style="width:50%; vertical-align:top; padding-right:1em; padding-top:0;">
<strong style="display:block; margin-top:0;">Tools</strong>
<ul>
<li>JCVI/MCScanX: github.com/tanghaibao/jcvi</li>
<li>OrthoFinder: github.com/davidemms/OrthoFinder</li>
<li>FastOMA: github.com/DessimozLab/FastOMA</li>
<li>PLAZA: bioinformatics.psb.ugent.be/plaza/</li>
<li>OrthoDB: orthodb.org</li>
<li>Mercator7/MapMan4: www.plabipd.de/mercator_download.faces</li>
</ul>
</td>
<td style="width:50%; vertical-align:top; padding-top:0;">
<strong style="display:block; margin-top:0;">Key Papers</strong>
<ul>
<li>Tang et al. (2024) JCVI. <em>iMeta</em></li>
<li>Majidian et al. (2025) FastOMA. <em>Nature Methods</em></li>
<li>Van Bel et al. (2022) PLAZA 5.0. <em>Nucleic Acids Research</em></li>
<li>Emms & Kelly (2019) OrthoFinder. <em>Genome Biology</em></li>
<li>Tegenfeldt et al. (2025) OrthoDB v12. <em>Nucleic Acids Research</em></li>
</ul>
</td>
</tr></table>


<!-- ---

*End of module · Approaches for Orthology, Annotation, and Cross-Species Comparisons*

**Merge with other modules:**
```bash
cat 01_orthology_annotation.md 02_*.md > main_combined.md
``` -->
